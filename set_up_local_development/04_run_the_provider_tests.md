## 4. Run the provider tests

Go to the `Settings > API Tokens` page in your Pactflow account, and copy the read only developer token.

In the terminal for the provider project, run:

```
export PACT_BROKER_BASE_URL=<the base URL of your Pactflow account>
export PACT_BROKER_TOKEN=<the read only token you copied from your settings page>

make test
```

This runs the test suite for the provider codebase. The pacts for this provider are verified in `product/product.pact.test.js`. It is configured to fetch the latest pacts for this provider that have been tagged by the consumer as 'master' or 'prod'.

When we run the verification step on a local development machine, we do not publish the verification results. This is why we only need a read only token for development work.

[Home](/README.md)
