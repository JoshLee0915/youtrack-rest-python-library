# YTClient - A light weight YouTrack REST API Client for Python
YTClient is a light weight HTTP client for interfacing with Jetbrain's YouTrack issue tracker via its REST API. This was based
lightly off of the original [youtrack-rest-python-library](https://github.com/JetBrains/youtrack-rest-python-library), but 
unlike the original project YTClient was built to be Python 3 compatible and use [YouTrack's new REST API](https://www.jetbrains.com/help/youtrack/standalone/youtrack-rest-api-reference.html#youtrack-api-based-tools) 
instead of the [old REST API](https://www.jetbrains.com/help/youtrack/standalone/deprecated-rest-api-reference.html) that the original project currently uses.

It should be noted that this fork was created for the [Tattler-Discord Issue Reporter](https://github.com/JoshLee0915/Tattler-DiscordIssueReporter)
and the current release only contains the minimum functionality that was needed for that project. Additional functionality outside
of this basic functionality is planned to be implemented in future versions of YTClient.

## Compatibility
This client library and the import scripts that use the library are compatible with Python 3.6+. Python 2.7 is not supported.

This library supports any YouTrack Standalone versions that supports the new [REST API](https://www.jetbrains.com/help/youtrack/standalone/youtrack-rest-api-reference.html#youtrack-api-based-tools)
as well as the current version of YouTrack InCloud. The REST API is enabled by default in all YouTrack installations.

## Getting Started
This package has been published to PyPI and can be installed with pip. 

`pip install YTClient`

Alternatively you can clone this repo and run setup.py.

`setup.py install`

## Authentication
YTClient currently only supports using a permanent token for authentication requests. You can generate your own permanent 
tokens in your user profile. For instructions, refer to the [YouTrack documentation](https://www.jetbrains.com/help/youtrack/standalone/Manage-Permanent-Token.html#obtain-permanent-token).
```python
from YTClient.YTClient import YTClient

# authentication request with permanent token
client = YTClient('https://instance_name.myjetbrains.com/youtrack/', token='perm:abcdefghijklmn')
```
This request requires that you specify the base URL of the target YouTrack server. For YouTrack InCloud instances, your 
base URL includes the trailing `/youtrack`, as shown in the previous example.

Once you have established a connection, your credentials are cached for subsequent requests.

You can reset the auth token at any time by using:

```python
client.set_auth_token()
```

## Supported Operations
Currently YTClient only supports the following operations:
- **create_issue** - Create a new issue
   - Args
      - **[REQ] project** - The project the issue belongs to
      - **[REQ] summary** - The summary line of the issue
      - **[OPT] description** - The description of the issue
      - **[OPT] additional_fields** - A dictionary of additional fields to set **(NOTE: I have had issues getting this to work. I suggest
        for now to use run_command to set additional fields).**
      - **[OPT] return_fields** - A list of fields that should be returned with the request. The issue ID is returned by default.
- **run_command** - Run a YouTrack command
   - Args
      - **[REQ] command** - The command to run. Uses a named tuple that contains the command to run (specified as query in the command tuple) 
        and a list of issues that the command is to run against.
      - **[OPT] return_fields** - A list of fields that should be returned with the request. The command ID is returned by default.
- **get_issues** - Get a list of issues that match the specified query
   - Args
      - **[REQ] query** - The query string to use
      - **[OPT] fields** - A list of fields that should be returned with the request. The issue ID is returned by default.
      - **[OPT] skip** - The number of records to skip
      - **[OPT] top** - The number of records to return
- **get_projects** - Get a list of all projects
   - Args
      - **[OPT] fields** - A list of fields that should be returned with the request. The project ID is returned by default.
      - **[OPT] skip** - The number of records to skip
      - **[OPT] top** - The number of records to return