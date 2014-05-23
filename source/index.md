---
title: Nudgespot API Reference

language_tabs:
  - shell
  - javascript

toc_footers:
  - <a href='http://www.nudgespot.com'>Sign Up for Nudgespot</a>

includes:

search: true
---

# Introduction

Thank you for your interest in Nudgespot's APIs! Nudgespot is the easiest way to send emails from your code. Easier than using Amazon SES/SendGrid. You can read more about our service [here](http://www.nudgespot.com).

### Feedback

If you have feedback on our APIs or would like to suggest feature requests for the APIs, contact [devs@nudgespot.com](mailto:devs@nudgespot.com).

# Overview
Nudgespot makes it easy to trigger emails from your code. Instead of writing mailer code, you only send events to Nudgespot, and then configure emails as triggers from our dashboard. This gives you the flexibility to change the email subject or body or template any time, without changing any code. Sending events to Nudgespot is easy, and works just like Google Analytics/Mixpanel.

To send events to Nudgespot, use our Tracking API below.

# Technical Overview

## REST
Our REST APIs are available at the following endpoint:

*Base API Endpoint:* https://api:&lt;your_api_secret_key&gt;@beta.nudgespot.com/

## Authentication
Nudgespot validates all your requests using authentication tokens. You can find your authentication tokens on your Nudgespot [settings page](https://beta.nudgespot.com/settings).

### REST
Our REST API expects that you pass your *API Secret Key* along with your requests. The format is:
https://api:&lt;api_secret_key&gt;@beta.nudgespot.com

```shell
# Authenticating via curl
curl "https://api:your_api_secret_key/api_endpoint_here"
```

# Tracking API

Nudgespot can accept events via a REST service or through javascript embedded on your website.

## Tracking activities via REST
To create a new activity via our REST API:

*POST* request should be made to the following url:

https://api:<api_secret_key>@beta.nudgespot.com/activities

The payload must be a *JSON* in the format below:

Parameter | Type | Description
--------- | ---- | -----------
event|String|Name of the activity. Use lowercase and underscore notation. *Examples:* "signup", "launch_campaign"
user|Hash|Email of the customer that performed this activity. Example: {"email": "foobar@example.com"}
timestamp|String| Date/time of the activity in ISO 8601 format
properties|Hash| Name value pairs of any additional properties of the activity {"name1": value1, "name2": "value2"},


```shell
# Sending an activity
curl -X POST "https://api:your_api_secret_key/activities" --header "Content-Type:application/json" -d "{'user' => { 'email' => 'foobar@example.com' },'timestamp' => '2014-01-27T09:15:00.000Z','properties' => {'plan': 'bronze'},'event' => 'Signup'}"
```

## Tracking activities via Javascript

Nudgespot's javascript exposes one global object: *nudgespot* with methods that allow you to identify a customer and track activities of that customer.

### Identify
Use **nudgespot.identify()** to identify a customer.

*Parameters:*

|Parameter|Type|Required|Description|
----------|---|--------|-----------|
email|String|Yes|Uniquely identifies a customer|
properties|Hash|Optional|Name value pair of properties of the customer. Example: {first_name: "Joe", last_name: "Dash", city: "Bangalore", phone: "+91 90000 00001", created: "2014-01-27T09:15:00.000Z"}

```javascript
//Send an activity
nudgespot.identify("foobar@example.com", {first_name: "Joe", last_name: "Dash", created: '2014-01-27T09:15:00.000Z', city: "Bangalore", phone: "+91 90000 00001"});
```

### Send event
Use **nudgespot.track()** to send an activity.

*Parameters:*

|Parameter|Type|Required|Description|
----------|---|--------|-----------|
name|String|Yes|Name of the event. Use lowercase and underscore notations. Examples: "signup", "view_catalog" |
properties|Hash|Optional|Name value pair of properties of the activity. Example: {plan: "bronze", currency: "USD", value: 100.23}

```javascript
//Send an activity
nudgespot.track("signup", {plan: "bronze", currency: "USD", value: 100.23});
```


# Nudgespot vs SendGrid (or SES)
SendGrid and SES are transactional email providers. But Nudgespot is a service that makes it easy to send and schedule emails based on events/actions performed by a customer. In fact, Nudgespot uses a provider like SendGrid to send emails.

I know we keep saying that it is easier to send mails via Nudgespot, than via SES/SendGrid. Time to back it up with facts. So here is the faceoff:

## Round 1 - Send an email on signup
A simple transactional email that you send to your customers, based on a transaction - in this case, a sign up event.

With Nudgespot, it is as easy as:

<code>nudgespot.track("signup", {email: "foobar@example.com", first_name: "Foo", plan: "Bronze"})</code>

Now on our dashboard, you can configure a trigger that sends an email on "signup" event. You can edit the mailer code using our rich composer, and can test out different subject lines, message body any time, without any code changes. Easy right?

Now compare this with SendGrid/SES. You'll need to code your html message. You also need to separately manage background jobs (or cron jobs) that queues these emails with these ESPs.

*Verdict*: We think it is a 1-0 in favour of Nudgespot. Right?

## Round 2 - A follow up email 3 days after signup

Want to test if it is better to send these welcome emails a week after someone signs up? With SendGrid/SES, you need a background job scheduled to queue these emails at the right time. Unnecessary overhead we say.

With Nudgespot, you can schedule your emails to be delivered any time after the event was processed - either a few hours or a couple of days after the event has been processed.

*Verdict*: Very easy with Nudgespot, right?

## Round 3 - Email with an offer of a demo
Let's say you want to send an email with an offer for a demo, only to customers that haven't done a particular activity. This is where it starts getting really easy with Nudgespot.

You can configure triggers with really complex rules. No messy database queries, no cron dependencies. To give you an idea of the power of Nudgespot, here are some triggers you can create with Nudgespot:

Customers that:
* did signup but did not create_campaign
* did add_to_cart but did not place_order within 3 days of add_to_cart
* did order a product in category "silk"

*Verdict*: You are looking for our signup page right? [Here](https://beta.nudgespot.com) you go!

## Conclusion
I hope you are seeing the benefits of Nudgespot. While you are used to sending emails directly from code, we hope you now see the benefits of using Nudgespot.
