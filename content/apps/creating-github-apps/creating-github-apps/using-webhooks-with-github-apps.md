---
title: Using webhooks with GitHub Apps
shortTitle: Using webhooks
intro: 'Your {% data variables.product.prodname_github_app %} can subscribe to webhook events to receive notifications whenever certain activity occurs.'
versions:
  fpt: '*'
  ghes: '*'
  ghae: '*'
  ghec: '*'
topics:
  - GitHub Apps
---

## About webhooks and {% data variables.product.prodname_github_apps %}

Webhooks enable your {% data variables.product.prodname_github_app %} to receive real-time notifications when events happen on {% data variables.product.prodname_dotcom %}, such as when someone pushes a commit or opens a pull request in a repository that your app can access. For more information about webhooks, see "[AUTOTITLE](/webhooks-and-events/webhooks/about-webhooks)."

You can configure your {% data variables.product.prodname_github_app %} to receive webhooks for specific events on {% data variables.product.prodname_dotcom %} and automatically take action on them. For more information about the types of webhooks you can receive, see "[AUTOTITLE](/webhooks-and-events/webhooks/webhook-events-and-payloads)."

To receive webhook events in your {% data variables.product.prodname_github_app %}, you must activate webhooks in the app settings and specify a webhook URL where {% data variables.product.prodname_dotcom %} will send the webhook payloads. For more information about creating and configuring a {% data variables.product.prodname_github_app %}, see "[AUTOTITLE](/apps/creating-github-apps/creating-github-apps/creating-a-github-app)."

If your app does not need to respond to webhooks or will only be used for authentication, you can turn off the webhook function in your app settings. You do not need to specify a webhook URL. For more information, see "[AUTOTITLE](/apps/creating-github-apps/creating-github-apps/creating-a-github-app)."

## Choosing a webhook URL

When you activate webhooks in the settings for your {% data variables.product.prodname_github_app %}, you will need to specify a webhook URL. The webhook URL is the address of a web server that will receive the webhook event payloads sent to your {% data variables.product.prodname_github_app %}. The server can then take action based on the content of the payload. You should choose a web server that's appropriate for the volume of webhook traffic that your {% data variables.product.prodname_github_app %} will encounter.

### Choosing a webhook URL for development and testing

While you develop and test your app, you can use a webhook payload delivery service like [Smee](https://smee.io/) to capture and forward webhook payloads to your local development environment. Never use Smee for an application in production, because Smee channels are not authenticated or secure. Alternatively, you can use a tool like [ngrok](https://dashboard.ngrok.com/get-started) or [localtunnel](https://localtunnel.github.io/www/) that exposes your local machine to the internet to receive the payloads.

#### Creating a webhook URL with Smee

You can use Smee to create a unique domain where {% data variables.product.prodname_dotcom %} can send webhook payloads, without exposing your local development to the internet. Smee calls this unique domain a "Webhook Proxy URL." You can use Smee's Webhook Proxy URL as the webhook URL for your {% data variables.product.prodname_github_app %}.

1. To use Smee to create a unique domain, go to https://smee.io and click **Start a new channel**.
1. On the Smee channel page, follow the instructions under "Use the CLI" to install and run the Smee client.
1. To connect your Smee webhook URL to your {% data variables.product.prodname_github_app %}, enter your unique Smee domain in the "Webhook URL" field of your app settings. For more information, see "[AUTOTITLE](/apps/creating-github-apps/creating-github-apps/creating-a-github-app)."

### Choosing a webhook URL for production

For an application in production that receives a low volume of webhook traffic, you can host it on any dynamic application server. The server-side code for handling the webhook can receive the event, deserialize its JSON payload, and decide what action to take, such as storing the data in a database or calling the {% data variables.product.prodname_dotcom %} API.

To handle a higher volume of webhook traffic for a large app in production, consider using asynchronous webhook handling on a dedicated server. You can achieve this by employing a queue, where the webhook handler pushes data to the queue, and separate processes perform subsequent actions based on the events. Additionally, you can use cloud functions such as [Azure Functions](https://azure.microsoft.com/en-us/products/functions/)
 or [AWS Lambda](https://aws.amazon.com/lambda/) to help scale the app for handling large volumes of webhook events.

## Securing your webhooks with a webhook secret

Once you've configured your server to receive payloads, it will listen for any payload sent to the server. For security reasons, you should limit incoming requests to only those originating from {% data variables.product.prodname_dotcom %}. You can do that by creating a webhook secret for your app.

To create a webhook secret for your GitHub App, type a secret token in your app settings under "Webhook secret." You should choose a random string of text with high entropy. For more information about how to create a webhook secret in your app settings, see "[AUTOTITLE](/apps/creating-github-apps/creating-github-apps/creating-a-github-app)."

After creating a webhook secret for your app, you will need to configure your server to securely store and validate the webhook secret token. For more information, see "[AUTOTITLE](/webhooks-and-events/webhooks/securing-your-webhooks)."

## Subscribing to webhook events

You can subscribe your {% data variables.product.prodname_github_app %} to receive webhook payloads for specific events. The specific webhook events that you can select in your app settings are determined by the type of permissions you selected for your app. You will first need to select the permissions you would like your app to have, and then you can subscribe your app to webhook events that are related to that set of permissions.

For example, if you would like your app to receive a webhook event payload whenever a new issue is opened in your repository, you would first need to give your app permission to access "Issues" under "Repository permissions." Then under "Subscribe to events" you can select "Issues."

For more information about the permissions that are required for each webhook event, see "[AUTOTITLE](/webhooks-and-events/webhooks/webhook-events-and-payloads)."
