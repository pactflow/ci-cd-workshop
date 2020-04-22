## Prerequisites

* CI/CD pipelines for the consumer and provider as per the [Setup CI/CD page](/set_up_ci/README.md)
* A working local development set up as per the [Setup development environment](/set_up_local_development/README.md) page.
* Both consumer and provider builds in Travis CI should both be passing on master.
* Inspect the configuration in the consumer and provider projects that deals with the *tags*. Read more below.

### Tag configuration

In the [publish.pact.js](https://github.com/pactflow/example-consumer/blob/master/publish.pact.js) file in the consumer project, we tag the consumer version with the name of the branch.

```js

const branch = process.env.TRAVIS_BRANCH;
const opts = {
  ...,
  tags: [branch]
};
```

In the [src/product.pact.test.js](https://github.com/pactflow/example-provider/blob/master/src/product/product.pact.test.js) file in the provider project, we have configured the verification task to fetch the pacts that belong to the latest consumer versions with `master` and `prod` tags.

```js

const fetchPactsDynamicallyOpts = {
  provider: "pactflow-example-provider",
  consumerVersionTag: ['master', 'prod'],
  ...
}
```
