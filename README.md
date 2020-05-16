# Pactflow CI/CD Workshop

A workshop demonstrating how to set up a CI/CD pipeline for a consumer and provider using Pact, Pactflow and Travis CI.

It uses the Pactflow [example-consumer][example-consumer] and [example-provider][example-provider] repositories. These are written in Node, however, extensive node experience will not be required for the workshop.

## Goals

* To understand how Pact and Pactflow fit into the CI/CD pipelines of a consumer and provider.
* To understand the workflows involved in making changes to both consumer and provider.
* To understand how Pact + Pactflow stop breaking changes from being deployed to a given environment.

## Table of contents

* [Setup CI](/set_up_ci/README.md) - covers forking and configuring the example projects to get a working CI/CD pipeline.
* [Setup development environment](/set_up_local_development/README.md) - get the consumer and provider projects running and building on your local development machine.
* [Workshop](/workshop) - how to make changes to an integration following a consumer driven contracts approach.

[example-consumer]: https://github.com/pactflow/example-consumer
[example-provider]: https://github.com/pactflow/example-provider
