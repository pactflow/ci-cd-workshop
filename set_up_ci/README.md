# Setup CI/CD

## Goal

By the end of this step, you will have a CI/CD pipeline for a consumer and provider project. It will run the builds on Travis CI, and use Pactflow to exchange pacts and verification results, to trigger webhooks when pacts change, and to make sure each project is safe to deploy.

## Method

You will be using the Pactflow [example-consumer][example-consumer] and [example-provider][example-provider] projects as starting points for the workshop. You will be forking the repositories into your own Github account, setting up new Travis CI builds, and configuring the projects to point to your own Pactflow account.

[Next](./00_prerequisites.md)

[example-consumer]: https://github.com/pactflow/example-consumer
[example-provider]: https://github.com/pactflow/example-provider
