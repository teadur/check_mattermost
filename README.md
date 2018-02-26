Check Mattermost Version
========================

Nagios/Icinga2 plugin to check the version of a running Mattermost instance
for available updates.

The currently runnung version is determined by the "X-Version-Id" HTTP header
sent by Mattermost. Available updates are determined from release branches of
the public Mattermost Community Edition server Git repository at GitHub:
https://github.com/mattermost/mattermost-server.git

Requirements
------------
  * POSIX shell environment
  * Git
  * Wget or Curl

