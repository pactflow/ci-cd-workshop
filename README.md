# Pactflow CI/CD Workshop

A workshop demonstrating how to set up a CI/CD pipeline for a consumer and provider using Pact, Pactflow and Travis CI.

It uses the Pactflow [example-consumer][example-consumer] and [example-provider][example-provider] repositories.

### Prerequisites

1. [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) installed. You need to know how to git clone, pull, push and commit.
1. [Make](https://www.gnu.org/software/make/manual/make.html) - it should be installed by default on Linux/Mac. If you have Windows, install it from [sourceforge](http://gnuwin32.sourceforge.net/packages/make.htm). Note - you won't need any previous experience in using make for this workshop.
1. [Node](https://nodejs.org/) 10 or thereabouts installed.
1. [Docker](https://www.docker.com/products/docker-desktop) installed.
1. A [Github](http://github.com/) account.
1. A Travis CI account that is attached to your Github account.
    1. Go to [travis-ci.com][travis-ci] and sign in with your Github account.
    1. Click on your profile picture in the top right of your Travis Dashboard, click Settings and then the green Activate button. See https://docs.travis-ci.com/user/tutorial/#to-get-started-with-travis-ci-using-github for more instructions if you get stuck.
1. A Pactflow account. Sign up for a free developer account [here](https://pactflow.io/pricing/).

:warning: Note: these instructions have been written for and tested in a bash shell on Mac. They should work the same on Linux. If you are using Windows, then you will need to know how to run git/make/node CLI commands, or pair with someone who does.

### Set up

#### Set up the repositories and builds

1. Fork the [example-consumer][example-consumer] and [example-provider][example-provider] projects in to your own Github account (click the 'Fork' button in the top right).
1. Add the example repositories to Travis CI.
    1. Go to [travis-ci][travis-ci] and click on your profile picture in the top right of the window, then select "Settings".
    1. Click the "Sync account" button.
    1. Click the "Activate" button under the "GitHub Apps Integration" section.
    1. Choose "All" or select the "example-consumer" and "example-provider" repositories.
    1. When your screen updates, you should see a list of repositories under "GitHub Apps Integration
". Click on the example-consumer item, and it will take you to the Travis build page.
    1. Click on "More options" > "Trigger build" > "Trigger custom build". The build will fail with an authentication error when it tries to publish the pact! That's expected. We need to update the environment variables to point it at your new Pactflow account.
1. Clone your forked example repositories on to your local machine.

    ```
    git clone git@github.com:<YOUR_GITHUB_USERNAME>/example-consumer.git
    git clone git@github.com:<YOUR_GITHUB_USERNAME>/example-provider.git
    ```

#### Configure consumer pipeline

Configure the consumer repository with your own Pactflow account details.

1. In .travis.yml, set `PACT_BROKER_BASE_URL` to the base URL of your own Pactflow account (you will have received an email with this information).
1. To update the encrypted PACT_BROKER_TOKEN you will need to use the Travis CI CLI which is released as a Rubygem. To avoid having to install yet another dependency, we are using the `lirantal/travis-cli` docker container, which is maintained by a long time Pact fan and user, the awesome [@lirantal](https://github.com/lirantal).
    1. Log in to your Pactflow account, and go to Settings > API Tokens.
    1. Click the Copy button for the read/write CI token.
    1. Open a terminal in the directory where you checked out your example-consumer.

        ```
        make travis_login # you'll need to input your github credentials and 2FA if configured
        export PACT_BROKER_TOKEN="<paste your token here>"
        make travis_encrypt_pact_broker_token
        ```
    1. Copy the output of the encrypt command, including the quotation marks, and replace the `- secure:` value in the env > global section of your .travis.yml file.
    1. Commit and push the changes to your .travis.yml file.
    1. Open the example-consumer project in Travis CI. This build should now successfully publish the pact, but it will fail on the `can-i-deploy` step when it tries to deploy. This is because the provider has not published a successful verification result for the pact.

#### Configure provider pipeline

:repeat: Repeat the above instructions to configure the Pactflow account for your provider project.

* You shouldn't need to repeat the Travis login step.
* Note that the environment variable must be encrypted in the context of the repository to which it will be added, so you can't just reuse the output of the encrypt step from the consumer project.

After you have pushed your changes to .travis.yml, the provider pipeline will run, fetching and verifying the configured pacts from your Pactflow account, and publishing the results back. The `can-i-deploy` command will pass, and allow the provider to be deployed.

## Configure webhook

To ensure that the verification step is run whenever a pact changes, we need to configure a webhook to trigger a provider verification build in Travis CI. The webhook will need an authentication token to be able to make this call to the Travis API. We don't want the Travis token to be stored in clear text in the webhook, so we will create a secret in Pactflow to contain token.

1. Copy your Travis CI token
    1. Click on your profile picture in the top right of your Travis CI window, and select "Settings"
    1. Select the "Settings" tab.
    1. Under the API Authentication section, click "COPY TOKEN".

1. Create a Pactflow secret for the Travis token. These steps are run from your local machine. You could also do this through the Pactflow UI.

    ```
    export TRAVIS_TOKEN=<paste your travis token here>
    export PACT_BROKER_BASE_URL=<your Pactflow account base url>
    export PACT_BROKER_TOKEN=<your read write token>

    make create_travis_token_secret
    ```

You can see that the secret has been created by opening up your Pactflow account, and going to Settings > Secrets.

1. Create the webhook (requires the `PACT_BROKER_BASE_URL` and `PACT_BROKER_TOKEN` from the previous step to be still set). You could also do this through the Pactflow UI.

   ```
   make create_or_update_travis_webhook
   ```

You can see that the webhook has been created by opening up your Pactflow account, and going to Settings > Webhooks.

1. Test the webhook.

You can either do this though the Pactflow UI or by running `make test_travis_webhook`.

To test a webhook through the UI, go to Settings > Webhooks and click on the EDIT button for your webhook. At the bottom of the edit screen, click the TEST button.



[example-consumer]: https://github.com/pactflow/example-consumer
[example-provider]: https://github.com/pactflow/example-provider
[travis-ci]: https://travis-ci.com
