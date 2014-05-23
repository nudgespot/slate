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
