## Configure consumer pipeline

The source repositories are configured to use the Pactflow Github and Travis accounts, and the public broker at test.pact.dius.com.au. You will need to update these settings to point to your own accounts.

1. In `.travis.yml` of the example-consumer project, set `PACT_BROKER_BASE_URL` to the base URL of your own Pactflow account (you will have received an email with this information).
1. To update the encrypted `PACT_BROKER_TOKEN` you will need to use the Travis CI CLI which is released as a Rubygem. To avoid having to install yet another dependency, we are using the `lirantal/travis-cli` docker image, which coincidentally is maintained by a long time Pact fan and user, the awesome [@lirantal](https://github.com/lirantal).
    1. Log in to your Pactflow account (`https://<your-subdomain>.pact.dius.com.au`), and go to Settings > API Tokens.
    1. Click the Copy button for the read/write CI token.
    1. Open a terminal in the directory where you checked out your example-consumer.

        ```
        export PACT_BROKER_TOKEN="<paste your token here>"

        make travis_login # you'll need to input your github credentials and 2FA if configured
        make travis_encrypt_pact_broker_token
        ```
    1. Copy the output of the encrypt command, including the quotation marks, and replace the `- secure:` value in the `env > global` section of your `.travis.yml` file.
    1. Commit and push the changes to your `.travis.yml` file.
    1. Open the example-consumer project in Travis CI. This build should now successfully publish the pact, but it will fail on the `can-i-deploy` step when it tries to deploy. This is because the provider has not published a successful verification result for the pact.

## Configure provider pipeline

:repeat: Repeat the above instructions to configure the Pactflow account for your provider project.

Note:

* You shouldn't need to repeat the Travis login step.
* The environment variable must be encrypted in the context of the repository to which it will be added, so you can't just reuse the output of the encrypt step from the consumer project.

After you have pushed your changes to `.travis.yml`, the provider pipeline will run, fetching and verifying the configured pacts from your Pactflow account, and publishing the results back. The `can-i-deploy` command will pass, and allow the provider to be deployed.

## Back to the consumer

:white_check_mark: If you would like to see all your builds go green, you can re-trigger the consumer build by selecting "More options" > "Trigger build" > "Trigger custom build".

[Next](./05_configure_webhook.md)
