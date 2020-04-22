# Exercise 1 - Making changes to the pact

## Goal

To understand some approaches for making changes to a pact while ensuring continuous delivery is supported.

## Making changes on master; or, how to break everything

### Change the pact

Let's add a new field to the expectations we have for the product API. We're going to make the change on master, and it's going to break everything. This is an exercise in "what not to do, and why".

Open up `src/api.pact.spec.js`, scroll down to the first test, and in the `eachLike` definition for the `body`, add a new field e.g. `color: "red"`. Update the `expect(product).toStrictEqual` to expect the new field and value.

Run `make test`. These tests should be green - the consumer code is consistent with its own expectations.

Commit and push the changes.

Open up the the consumer build in [Travis CI][travis-ci]. It will generate and publish the pact successfully, then fail on the can-i-deploy step, as there is no successful verification from the provider.

Next, open up the provider build in Travis CI. The changed pact will have triggered a pact verification build of the provider project. This will have failed, as the new field does not exist in the API. This particular failed build is not necessarily a problem, as the pact verification build is generally separate from the provider's normal pipeline. For a more detailed explanation of this see https://github.com/pactflow/example-provider#pact-verifications.

### Run the provider build

The real problem is that publishing the pact from master breaks the provider's main pipeline, as the consumer versions are tagged with the name of the branch (master) and the provider is configured to verify pacts tagged with 'master'.

You can demonstrate that this breaks the provider's main build by running a provider build in Travis by selecting "More options" > "Trigger build" > "Trigger custom build".

### Reverse the consumer change

When we made a change on master, we ended up breaking both the consumer and provider builds, blocking both of the pipelines.

Let's back out the change we made to the master branch, so our builds can be green again.


## Making changes on a feature branch; or, how not to break everything




[travis-ci]: https://travis-ci.com