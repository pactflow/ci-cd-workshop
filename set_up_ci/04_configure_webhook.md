## 4. Configure webhook

To ensure that the verification step is run whenever a pact changes, we need to configure a webhook to trigger a provider verification build in Travis CI. The webhook will need an authentication token to be able to make this call to the Travis API. We don't want the Travis token to be stored in clear text in the webhook, so we will create a secret in Pactflow to contain token.

1. Copy your Travis CI token
    1. In [Travis CI][travis-ci], click on your profile picture in the top right of the window, and select "Settings".
    1. Select the "Settings" tab.
    1. Under the API Authentication section, click "COPY TOKEN".

1. Create a Pactflow secret for the Travis token.
    1. In your Pactflow account, click on the Settings icon.
    1. Select the Secrets tab from the menu on the left.
    1. Click "ADD SECRET"
    1. Enter the name `travisToken` and paste the value that you copied earlier.
    1. Click "CREATE"

1. Create the webhook.
    1. In your Pactflow account, select the Webhooks tab from the settings page.
    1. Click "ADD WEBHOOK".
    1. Set:
        * Description: `Travis CI webhook for pactflow-example-provider`
        * Consumer: leave as "ALL"
        * Provider: select `pactflow-example-provider`
        * Events: select `Contract published with changed content or tags`
        * URL: `https://api.travis-ci.com/repo/<YOUR GITHUB ACCOUNT HERE>%2Fexample-provider/requests` (eg. `https://api.travis-ci.com/repo/bethesque%2Fexample-provider/requests`)
        * Headers:

            ```
            Content-Type: application/json
            Accept: application/json
            Travis-API-Version: 3
            Authorization: token ${user.travisToken}
            ```
        * Body:

            ```
            {
              "request": {
                "message": "Triggered by changed pact for ${pactbroker.consumerName} version ${pactbroker.consumerVersionNumber}",
                "branch": "master",
                "merge_mode": "deep_merge_append",
                "config": {
                  "env": {
                    "global": [
                      "PACT_URL=${pactbroker.pactUrl}"
                    ]
                  }
                }
              }
            }
            ```
      1. Click the "TEST" button and ensure that it runs successfully.
      1. Click the "CREATE" button.

1. Verify that the pact verification build for the provider is running correctly by opening [Travis CI][travis-ci] and checking the output of the triggered build.

:arrow_right: Each of the above steps can be automated - you can see the targets for them in the provider's Makefile.

## Expected state by the end of this step

Both consumer and provider builds passing, and a webhook that has been tested and shown to trigger a pact verification build of the provider.

[Next](./05_conclusion.md)

[travis-ci]: https://travis-ci.com