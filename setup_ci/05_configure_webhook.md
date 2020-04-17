## Configure webhook

To ensure that the verification step is run whenever a pact changes, we need to configure a webhook to trigger a provider verification build in Travis CI. The webhook will need an authentication token to be able to make this call to the Travis API. We don't want the Travis token to be stored in clear text in the webhook, so we will create a secret in Pactflow to contain token.

1. Copy your Travis CI token
    1. In [Travis CI][travis-ci], click on your profile picture in the top right of the window, and select "Settings".
    1. Select the "Settings" tab.
    1. Under the API Authentication section, click "COPY TOKEN".

1. Create a Pactflow secret for the Travis token. The following steps can be run from your local machine, but you can also do this through the Pactflow UI.

    ```
    export TRAVIS_TOKEN=<paste your travis token here>
    export PACT_BROKER_BASE_URL=<your Pactflow account base url>
    export PACT_BROKER_TOKEN=<your read write token>

    make create_travis_token_secret
    ```

1. You can verify that the secret has been created by opening up your Pactflow account, and going to Settings > Secrets.

1. :point_right: At the top of the Makefile, in the `TRIGGER_PROVIDER_BUILD_URL`, replace the word `pactflow` with the name of your github account. :point_left:
1. Create the webhook by running the following command. You can also do this through the Pactflow UI.

   ```
   # Assuming that the `PACT_BROKER_BASE_URL` and `PACT_BROKER_TOKEN` from the previous step are still set...

   make create_or_update_travis_webhook
   ```

1. You can verify that the webhook has been created by opening up your Pactflow account, and going to Settings > Webhooks.

1. Test the webhook. You can either do this though the Pactflow UI or via the CLI.
    * To test via the CLI, run `make test_travis_webhook`.
    * To test a webhook through the Pactflow UI, go to Settings > Webhooks and click on the EDIT button for your webhook. At the bottom of the edit screen, click the TEST button.

1. Verify that the pact verification build for the provider is running correctly by opening [Travis CI][travis-ci] and checking the output of the triggered build.

[Next](./06_conclusion.md)

[travis-ci]: https://travis-ci.com