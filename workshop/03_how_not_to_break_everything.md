## Making changes with a feature tag; or, how NOT to break everything

When we made first made the change to the pact on the master branch of the consumer, we ended up with broken consumer and provider builds, stopping them both from releasing. In the previous step, we learned how to configure the provider so that it could still continue to be released, even if it was not able to successfully verify the new pact.

The consumer is still unable to make a release from its master however, as the `can-i-deploy` step correctly identifies that the verification for the `master` pact has failed. This is a reflection of the reality of the state of this integration - the API does not yet implement the features we require to deploy.

Let's make our changes on a branch this time.

1. Revert the change to master and push to make the build go green again.

    ```
    git revert HEAD
    git push
    ```

1. Create a new branch

    ```
      git checkout -b feat/new-field
    ```

1. Add the new field to the `expectedProduct` again (e.g. `color: "red"`).

1. Make sure the tests pass locally by running `make test`.

1. Push your changes by running `git push --set-upstream origin feat/new-field`.
    * The consumer tests will pass, and then the CI build will fail as `can-i-deploy` correctly identifies that this branch is not yet compatible with the API.
    * The webhook-triggered pact verification build will still fail - that's ok, as it doesn't stop the provider from deploying.

### Expected state by the end of this step

* A passing `master` consumer build in Travis CI.
* A failing `feat/new-field` consumer build in Travis CI.
* A `master` pact in Pactflow with a successful verification.
* A `feat/new-field` pact in Pactflow that hasn't yet been verified.

### Conclusion

By making changes on a branch of the consumer, and publishing a 'feature pact', we keep our main release branch green, and make sure we're not blocked from deploying.

[Next](./04_implementing_the_provider_changes.md)
