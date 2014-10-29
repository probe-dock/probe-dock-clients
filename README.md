# ROX Center Clients
<a name="readme"></a>

> Documentation and integration guide for [ROX Center](https://github.com/lotaris/rox-center) clients.

* [List of Clients](#clients)
* [Setup Procedure](#setup-procedure)
* [Configuration Files](#configuration-files)
* [Environment Variables](#environment-variables)

<a name="clients"></a>
## List of Clients

These clients can be used with testing frameworks in various languages to publish test results to a ROX Center server:

* [RSpec Client](https://github.com/lotaris/rox-client-rspec)
* [Karma Client](https://github.com/lotaris/rox-client-karma)
* [Jasmine (Grunt) Client](https://github.com/lotaris/rox-client-grunt-jasmine)
* [Jasmine (grunt-contrib-jasmine) Client](https://github.com/lotaris/rox-client-grunt-contrib-jasmine)
* [PHPUnit Client](https://github.com/lotaris/rox-client-phpunit)
* [XCTest Client](https://github.com/lotaris/rox-client-xctest)

The following libraries can be used to develop new clients:

* [Node.js Client Library](https://github.com/lotaris/rox-client-node)
* [Grunt Client Library](https://github.com/lotaris/rox-client-grunt)
* [Ruby Client Library](https://github.com/lotaris/rox-client-ruby)

<a name="setup-procedure"></a>
## Setup Procedure

This procedure documents the minimal recommended setup for a standard ROX Center client.

Create the `~/.rox/config.yml` configuration file (in your **home directory**).
This file lists the ROX Center servers you can publish results to, along with your credentials for these servers.
This information is personal and should not be committed into your project repository, hence why it should be in the home configuration file.


```yml
servers:
  rox.example.com:   # ROX Center server identifier (e.g. domain name)
    apiUrl: https://rox.example.com/api   # ROX Center API root URL
    apiKeyId: 39fuc7x85lsoy9c0ek2d        # Your API credentials (key & secret) on that ROX Center server
    apiKeySecret: mwpqvvmagzoegxnqptxdaxkxonjmvrlctwcrfmowibqcpnsdqd

publish: true
```

In the **project directory** where you run your tests, create the `rox.yml` client configuration file.
This file is used to identify your project and select the default ROX Center server you want to publish to.
You should commit this file into your project repository.

```yml
project:
  apiId: 154sic93pxs0
  version: 1.2.3

server: rox.example.com
```

<a name="configuration-files"></a>
## Configuration Files

ROX clients use [YAML](http://yaml.org) files for configuration.
The following files will be loaded if they exist:

* the home configuration file: `~/.rox/config.yml`;
* the project configuration file: `/path/to/your/project/rox.yml`.

Settings in the project configuration file always override those in the home configuration file.

Both files can contain all of the configuration properties.
However, since part of the configuration consists of personal credentials,
it is recommended to put these private settings in the home file,
and project-related settings in the project file.

### Full Configuration File

Here is an annotated example of a full configuration file.

```yml
# List of ROX Center servers you can submit test results to.
servers:
  rox.example.com:                        # A custom name identifying your ROX Center server.
                                          # You can use this name to select which server to publish
                                          # to when there are multiple servers.
                                          # We recommend using the domain name where you deployed it.

    apiUrl: https://rox.example.com/api   # The URL of your ROX Center server's API.
    apiKeyId: 39fuc7x85lsoy9c0ek2d        # Your user credentials on that server.
    apiKeySecret: mwpqvvmagzoegxnqptxdaxkxonjmvrlctwcrfmowibqcpnsdqd

# If "publish" is true, test results will be uploaded to ROX Center.
# Set it to false to temporarily disable publishing.
# You can change this at runtime from the command line by setting the
# ROX_PUBLISH environment variable to 0 (false) or 1 (true).
publish: true

# Project-related configuration.
project:
  apiId: 154sic93pxs0   # The API key of your project in the ROX Center server.
  version: 1.2.3

# Where the client should store its temporary files.
# The client will work without it but it is required for some features.
workspace: tmp/rox

# Test payload options.
payload:
  
  # Save a copy of the last test payload sent to the ROX Center server for debugging.
  # The file will be saved in <CLIENT>/servers/<SERVER_NAME>/payload.json, where CLIENT
  # is the name of the ROX client, e.g. rspec or phpunit, and SERVER_NAME is the name
  # of the ROX Center server the results were last sent to.
  # Temporarily enable at runtime by setting the ROX_SAVE_PAYLOAD environment variable to 1.
  save: false

  # If you track a large number of tests (more than a thousand), enabling this feature
  # will reduce the size of the JSON test payloads sent to ROX Center server by caching
  # test information that doesn't change often (such as the name).
  # Temporarily enable at runtime by setting the ROX_CACHE_PAYLOAD environment variable to 1.
  cache: false

  # Print a copy of the test payload sent to the ROX Center server for debugging.
  # Temporarily enable at runtime by setting the ROX_PRINT_PAYLOAD environment variable to 1.
  print: false

# Use "server" to select which ROX Center server to publish test results to.
# It should be one of the server names you defined in the configuration.
# Temporarily switch to another server at runtime by giving another name in the ROX_SERVER environment variable.
server: rox.example.com
```

<a name="environment-variables"></a>
## Environment Variables

The following environment variables can be used to control the behavior of a ROX Center client at runtime.
They always override the corresponding setting in the configuration files.

* `ROX_PUBLISH` - `0|1` - Disable (0) or enable (1) publishing of test results to the ROX Center server.
* `ROX_SERVER` - server name - Select which ROX Center server to publish test results to (this must be one of the servers defined in the configuration files).
* `ROX_TEST_RUN_UID` - custom identifier - Group test results sent separately under the same test run in the ROX Center server by setting the same test run UID when you run your tests.
* `ROX_WORKSPACE` - local path
* `ROX_SAVE_PAYLOAD` - `0|1`
* `ROX_CACHE_PAYLOAD` - `0|1`
* `ROX_PRINT_PAYLOAD` - `0|1`

## Contributing

* [Fork](https://help.github.com/articles/fork-a-repo)
* Create a topic branch - `git checkout -b feature`
* Push to your branch - `git push origin feature`
* Create a [pull request](http://help.github.com/pull-requests/) from your branch

Please add a changelog entry with your name for new features and bug fixes.

## License

**rox-client** is licensed under the [MIT License](http://opensource.org/licenses/MIT).
See [LICENSE.txt](LICENSE.txt) for the full text.
