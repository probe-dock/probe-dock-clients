# Probe Dock Clients

**Documentation and integration guide for [Probe Dock](https://github.com/probe-dock/probe-dock) clients.**

* [List of Clients](#clients)
* [List of Libraries](#libraries)
* [Setup Procedure](#setup-procedure)
* [Configuration Files](#configuration-files)
* [Environment Variables](#environment-variables)

<a name="clients"></a>
## List of Clients

Clients exist for the following test frameworks:

* [RSpec Client](https://github.com/probe-dock/probe-dock-rspec)
* [Junit Client](https://github.com/probe-dock/probe-dock-junit)

<a name="libraries"></a>
## List of Libraries

Libraries exist for the following test languages:

* [Java](https://github.com/probe-dock/probe-dock-java)

<a name="setup-procedure"></a>
## Setup Procedure

This procedure documents the minimal setup for a standard Probe Dock client.

Create the `~/.probe-dock/config.yml` configuration file (in your **home directory**).
This file lists the Probe Dock servers you can publish results to, along with your API credentials for these servers.
This information is personal and should not be committed into your project repository, hence why it should be in the home configuration file.

```yml
servers:
  probe-dock.example.com:                 # Probe Dock server identifier (e.g. domain name)
    apiUrl: https://probe-dock.example.com/api   # Probe Dock server API URL
    apiToken: mwpqvvmagzoegxnqas45nfnptxdaxkxo   # A personal API access token which you can generate from your profile page in Probe Dock

publish: true
```

In the **project directory** where you run your tests, create the `probe-dock.yml` client configuration file.
This file is used to identify your project and select the default Probe Dock server you want to publish to.
You should commit this file into the project repository.

```yml
project:
  apiId: 154sic93pxs0
  version: 1.2.3

server: probe-dock.example.com
```

Read on to learn about other [configuration properties](#configuration-files).

<a name="configuration-files"></a>
## Configuration Files

Most Probe Dock clients use [YAML](http://yaml.org) files for configuration.
The following files will be loaded if they exist:

* the home configuration file: `~/.probe-dock/config.yml`;
* the project configuration file: `/path/to/your/project/probe-dock.yml`.

Settings in the project configuration file always override those in the home configuration file.

Both files can contain all of the configuration properties.
However, since part of the configuration consists of personal credentials,
it is recommended to put these private settings in the home file,
and project-related settings in the project file.

### Full Configuration File

Here is an annotated example of a full configuration file.

```yml
# List of Probe Dock servers you can submit test results to.
servers:
  probe-dock.example.com:                 # A custom name identifying the Probe Dock server.
                                          # You can use this name to select which server to publish
                                          # to when there are multiple servers.
                                          # We recommend using the domain name where it is running.

    apiUrl: https://probe-dock.example.com/api   # The URL of the Probe Dock server's API.
    apiToken: mwpqvvma2rdagzoegxnqptxdaxkxonjm   # A personal API access token which you can generate from your profile page in Probe Dock.

# If "publish" is true, test results will be published to Probe Dock.
# Set it to false to temporarily disable publishing.
# You can override this at runtime from the command line by setting the
# PROBE_DOCK_PUBLISH environment variable to 0 (false) or 1 (true).
publish: true

# Project-related configuration.
project:
  apiId: 154sic93pxs0   # The API key of your project in Probe Dock.
  version: 1.2.3        # The current version of your project.

# Where the client should store its temporary files.
# The client will work without it but it is required for some features.
workspace: tmp/probe-dock

# Test payload options.
payload:

  # Save a copy of the last test payload sent to Probe Dock for debugging.
  # The file will be saved in <TMP_DIR>/<CLIENT>/servers/<SERVER_NAME>/payload.json,
  # where TMP_DIR is the optional "workspace" property, CLIENT is the name of the
  # Probe Dock client, e.g. rspec or phpunit, and SERVER_NAME is the name of the
  # Probe Dock server the results were last sent to.
  # You can temporarily enable this at runtime by setting the PROBE_DOCK_SAVE_PAYLOAD environment variable to 1.
  # This feature requires the "workspace" property to be set.
  save: false

  # If you track a large number of tests (more than a thousand), enabling this feature
  # will reduce the size of the JSON test payloads sent to Probe Dock by caching
  # test information that doesn't change often (such as the name).
  # You can temporarily enable this at runtime by setting the PROBE_DOCK_CACHE_PAYLOAD environment variable to 1.
  # This feature requires the "workspace" property to be set.
  cache: false

  # When publishing test results to Probe Dock, Print a copy of the test payload in the console for debugging.
  # You can temporarily enable this at runtime by setting the PROBE_DOCK_PRINT_PAYLOAD environment variable to 1.
  print: false

# Use "server" to select which Probe Dock server to publish test results to.
# It should be one of the server names you defined in the configuration.
# You can temporarily switch to another server at runtime by giving another name in the PROBE_DOCK_SERVER environment variable.
server: probe-dock.example.com
```

<a name="environment-variables"></a>
## Environment Variables

The following environment variables can be used to control the behavior of Probe Dock client at runtime.
They always take precedence over the corresponding setting in the configuration files.

* `PROBE_DOCK_PUBLISH` - `0|1` - Disable (0) or enable (1) publishing of test results to Probe Dock.
* `PROBE_DOCK_SERVER` - server name - Select which Probe Dock server to publish test results to (this must be one of the servers defined in the configuration files).
* `PROBE_DOCK_TEST_REPORT_UID` - custom identifier - Group test results sent separately under the same test report in Probe Dock by setting the same test report UID when you run your tests.
* `PROBE_DOCK_WORKSPACE` - local path
* `PROBE_DOCK_SAVE_PAYLOAD` - `0|1`
* `PROBE_DOCK_CACHE_PAYLOAD` - `0|1`
* `PROBE_DOCK_PRINT_PAYLOAD` - `0|1`

## Contributing

* [Fork](https://help.github.com/articles/fork-a-repo)
* Create a topic branch - `git checkout -b feature`
* Push to your branch - `git push origin feature`
* Create a [pull request](http://help.github.com/pull-requests/) from your branch

Please add a changelog entry with your name for new features and bug fixes.

## License

This guide and its examples are licensed under the [MIT License](http://opensource.org/licenses/MIT).
See [LICENSE.txt](LICENSE.txt) for the full text.
