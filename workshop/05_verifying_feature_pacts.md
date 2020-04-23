## Strategies for verifying feature pacts

When a new "feature" pact is created, there are a few ways you could bring that pact into the verification process so that the verification results get published back to Pactflow from your CI.

1. Manually add the feature tag to the consumerVersionSelectors, commit it, and then remove it once the feature is released.

    ```js
    consumerVersionSelectors: [{ tag: 'feat/new-field', latest: true }, { tag: 'master', latest: true }, ... ]
    ```

    * This does the job, but there are better options.

1. Make a matching feature branch in the provider, and dynamically fetch the pact for the matching consumer branch, falling back to master if a pact for the specified tag does not exist.

    ```js
    consumerVersionSelectors: [{ tag: process.env.TRAVIS_BRANCH, fallbackTag: 'master', latest: true }, { tag: 'prod', latest: true } ],
    ```

    * This is a reasonably common approach, where the two teams coordinate feature development using matching branch names.

1. Enable 'work in progress' pacts'

### Work In Progress Pacts

A "work in progress" pact is a pact that is the latest for its tag that does not have any successful verification results (ie. is still in pending state). At this stage in the exercise, the `feat/new-field` pact is still in pending state for the `master` branch of the provider, so it is considered a "work in progress" pact.

The verification task can be configured to automatically include work in progress pacts, so that the consumer team can get feedback on their changed pacts without having to wait on action from the provider team.

:arrow_right: Something that is important to understand about the calculation of the WIP pacts list is that it is *calculated based on the tag that will be applied to the provider version*. For example, if a pact only has a successful verification from a provider version with tag `feat/x`, it will still be pending for a provider version with tag `master` (and every other tag).

The reason for this is that if support for a new feature pact is added on a `feat/x` branch of the provider, you still want to keep getting the failed verification results from `master` until the `feat/x` branch is merged.

### Enable Work In Progress Pacts for the provider

1. In the provider project, open `src/product/product.pact.test.js` and in the options for the dynamically fetched pacts, set `includeWipPactsSince: "2020-01-01"`

1. Run `TRAVIS_BRANCH=master make test` and you will see that the `feat/new-field` pact has been included in the verifications, running in pending mode.
    * We need to include set the TRAVIS_BRANCH so that the WIP calculations know which pending pacts to include.

1. Commit and push

1. Open the provider build in Travis CI and wait for the successful verification result for `feat/new-field` to be be published.
    * :arrow_right: Note that the provider has now successfully deployed this change to production, so the consumer is now free to release their code.

1. On your local machine, run `TRAVIS_BRANCH=master make test` - you will now see that the `feat/new-field` pact is not included, as it is no longer a work in progress pact.

### Expected state by the end of this step

* A `feat/new-field` pact in Pactflow that has successful verification results from a version of the provider that has both `master` and `prod` tags.

### Conclusion

Enabling 'work in progress' pacts allows the consumer to get feedback on a changed pact without the provider having to make configuration changes on their end.

If the verification for the changed pact passes without the provider having to make any changes to the code, then the consumer is free to release their feature, without making the provider a bottleneck.

[Next](./06_releasing_the_consumer_code.md)
