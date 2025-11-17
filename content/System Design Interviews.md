---
title: System Design Interviews
date: 2023-10-24
tags:
  - seed
enableToc: true
---
## Notes
- It's a good idea to ask a lot of clarifying questions to avoid going down the wrong path:
	- who are the users?
	- how are they going to use it? 
	- what use cases are there?
	- what are the inputs and outputs? 
	- how much data do we expect to handle? 
	- how many requests per second do we expect?

Data types:
- Tradeoff with enums: more performant but cannot be removed

## Resources
### Design Parking Garage
YouTube: https://www.youtube.com/watch?v=NtMvNh0WFVM

### Design Tinder
YouTube: https://www.youtube.com/watch?v=iyLqwyFL0Zc

As always, the real alpha lies in the comments:
> I really like how she is driving the interview! However, the design seems to be very high-level and monolithic, not necessarily a system you can deploy at scale. I think the functional requirements need to be narrowed even further, because recommendation is a system design question on its own. We touched geosharding, but that discussion seemed out of place for something that was supposed to be high level.
> 
> I think back of the envelope estimations were a bit out of place, there was really no reason given for why those calculations were done. We calculated the amount of metadata storage we might need, but not much discussion other than that. No mention of whether this is a read heavy vs. write heavy system and what implications that has on the rest of the design. Do we need to be consistent or highly available? This is a very read-heavy service where people are swiping left and right like crazy, refreshing feeds all the time. You're going to need to be highly available, so someone that is new on the app might not have their profile readily seen until replicas pick them up. 
> 
> I'm thinking for the feed generation / finding other users in your area, you'd have some sort of document store that you can scale as you go (like Mongo), with support for queries / searching based on strings. We'd have location in the user's metadata, so you'd probably let Google S2 handle the geometry and get all the users in a particular user's location. Honestly you might even consider a Graph DB since you're essentially building a recommended network of users based on a bunch of preferences and biographical information. 
> 
> There seemed to be no goal in getting a complete, scalable system on the whiteboard, and the interviewer also didn't really do much to steer the candidate in the right direction unfortunately.

### Blind Post
This blind post is legendary: https://www.teamblind.com/post/Giving-back---how-I-cleared-L6-System-Design---Part-1-4yufM3RY

### Google SRE Workbook
Non-Abstract Large System Design: https://sre.google/workbook/non-abstract-design/