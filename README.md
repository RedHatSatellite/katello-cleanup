# katello-cleanup

automatically remove content-hosts that seem unsused

## Description

`katello-cleanup` can be used to remove stale content-hosts from Katello. It is needed when a machine gets re-deployed but the old content-host entry is not removed or when a machine was decomissioned on the VM/HW side, but never removed from Katello.

When run, `katello-cleanup` will execute the following steps:

* Iterate over all content hosts of your organization
* Verify that the hostname is not on the ignore list
* Check if the last checkin time is more than `age` seconds ago
* Check if there is another machine with a newer checkin time and the same hostname
* Remove the machine if it is either an older duplicate or too old by itself.

## Requirements

* [Ruby](https://www.ruby-lang.org/)
* [Apipie Bindings](https://github.com/Apipie/apipie-bindings)


## Options

* `-U`, `--uri=URI` URI to the Foreman/Katello
* `-t`, `--timeout=TIMEOUT` timeout (in seconds) for the API calls
* `-u`, `--user=USER` User to log in to Foreman/Katello
* `-p`, `--pass=PASS` Password to log in to Foreman/Katello
* `-o`, `--organization-id=ID` ID of the Organization
* `-c`, `--config=FILE` configuration in YAML format
* `-n`, `--noop` do not actually execute anything

## Configuration

`katello-cleanup` can be configured using an YAML file (`katello-cleanup.yaml` by default).

The configuration file consists of one main sections: `settings`.

The `settings` section allows to set the same details as the commandline options. Any options given on the command line will override the respective config file settings.

    :settings:
      :user: admin
      :pass: changeme
      :uri: https://localhost
      :org: 1
      :timeout: 300
      :age: 
      :ignore:
        - esx.*\.example\.com

The `age` and `ignore` settings have no command line name. `age` is the time in seconds since the last checkin of a machine to be considered stale. `ignore` is an array of regular expressions that will be matched against the hostnames of content hosts in Katello. Hostnames matching will not be removed even if the logic says they should be.
