## Prerequisites

* CI/CD pipelines for the consumer and provider as per the [Setup CI/CD page](/set_up_ci/README.md)
* A working local development set up as per the [Setup development environment](/set_up_local_development/README.md) page.
* Both consumer and provider builds in Travis CI should both be passing on master.
* Suggested window configuration:
    * In [Travis CI][travis-ci]:
        * One tab for the example-consumer build
        * One tab for the example-provider build
    * In Pactflow
        * A tab for the example pact dashboard
    * In your editor of choice:
        * One window for the example-consumer
        * One window for the example-provider
    * In your terminal of choice:
        * One shell for the example-consumer
        * One shell for the example-provider
    * Close everything else that you can! It can get confusing switching backwards and forwards between all the windows in the workshop.
* Have a look at the configuration in the consumer and provider projects that deals with the *tags*. Read more below.

### Tag configuration

Tags are used to wire up the consumer project to the provider project and make sure we are verifying the right pacts.

In the [publish.pact.js](https://github.com/pactflow/example-consumer/blob/master/publish.pact.js) file in the consumer project, we tag the consumer version with the name of the branch.

```js

const opts = {
  ...,
  tags: [process.env.TRAVIS_BRANCH]
};
```

In the [src/product.pact.test.js](https://github.com/pactflow/example-provider/blob/master/src/product/product.pact.test.js) file in the provider project, we have configured the verification task to fetch the pacts that belong to the latest consumer versions with `master` and `prod` tags.

```js

const baseOpts = {
  ...,
  providerVersionTag: process.env.TRAVIS_BRANCH
}

const fetchPactsDynamicallyOpts = {
  ...,
  provider: "pactflow-example-provider",
  consumerVersionSelectors: [{ tag: 'master', latest: true }, { tag: 'prod', latest: true } ],
}
```

[Next](./01_how_to_break_everything.md)

[travis-ci]: https://travis-ci.com