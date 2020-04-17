# Setup development environment

## Goal

Install, run and test the consumer and provider applications in a local development environment.

## Prerequisites

* Github, Travis CI and Pactflow set up according to the instructions in the [Setup CI](./SETUP_CI.md) page.
* Node.

## Install dependencies

In each of the consumer and provider projects run:

```
npm install
```

## Run the app and API

Start up the provider API by running `npm run start`.

Open a separate terminal for the consumer.

Before starting the consumer, export the location of the API by running:

`export REACT_APP_API_BASE_URL=http://localhost:8080`

Then run:

`npm run start`

Open [http://localhost:3000](http://localhost:3000) in your browser. You should see a list of products.

## Run the consumer tests

Shut down the consumer and provider by pressing `ctl+c` in each of the terminals.

In the consumer project, run:

`make test`

This will run the test suite for the consumer codebase. The pact tests in `src/api.pact.spec.js` generate a pact file (the contract) which can be found in `pacts/pactflow-example-consumer-pactflow-example-provider.json`. As you can see from the test output, pacts are not published when running tests on your local machine.

## Run the provider tests

Go to the `Settings > API Tokens` page in your Pactflow account, and copy the read only developer token.

In the terminal for the provider project, run:

```
export PACT_BROKER_BASE_URL=<the base URL of your Pactflow account>
export PACT_BROKER_TOKEN=<the read only token you copied from your settings page>

make test
```

This runs the test suite for the provider codebase. The pacts for this provider are verified in `product/product.pact.test.js`. It is configured to fetch the latest pacts for this provider that have been tagged by the consumer as 'master' or 'prod'.

When we run the verification step on a local development machine, we do not publish the verification results. This is why we only need a read only token for development work.
