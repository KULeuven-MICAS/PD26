# Docker container

This directory contains a Dockerfile that can be used to build Docker containers for hardware and software development in PD.

## Installation

### Pre-built container

There is a pre-built version of the container available online. This version is up to date with the latest developments on the `main` branch. 

```shell
$ podman pull -t ghcr.io/kuleuven-micas/pd26:main
```

### Build instructions
In case you need to build the docker by yourself: 

```shell
$ echo "<your token>" | podman login ghcr.io -u <your-github-username> --password-stdin
```
As a password you should use a
[personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)
that at least has package registry read permission.


And then you need the build the image from the Dockerfile:

```shell
$ podman build -t ghcr.io/kuleuven-micas/pd26:main -f util/container/Dockerfile .
```

Finally we push to the git:

```shell
$ podman push ghcr.io/kuleuven-micas/pd26:main
```