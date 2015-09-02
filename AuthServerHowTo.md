Please note, this page is now no longer valid due to me being in the process of a re-write. See the AuthServerV2 page for more up-to-date info.

# Introduction #

The radius authenticator exists in the subdirectory "authserver". It'll have a couple of goals:

  * connect to various backends for auth (i.e. AD, ldap, etc)
  * use multiple storage types (sqlite, mysql, ldap, ad)
  * allow for user based provisioning (self-provisioning)
  * simple deployment via a package (rpm, dpkg, etc)


What it will do day one:

  * simple web interface for provisioning users
  * simple web interface that takes over freeradius config (completely)
  * one-time url for users to get their tokens from
  * ability to add non-ga tokens (i.e. direct string input)
  * simple script that downloads (via apt-get) required components and gets gaas via svn for installation.
  * provide a radius server (Freeradius) to authenticate against (only passcodes)
  * more or less assumes its the only thing on the box and the box is relatively secure


How to install it right now:

  * dont use in production
  * create a directory for it somewhere; mkdir /opt/gaas for example
  * get the code, cd /opt/gaas; svn checkout http://ga4php.googlecode.com/svn/trunk/ ga4php
  * point apache at it (add something like "Alias /gaas /opt/gaas/ga4php/authserver/www" to the config
  * install freeradius
  * copy /opt/gaas/ga4php/control/freeradius-users /etc/freeradius/users (or whevere your freeradius configs are - backup users file first)
  * start authd - cd /opt/gaas/ga4php/authserver/authd/; php authd
  * create a base user; php /opt/gaas/ga4php/authserver/usercmd.php add username; php /opt/gaas/ga4php/authserver/usercmd.php setpass username password
  * login to the web interface
  * enjoy...