## Releasing the consumer code

The provider has now successfully deployed to prod the changes requested by the consumer.

Refresh the example pact page in Pactflow to see that the `feat/new-field` pact has a successful verification from the `master` provider.

1. Open the terminal for the consumer project.

1. Run:

    ```
    git checkout master
    git merge feat/new-field
    git push origin master
    ```

1. Open up the build in Travis CI. This build will pass, and the consumer will be able to deploy to production as the production version of the provider already supports this new feature.

### Some ideas on how to communicate that the feature pact has been successfully verified to the consumer team

The most elegant solution for this is to create a webhook for the 'provider verification succeeded' event. Some common approaches are to use chat notifications or Github/Bitbucket commit statuses that show up in the PR page for a branch. You can read more about that here: http://blog.pact.io/2018/07/16/publishing-pact-verification-statuses-to-github/

### Conclusion

Even though the new expectations from the consumer come first, the provider still has to release the code to support the new feature before the new features in the consumer can be released.

[Next](./07_conclusion.md)
