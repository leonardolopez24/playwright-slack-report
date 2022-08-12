# playwright-slack-report ![Biulds](https://github.com/ryanrosello-og/playwright-slack-report/actions/workflows/playwright.yml/badge.svg) [![GitHub license](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/ryanrosello-og/playwright-slack-report/blob/master/LICENSE)

Publish your Playwright test results to your favorite Slack channel(s).

[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/ryanrosello-og/playwright-slack-report)

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
2. Click the Create New App button and select "From scratch"

![Navigate to https://api.slack.com/apps](https://github.com/ryanrosello-og/playwright-slack-report/blob/dev/assets/2022-08-09_5-37-11.png?raw=true)

3. Input a name for your app and select the target workspace, then click on the **Create App** button

![Input a name for your app and select the target workspace](https://github.com/ryanrosello-og/playwright-slack-report/blob/dev/assets/2022-08-09_5-40-51.png?raw=true)

4. Under the Features menu, select **OAuth & Permissions** and scroll down to **Scopes** section

![Under the Features menu select](https://github.com/ryanrosello-og/playwright-slack-report/blob/dev/assets/2022-08-09_5-44-29.png?raw=true)

5. Click the **Add an OAuth Scope** button and select the following scopes:

![Click the Add an OAuth Scope](https://github.com/ryanrosello-og/playwright-slack-report/blob/dev/assets/2022-08-09_5-48-30.png?raw=true)

* chat:write 
* chat:write.public 
* chat:write.customize  

6. Scroll up to the OAuth Tokens for Your Workspace and click the **Install to Workspace** button

![Install](https://github.com/ryanrosello-og/playwright-slack-report/blob/dev/assets/2022-08-09_5-55-22.png?raw=true)

> You will be prompted with the message below, click the Allow button

![click the Allow button](https://github.com/ryanrosello-og/playwright-slack-report/blob/dev/assets/2022-08-09_5-49-49.png?raw=true)

The final step will be to copy the generated Bot User OAuth Token aka `SLACK_BOT_USER_OAUTH_TOKEN`.  

>**Treat this token as a secret.**

![Final](https://github.com/ryanrosello-og/playwright-slack-report/blob/dev/assets/2022-08-09_5-53-17.png?raw=true)

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

You can define your own Slack message layout to suit your needs.

Firstly, install the necessary type definitions:

`yarn add @slack/types -D`


Next, define your layout function.  The signature of this should adhere to example below:

```typescript
import { Block, KnownBlock } from "@slack/types";
import { SummaryResults } from "playwright-slack-report/dist/src";

export function generateCustomLayout(summaryResults: SummaryResults) :Array<Block | KnownBlock>{
  // your implementation goes here
}
```

In your, `playwright.confing.ts` file, add your function into the config.

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
      ...   
    ],
  ],
```

>Pro Tip:  You can use the [block-kit provided by Slack when creating your layout.](https://api.slack.com/block-kit/building)

# License

# Contributing

# Something not working for you?

Feel free to [raise a github issue](https://github.com/ryanrosello-og/playwright-slack-report/issues) for any bugs or feature requests.
