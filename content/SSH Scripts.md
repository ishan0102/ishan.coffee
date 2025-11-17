---
title: SSH Scripts
date: 2023-10-08
tags:
  - evergreen
enableToc: true
---
**Always read and understand anything you copy into your terminal.**

I'll document the SSH scripts I regularly use here. I suggest creating a `.functions` file in your root directory, adding whichever functions you find interesting to it, and finally appending `source .functions` to your `.bash_profile` to initialize the functions on startup. You can also add these colors to the top of your `.functions` file since a couple functions use colors.
```sh
bold=$(tput bold)
underline=$(tput smul)
reset=$(tput sgr0)
red=$(tput setaf 1)
green=$(tput setaf 2)
blue=$(tput setaf 4)
```

## cdls
`cd` and `ls` at the same time. Useful if you're like me and always want to see what files are available when navigating the terminal.
```sh
function cdls() {
	builtin cd "$@" && ls
}
```

## mkvenv
Make a new Python virtual environment and activate it.
```sh
function mkvenv() {
    python3 -m venv venv
    source venv/bin/activate
    pip install --upgrade pip
}
```
## mkgit
Create a new Git repository locally and push it to GitHub. Requires having the [GitHub CLI](https://cli.github.com/) installed (run `brew install gh` to install with Homebrew).
```sh
#!/bin/bash

# Style variables
bold=$(tput bold)
blue=$(tput setaf 4)
red=$(tput setaf 1)
green=$(tput setaf 2)
reset=$(tput sgr0)

# Function to check if a command exists
command_exists() {
    command -v "$1" &> /dev/null
}

# Function to print a step message
print_step() {
    echo "${bold}${blue}=> $1${reset}"
}

# Function to print success message
print_success() {
    echo "${bold}${green}$1${reset}"
}

# Function to print error message
print_error() {
    echo "${bold}${red}$1${reset}"
}

# Function to initialize the git repository
initialize_git() {
    if ! command_exists git; then
        print_error "Git could not be found. Please install it before proceeding."
        return 1
    fi

    print_step "Initializing the git repository..."
    git init -b main
    git add .
    git commit -m "Initial commit"
    print_success "Git repository initialized successfully."
    
    # Setup Python project if applicable
    if [[ "$is_python_project" == "yes" ]]; then
        setup_python_project
    fi
}

# Function to setup a Python project
setup_python_project() {
    print_step "Setting up the Python project environment..."

    # Create .gitignore
    echo "venv/" >> .gitignore

    # Create the virtual environment and activate it
    python3 -m venv venv
    source venv/bin/activate
    print_success "Python virtual environment created and activated."

    # Create .pre-commit-config.yaml with the provided configuration
    cat << EOF > .pre-commit-config.yaml
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.4.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
  - repo: https://github.com/bwhmather/ssort
    rev: v0.11.6
    hooks:
      - id: ssort
  - repo: https://github.com/pycqa/isort
    rev: 5.12.0
    hooks:
      - id: isort
        args: ["--profile", "black", "--filter-files"]
  - repo: https://github.com/psf/black
    rev: 23.3.0
    hooks:
      - id: black
        args: ["--line-length", "120"]
EOF

    # Install pre-commit hooks
    pre-commit install
    print_success "Pre-commit hooks installed."
}

# Function to create a GitHub repository
create_github_repo() {
    if ! command_exists gh; then
        print_error "GitHub CLI could not be found. Please install it before proceeding."
        return 1
    fi

    print_step "Creating the GitHub repository..."
    if gh repo create "$directory" --source=. --remote=upstream --push --${visibility}; then
        print_success "Created the repo $directory on GitHub!"
    else
        print_error "Failed to create the GitHub repo. Please check your network connection and try again."
        return 1
    fi
}

# Main function to make a git repository
mkgit() {
    # Ask for the directory name
    echo -n "${bold}${blue}Please enter the directory name: ${reset}"
    read directory
    if test -d "$directory"; then
        echo "${bold}${red}Error: Folder already exists.${reset}"
        return 1
    fi

    # Make sure this is running in the Documents folder
    if ! [[ "$(pwd)" =~ ~/Documents.*$ ]]; then
        echo "${bold}${red}Error: Please navigate to the documents folder before running this script.${reset}"
        return 1
    fi

    # Create a local directory and enter it
    mkdir "$directory" && cd "$directory"

    # Ask for repository visibility
    echo -n "${bold}${blue}Should the repository be public or private? (public/private): ${reset}"
    read visibility
    while [[ "$visibility" != "public" && "$visibility" != "private" ]]; do
        echo "${bold}${red}Invalid input. Please enter either 'public' or 'private': ${reset}"
        read visibility
    done

    # Ask for README contents
    echo -n "${bold}${blue}What would you like the README to say?: ${reset}"
    read readme_content
    echo "$readme_content" > README.md

    # Default .gitignore contents
    echo ".vscode/" >> .gitignore

    # Check if it's a Python project
    echo -n "${bold}${blue}Is this a Python project? (yes/no): ${reset}"
    read is_python_project

    # Ask for vscode settings
    echo -n "${bold}${blue}Would you like to add a .vscode/settings.json file? (yes/no): ${reset}"
    read vscode_answer
    if [[ "$vscode_answer" == "yes" ]]; then
        mkdir -p ".vscode" && touch ".vscode/settings.json"
    fi

    # Initialize local git repo
    initialize_git

    # Prepare for linking local repo to GitHub
    create_github_repo
}
```

## sshgo
This is handy if you SSH into servers a lot from your terminal. It gives you a quick dropdown of your servers and uses the usernames and hostnames you provided ahead of time.
```sh
function sshgo() {
    # Define the colors and styles using ANSI escape codes
    local RED="\033[31m"
    local GREEN="\033[32m"
    local YELLOW="\033[33m"
    local CYAN="\033[36m"
    local BOLD="\033[1m"
    local RESET="\033[0m"

    # Define your SSH accounts as indexed arrays
    local labels=("1" "2" "3") # Labels for each account
    local descriptions=("GPU 1" "GPU 2" "Digital Ocean") # Descriptions for each account
    local commands=("user1@gpu1" "user2@gpu2" "root@123.123.123.123") # SSH commands

    # Print a pretty list of your SSH accounts
    echo -e "${GREEN}${BOLD}SSH Accounts:${RESET}"
    echo -e "${CYAN}-------------${RESET}"
    for i in "${!labels[@]}"; do
        echo -e "[${YELLOW}${BOLD}${labels[$i]}${RESET}] ${CYAN}${descriptions[$i]}${RESET}"
    done
    echo -e "${CYAN}-------------${RESET}"

    # Prompt the user to select an account
    read -p "Enter the number corresponding to the account you want to SSH into: " selection

    # Check if the entered number is valid
    if [[ ! " ${labels[@]} " =~ " ${selection} " ]]; then
        echo -e "${RED}Invalid selection. Please try again.${RESET}"
        return 1
    fi

    # Find the index of the selected label
    local index
    for i in "${!labels[@]}"; do
        if [[ "${labels[$i]}" == "${selection}" ]]; then
            index="$i"
            break
        fi
    done
    
    # SSH into the selected account
    echo -e "${GREEN}Connecting to ${descriptions[$index]}...${RESET}"
    ssh ${commands[$index]}
}
```

## refresh_dots
Updates a [dotfiles repository](https://github.com/ishan0102/dotfiles) with relevant dotfiles. Will need to change file paths to match your machine.
```sh
function refresh_dots() {
    # Go to root
    cd ~

    # Refresh Brewfile
    rm Brewfile
    HOMEBREW_NO_AUTO_UPDATE=1 brew bundle dump

    # Copy main dotfiles
    cp .bash_prompt .bash_profile .bashrc .aliases .exports .functions .gitconfig .gitignore_global .git-completion.bash .hushlogin .inputrc .vimrc Brewfile .tmux.conf Documents/code/dotfiles

    # VSCode
    cp ~/Library/Application\ Support/Code/User/{settings.json,keybindings.json} Documents/code/dotfiles/vscode
    cp -R ~/Library/Application\ Support/Code/User/snippets Documents/code/dotfiles/vscode
    cp ~/.vscode/extensions/extensions.json Documents/code/dotfiles/vscode

    # Go to dotfiles folder
    cd ~/Documents/code/dotfiles
}
```

## execution_time
A handy add-on to Bash shells showing you how long each command takes to run. Great way to start gaining intuition on the relative amounts of time functions take. May need to run `brew install coreutils`. Idea credit to [Swyx](https://twitter.com/swyx/status/1677180561411153920).
```sh
preexec() {
  timer=$(gdate +%s.%N)
}

precmd() {
  if [ -n "$timer" ]; then
    now=$(gdate +%s.%N)
    elapsed=$(echo "$now - $timer" | bc)
    timer_show=$(printf "%.2f" $elapsed)
    echo "Execution time: ${timer_show}s"
    unset timer
  fi
}
```