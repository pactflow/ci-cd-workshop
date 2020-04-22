## Conclusion

We have demonstrated how the CI/CD pipelines of the consumer and provider work with Pact and Pactflow, and how to introduce and release new features using consumer driven contracts, while keeping each project deployable throughout the process.

Principles to remember:
  * Tag application versions with the name of the git branch
  * Tag application versions with the stage name when deploying
  * Configure the provider to verify the latest pact for the main line of development, and for the deployment environments that you care about (eg. `master`, `test`, and `prod`).
  * Enable pending pacts and work in progress pacts for the provider
  * Make changes to pacts on feature branches for the consumer, and merge once the feature pact has been successfully verified.

[Home](/README.md)