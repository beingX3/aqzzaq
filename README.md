# RUST API SERVER


## Introduction

Welcome to the Rust API Server! This server provides a simple REST interface for your applications. This README will
guide you through setting up and running the server, as well as configuring its various options.

## Deploy

**Automated Builds:**
Builds are automatically generated for each commit to the repository in the `main` branch and are subsequently pushed to Docker Hub.
Tags are applied using the commit SHA, branch name, and the latest tag if released on the main branch. You can find a list of available
tags [here](https://hub.docker.com/r/thuan2172001/rust-server/tags).

**Release Binaries:**
For every release, separate `cli` binaries are built. These binaries can be downloaded
from [here](https://github.com/sonntuet1997/rust-web-api-microservice-template/releases) and are available for various operating
systems and architectures. You are welcome to use the automated binaries or build your own.

**Contributions and PRs:**
If you submit a pull request, please note that images are not built by default. A maintainer will need to retag them for the build
process to take place.

## How To Run

To get started, execute the following command in your terminal:

```shell
./cli --help
```

This will display the available options for running the server:

```
Simple REST server

Usage: cli [OPTIONS] [COMMAND]

Commands:
  config  Print config
  help    Print this message or the help of the given subcommand(s)

Options:
  -c, --config-path <CONFIG_PATH>  Config file [default: config/default.toml]
  -v, --version                    Print version
  -h, --help                       Print help
```

### Example

- Multiple config locations

```shell
./cli -c ./config/*.toml -c deploy/local/custom.toml
```

- Pipe the output with [bunyan](https://github.com/trentm/node-bunyan)

```shell
cargo install bunyan
./cli -c ./config/*.toml -c deploy/local/custom.toml | bunyan
```

## Configuration

### Order of apply

Configuration is applied in the following order: config files -> environment variables -> command-line arguments.

If you use `-c *.toml` to load config files, please be mindful of the order in which the files are applied.

### Environment Variable Examples

The server can be configured using environment variables. Below is a table outlining the available configuration
options:

Hierarchical child config via env, separated by using `__`. Specify list values by using `,` separator

| ENV                                                                      | DEFAULT VALUE | NOTE      |
| ------------------------------------------------------------------------ | ------------- | --------- |
| [RUST_LOG](https://docs.rs/env_logger/latest/env_logger/) > LOG\_\_LEVEL | "INFO"        | Log level |
| SERVER\_\_URL                                                            |               |           |
| SERVER\_\_PORT                                                           |               |           |
| SERVICE_NAME                                                             |               |           |
| EXPORTER_ENDPOINT                                                        |               |           |
| DB\_\_PG\_\_URL                                                          | "localhost"   |           |
| DB\_\_PG\_\_MAX_SIZE                                                     | 5432          |           |
| REDIS\_\_HOST                                                            | "localhost"   |           |
| REDIS\_\_PORT                                                            | 6379          |           |

Make sure to set these environment variables according to your needs before running the server.

## GitHub Flow CI Configuration

1. **Set Docker Hub Secrets:**

   - Go to repository Settings > Secrets.
   - Add `DOCKER_USERNAME` and `DOCKERHUB_TOKEN`.

2. **Enable Dependabot Alerts:**

   - In repository Insights, enable "Dependabot alerts" and "Security & Analysis".

## Load Testing and Profiling

For load testing and profiling your Rust API server, refer to
the [Load Testing and Profiling with K6 and Flamegraph](./load-tests/README.md) guide. This document provides
detailed instructions on using K6 and Flamegraph for load testing and profiling purposes.
