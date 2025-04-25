
# vbulletin-3.x-crash

# ⚠️ Disclaimer
This writeup is meant to be purely educational and shed light on the effects of old, unupdated software, and mitigations to take


## Overview

In trying to automate friend requests on unknowncheats.me, I accidentally triggered a serverside overload that took the site offline when I clicked on my friends list through a tab at the top, as opposed to through the control panel. This writeup is meant to explore how relatively large-scale queries and lack of pagination can cause performance bottlenecks

It took 130,000 friend requests to do so, but that doesn't matter when 130k requests can be sent in about an hour or two.

![Screenhot](https://i.imgur.com/wIgjgjR.png)

## Key Details
This occured with vBulletin 3.8.7, but as far as I'm aware vB4 may also have this issue. vB5 and vB6 likely do not work, as they changed how friends work, and are likely more efficient anyway.

## Affected Endpoints

### ✅ Stable: [/profile.php?do=buddylist](https://gitlab.com/hub/vbulletin/-/blob/vBulletin-3.8.7/profile.php#L1497)

### ❌ Unstable: [/misc.php?do=buddylist](https://gitlab.com/hub/vbulletin/-/blob/vBulletin-3.8.7/misc.php#L103)

the misc.php endpoint does not paginate the results nor limit them in any way, while joining the two tables to sort for online status

profile.php does not however join with another table

# ❌ What Not to Do as an Administrator
* Don't leave end points like adding friends unadded 
* Ensure pagination on all queries, especially the random ones that nobody would use (anything a user can make and manage)
* Don’t run expensive database joins
* Don't use two decade old forum software :)
* Sequential ID's also make it very easy for someone to create a list of users
