## 2. Configure the builds in Travis CI

1. Go to [travis-ci][travis-ci] and click on your profile picture in the top right of the window, then select "Settings".
1. Click the "Sync account" button.
1. Click the "Activate" button under the "GitHub Apps Integration" section.
1. Choose "All" or select the "example-consumer" and "example-provider" repositories.
1. When your screen updates, you should see a list of repositories under "GitHub Apps Integration
". Click on the example-consumer item, and it will take you to the Travis build page.
1. Click on "More options" > "Trigger build" > "Trigger custom build". The build will fail with an authentication error when it tries to publish the pact - that's expected. We need to update the environment variables to point it at your new Pactflow account.

[Next](./03_configure_consumer_and_provider.md)

[travis-ci]: https://travis-ci.com
