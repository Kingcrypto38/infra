## moov-io/infra

This repository holds the configurion for [moov.io](https://github.com/moov-io)'s infrastructure. If you believe you have identified a security vulnerability please responsibly report the issue as via email to security@moov.io. Please do not post it to a public issue tracker.

[![GoDoc](https://godoc.org/github.com/moov-io/infra?status.svg)](https://godoc.org/github.com/moov-io/infra)
[![Build Status](https://github.com/moov-io/infra/workflows/Go/badge.svg)](https://github.com/moov-io/infra/actions)
[![Apache 2 licensed](https://img.shields.io/badge/license-Apache2-blue.svg)](https://raw.githubusercontent.com/moov-io/infra/master/LICENSE)

Links: [Graphs](https://infra-oss.moov.io/grafana/) | [Logs](https://infra-oss.moov.io/grafana/explore) | [Metrics](https://infra-oss.moov.io/prometheus/) | [Alerts](https://infra-oss.moov.io/alertmanager/#/alerts)

### Project Goals

This project aims to be the structure and runtime for a fully automated hosting of moov.io services.

### Getting started

To get started working on this project, pull down (`git clone git@github.com:moov-io/infra`) the source code and setup some tools: Golang, Terraform, kubectl, etc. The following list consists of all tools used and expected by the infra repository.

- [Golang](https://golang.org/dl)
- Terraform 0.12+ | [Download](https://www.terraform.io/downloads.html) | [Intro](https://www.terraform.io/intro/index.html)
- kubectl | [Download](https://kubernetes.io/docs/tasks/tools/install-kubectl/) | [Intro](https://www.digitalocean.com/community/tutorials/an-introduction-to-kubernetes)
- [Blackbox](https://github.com/StackExchange/blackbox#blackbox-) (Installed at `./lib/blackbox/bin`, add this to `PATH`)
- [gcloud CLI](docs/google-cloud.md)
- Docker for either [Linux](https://docs.docker.com/install/linux/docker-ce/ubuntu/) or [macOS](https://docs.docker.com/docker-for-mac/install/).

On macOS these tools can be installed with the following homebrew command:

```
$ brew install gpg2 jq make terraform go
```

The project has the following directories:

- [`cmd/`](https://github.com/moov-io/infra/tree/master/cmd) - Various command line utilities used in this project and as tests
- [`docs/`](https://github.com/moov-io/infra/tree/master/docs) - Human readable documentation.
- `envs/`
  - [`envs/oss`](https://github.com/moov-io/infra/tree/master/envs/oss) - Our Open Source environment
- [`images/`](https://github.com/moov-io/infra/tree/master/images) - Docker images we create and manage
- [`lib/`](https://github.com/moov-io/infra/tree/master/lib) - Shared (Kubernetes) resources across environments

### Local Development

We support a local development environment to allow anyone the ability to run Moov's application stack locally on a machine. This currently requires a few tools to be installed, such as:

- Docker
  - [Docker for Mac](https://docs.docker.com/docker-for-mac/) - [Install Guide](https://docs.docker.com/docker-for-mac/install/)
  - [Docker on Linux](https://docs.docker.com/install/)
  - [Docker for Windows](https://docs.docker.com/docker-for-windows/) - [Install Guide](https://docs.docker.com/docker-for-windows/install/)

### Testing

We run several kinds of tests against this repository. Linters/validators (over Kubernetes, Terraform, Prometheus configs) as well as Docker image builds. To run all these:

```
$ make test
```

Note: To run these tests you need Docker installed.

### Runbooks

- [Alerting](docs/alerts.md)
- [Google Cloud (GKE)](docs/google-cloud.md)
- [In-repo Secrets (Blackbox)](docs/secrets.md)
- [Kubernetes](docs/kubernetes.md)
- [Load balancing / Routing (Traefik)](docs/traefik.md)
- [Downloading fuzz data](docs/fuzzing.md)

### Getting Help

 channel | info
 ------- | -------
Twitter [@moov](https://twitter.com/moov) | You can follow Moov.io's Twitter feed to get updates on our project(s). You can also tweet us questions or just share blogs or stories.
[GitHub Issue](https://github.com/moov-io) | If you are able to reproduce a problem please open a GitHub Issue under the specific project that caused the error.
[moov-io slack](https://slack.moov.io/) | Join our slack channel to have an interactive discussion about the development of the project.

### Contributing

Yes please! Please review our [Contributing guide](CONTRIBUTING.md) and [Code of Conduct](CODE_OF_CONDUCT.md) to get started!

### License

Apache License 2.0 See [LICENSE](LICENSE) for details.
