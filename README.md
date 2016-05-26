# Probe Dock Clients

**Documentation and integration guide for [Probe Dock](https://github.com/probedock/probedock) clients.**

* [List of Clients](#clients)
* [List of Libraries](#libraries)
* [Setup Procedure](#setup-procedure)
* [Configuration Files](#configuration-files)
* [Environment Variables](#environment-variables)



<a name="clients"></a>
## List of Clients

Clients exist for the following test frameworks:

**Note:** Arquillian can use JUnit to write the tests and then the JUnit probe can be reused to collect the results.
* [Ruby Cucumber](https://github.com/probedock/probedock-cucumber-ruby)
* [Java Arquillian](https://github.com/probedock/probedock-junit) ([demo](https://github.com/probedock/probedock-demo-arquillian)) * [Java ITF Client](https://github.com/probedock/probedock-itf) ([demo](https://github.com/probedock/probedock-demo-itf))
* [Java JUnit Client](https://github.com/probedock/probedock-junit) ([demo](https://github.com/probedock/probedock-demo-junit))
* [Javascript Jasmine (with Grunt.js)](https://github.com/probedock/probedock-grunt-jasmine) ([demo with protractor](https://github.com/probedock/probedock-demo-grunt-jasmine-protractor))
* [JavaScript Karma Client](https://github.com/probedock/probedock-karma) ([demo](https://github.com/probedock/probedock-demo-karma))
* [Ruby RSpec Client](https://github.com/probedock/probedock-rspec) ([demo](https://github.com/probedock/probedock-demo-rspec))
* [Python nose2 Client](https://github.com/probedock/probedock-nose2) 
* [Python pytest Client](https://github.com/probedock/probedock-pytest)
* [Python unittest Client](https://github.com/probedock/probedock-py-unittest)



<a name="libraries"></a>
## List of Libraries

The following libraries can be used to develop new clients:

* [Grunt.js](https://github.com/probedock/probedock-grunt)
* [Java](https://github.com/probedock/probedock-java)
* [Node.js](https://github.com/probedock/probedock-node)
* [Python](https://github.com/probedock/probedock-python)
* [Ruby](https://github.com/probedock/probedock-ruby)



<a name="setup"></a>
<a name="setup-procedure"></a>
## Setup Procedure

This procedure documents the minimal setup for a standard Probe Dock client.

Create the `~/.probedock/config.yml` configuration file (in your **home directory**).
This file lists the Probe Dock servers you can publish results to, along with your API credentials for these servers.
This information is personal and should not be committed into your project repository, hence why it should be in the home configuration file.

```yml
servers:
  probedock.example.com:                         # Probe Dock server identifier (e.g. domain name)
    apiUrl: https://probedock.example.com/api    # Probe Dock server API URL
    apiToken: mwpqvvmagzoegxnqas45nfnptxdaxkxo   # A personal API access token which you can generate from your profile page in Probe Dock

publish: true
```

In the **project directory** where you run your tests, create the `probedock.yml` client configuration file.
This file is used to identify your project and select the default Probe Dock server you want to publish to.
You should commit this file into the project repository.

```yml
project:
  apiId: 154sic93pxs0
  version: 1.2.3

server: probedock.example.com   # The identifier of one of the Probe Dock servers defined in the home configuration file
```

Read on to learn about other [configuration properties](#configuration-files).



<a name="configuration-files"></a>
## Configuration Files

Most Probe Dock probes use [YAML](http://yaml.org) files for configuration.

Probes will attempt to load two files if they exist:

* the home configuration file;
* the project configuration file.

The home configuration file is expected to be in the home directory of the user running the probes at `~/.probedock/config.yml`.

The project configuration file is usually expected to be in the root directory of the project being tested: `/path/to/your/project/probedock.yml`.
However, that's not always the case depending on the language or framework.
**Always check the documentation of each probe to know where the project configuration file should be located.**

Settings in the project configuration file always override those in the home configuration file.

Both files can contain all of the configuration properties.
However, since part of the configuration consists of personal credentials,
it is recommended to put these private settings in the home file,
and project-related settings in the project file.

You may use only one file if that fits your needs better,
for example on a continuous integration server. Also check out the
[$PROBEDOCK\_CONFIG](#environment-variables) environment variable
which can be used to override the path from which the project configuration
file is loaded.



### Full Configuration File

Here is an annotated example of a full configuration file.

```yml
# List of Probe Dock servers you can submit test results to.
servers:
  probedock.example.com:                 # A custom name identifying the Probe Dock server.
                                          # You can use this name to select which server to publish
                                          # to when there are multiple servers.
                                          # We recommend using the domain name where it is running.

    apiUrl: https://probedock.example.com/api   # The URL of the Probe Dock server's API.
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
workspace: tmp/probedock

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

  # When publishing test results to Probe Dock, Print a copy of the test payload in the console for debugging.
  # You can temporarily enable this at runtime by setting the PROBE_DOCK_PRINT_PAYLOAD environment variable to 1.
  print: false

# Use "server" to select which Probe Dock server to publish test results to.
# It should be one of the server names you defined in the configuration.
# You can temporarily switch to another server at runtime by giving another name in the PROBE_DOCK_SERVER environment variable.
server: probedock.example.com
```



<a name="environment-variables"></a>
## Environment Variables

The following environment variables can be used to control the behavior of Probe Dock probes at runtime.
They always take precedence over the corresponding setting in the configuration files.

### General configuration

* `PROBEDOCK_CONFIG` - *local path* - Path to the project configuration file (overrides the default path `./probedock.yml`).
* `PROBEDOCK_PUBLISH` - `0|1` - Disable (0) or enable (1) publishing of test results to Probe Dock.
* `PROBEDOCK_SERVER` - *server name* - Select which Probe Dock server to publish test results to (this must be one of the servers defined in the configuration files).
* `PROBEDOCK_WORKSPACE` - *local path*
* `PROBEDOCK_SAVE_PAYLOAD` - `0|1`
* `PROBEDOCK_PRINT_PAYLOAD` - `0|1`

### Test run report configuration

* `PROBEDOCK_TEST_REPORT_UID` - *string (max 100 characters)* - Group test results sent separately under the same test report in Probe Dock by setting the same test report UID when you run your tests.

### Server configuration

These variables can be used in a continuous integration environment to avoid storing the API URL and credentials in a file:

* `PROBEDOCK_SERVER_API_URL` - *URL* - URL of the Probe Dock server API to publish the test results to
* `PROBEDOCK_SERVER_API_TOKEN` - *string* - Bearer token to use to authenticate to the Probe Dock server API



## Contributing

* [Fork](https://help.github.com/articles/fork-a-repo)
* Create a topic branch - `git checkout -b feature`
* Push to your branch - `git push origin feature`
* Create a [pull request](http://help.github.com/pull-requests/) from your branch

Please add a changelog entry with your name for new features and bug fixes.



## License

This guide and its examples are licensed under the [MIT License](http://opensource.org/licenses/MIT).
See [LICENSE.txt](LICENSE.txt) for the full text.
