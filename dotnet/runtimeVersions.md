---

copyright:
  years: 2015, 2019
lastupdated: "2019-12-18"
subcollection: "Dotnet"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}


# Runtime Versions
{: #runtime_versions}

## Supported versions
{: #supported_versions}

This buildpack supports the following versions, those marked as deprecated will be removed in a future buildpack release.  See [Microsoft's support statement for LTS and Current releases](https://www.microsoft.com/net/core/support).


### .NET SDK version

| .NET SDK version        | Default          |
|-------------------------|------------------|
| 3.0.101                 |   Yes            |
| 3.0.100                 |   No             |
| 2.2.402                 |   No             |
| 2.2.301                 |   No             |
| 2.1.802                 |   No             |
| 2.1.701                 |   No             |
| 2.1.606                 |   No             |
| 2.1.605                 |   No             |
| 2.1.509                 |   No             |
| 2.1.508                 |   No             |
| 2.1.403                 |   No             |



### .NET Core runtime versions

| .NET Core runtime version | Release type      |
|---------------------------|-------------------|
| 3.0.1                     | Current           |
| 2.2.8                     | Normal            |  
| 2.2.7                     | Normal            |
| 2.1.14                    | Normal            |
| 2.1.13                    | Normal            |


### .NET aspnetcore versions

| .NET aspnetcore version | Release type        |
|---------------------------|-------------------|
| 3.0.1                     | Current           |
| 2.2.8                     | Normal            |  
| 2.2.7                     | Normal            |
| 2.1.14                    | Normal            |
| 2.1.13                    | Normal            |




## Specifying the .NET SDK version

Control the .NET SDK version with an optional `global.json` file in the application's root directory. For example:
```
   {
      "sdk": {
        "version": "3.0.101"
      }
   }
```
{: codeblock}

If not specified, the latest MSBuild tooling is used.  To use project.json tooling, you can specify one of the listed project.json versions, but keep in mind that these versions will be removed in the future.  If the specified version is not included in the buildpack, the default version is used, and a warning appears in staging logs.
