# Pactflow CI/CD Workshop

A workshop demonstrating how to set up a CI/CD pipeline for a consumer and provider using Pact, Pactflow and Travis CI.

It uses the Pactflow [example-consumer][example-consumer] and [example-provider][example-provider] repositories. These are written in Node, however, extensive node experience will not be required for the workshop.

## Goals

* To understand how Pact and Pactflow fit into the CI/CD pipelines of a consumer and provider.
* To understand the workflows involved in making changes to both consumer and provider.
* To understand how Pact + Pactflow stop breaking changes from being deployed to a given environment.

## Table of contents

* [Setup CI](./setup_ci/01_introduction.md) - covers forking and configuring the example projects to get a working CI/CD pipeline.
* [Setup development environment](./SETUP_DEVELOPMENT_ENVIRONMENTS.md) - get the consumer and provider projects running and building on your local development machine.
* [Exercise 1](./EXERCISE_01.md) - making changes to the pact


[example-consumer]: https://github.com/pactflow/example-consumer
[example-provider]: https://github.com/pactflow/example-provider

