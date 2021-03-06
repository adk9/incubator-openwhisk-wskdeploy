<!--
#
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
-->

# Whisk Deploy `wskdeploy`

[![License](https://img.shields.io/badge/license-Apache--2.0-blue.svg)](http://www.apache.org/licenses/LICENSE-2.0)
[![Build Status](https://travis-ci.org/apache/incubator-openwhisk-wskdeploy.svg?branch=master)](https://travis-ci.org/apache/incubator-openwhisk-wskdeploy)

`wskdeploy` is a utility to help you describe and deploy any part of the OpenWhisk programming model using a Manifest file written in YAML. Use it to deploy all your OpenWhisk [Packages](https://github.com/apache/incubator-openwhisk/blob/master/docs/packages.md), [Actions](https://github.com/apache/incubator-openwhisk/blob/master/docs/actions.md), [Triggers, and Rules](https://github.com/apache/incubator-openwhisk/blob/master/docs/triggers_rules.md) using a single command!

`wskdeploy export --projectname managed_project_name` allows to "export" a specified managed project into a local file system. Namely, a `managed_project_name.yml` Manifest file will be created automatically. This Manifest file can be used with `wskdeploy` to redeploy the managed project at a different OpenWhisk instance. If the managed project contains dependencies on other managed projects, then these projects will be exported automatically into their respective manifests.

You can use this in addition to the OpenWhisk CLI.  In fact, this utility uses the [OpenWhisk "Go" Client](https://github.com/apache/incubator-openwhisk-client-go) to create its HTTP REST calls for deploying and undeploying your packages.

## Here are some quick links for:

- [Downloading wskdeploy](#downloading-released-binaries) - released binaries for Linux, Mac OS and Windows
- [Running wskdeploy](#running-wskdeploy) - run wskdeploy as a binary or Go program
- :eight_spoked_asterisk: [Writing Package Manifests](docs/programming_guide.md#wskdeploy-utility-by-example) - a step-by-step guide on writing Package Manifest files for ```wskdeploy```
- :eight_spoked_asterisk: [Exporting OpenWhisk assets](docs/export.md) - how to use `export` feature
- [Building the project](#building-the-project) - download and build the GoLang source code
- [Contributing to the project](#contributing-to-the-project) - join us!
- [Debugging wskdeploy](docs/wskdeploy_debugging.md) - helpful tips for debugging the code and your manifest files
- [Troubleshooting](#troubleshooting) - known issues (e.g., Git)

---

## Building the project

### GoLang setup

The wskdeploy utility is a GoLang program so you will first need to [Download and install GoLang](https://golang.org/doc/install) onto your local machine.

Make sure your `$GOPATH` is defined correctly in your environment. For detailed setup of your GoLang development environment, please read [How to Write Go Code](https://golang.org/doc/code.html).


### Get the source code from GitHub

Once your environment is setup, download `wskdeploy` and its dependencies:

```sh
$ cd $GOPATH
$ go get github.com/apache/incubator-openwhisk-wskdeploy  # see known issues below if you get an error
$ go get github.com/tools/godep # get the dependency manager
```

### Build the binary

Use the Go utility to build the ```wskdeploy``` binary as follows:
```sh
$ cd src/github.com/apache/incubator-openwhisk-wskdeploy/
$ godep restore
$ go build -o wskdeploy
```

### building for other Operating Systems (GOOS) and Architectures (GOARCH)

If you would like to build the binary for a specific operating system, you may add the arguments GOOS and GOARCH into the Go build command. You may set
- ```GOOS``` to "linux", "darwin" or "windows"
- ```GOARCH``` to "amd64" or "386"

For example, run the following command to build the binary for 64-bit Linux:

```sh
$ GOOS=linux GOARCH=amd64 go build -o wskdeploy
```

### build using GoDep tool

If you want to build with the godep tool, please execute the following commands.

```sh
$ go get github.com/tools/godep # Install the godep tool.
$ godep get                     # Download and install packages with specified dependencies.
$ godep go build -o wskdeploy   # build the wskdeploy tool.
```

<!-- ----------------------------------------------------------------------------- -->

## Running ```wskdeploy```

After building the wskdeploy binary, you can run it as follows:

#### Running the Binary file

Start by verifying the utility can display the command line help:
```sh
$ ./wskdeploy --help
```

then try deploying an OpenWhisk Manifest and Deployment file:
```sh
$ ./wskdeploy -m tests/usecases/triggerrule/manifest.yml -d tests/usecases/triggerrule/deployment.yml
```

#### Running as a Go program

Since ```wskdeploy``` is a GoLang program, you may choose to run it using the Go utility:
```sh
$ go run main.go --help
```

and deploying using the Go utility would look like:
```sh
$ go run main.go -m tests/usecases/triggerrule/manifest.yml -d tests/usecases/triggerrule/deployment.yml
```
<!-- ----------------------------------------------------------------------------- -->

## Downloading released binaries

Binaries of `wskdeploy` are available for download on the project's GitHub release page:
- [https://github.com/apache/incubator-openwhisk-wskdeploy/releases](https://github.com/apache/incubator-openwhisk-wskdeploy/releases).

For each release, we typically provide binaries built for Linux, Mac OS (Darwin) and Windows on the AMD64 architecture. However, we provide instructions on how to build your own binaries as well from source code with the Go tool.  See [Building the project](#building-the-project).

_If you are a Developer or Contributor, **we recommend building from the latest source code** from the project's master branch._

<!-- ----------------------------------------------------------------------------- -->

## Contributing to the project

Start by creating a fork of `incubator-openwhisk-wskdeploy` and then change the git `origin` to point to your forked repository, as follows:

```sh
$ cd $GOPATH/src/github.com/apache/incubator-openwhisk-wskdeploy
$ git remote rename origin upstream
$ git remote add origin https://github.com/<your fork>/incubator-openwhisk-wskdeploy
$ git fetch --all
$ git branch --set-upstream-to origin/master  # track master from origin now
```

You can now use `git push` to push changes to your repository and submit pull requests.

### Developers should use "go deps" and "go build" not "go get"

The Whisk deploy project is setup for development purposes and uses "go deps" for dependency management. We do NOT recommend using "go get" as this will use the latest dependencies for all imported GitHub repos. which is not supported.

- See: [https://github.com/tools/godep](https://github.com/tools/godep)

Specifically, for development please use:

```
$ git clone git@github.com:mrutkows/incubator-openwhisk-wskdeploy
$ go build
```

for end-users, please use versioned releases of binaries.
- [https://github.com/apache/incubator-openwhisk-wskdeploy/releases](https://github.com/apache/incubator-openwhisk-wskdeploy/releases)

### How to Cross Compile Binary with Gradle/Docker

If you don't want to bother with go installation, build, git clone etc, and you can do it with Gradle/Docker.

After compiling, a suitable wskdeploy binary that works for your OS platform will be available under /bin directory.

1. First you need a docker daemon running locally on your machine.

2. Make sure you have Java 1.7 or above installed.

3. Clone the wskdeploy repo with command ```git clone https://github.com/apache/incubator-openwhisk-wskdeploy.git```

4. If you use Windows OS, type ```gradlew.bat -version ```. For Unix/Linux/Mac, please type ```./gradlew -version```.

5. Make sure you can see the correct Gradle version info on your console. Currently the expected Gradle
version is 3.3.

6. For Windows type ```gradlew.bat distDocker```. For Linux/Unix/Mac, please type ```./gradlew distDocker```. These
commands will start the wskdeploy cross compile for your specific OS platform inside a Docker container.

7. After build success, you should find a correct binary under current /bin dir of you openwhisk-deploy clone dir.

If you would like to build the binaries available for all the operating systems and architectures, run the following command:

```sh
$ ./gradlew distDocker -PcrossCompileWSKDEPLOY=true
```

Then, you will find the binaries and their compressed packages generated under the folder ```bin/<os>/<arch>/``` for each supported Operating System and CPU Architecture pair.

### Building for Internationalization

Please follow this process for building any changes to translatable strings:
- [How to generate the file i18n_resources.go for internationalization](https://github.com/apache/incubator-openwhisk-wskdeploy/blob/master/wski18n/README.md)

<!-- ----------------------------------------------------------------------------- -->

## Troubleshooting

### Known issues

#### Git commands using HTTPS, not SSH

The "go get" command uses HTTPS with GitHub and when you attempt to "commit" code you might be prompted with your GitHub credentials.  If you wish to use your SSH credentials, you may need to issue the following command to set the appropriate URL for your "origin" fork:

```
git remote set-url origin git@github.com:<username>/incubator-openwhisk-wskdeploy.git
```

<or> you can manually change the remote (origin) url within your .git/config file:
```
[remote "origin"]
    url = git@github.com:<username>/incubator-openwhisk-wskdeploy
```

while there, you can verify that your upstream repository is set correctly:
```
[remote "upstream"]
    url = git@github.com:apache/incubator-openwhisk-wskdeploy
```

#### Git clone RPC failed: HTTP 301

This sometimes occurs using "go get" the wskdeploy code (which indirectly invokes "git clone").

<b>Note: Using "go get" for development is unsupported; instead, please use "go deps" for dependency management.</b>

You might get this error when downloading `incubator-openwhisk-wskdeploy`:

     Cloning into ''$GOAPTH/src/gopkg.in/yaml.v2'...
     error: RPC failed; HTTP 301 curl 22 The requested URL returned error: 301
     fatal: The remote end hung up unexpectedly

This is caused by newer `git` versions not forwarding requests anymore. One solution is to allow forwarding for `gopkg.in`

```
$ git config --global http.https://gopkg.in.followRedirects true
```

## Creating Tagged Releases

Committers can find instructions on how to create tagged releases here:
- [creating_tagged_releases.md](https://github.com/apache/incubator-openwhisk-wskdeploy/tree/master/docs/creating_tagged_releases.md)

# Disclaimer

Apache OpenWhisk Whisk Deploy(wskdeploy) is an effort undergoing incubation at The Apache Software Foundation (ASF), sponsored by the Apache Incubator. Incubation is required of all newly accepted projects until a further review indicates that the infrastructure, communications, and decision making process have stabilized in a manner consistent with other successful ASF projects. While incubation status is not necessarily a reflection of the completeness or stability of the code, it does indicate that the project has yet to be fully endorsed by the ASF.
