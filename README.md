# OpenShift S2I Elixir  Builder

This repository contains the source for building various versions of
Erlang applications as a reproducible Docker image using
[source-to-image](https://github.com/openshift/source-to-image).
The resulting image can be run using [Docker](http://www.docker.com).

For more information about using this image with OpenShift, please see the
official [OpenShift
Documentation](https://docs.openshift.org/latest/architecture/core_concepts/builds_and_image_streams.html#source-build).

## Versions

Erlang versions currently provided are:

* `1.6.6` (Tags: `1`, `1.6`, `latest`)
  * OTP `20` / `21`

CentOS versions currently supported are:

* CentOS7

## OpenShift Usage

To build an Elixir image:

1. Clone this repo and enter into the directory
  ```
  git clone git@github.com:jshmrtn/s2i-elixir.git
  cd s2i-elixir
  ```

1. Install the example image stream configuration into OpenShift
  ```
  oc create -f imagestream.json
  ```

1. Create the application
  1. WebUI - Navigate to the project, click 'Add to Project' and search for
     the new builder image, select it and populate the fields to create your
     application
  1. Command Line -
    ```
    oc new-app --image-stream=elixir --code https://your.git/repo
    ```

### Distillery

The image support to be used via [`distillery`](https://github.com/bitwalker/distillery).

To use distillery, set the environment variable `ENABLE_DISTILLERY` to `1` and
`DISTILLERY_APP_NAME` to the application name.

## Local Usage

To build the Elixir Builder image:

* This image is available on DockerHub. To download it run:

```
$ docker pull jshmrtn/s2i-elixir
```

* To build an Elixir builder image from scratch run:

```
$ git clone https://github.com/jshmrtn/s2i-elixir.git
$ cd s2i-elixir/[ELIXIR_VERSION]/otp-[ERLANG_VERSION]
$ docker build -t jshmrtn/s2i-elixir:[VERSION]-otp-[ERLANG_VERSION] .
```
