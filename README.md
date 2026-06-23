# Google Meet Connector for Camunda 8

## Overview

The Google Meet Connector for Camunda 8 enables users to create instant Google Meet links, schedule meetings through Google Calendar, and manage existing meetings (update or delete) directly from Camunda workflows. It uses Google OAuth 2.0 for secure authentication and integrates with Google Meet and Calendar APIs. The connector supports attendee invitations, automated meeting link generation, and can be configured entirely through Camunda Connector Templates without requiring custom Java code.

---
## Connector UI

<img width="948" height="478" alt="connector" src="https://github.com/user-attachments/assets/e34c4379-99c2-4867-bff8-0504ec5bb3a1" />

## Features

- Create Instant Google Meet meetings
- Schedule meetings using Google Calendar
- Update scheduled meetings
- Cancel scheduled meetings
- Invite attendees
- Generate Google Meet links automatically
- Send calendar invitations to participants
---

## Compatibility

* Works with Camunda SaaS
* Works with Self-Managed Camunda

---

## Setup Guide


### Setting Up in Saas/Self managed Environment:
1.	Navigate to Camunda SaaS.
2.	Upload the connector template from https://github.com/ak-chaudhari423/google-meet-connector/blob/google-meet-connector.json or download it from marketplace.

---

## Tools Required

* Download Desktop Modeler (if needed):
  https://camunda.com/download/modeler/

---


## Using in Desktop Modeler

1. Go to:

```
Camunda Modeler → resources → element-templates
```

2. Paste the downloaded **Google Meet Connector Template JSON**

3. Restart Modeler


---

## Prerequisites

### 1. Create a Google Cloud Project

1. Open Google Cloud Console
2. Create a new project
3. Select the project

---

### 2. Enable Required APIs

Navigate to:

APIs & Services → Library

Enable the following APIs:

- Google Calendar API
- Google Meet API

---

### 3. Configure OAuth Consent Screen

Navigate to:

APIs & Services → OAuth Consent Screen

#### App Information

- App Name: Camunda Google Meet Connector
- User Support Email: your-email@gmail.com
- Developer Contact: your-email@gmail.com

#### Add Scopes

Add:

https://www.googleapis.com/auth/calendar

https://www.googleapis.com/auth/meetings.space.created

#### Add Test Users

Add the Gmail accounts that will authorize the application.

Example:

your-email@gmail.com

---

## Create OAuth Client

Navigate to:

APIs & Services → Credentials → Create Credentials → OAuth Client ID

Select:

Web Application

### Authorized Redirect URI

Example:

http://localhost:8080/oauth/callback

Google will generate:

- Client ID
- Client Secret

Example:

Client ID:
xxxxxxxx.apps.googleusercontent.com

Client Secret:
GOCSPX-xxxxxxxx

---

# Generate Authorization Code

Open the following URL in your browser:

https://accounts.google.com/o/oauth2/v2/auth?client_id=YOUR_CLIENT_ID&redirect_uri=http://localhost:8080/oauth/callback&response_type=code&scope=https://www.googleapis.com/auth/calendar+https://www.googleapis.com/auth/meetings.space.created&access_type=offline&prompt=consent

After login and consent, Google redirects to:

http://localhost:8080/oauth/callback?code=AUTHORIZATION_CODE

Copy the authorization code.

---

# Generate Access Token

Execute:

curl --location 'https://oauth2.googleapis.com/token' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'code=AUTHORIZATION_CODE' \
--data-urlencode 'client_id=CLIENT_ID' \
--data-urlencode 'client_secret=CLIENT_SECRET' \
--data-urlencode 'redirect_uri=http://localhost:8080/oauth/callback' \
--data-urlencode 'grant_type=authorization_code'

Response:

{
  "access_token": "ya29.xxxxx",
  "expires_in": 3599,
  "refresh_token": "1//0gxxxxx",
  "scope": "https://www.googleapis.com/auth/calendar https://www.googleapis.com/auth/meetings.space.created",
  "token_type": "Bearer"
}


---

# Camunda Integration



## Connector Inputs

| Field            |       Description         |
|------------------|---------------------------|
| accessToken      | Google OAuth Access Token |
| meetingOperation | Operation Type            |
| summary          | Meeting Title             |
| description      | Meeting Description       |
| startTime        | Start Date Time           |
| endTime          | End Date Time             |
| timeZone         | Optional Time Zone        |
| attendees        | Attendees JSON Array      |
| eventId          | Event ID for Update/Cancel|
| requestId        | Unique Request ID         |

---


# Advantages

- No custom Java code required
- Fully configurable from Camunda
- Supports Instant Meetings
- Supports Scheduled Meetings
- Supports Update/Cancel Operations
- Supports Google Calendar Invitations
- Uses Google OAuth 2.0 Standard Authentication

---

