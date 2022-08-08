# playwright-slack-report ![Biulds](https://github.com/ryanrosello-og/playwright-slack-report/actions/workflows/playwright.yml/badge.svg) [![GitHub license](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/ryanrosello-og/playwright-slack-report/blob/master/LICENSE)

Publish your Playwright test results to your favorite Slack channel(s).


[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/ryanrosello-og/playwright-slack-report>)

# Installation

Run following commands:

**yarn**

`yarn add playwright-slack-report -D`

**npm**

`npm install playwright-slack-report -D`

Modify your `playwright.config.ts` file to include the following:

```typescript
  reporter: [
    [
      "./node_modules/playwright-slack-report/dist/src/SlackReporter.js",
      {
        channels: ["pw-tests", "ci"], // provide one or more Slack channels
        sendResults: "always", // "always" , "on-failure", "off"
      },
    ],
    ["dot"], // other reporters
  ],
```

Run your tests by providing your` SLACK_BOT_USER_OAUTH_TOKEN` as an environment variable:

`SLACK_BOT_USER_OAUTH_TOKEN=[your Slack bot user OAUTH token] npx playwright test`

# How do I find my Slack bot oauth token?

You will need to have Slack administrator rights to perform the steps below.

1. Navigate to https://api.slack.com/apps

[screenshot]

2. Click the Create New App button and select "From scratch"

[screenshot]

3. Input a name for your app and select the target workspace, then click on the "Create app" button

[screenshot]

4. Under the Features menu, select "OAuth & Permissions" and scroll down to "Scopes" section

5. Click the "Add an OAuth Scope" button and select the following scopes:

[screenshot]

* chat:write 
* chat:write.public 
* chat:write.customize  

6. Scroll up to the OAuth Tokens for Your Workspace and click the  Install to Workspace button

You will be prompted with the message below, click the Allow button

[screenshot]


The final step will be to copy the generated Bot User OAuth Token aka `SLACK_BOT_USER_OAUTH_TOKEN`.  Treat this token as a secret.

# Configuration

An example advanced configuration is shown below:


```typescript
  import { generateCustomLayout } from "./my_custom_layout";

  ...

  reporter: [
    [
      "./node_modules/playwright-slack-report/dist/src/SlackReporter.js",
      {
        channels: ["pw-tests", "ci"], // provide one or more Slack channels
        sendResults: "always", // "always" , "on-failure", "off"
      },
      layout: generateCustomLayout,
      meta: [
        {
          key: 'BUILD_NUMBER',
          value: '323332-2341',
        },
        {
          key: 'WHATEVER_ENV_VARIABLE',
          value: process.env.SOME_ENV_VARIABLE,
        },
      ],      
    ],
  ],
```

### **channels**
An array of Slack channels to post to, atleast one channel is required
### **sendResults**
Can either be *"always"*, *"on-failure"* or *"off"*, this configuration is required:
  * **always** - will send the results to Slack at completion of the test run
  * **on-failure** - will send the results to Slack only if a test failures are encountered
  * **off** - turns off the reporter, it will not send the results to Slack
### **layout**
A function that returns a layout object, this configuration is optional.  See section below for more details.
* meta - an array of meta data to be sent to Slack, this configuration is optional.

**Examples:**
```typescript
...
meta: [
  {
    key: 'Suite',
    value: 'Nightly full regression',
  },
  {
    key: 'GITHUB_REPOSITORY',
    value: 'octocat/telsa-ui',
  },
  {
    key: 'GITHUB_REF',
    value: process.env.GITHUB_REF,
  },
], 
...
```

# Define your own Slack message custom layout

You can define your own Slack message layout us

This function must return

See https://api.slack.com/block-kit/building

# License

# Contributing

# Something not working for you?

Feel free to [raise a github issue](https://github.com/ryanrosello-og/playwright-slack-report/issues) for any bugs or feature requests.
