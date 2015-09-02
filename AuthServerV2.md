# Introduction #

This page descibes the work going on in the re-write of the authentication server for v2 of the auth server code.

# Details #

The v2 re-write took place for several reasons:
  * Old code was messy
  * lots of limiting factors that were hard to code around
  * really wanted to concentrate on third party authenication (i.e AD)
  * needed easily extend-able code.
  * wanted to be able to allow for pins and self-enrollment.

The first step was figuring out a way of handling extensions to the code base in a sensible way such that adding backends wouldnt be a drama.

# AD implementation #

Originally, i wanted to house the entire data for a users authentication information within AD. This proved difficult as while there are places you could put this info in the existing schema they were not necessarily all that nice or well documented (extensionAttributex for eg). Which then led me down the path of extending either the user objectclass or writing my own objectclass. Doing this is not fun. Not because writing an LDAP schema is hard, but writing one you can use in a production AD environment is hard to fathom from a support perspective. I.e. will my schema be something microsoft say is good or bad? I could never really figure out whether its ok to just write a schema and use it or whether it needs to be signed off by MS for people to use. So i gave it a miss.

Its had some interesting side benefits though. If i store most of the info in a local db and only go to the AD when i need to, then i can be independent of the AD servers when it matters. What i am planning on doing is simply keeping "in sync" with AD, in that if AD is available every time someone tries to token authenticate it'll check for the user validity in AD and store/update the info locally (currently this only matters to whether the user is enabled/disabled). the benifit i see of this is that it makes me independent of the AD servers themselves, with the information stored locally I can still authenticate even if the information gets old quickly. Consider a scenario where a network or server fault kills the connection between my auth token system and the AD infrastructure. If your using the token to auth on your VPN system you stuffed, and considering your probably going to need the vpn when you have a system failure, your likely to appreciate the token system not being utterly dependant on it.

Still, its early days in the development, so time will tell how it goes. There are lots of code problems to yet be fixed as such.