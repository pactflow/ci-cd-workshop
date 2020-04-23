## Making changes on master; or, how to break everything

### Change the pact

Let's add a new field to the expectations we have for the product API. We're going to make the change on master, and it's going to break everything. This is an exercise in "what not to do, and why".

1. Open up `src/api.pact.spec.js`, scroll down to the first test, and in the `eachLike` definition for the `body`, add a new field e.g. `color: "red"`. Update the `expect(product).toStrictEqual` to expect the new field and value.

1. Run `make test`. These tests should be green - the consumer code is consistent with its own expectations.

1. Commit and push the changes.

    :arrow_right: Note that in the `publish.pact.js` file, the consumer version is tagged with the name of the git branch.

1. Open up the the consumer build in [Travis CI][travis-ci]. It will generate and publish the pact successfully, then fail on the `can-i-deploy` step, as there is no successful verification from the provider.

1. Open up the provider build in Travis CI. The changed pact will have triggered a pact verification build of the provider project. This will have failed, as the new field does not exist in the API. This particular failed build is expected, and not a problem, as the pact verification build is generally separate from the provider's normal pipeline. For a more detailed explanation of this see https://github.com/pactflow/example-provider#pact-verifications.

### Run the provider build

The real problem is that the provider is now unable to deploy from their master branch :anguished:.

This is because the provider is configured to verify the latest `master` pact, so publishing a pact with a new expectation and tagging its consumer version as `master` causes the verification step to fail, breaking the provider's build through no fault of its own.

You can demonstrate this by running a provider build in Travis by selecting "More options" > "Trigger build" > "Trigger custom build".

### Check the pact's status in Pactflow

1. Open up the pact in Pactflow. You'll see that there is a pact tagged `master` with a failed verification result.

1. Click on "VIEW PACT" and you'll see that each interaction has a status next to it.

1. Expand the failing interaction, and you'll see the field level mismatches.

### Expected state by the end of this step

* A consumer build that is failing at the `can-i-deploy` step in Travis CI.
* A provider build that is failing during verification in Travis CI.
* A `master` pact in Pactflow that has a failed verification result.

### Conclusion

Making changes to the pact for the on the master branch can break both consumer and provider builds, and may stop both projects from being able to deploy.

[Next](./02_protecting_the_provider.md)

[travis-ci]: https://travis-ci.com
