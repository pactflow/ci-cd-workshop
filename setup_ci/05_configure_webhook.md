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

1. You can verify that the secret has been created by opening up your Pactflow account, and going to Settings > Secrets.

1. :point_right: At the top of the Makefile, replace the word `pactflow` with the name of your github account in the `TRIGGER_PROVIDER_BUILD_URL`. :point_left:
1. Create the webhook (requires the `PACT_BROKER_BASE_URL` and `PACT_BROKER_TOKEN` from the previous step to be still set). You could also do this through the Pactflow UI.

   ```
   make create_or_update_travis_webhook
   ```

1. You can verify that the webhook has been created by opening up your Pactflow account, and going to Settings > Webhooks.

1. Test the webhook.

You can either do this though the Pactflow UI or by running `make test_travis_webhook`.

To test a webhook through the UI, go to Settings > Webhooks and click on the EDIT button for your webhook. At the bottom of the edit screen, click the TEST button.
