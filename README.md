Check Mattermost Version
========================

Nagios/Icinga2 plugin to check the version of a running Mattermost instance
for available updates.

The currently runnung version is determined by the "X-Version-Id" HTTP header
sent by Mattermost. Available updates are determined from release branches of
the public Mattermost Community Edition server Git repository at GitHub:
https://github.com/mattermost/mattermost-server.git

REQUIREMENTS
------------
  * POSIX shell environment
  * Git
  * `wget` or `curl`


EXAMPLE ICINGA2 CONFIGURATION
-----------------------------

### Command definition

```
/* Check Mattermost version
 */
object CheckCommand "mattermost" {
  import "plugin-check-command"

  command = [ PluginDir + "/check_mattermost" ]

  arguments = {
    "-u" = {
      value = "$mattermost_url$"
      description = "URL to the Mattermost instance."
    }
    "-p" = {
      set_if = "$mattermost_patch$"
      description = "Warn on patch version updates."
    }
  }
}
```


### Service definition example (using apply)

```
/* Check Mattermost version
 */
apply Service "mattermost-version" {
  check_command = "mattermost"
  // Mattermost minor versions change once a month. So it should be
  // sufficient to check once a day only.
  check_interval = 24h

  display_name = "Mattermost version"
  notes = "Checks Mattermost version for available updates."

  // Service variables from php definition.
  vars.mattermost_url = host.vars.mattermost_url
  if ( host.vars.mattermost_patch ) {
    vars.mattermost_patch = host.vars.mattermost_patch
  }

  // Application rules.
  assign where host.name = NodeName && host.vars.mattermost_url
}
```


### Host object definition

```
object Host "node.example.com" {

  // Other settings.

  vars.mattermost_url = "https://mattermost.example.com"
  vars.mattermost_patch = true
}
```


USAGE
-----

The script requires the URL to the Mattermost instance to check.

```shell
Usage: check_mattermost [-u <url>] [-p|--patch] [-h|--help] [-V|--version]

Options/switches:
  -u <url>               URL to the Mattermost instance.
  -p / --patch           Warn on patch version updates. This also checks
                         patch version tags, but may cause false warnings,
                         as patch versions are not always properly included
                         in Mattermost release version strings.
  -h / --help            Display help.
  -V / --version         Display version.
```


LICENSE
-------

[MIT License (MIT)](http://opensource.org/licenses/mit)

