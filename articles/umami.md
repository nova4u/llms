# https://umami.is/docs/api llms-full.txt

## Umami API Overview

[Umami](https://umami.is/docs)

[API](https://umami.is/docs/api)

[Reports](https://umami.is/docs/reports)

[Guides](https://umami.is/docs/guides)

[Cloud](https://umami.is/docs/cloud)

Menu

Getting started [Overview](https://umami.is/docs/api) [Authentication](https://umami.is/docs/api/authentication) [Sending stats](https://umami.is/docs/api/sending-stats)

Endpoints [Users](https://umami.is/docs/api/users-api) [Teams](https://umami.is/docs/api/teams-api) [Events](https://umami.is/docs/api/events-api) [Sessions](https://umami.is/docs/api/sessions-api) [Websites](https://umami.is/docs/api/websites-api) [Website stats](https://umami.is/docs/api/website-stats-api)

Clients [API client](https://umami.is/docs/api/api-client) [Node client](https://umami.is/docs/api/node-client)

Getting started

# API

Umami allows you to pull data directly by calling the application's API endpoints.
Any operation you can do through the application is available in the API. All data is returned in JSON format.

### Self-hosting

When self-hosting, the API endpoints are available at `http://<your-umami-instance>/api`. You would first need
to [authenticate](https://umami.is/docs/api/authentication) before making calls.

### Umami Cloud

If using Umami Cloud, you would make calls to `https://api.umami.is` using an [API key](https://umami.is/docs/cloud/api-key).

[Authentication](https://umami.is/docs/api/authentication)

On this page [Self-hosting](https://umami.is/docs/api#self-hosting) [Umami Cloud](https://umami.is/docs/api#umami-cloud)

## Umami API Authentication

[Umami](https://umami.is/docs)

[API](https://umami.is/docs/api)

[Reports](https://umami.is/docs/reports)

[Guides](https://umami.is/docs/guides)

[Cloud](https://umami.is/docs/cloud)

Menu

Getting started [Overview](https://umami.is/docs/api) [Authentication](https://umami.is/docs/api/authentication) [Sending stats](https://umami.is/docs/api/sending-stats)

Clients [API client](https://umami.is/docs/api/api-client) [Node client](https://umami.is/docs/api/node-client)

Getting started

# Authentication

The following authentication method is only for self-hosted Umami. For **Umami Cloud**, you simply
need to generate an [API key](https://umami.is/docs/cloud/api-key).

## POST /api/auth/login

First you need to get a _token_ in order to make API requests. You need to make a
`POST` request to the `/api/auth/login` endpoint with the following data:

```Code_code__NmYxN
{
  "username": "your-username",
  "password": "your-password"
}

```

If successful you should get a response like the following:

```Code_code__NmYxN
{
  "token": "eyTMjU2IiwiY...4Q0JDLUhWxnIjoiUE_A",
  "user": {
    "id": "cd33a605-d785-42a1-9365-d6cad3b7befd",
    "username": "your-username",
    "createdAt": "2020-04-20 01:00:00"
  }
}

```

Save the token value and send an `Authorization` header with all your data requests with the value `Bearer <token>`. Your request header should look something like this:

```Code_code__NmYxN
Authorization: Bearer eyTMjU2IiwiY...4Q0JDLUhWxnIjoiUE_A

```

For example, with `curl` it would look like this:

```Code_code__NmYxN
curl https://{yourserver}/api/websites
   -H "Accept: application/json"
   -H "Authorization: Bearer <token>"

```

The authorization token is expected with every API call that requires permissions.

---

## POST /api/auth/verify

You can verify if the token is still valid.

**Sample response**

```Code_code__NmYxN
{
  "id": "1a457e1a-121a-11ee-be56-0242ac120002",
  "username": "umami",
  "role": "admin",
  "isAdmin": true
}

```

[Overview](https://umami.is/docs/api) [Sending stats](https://umami.is/docs/api/sending-stats)

On this page [POST /api/auth/login](https://umami.is/docs/api/authentication#post-apiauthlogin) [POST /api/auth/verify](https://umami.is/docs/api/authentication#post-apiauthverify)

## Umami Sessions API

[Umami](https://umami.is/docs)

[API](https://umami.is/docs/api)

[Reports](https://umami.is/docs/reports)

[Guides](https://umami.is/docs/guides)

[Cloud](https://umami.is/docs/cloud)

Menu

Getting started [Overview](https://umami.is/docs/api) [Authentication](https://umami.is/docs/api/authentication) [Sending stats](https://umami.is/docs/api/sending-stats)

Endpoints [Users](https://umami.is/docs/api/users-api) [Teams](https://umami.is/docs/api/teams-api) [Events](https://umami.is/docs/api/events-api) [Sessions](https://umami.is/docs/api/sessions-api) [Websites](https://umami.is/docs/api/websites-api) [Website stats](https://umami.is/docs/api/website-stats-api)

Clients [API client](https://umami.is/docs/api/api-client) [Node client](https://umami.is/docs/api/node-client)

Endpoints

# Sessions

Operations around Sessions and Session data.

**Endpoints**

```Code_code__NmYxN
GET /api/websites/:websiteId/sessions
GET /api/websites/:websiteId/sessions/stats
GET /api/websites/:websiteId/sessions/:sessionId
GET /api/websites/:websiteId/sessions/:sessionId/activity
GET /api/websites/:websiteId/sessions/:sessionId/properties
GET /api/websites/:websiteId/session-data/properties
GET /api/websites/:websiteId/session-data/values

```

---

## GET /api/websites/:websiteId/sessions

Gets website session details within a given time range.

**Parameters**

- `startAt`: Timestamp (in ms) of starting date.
- `endAt`: Timestamp (in ms) of end date.
- `query`: (optional string) Search text.
- `page`: (optional number, default 1) Determines page.
- `pageSize`: (optional string) Determines how many results to return.
- `orderBy`: (optional string) Order by column name.

**Sample response**

URL: `https://umami.mydomain.com/api/websites/86d4095c-a2a8-4fc8-9521-103e858e2b41/sessions?page=1&startAt=1725865200000&endAt=1725951599999&pageSize=20`

```Code_code__NmYxN
{
  "data": [\
    {\
      "id": "5a45d799-e6f5-56ac-9893-fde6f769c36a",\
      "websiteId": "86d4095c-a2a8-4fc8-9521-103e858e2b41",\
      "hostname": "umami.is",\
      "browser": "chrome",\
      "os": "Windows 10",\
      "device": "laptop",\
      "screen": "1536x960",\
      "language": "en-US",\
      "country": "US",\
      "subdivision1": "US-CA",\
      "city": "Roseville",\
      "firstAt": "2024-09-09T17:44:35Z",\
      "lastAt": "2024-09-09T17:44:35Z",\
      "visits": 1,\
      "views": 1,\
      "createdAt": "2024-09-09T17:44:35Z"\
    },\
    {\
      "id": "753ce23b-0dab-5a33-badc-4d2239528dd9",\
      "websiteId": "86d4095c-a2a8-4fc8-9521-103e858e2b41",\
      "hostname": "umami.is",\
      "browser": "chrome",\
      "os": "Windows 10",\
      "device": "desktop",\
      "screen": "2560x1440",\
      "language": "de-DE",\
      "country": "DE",\
      "subdivision1": "DE-BY",\
      "city": "Munich",\
      "firstAt": "2024-09-09T08:54:18Z",\
      "lastAt": "2024-09-09T17:44:29Z",\
      "visits": 5,\
      "views": 66,\
      "createdAt": "2024-09-09T17:44:29Z"\
    }\
  ],
  "count": 1000,
  "page": 1,
  "pageSize": 20
}

```

---

## GET /api/websites/:websiteId/sessions/stats

Gets summarized website session statistics.

**Parameters**

- `startAt`: Timestamp (in ms) of starting date.
- `endAt`: Timestamp (in ms) of end date.
- `url`: (optional) Name of URL.
- `referrer`: (optional) Name of referrer.
- `title`: (optional) Name of page title.
- `query`: (optional) Name of query.
- `event`: (optional) Name of event.
- `host`: (optional) Name of hostname.
- `os`: (optional) Name of operating system.
- `browser`: (optional) Name of browser.
- `device`: (optional) Name of device (ex. Mobile)
- `country`: (optional) Name of country.
- `region`: (optional) Name of region/state/province.
- `city`: (optional) Name of city.

**Sample response**

URL: `https://umami.mydomain.com/api/websites/86d4095c-a2a8-4fc8-9521-103e858e2b41/sessions/stats?startAt=1656679719687&endAt=1656766119687`

```Code_code__NmYxN
{
  "pageviews": { "value": 3018 },
  "visitors": { "value": 847 },
  "visits": { "value": 984 },
  "countries": { "value": 537 },
  "events": { "value": 150492 }
}

```

- `pageviews`: Pages hits
- `visitors`: Number of unique visitors
- `visits`: Number of sessions
- `countries`: Number of unique countries
- `events`: Number of custom events

---

## GET /api/websites/:websiteId/sessions/:sessionId

Gets session details for a individual session

**Parameters**

None

**Sample response**

URL: `https://umami.mydomain.com/api/websites/86d4095c-a2a8-4fc8-9521-103e858e2b41/sessions/a35fe227-2fe9-5147-85a0-15f0fd48faed`

```Code_code__NmYxN
{
  "id": "a35fe227-2fe9-5147-85a0-15f0fd48faed",
  "websiteId": "86d4095c-a2a8-4fc8-9521-103e858e2b41",
  "hostname": "umami.is",
  "browser": "ios",
  "os": "iOS",
  "device": "mobile",
  "screen": "390x844",
  "language": "en-US",
  "country": "US",
  "subdivision1": "US-IN",
  "city": "Bloomington",
  "firstAt": "2024-09-09T18:12:01Z",
  "lastAt": "2024-09-09T18:12:08Z",
  "visits": 1,
  "views": 3,
  "events": 0,
  "totaltime": 7
}

```

---

## GET /api/websites/:websiteId/sessions/:sessionId/activity

Gets session activity for a individual session

**Parameters**

- `startAt`: Timestamp (in ms) of starting date.
- `endAt`: Timestamp (in ms) of end date.

**Sample response**

URL: `https://umami.mydomain.com/api/websites/86d4095c-a2a8-4fc8-9521-103e858e2b41/sessions/a35fe227-2fe9-5147-85a0-15f0fd48faed/activity?startAt=1725905521000&endAt=1725905528000`

```Code_code__NmYxN
[\
  {\
    "createdAt": "2024-09-09T18:12:08Z",\
    "urlPath": "/websites/fd4bdd56-2135-4c42-9518-55e72669f23c",\
    "urlQuery": "",\
    "referrerDomain": "",\
    "eventId": "a154aebc-10cf-4a26-8187-c541ef0d2929",\
    "eventType": 1,\
    "eventName": "",\
    "visitId": "609a5116-b139-5c6d-9293-fbea9b8a3b61"\
  },\
  {\
    "createdAt": "2024-09-09T18:12:01Z",\
    "urlPath": "/settings/websites",\
    "urlQuery": "",\
    "referrerDomain": "",\
    "eventId": "92246b7b-e3e2-4c9d-b61e-fab69d993ad7",\
    "eventType": 1,\
    "eventName": "",\
    "visitId": "609a5116-b139-5c6d-9293-fbea9b8a3b61"\
  }\
]

```

---

## GET /api/websites/:websiteId/sessions/:sessionId/properties

Gets session properties for a individual session

**Parameters**

None

**Sample response**

URL: `https://umami.mydomain.com/api/websites/86d4095c-a2a8-4fc8-9521-103e858e2b41/sessions/a35fe227-2fe9-5147-85a0-15f0fd48faed/properties`

```Code_code__NmYxN
[\
  {\
    "websiteId": "1ccddccf-7d34-432b-84e6-e46903b2acf3",\
    "sessionId": "a35fe227-2fe9-5147-85a0-15f0fd48faed",\
    "dataKey": "email",\
    "dataType": 1,\
    "stringValue": "johndoe@gmail.com",\
    "numberValue": null,\
    "dateValue": null,\
    "createdAt": "2024-09-09T18:12:01Z"\
  },\
  {\
    "websiteId": "1ccddccf-7d34-432b-84e6-e46903b2acf3",\
    "sessionId": "a35fe227-2fe9-5147-85a0-15f0fd48faed",\
    "dataKey": "name",\
    "dataType": 1,\
    "stringValue": "John Doe",\
    "numberValue": null,\
    "dateValue": null,\
    "createdAt": "2024-09-09T18:12:01Z"\
  }\
]

```

## GET /api/websites/:websiteId/session-data/properties

Gets session data counts by property name

**Parameters**

- `startAt`: Timestamp (in ms) of starting date.
- `endAt`: Timestamp (in ms) of end date.

**Sample response**

URL: `https://umami.mydomain.com/api/websites/86d4095c-a2a8-4fc8-9521-103e858e2b41/session-data/properties?startAt=1717916400000&endAt=1725692399999`

```Code_code__NmYxN
[\
  {\
    "propertyName": "id",\
    "total": 1039\
  },\
  {\
    "propertyName": "region",\
    "total": 1039\
  },\
  {\
    "propertyName": "name",\
    "total": 1039\
  },\
  {\
    "propertyName": "email",\
    "total": 1039\
  }\
]

```

---

## GET /api/websites/:websiteId/session-data/values

Gets session data counts for a given property

**Parameters**

- `startAt`: Timestamp (in ms) of starting date.
- `endAt`: Timestamp (in ms) of end date.
- `propertyName`: Property name.

**Sample response**

URL: `https://umami.mydomain.com/api/websites/86d4095c-a2a8-4fc8-9521-103e858e2b41/session-data/values?startAt=1717916400000&endAt=1725692399999&propertyName=region`

```Code_code__NmYxN
[\
  { "value": "EU", "total": 609 },\
  { "value": "US", "total": 431 }\
]

```

[Events](https://umami.is/docs/api/events-api) [Websites](https://umami.is/docs/api/websites-api)

On this page [GET /api/websites/:websiteId/sessions](https://umami.is/docs/api/sessions-api#get-apiwebsiteswebsiteidsessions) [GET /api/websites/:websiteId/sessions/stats](https://umami.is/docs/api/sessions-api#get-apiwebsiteswebsiteidsessionsstats) [GET /api/websites/:websiteId/sessions/:sessionId](https://umami.is/docs/api/sessions-api#get-apiwebsiteswebsiteidsessionssessionid) [GET /api/websites/:websiteId/sessions/:sessionId/activity](https://umami.is/docs/api/sessions-api#get-apiwebsiteswebsiteidsessionssessionidactivity) [GET /api/websites/:websiteId/sessions/:sessionId/properties](https://umami.is/docs/api/sessions-api#get-apiwebsiteswebsiteidsessionssessionidproperties) [GET /api/websites/:websiteId/session-data/properties](https://umami.is/docs/api/sessions-api#get-apiwebsiteswebsiteidsession-dataproperties) [GET /api/websites/:websiteId/session-data/values](https://umami.is/docs/api/sessions-api#get-apiwebsiteswebsiteidsession-datavalues)

## Umami API Client

[Umami](https://umami.is/docs)

[API](https://umami.is/docs/api)

[Reports](https://umami.is/docs/reports)

[Guides](https://umami.is/docs/guides)

[Cloud](https://umami.is/docs/cloud)

Menu

Getting started [Overview](https://umami.is/docs/api) [Authentication](https://umami.is/docs/api/authentication) [Sending stats](https://umami.is/docs/api/sending-stats)

Endpoints [Users](https://umami.is/docs/api/users-api) [Teams](https://umami.is/docs/api/teams-api) [Events](https://umami.is/docs/api/events-api) [Sessions](https://umami.is/docs/api/sessions-api) [Websites](https://umami.is/docs/api/websites-api) [Website stats](https://umami.is/docs/api/website-stats-api)

Clients [API client](https://umami.is/docs/api/api-client) [Node client](https://umami.is/docs/api/node-client)

Clients

# API client

## Overview

Umami API Client is built in TypeScript and contains functions to call every API endpoint available in Umami.

## Requirements

- [Node.js](https://nodejs.org/) version 18.18 or newer

## Installation

```Code_code__NmYxN
npm install @umami/api-client

```

## Configure

The following environment variables are required to call your own API.

```Code_code__NmYxN
UMAMI_API_CLIENT_USER_ID
UMAMI_API_CLIENT_SECRET
UMAMI_API_CLIENT_ENDPOINT

```

To access Umami Cloud, these environment variables are required.

```Code_code__NmYxN
UMAMI_API_KEY
UMAMI_API_CLIENT_ENDPOINT

```

More details on accessing Umami Cloud can be found under [API key](https://umami.is/docs/cloud/api-key).

## Usage

Import the configured api-client and query using the available class methods.

```Code_code__NmYxN
import { getClient } from '@umami/api-client';

const client = getClient();

const { ok, data, status, error } = await client.getWebsites();

```

The result will come back in the following format.

```Code_code__NmYxN
{
  ok: boolean;
  status: number;
  data?: T;
  error?: any;
}

```

## API Client function mapping

### Me

```Code_code__NmYxN
getMe() ⇒ GET /me
updateMyPassword(data) ⇒ POST /me/password
getMyWebsites() ⇒ GET /me/websites

```

### Users

```Code_code__NmYxN
getUsers() ⇒ GET /users
createUser(data) ⇒ POST /users
getUser(id) ⇒ GET /users/{id}
updateUser(id, data) ⇒ POST /users/{id}
deleteUser(id) ⇒ DEL /users/{id}
getUserWebsites(id) ⇒ GET /users/{id}/websites
getUserUsage(id, data) ⇒ GET /users/{id}/usage

```

### Teams

```Code_code__NmYxN
getTeams() ⇒ GET /teams
createTeam(data) ⇒ POST /teams
joinTeam(data) ⇒ POST /teams/join
getTeam(id) ⇒ GET /teams/{id}
updateTeam(id) ⇒ POST /teams/{id}
deleteTeam(id) ⇒ DEL /teams/{id}
getTeamUsers(id) ⇒ GET /teams/{id}/users
deleteTeamUser(teamId, userId): DEL /teams/{teamId}/users/{userId}
getTeamWebsites(id) ⇒ GET /teams/{id}/websites
createTeamWebsites(id, data) ⇒ GET /teams/{id}/websites
deleteTeamWebsite(teamId, websiteId) ⇒ DEL /teams/{teamId}/websites/{websiteId}

```

### Websites

```Code_code__NmYxN
getWebsites() ⇒ GET /websites
createWebsite(data) ⇒ POST /websites
getWebsite(id) ⇒ GET /websites/{id}
updateWebsite(id, data) ⇒ POST /websites/{id}
deleteWebsite(id) ⇒ DEL /websites/{id}
getWebsiteActive(id) ⇒ GET /websites/{id}/active
getWebsiteEvents(id, data) ⇒ GET /websites/{id}/events
getWebsiteMetrics(id, data) ⇒ GET /websites/{id}/metrics
getWebsitePageviews(id, data) ⇒ GET /websites/{id}/pageviews
resetWebsite(id) ⇒ GET /websites/{id}/reset
getWebsiteStats(id, data) ⇒ GET /websites/{id}/stats

```

### Event Data

```Code_code__NmYxN
getEventDataEvents(id, data) ⇒ GET /event-data/events
getEventDataFields(id, data) ⇒ GET /event-data/fields
getEventDataStats(id, data) ⇒ GET /event-data/stats

```

## Environment Variables

**UMAMI_API_CLIENT_USER_ID = <user uuid>**

The `USER_ID` of the User performing the API calls. Permission restrictions will apply based on application settings.

**UMAMI_API_CLIENT_SECRET = <random string>**

A random string used to generate unique values. This needs to match the `APP_SECRET` used in the Umami application.

**UMAMI_API_CLIENT_ENDPOINT = <API endpoint>**

The endpoint of your Umami API. Example: `https://{yourserver}/api/`

**UMAMI_API_KEY = <API Key string>**

A unique string provided by Umami Cloud.

[Website stats](https://umami.is/docs/api/website-stats-api) [Node client](https://umami.is/docs/api/node-client)

On this page [Overview](https://umami.is/docs/api/api-client#overview) [Requirements](https://umami.is/docs/api/api-client#requirements) [Installation](https://umami.is/docs/api/api-client#installation) [Configure](https://umami.is/docs/api/api-client#configure) [Usage](https://umami.is/docs/api/api-client#usage) [API Client function mapping](https://umami.is/docs/api/api-client#api-client-function-mapping) [Me](https://umami.is/docs/api/api-client#me) [Users](https://umami.is/docs/api/api-client#users) [Teams](https://umami.is/docs/api/api-client#teams) [Websites](https://umami.is/docs/api/api-client#websites) [Event Data](https://umami.is/docs/api/api-client#event-data) [Environment Variables](https://umami.is/docs/api/api-client#environment-variables)

## Teams API Overview

[Umami](https://umami.is/docs)

[API](https://umami.is/docs/api)

[Reports](https://umami.is/docs/reports)

[Guides](https://umami.is/docs/guides)

[Cloud](https://umami.is/docs/cloud)

Menu

Getting started [Overview](https://umami.is/docs/api) [Authentication](https://umami.is/docs/api/authentication) [Sending stats](https://umami.is/docs/api/sending-stats)

Endpoints [Users](https://umami.is/docs/api/users-api) [Teams](https://umami.is/docs/api/teams-api) [Events](https://umami.is/docs/api/events-api) [Sessions](https://umami.is/docs/api/sessions-api) [Websites](https://umami.is/docs/api/websites-api) [Website stats](https://umami.is/docs/api/website-stats-api)

Clients [API client](https://umami.is/docs/api/api-client) [Node client](https://umami.is/docs/api/node-client)

Endpoints

# Teams

Operations around Team management.

**Endpoints**

```Code_code__NmYxN
POST /api/teams
POST /api/teams/join
POST /api/teams/:teamId
GET /api/teams/:teamId
DELETE /api/teams/:teamId
POST /api/teams/:teamId/users
GET /api/teams/:teamId/users
GET /api/teams/:teamId/users/:userId
POST /api/teams/:teamId/users/:userId
DELETE /api/teams/:teamId/users/:userId
GET /api/teams/:teamId/websites

```

---

## POST /api/teams

Creates a team.

**Parameters**

- `name`: (string) The team's name.

**Sample response**

```Code_code__NmYxN
[\
  {\
    "accessCode": "xxwtoY8pzKjDIUQi",\
    "createdAt": "2023-04-13T20:22:55.756Z",\
    "id": "ae09865c-ff82-4c43-acba-dd1e6540cda5",\
    "name": "My Team",\
    "updatedAt": null\
  },\
  {\
    "createdAt": "2023-04-13T20:22:55.756Z",\
    "id": "a9b1fbbc-ac22-4422-aa74-b2a2751ad87d",\
    "role": "team-owner",\
    "teamId": "ae09865c-ff82-4c43-acba-dd1e6540cda5",\
    "updatedAt": null,\
    "userId": "1a457e1a-121a-11ee-be56-0242ac120002"\
  }\
]

```

---

## POST /api/teams/join

Join a team.

**Parameters**

- `accessCode`: (string) The team's access code.

**Sample response**

```Code_code__NmYxN
[\
  {\
    "accessCode": "xxwtoY8pzKjDIUQi",\
    "createdAt": "2023-04-13T20:22:55.756Z",\
    "id": "ae09865c-ff82-4c43-acba-dd1e6540cda5",\
    "name": "My Team",\
    "teamUser": [\
      {\
        "createdAt": "2023-04-13T20:22:55.756Z",\
        "id": "a9b1fbbc-ac22-4422-aa74-b2a2751ad87d",\
        "role": "team-owner",\
        "teamId": "ae09865c-ff82-4c43-acba-dd1e6540cda5",\
        "updatedAt": null,\
        "user": { "id": "1a457e1a-121a-11ee-be56-0242ac120002", "username": "admin" },\
        "userId": "1a457e1a-121a-11ee-be56-0242ac120002"\
      }\
    ]\
  }\
]

```

---

## GET /api/teams/:teamId

Get a team.

**Parameters**

None

**Sample response**

```Code_code__NmYxN
{
  "id": "a1645e87-6107-4dcf-96a4-1583c5a4d5cf",
  "name": "My Team",
  "accessCode": "team_cVQf0mYPriZlPAgK",
  "logoUrl": null,
  "createdAt": "2025-03-10T17:59:42.325Z",
  "updatedAt": "2025-03-10T17:59:42.325Z",
  "deletedAt": null,
  "teamUser": [\
    {\
      "id": "9bd52350-e1d4-457c-8181-45b2128b8810",\
      "teamId": "a1645e87-6107-4dcf-96a4-1583c5a4d5cf",\
      "userId": "41e2b680-648e-4b09-bcd7-3e2b10c06264",\
      "role": "team-owner",\
      "createdAt": "2025-03-10T17:59:42.325Z",\
      "updatedAt": "2025-03-10T17:59:42.325Z"\
    }\
  ]
}

```

---

## POST /api/teams/:teamId

Update a team.

**Parameters**

- `name`: (optional string) The team's name.
- `accessCode`: (optional string) The team's access code.

**Sample response**

```Code_code__NmYxN
{
  "accessCode": "xxwtoY8pzKjDIUQi",
  "createdAt": "2023-04-13T20:22:55.756Z",
  "id": "ae09865c-ff82-4c43-acba-dd1e6540cda5",
  "name": "My Team",
  "updatedAt": null
}

```

---

## DELETE /api/teams/:teamId

Delete a team.

**Parameters**

None

**Sample response**

```Code_code__NmYxN
ok

```

---

## GET /api/teams/:teamId/users

Get all users that belong to a team.

**Parameters**

- `query`: (optional string) Search text.
- `page`: (optional number, default 1) Determines page.
- `pageSize`: (optional string) Determines how many results to return.
- `orderBy`: (optional string, default `name`) Order by column name.

**Sample response**

```Code_code__NmYxN
{
  "data": [\
    {\
      "id": "cbb58c3d-4f84-47c7-a394-b6efd3997c22",\
      "teamId": "f5f787dc-4cee-415c-a266-16fcab6520e5",\
      "userId": "41e2b680-648e-4b09-bcd7-3e2b10c06264",\
      "role": "team-owner",\
      "createdAt": "2025-03-10T18:20:13.333Z",\
      "updatedAt": "2025-03-10T18:20:13.333Z",\
      "user": { "id": "41e2b680-648e-4b09-bcd7-3e2b10c06264", "username": "admin" }\
    }\
  ],
  "count": 1,
  "page": 1,
  "pageSize": 10
}

```

---

## POST /api/teams/:teamId/users

Add a user to a team.

**Parameters**

- `userId`: ID of user to be added.
- `role`: Role user will be added as ( `team-member/team-view-only/team-manager`).

**Sample response**

```Code_code__NmYxN
{
  "id": "dcf462b4-0eb3-4ad9-8c5d-9b18bc7825e2",
  "teamId": "f5f787dc-4cee-415c-a266-16fcab6520e5",
  "userId": "6858ce93-22d9-48e3-8b33-fb355d63b6b4",
  "role": "team-member",
  "createdAt": "2025-03-10T19:31:21.773Z",
  "updatedAt": "2025-03-10T19:31:21.773Z"
}

```

---

## GET /api/teams/:teamId/users/:userId

Get a user belonging to a team.

**Parameters**

None

**Sample response**

```Code_code__NmYxN
{
  "id": "a9b1fbbc-ac22-4422-aa74-b2a2751ad87d",
  "teamId": "ae09865c-ff82-4c43-acba-dd1e6540cda5",
  "userId": "1a457e1a-121a-11ee-be56-0242ac120002",
  "role": "team-owner",
  "createdAt": "2023-04-13T20:22:55.756Z",
  "updatedAt": null
}

```

---

## POST /api/teams/:teamId/users/:userId

Update a user's role on a team.

**Parameters**

- `role`: Role user will be added as ( `member` or `view-only`).

**Sample response**

```Code_code__NmYxN
ok

```

---

## DELETE /api/teams/:teamId/users/:userId

Remove a user from a team.

**Parameters**

None

**Sample response**

```Code_code__NmYxN
ok

```

---

## GET /api/teams/:teamId/websites

Get all websites that belong to a team.

**Parameters**

- `query`: (optional string) Search text.
- `page`: (optional number, default 1) Determines page.
- `pageSize`: (optional string) Determines how many results to return.
- `orderBy`: (optional string, default `name`) Order by column name.

**Sample response**

```Code_code__NmYxN
[\
  {\
    "id": "40743097-fb48-411c-98bb-efb2aa7d3946",\
    "name": "website",\
    "domain": "website.com",\
    "shareId": "C36MKJqBlRT9UQgq",\
    "resetAt": "2023-10-25T17:29:44.340Z",\
    "userId": "a36560ed-e21c-4ed9-84aa-c86654460cae",\
    "createdAt": "2022-10-07T18:58:52.435Z",\
    "updatedAt": "2022-10-25T17:29:44.410Z",\
    "deletedAt": null,\
    "teamWebsite": [\
      {\
        "id": "40743097-fb48-411c-98bb-efb2aa7d3946",\
        "teamId": "66c8f57c-dec0-46d1-b267-886833d78982",\
        "websiteId": "40723097-fb48-411c-98bb-efb2ca7d3946",\
        "createdAt": "2022-10-15T16:51:44.711Z",\
        "team": {\
          "id": "66c8f87c-dec0-46d1-b267-886833d78982",\
          "name": "Caoboys",\
          "accessCode": "4bwY3JoHUXcKUiMP",\
          "createdAt": "2022-04-03T05:50:28.601Z",\
          "updatedAt": "2022-09-06T17:08:25.585Z",\
          "teamUser": [\
            {\
              "id": "3509ece6-3f7d-4a45-8842-2c070f1138e3",\
              "teamId": "66c8f87c-dec0-46d1-b267-886833d78982",\
              "userId": "a36560ed-e21c-4ed9-84aa-c86654460cae",\
              "role": "team-owner",\
              "createdAt": "2022-04-03T05:50:28.601Z",\
              "updatedAt": null\
            }\
          ]\
        }\
      }\
    ],\
    "user": { "id": "a36560ed-e21c-4ed9-84aa-c86654460cae", "username": "admin@website.com" }\
  }\
]

```

[Users](https://umami.is/docs/api/users-api) [Events](https://umami.is/docs/api/events-api)

On this page [POST /api/teams](https://umami.is/docs/api/teams-api#post-apiteams) [POST /api/teams/join](https://umami.is/docs/api/teams-api#post-apiteamsjoin) [GET /api/teams/:teamId](https://umami.is/docs/api/teams-api#get-apiteamsteamid) [POST /api/teams/:teamId](https://umami.is/docs/api/teams-api#post-apiteamsteamid) [DELETE /api/teams/:teamId](https://umami.is/docs/api/teams-api#delete-apiteamsteamid) [GET /api/teams/:teamId/users](https://umami.is/docs/api/teams-api#get-apiteamsteamidusers) [POST /api/teams/:teamId/users](https://umami.is/docs/api/teams-api#post-apiteamsteamidusers) [GET /api/teams/:teamId/users/:userId](https://umami.is/docs/api/teams-api#get-apiteamsteamidusersuserid) [POST /api/teams/:teamId/users/:userId](https://umami.is/docs/api/teams-api#post-apiteamsteamidusersuserid) [DELETE /api/teams/:teamId/users/:userId](https://umami.is/docs/api/teams-api#delete-apiteamsteamidusersuserid) [GET /api/teams/:teamId/websites](https://umami.is/docs/api/teams-api#get-apiteamsteamidwebsites)

## Websites API Overview

[Umami](https://umami.is/docs)

[API](https://umami.is/docs/api)

[Reports](https://umami.is/docs/reports)

[Guides](https://umami.is/docs/guides)

[Cloud](https://umami.is/docs/cloud)

Menu

Getting started [Overview](https://umami.is/docs/api) [Authentication](https://umami.is/docs/api/authentication) [Sending stats](https://umami.is/docs/api/sending-stats)

Endpoints [Users](https://umami.is/docs/api/users-api) [Teams](https://umami.is/docs/api/teams-api) [Events](https://umami.is/docs/api/events-api) [Sessions](https://umami.is/docs/api/sessions-api) [Websites](https://umami.is/docs/api/websites-api) [Website stats](https://umami.is/docs/api/website-stats-api)

Clients [API client](https://umami.is/docs/api/api-client) [Node client](https://umami.is/docs/api/node-client)

Endpoints

# Websites

Operations around Website management and statistics.

**Endpoints**

```Code_code__NmYxN
GET /api/websites
POST /api/websites
GET /api/websites/:websiteId
POST /api/websites/:websiteId
DELETE /api/websites/:websiteId
POST /api/websites/:websiteId/reset

```

---

## GET /api/websites

Returns all tracked websites.

**Parameters**

- `query`: (optional string) Search text.
- `page`: (optional number, default 1) Determines page.
- `pageSize`: (optional string) Determines how many results to return.
- `orderBy`: (optional string, default `name`) Order by column name.

**Sample response**

```Code_code__NmYxN
[\
  {\
    "id": "02d89813-7a72-41e1-87f0-8d668f85008b",\
    "name": "My Website",\
    "domain": "mywebsite.com",\
    "shareId": null,\
    "resetAt": null,\
    "websiteId": "1a457e1a-121a-11ee-be56-0242ac120002",\
    "createdAt": "2023-04-10T23:06:44.250Z",\
    "updatedAt": null,\
    "deletedAt": null\
  }\
]

```

---

## POST /api/websites

Creates a website.

**Parameters**

- `domain`: (string) The full domain of the tracked website.
- `name`: (string) The name of the website in Umami.
- `shareId`: (optional string) A unique string to enable a share url. Set `null` to unshare.
- `teamId`: (optional string) The ID of the team the website will be created under.

**Sample response**

```Code_code__NmYxN
{
  "id": 4,
  "websiteUuid": "51f73213-3f01-4343-a135-25496a3ffd31",
  "websiteId": 2,
  "name": "Umami",
  "domain": "umami.is",
  "shareId": "8PWex1pa",
  "createdAt": "2021-07-26T17:17:52.846Z"
}

```

---

## GET /api/websites/:websiteId

Gets a website by ID.

**Parameters**

None

**Sample response**

```Code_code__NmYxN
{
  "id": "02d89813-7a72-41e1-87f0-8d668f85008b",
  "name": "My Website",
  "domain": "mywebsite.com",
  "shareId": null,
  "resetAt": null,
  "userId": "1a457e1a-121a-11ee-be56-0242ac120002",
  "createdAt": "2023-04-10T23:06:44.250Z",
  "updatedAt": null,
  "deletedAt": null
}

```

---

## POST /api/websites/:websiteId

Updates a website.

**Parameters**

- `name`: (optional string) The name of the website in Umami.
- `domain`: (optional string) The full domain of the tracked website.
- `shareId`: (optional string) A unique string to enable a share url. Set `null` to unshare.

**Sample response**

```Code_code__NmYxN
{
  "id": "02d89813-7a72-41e1-87f0-8d668f85008b",
  "name": "My Website",
  "domain": "mywebsite.com",
  "shareId": null,
  "resetAt": null,
  "userId": "1a457e1a-121a-11ee-be56-0242ac120002",
  "createdAt": "2023-04-10T23:06:44.250Z",
  "updatedAt": null,
  "deletedAt": null
}

```

---

## DELETE /api/websites/:websiteId

Deletes a website.

**Parameters**

None

**Sample response**

```Code_code__NmYxN
ok

```

---

## POST /api/websites/:websiteId/reset

Resets a website by removing all data related to the website.

**Parameters**

None

**Sample response**

```Code_code__NmYxN
ok

```

[Sessions](https://umami.is/docs/api/sessions-api) [Website stats](https://umami.is/docs/api/website-stats-api)

On this page [GET /api/websites](https://umami.is/docs/api/websites-api#get-apiwebsites) [POST /api/websites](https://umami.is/docs/api/websites-api#post-apiwebsites) [GET /api/websites/:websiteId](https://umami.is/docs/api/websites-api#get-apiwebsiteswebsiteid) [POST /api/websites/:websiteId](https://umami.is/docs/api/websites-api#post-apiwebsiteswebsiteid) [DELETE /api/websites/:websiteId](https://umami.is/docs/api/websites-api#delete-apiwebsiteswebsiteid) [POST /api/websites/:websiteId/reset](https://umami.is/docs/api/websites-api#post-apiwebsiteswebsiteidreset)

## Umami Users API

[Umami](https://umami.is/docs)

[API](https://umami.is/docs/api)

[Reports](https://umami.is/docs/reports)

[Guides](https://umami.is/docs/guides)

[Cloud](https://umami.is/docs/cloud)

Menu

Getting started [Overview](https://umami.is/docs/api) [Authentication](https://umami.is/docs/api/authentication) [Sending stats](https://umami.is/docs/api/sending-stats)

Endpoints [Users](https://umami.is/docs/api/users-api) [Teams](https://umami.is/docs/api/teams-api) [Events](https://umami.is/docs/api/events-api) [Sessions](https://umami.is/docs/api/sessions-api) [Websites](https://umami.is/docs/api/websites-api) [Website stats](https://umami.is/docs/api/website-stats-api)

Clients [API client](https://umami.is/docs/api/api-client) [Node client](https://umami.is/docs/api/node-client)

Endpoints

# Users

Operations around User management.

**Endpoints**

```Code_code__NmYxN
POST /api/users
GET /api/admin/users
POST /api/users/:userId
GET /api/users/:userId
DELETE /api/users/:userId
GET /api/users/:userId/websites
GET /api/users/:userId/teams

```

---

## POST /api/users

Creates a user.

**Parameters**

- `username`: (string) The user's username.
- `password`: (string) The user's password.
- `role`: (string) Determines user access, `admin/user/view-only`.

**Sample response**

```Code_code__NmYxN
{
  "id": "1a457e1a-121a-11ee-be56-0242ac120002",
  "username": "umami",
  "role": "admin",
  "createdAt": "2023-04-13T20:22:55.756Z"
}

```

---

## GET /api/admin/users

Returns all users. Admin access is required.

**Parameters**

None

**Sample response**

```Code_code__NmYxN
[\
  {\
    "id:" "1a457e1a-121a-11ee-be56-0242ac120002",\
    "username": "umami",\
    "role": "admin",\
    "createdAt": "2023-04-13T20:22:55.756Z"\
  }\
]

```

---

## GET /api/users/:userId

Gets a user by ID.

**Parameters**

None

**Sample response**

```Code_code__NmYxN
{
  "id": "1a457e1a-121a-11ee-be56-0242ac120002",
  "username": "umami",
  "role": "admin"
}

```

---

## POST /api/users/:userId

Updates a user.

**Parameters**

- `username`: (optional string) The user's username.
- `password`: (optional string) The user's password.
- `role`: (optional string) Determines user access, `admin` or `user`.

**Sample response**

```Code_code__NmYxN
{
  "id": "1a457e1a-121a-11ee-be56-0242ac120002",
  "username": "umami",
  "role": "admin",
  "createdAt": "2023-04-13T20:22:55.756Z"
}

```

---

## DELETE /api/users/:userId

Deletes a user.

**Parameters**

None

**Sample response**

```Code_code__NmYxN
ok

```

---

## GET /api/users/:userId/websites

Gets all websites that belong to a user.

**Parameters**

- `query`: (optional string) Search text.
- `page`: (optional number, default 1) Determines page.
- `pageSize`: (optional string) Determines how many results to return.
- `orderBy`: (optional string, default `name`) Order by column name.

**Sample response**

```Code_code__NmYxN
[\
  {\
    "id": "02d89813-7a72-41e1-87f0-8d668f85008b",\
    "userId": "1a457e1a-121a-11ee-be56-0242ac120002",\
    "domain": "mywebsite.com",\
    "name": "My Website",\
    "shareId": null,\
    "createdAt": "2023-04-10T23:06:44.250Z",\
    "deletedAt": null,\
    "resetAt": null,\
    "updatedAt": null\
  }\
]

```

---

## GET /api/users/:userId/teams

Gets all teams that belong to a user.

**Parameters**

- `query`: (optional string) Search text.
- `page`: (optional number, default 1) Determines page.
- `pageSize`: (optional string) Determines how many results to return.
- `orderBy`: (optional string, default `name`) Order by column name.

**Sample response**

```Code_code__NmYxN
[\
  {\
    "id": "02d89813-7a72-41e1-87f0-8d668f85008b",\
    "name": "My Team",\
    "createdAt": "2023-04-10T23:06:44.250Z",\
    "deletedAt": null,\
    "updatedAt": null\
  }\
]

```

[Sending stats](https://umami.is/docs/api/sending-stats) [Teams](https://umami.is/docs/api/teams-api)

On this page [POST /api/users](https://umami.is/docs/api/users-api#post-apiusers) [GET /api/admin/users](https://umami.is/docs/api/users-api#get-apiadminusers) [GET /api/users/:userId](https://umami.is/docs/api/users-api#get-apiusersuserid) [POST /api/users/:userId](https://umami.is/docs/api/users-api#post-apiusersuserid) [DELETE /api/users/:userId](https://umami.is/docs/api/users-api#delete-apiusersuserid) [GET /api/users/:userId/websites](https://umami.is/docs/api/users-api#get-apiusersuseridwebsites) [GET /api/users/:userId/teams](https://umami.is/docs/api/users-api#get-apiusersuseridteams)

## Umami Events API

[Umami](https://umami.is/docs)

[API](https://umami.is/docs/api)

[Reports](https://umami.is/docs/reports)

[Guides](https://umami.is/docs/guides)

[Cloud](https://umami.is/docs/cloud)

Menu

Getting started [Overview](https://umami.is/docs/api) [Authentication](https://umami.is/docs/api/authentication) [Sending stats](https://umami.is/docs/api/sending-stats)

Endpoints [Users](https://umami.is/docs/api/users-api) [Teams](https://umami.is/docs/api/teams-api) [Events](https://umami.is/docs/api/events-api) [Sessions](https://umami.is/docs/api/sessions-api) [Websites](https://umami.is/docs/api/websites-api) [Website stats](https://umami.is/docs/api/website-stats-api)

Clients [API client](https://umami.is/docs/api/api-client) [Node client](https://umami.is/docs/api/node-client)

Endpoints

# Events

Operations around Events and Event data.

**Endpoints**

```Code_code__NmYxN
GET /api/websites/:websiteId/events
GET /api/websites/:websiteId/event-data/events
GET /api/websites/:websiteId/event-data/fields
GET /api/websites/:websiteId/event-data/values
GET /api/websites/:websiteId/event-data/stats

```

---

## GET /api/websites/:websiteId/events

Gets website event details within a given time range.

**Parameters**

- `startAt`: Timestamp (in ms) of starting date.
- `endAt`: Timestamp (in ms) of end date.
- `query`: (optional string) Search text.
- `page`: (optional number, default 1) Determines page.
- `pageSize`: (optional string) Determines how many results to return.
- `orderBy`: (optional string) Order by column name.

**Sample response**

URL: `https://umami.mydomain.com/api/websites/86d4095c-a2a8-4fc8-9521-103e858e2b41/events?startAt=1725580800000&endAt=1725667199999&query=&page=1&pageSize=20`

```Code_code__NmYxN
{
  "data": [\
    {\
      "id": "dbcfe5ef-99f4-44e1-ad3f-c157eb102a13",\
      "websiteId": "86d4095c-a2a8-4fc8-9521-103e858e2b41",\
      "sessionId": "970c97c5-2071-540a-be88-098eb730ccac",\
      "createdAt": "2024-09-06T23:36:44Z",\
      "urlPath": "/docs/api/authentication",\
      "urlQuery": "",\
      "referrerPath": "/docs/api",\
      "referrerQuery": "",\
      "referrerDomain": "",\
      "pageTitle": "Overview – Umami",\
      "eventType": 1,\
      "eventName": ""\
    },\
    {\
      "id": "9ba0da36-136b-4fef-afae-a5cdba78017b",\
      "websiteId": "86d4095c-a2a8-4fc8-9521-103e858e2b41",\
      "sessionId": "466cb31d-8cda-5dc3-9dd2-b259f29551a5",\
      "createdAt": "2024-09-06T23:36:33Z",\
      "urlPath": "/",\
      "urlQuery": "utm_source=apollo&utm_medium=outbound-email&utm_campaign=founders",\
      "referrerPath": "",\
      "referrerQuery": "",\
      "referrerDomain": "",\
      "pageTitle": "Umami",\
      "eventType": 1,\
      "eventName": ""\
    }\
  ],
  "count": 1000,
  "page": 1,
  "pageSize": 20
}

```

---

## GET /api/websites/:websiteId/event-data/events

Gets event data names, properties, and counts

**Parameters**

- `startAt`: Timestamp (in ms) of starting date.
- `endAt`: Timestamp (in ms) of end date.
- `event`: (optional) Event name filter.

**Sample response**

URL: `https://umami.mydomain.com/api/websites/86d4095c-a2a8-4fc8-9521-103e858e2b41/event-data/events?startAt=1692115200000&endAt=169220159999`

```Code_code__NmYxN
[\
  { "eventName": "button-click", "propertyName": "id", "dataType": 1, "total": 4 },\
  { "eventName": "button-click", "propertyName": "name", "dataType": 1, "total": 4 },\
  { "eventName": "track-product", "propertyName": "price", "dataType": 2, "total": 2 }\
]

```

---

## GET /api/websites/:websiteId/event-data/fields

Gets event data property and value counts within a given time range.

**Parameters**

- `websiteId`: (uuid) Website key identifier.
- `startAt`: Timestamp (in ms) of starting date.
- `endAt`: Timestamp (in ms) of end date.

**Sample response**

URL: `https://umami.mydomain.com/api/websites/86d4095c-a2a8-4fc8-9521-103e858e2b41/event-data/fields?startAt=1692115200000&endAt=169220159999`

```Code_code__NmYxN
[\
  { "propertyName": "age", "dataType": 2, "value": "33", "total": 1 },\
  { "propertyName": "age", "dataType": 2, "value": "31", "total": 4 },\
  { "propertyName": "gender", "dataType": 1, "value": "female", "total": 4 },\
  { "propertyName": "gender", "dataType": 1, "value": "male", "total": 1 }\
]

```

---

## GET /api/websites/:websiteId/event-data/values

Gets event data counts for a given event and property

**Parameters**

- `startAt`: Timestamp (in ms) of starting date.
- `endAt`: Timestamp (in ms) of end date.
- `eventName`: Event name.
- `propertyName`: Property name.

**Sample response**

URL: `https://umami.mydomain.com/api/websites/86d4095c-a2a8-4fc8-9521-103e858e2b41/event-data/values?startAt=1717916400000&endAt=1725692399999&eventName=survey&propertyName=gender`

```Code_code__NmYxN
[\
  { "value": "female", "total": 4 },\
  { "value": "male", "total": 1 }\
]

```

---

## GET /api/websites/:websiteId/event-data/stats

Gets summarized website events, fields, and records within a given time range.

**Parameters**

- `startAt`: Timestamp (in ms) of starting date.
- `endAt`: Timestamp (in ms) of end date.

**Sample response**

URL: `https://umami.mydomain.com/api/websites/86d4095c-a2a8-4fc8-9521-103e858e2b41/event-data/stats?startAt=1725580800000&endAt=1725667199999`

```Code_code__NmYxN
[{ "events": 16, "fields": 13, "records": 26 }]

```

[Teams](https://umami.is/docs/api/teams-api) [Sessions](https://umami.is/docs/api/sessions-api)

On this page [GET /api/websites/:websiteId/events](https://umami.is/docs/api/events-api#get-apiwebsiteswebsiteidevents) [GET /api/websites/:websiteId/event-data/events](https://umami.is/docs/api/events-api#get-apiwebsiteswebsiteidevent-dataevents) [GET /api/websites/:websiteId/event-data/fields](https://umami.is/docs/api/events-api#get-apiwebsiteswebsiteidevent-datafields) [GET /api/websites/:websiteId/event-data/values](https://umami.is/docs/api/events-api#get-apiwebsiteswebsiteidevent-datavalues) [GET /api/websites/:websiteId/event-data/stats](https://umami.is/docs/api/events-api#get-apiwebsiteswebsiteidevent-datastats)

## Sending Stats API

[Umami](https://umami.is/docs)

[API](https://umami.is/docs/api)

[Reports](https://umami.is/docs/reports)

[Guides](https://umami.is/docs/guides)

[Cloud](https://umami.is/docs/cloud)

Menu

Getting started [Overview](https://umami.is/docs/api) [Authentication](https://umami.is/docs/api/authentication) [Sending stats](https://umami.is/docs/api/sending-stats)

Endpoints [Users](https://umami.is/docs/api/users-api) [Teams](https://umami.is/docs/api/teams-api) [Events](https://umami.is/docs/api/events-api) [Sessions](https://umami.is/docs/api/sessions-api) [Websites](https://umami.is/docs/api/websites-api) [Website stats](https://umami.is/docs/api/website-stats-api)

Clients [API client](https://umami.is/docs/api/api-client) [Node client](https://umami.is/docs/api/node-client)

Getting started

# Sending stats

## POST /api/send

To register an `event`, you need to send a `POST` to `/api/send` with
the following data:

For **Umami Cloud** send a POST to `https://cloud.umami.is/api/send`.

**Parameters**

`payload`

- `hostname`: (string) Name of host.
- `language`: (string) Language of visitor (ex. "en-US")
- `referrer`: (string) Referrer URL.
- `screen`: (string) Screen resolution (ex. "1920x1080")
- `title`: (string) Page title.
- `url`: (string) Page URL.
- `website`: (string) Website ID.
- `name`: (string) Name of the event.
- `data`: (object)(optional) Additional data for the event.

`type`: (string) `event` is currently the only type available.

**Sample payload**

```Code_code__NmYxN
{
  "payload": {
    "hostname": "your-hostname",
    "language": "en-US",
    "referrer": "",
    "screen": "1920x1080",
    "title": "dashboard",
    "url": "/",
    "website": "your-website-id",
    "name": "event-name",
    "data": {
      "foo": "bar"
    }
  },
  "type": "event"
}

```

Note, for `/api/send` requests you do not need to send an
authentication token.

Also, you need to send a proper `User-Agent` HTTP header or
your request won't be registered.

**Programmatically**

You can generate most of these values programmatically with JavaScript using the browser APIs. For example:

```Code_code__NmYxN
const data = {
  payload: {
    hostname: window.location.hostname,
    language: navigator.language,
    referrer: document.referrer,
    screen: `${window.screen.width}x${window.screen.height}`,
    title: document.title,
    url: window.location.pathname,
    website: 'your-website-id',
    name: 'event-name',
  },
  type: 'event',
};

```

[Authentication](https://umami.is/docs/api/authentication) [Users](https://umami.is/docs/api/users-api)

On this page [POST /api/send](https://umami.is/docs/api/sending-stats#post-apisend)

## Umami Node Client

[Umami](https://umami.is/docs)

[API](https://umami.is/docs/api)

[Reports](https://umami.is/docs/reports)

[Guides](https://umami.is/docs/guides)

[Cloud](https://umami.is/docs/cloud)

Menu

Getting started [Overview](https://umami.is/docs/api) [Authentication](https://umami.is/docs/api/authentication) [Sending stats](https://umami.is/docs/api/sending-stats)

Endpoints [Users](https://umami.is/docs/api/users-api) [Teams](https://umami.is/docs/api/teams-api) [Events](https://umami.is/docs/api/events-api) [Sessions](https://umami.is/docs/api/sessions-api) [Websites](https://umami.is/docs/api/websites-api) [Website stats](https://umami.is/docs/api/website-stats-api)

Clients [API client](https://umami.is/docs/api/api-client) [Node client](https://umami.is/docs/api/node-client)

Clients

# Node Client

## Overview

The Umami node client allows you to send data to Umami on the server side.

## Installation

```Code_code__NmYxN
npm install @umami/node

```

## Usage

```Code_code__NmYxN
import umami from '@umami/node';

umami.init({
  websiteId: '50429a93-8479-4073-be80-d5d29c09c2ec', // Your website id
  hostUrl: 'https://umami.mywebsite.com', // URL to your Umami instance
});

umami.track({ url: '/home' });

```

If using Umami Cloud, you can use `https://cloud.umami.is` as the host URL.

The properties you can send using the `.track` function are:

- **hostname**: Hostname of server
- **language**: Client language (eg. en-US)
- **referrer**: Page referrer
- **screen**: Screen dimensions (eg. 1920x1080)
- **title**: Page title
- **url**: Page url
- **name**: Event name (for custom events)
- **data**: Event data properties

[API client](https://umami.is/docs/api/api-client)

On this page [Overview](https://umami.is/docs/api/node-client#overview) [Installation](https://umami.is/docs/api/node-client#installation) [Usage](https://umami.is/docs/api/node-client#usage)

## Website Stats API

[Umami](https://umami.is/docs)

[API](https://umami.is/docs/api)

[Reports](https://umami.is/docs/reports)

[Guides](https://umami.is/docs/guides)

[Cloud](https://umami.is/docs/cloud)

Menu

Getting started [Overview](https://umami.is/docs/api) [Authentication](https://umami.is/docs/api/authentication) [Sending stats](https://umami.is/docs/api/sending-stats)

Endpoints [Users](https://umami.is/docs/api/users-api) [Teams](https://umami.is/docs/api/teams-api) [Events](https://umami.is/docs/api/events-api) [Sessions](https://umami.is/docs/api/sessions-api) [Websites](https://umami.is/docs/api/websites-api) [Website stats](https://umami.is/docs/api/website-stats-api)

Clients [API client](https://umami.is/docs/api/api-client) [Node client](https://umami.is/docs/api/node-client)

Endpoints

# Website statistics

Operations around Website statistics.

**Endpoints**

```Code_code__NmYxN
GET /api/websites/:websiteId/active
GET /api/websites/:websiteId/events
GET /api/websites/:websiteId/pageviews
GET /api/websites/:websiteId/metrics
GET /api/websites/:websiteId/stats

```

**Unit Parameter**

The unit parameter buckets the data returned. The unit is automatically converted to the next largest applicable time unit if the maximum is exceeded.

- `minute`: Up to 60 minutes.
- `hour`: Up to 48 hours.
- `day`: Up to 12 months.
- `month`: No limit.
- `year`: No limit.

---

## GET /api/websites/:websiteId/active

Gets the number of active users on a website.

**Parameters**

None

**Sample response**

GET from url: `https://umami.mydomain.com/api/websites/86d4095c-a2a8-4fc8-9521-103e858e2b41/active`

```Code_code__NmYxN
{
  "x": 5
}

```

- `x`: Number of unique visitors within the last 5 minutes

---

## GET /api/websites/:websiteId/events/series

Gets events within a given time range.

**Parameters**

- `startAt`: Timestamp (in ms) of starting date.
- `endAt`: Timestamp (in ms) of end date.
- `unit`: Time unit (year \| month \| hour \| day).
- `timezone`: Timezone (ex. America/Los_Angeles).
- `url`: (optional) Name of URL.
- `referrer`: (optional) Name of referrer.
- `title`: (optional) Name of page title.
- `host`: (optional) Name of hostname.
- `os`: (optional) Name of operating system.
- `browser`: (optional) Name of browser.
- `device`: (optional) Name of device (ex. Mobile)
- `country`: (optional) Name of country.
- `region`: (optional) Name of region/state/province.
- `city`: (optional) Name of city.

**Sample response**

URL: `https://umami.mydomain.com/api/websites/86d4095c-a2a8-4fc8-9521-103e858e2b41/events/series?startAt=1683262800000&endAt=1683349199999&unit=hour&timezone=America%2FLos_Angeles`

```Code_code__NmYxN
[\
  { "x": "live-demo-button", "t": "2023-04-12T22:00:00Z", "y": 1 },\
  { "x": "get-started-button", "t": "2023-04-12T22:00:00Z", "y": 5 },\
  { "x": "get-started-button", "t": "2023-04-12T23:00:00Z", "y": 4 },\
  { "x": "live-demo-button", "t": "2023-04-12T23:00:00Z", "y": 4 },\
  { "x": "social-Discord", "t": "2023-04-13T00:00:00Z", "y": 1 }\
]

```

- `x`: Event name.
- `t`: Timestamp.
- `y`: Number of events.

---

## GET /api/websites/:websiteId/pageviews

Gets pageviews within a given time range.

**Parameters**

- `startAt`: Timestamp (in ms) of starting date.
- `endAt`: Timestamp (in ms) of end date.
- `unit`: Time unit (year \| month \| hour \| day).
- `timezone`: Timezone (ex. America/Los_Angeles).
- `url`: (optional) Name of URL.
- `referrer`: (optional) Name of referrer.
- `title`: (optional) Name of page title.
- `host`: (optional) Name of hostname.
- `os`: (optional) Name of operating system.
- `browser`: (optional) Name of browser.
- `device`: (optional) Name of device (ex. Mobile)
- `country`: (optional) Name of country.
- `region`: (optional) Name of region/state/province.
- `city`: (optional) Name of city.

**Sample response**

```Code_code__NmYxN
{
  "pageviews": [\
    { "x": "2020-04-20 01:00:00", "y": 3 },\
    { "x": "2020-04-20 02:00:00", "y": 7 }\
  ],
  "sessions": [\
    { "x": "2020-04-20 01:00:00", "y": 2 },\
    { "x": "2020-04-20 02:00:00", "y": 4 }\
  ]
}

```

- `x`: Timestamp.
- `y`: Number of visitors.

---

## GET /api/websites/:websiteId/stats

Gets summarized website statistics.

**Parameters**

- `startAt`: Timestamp (in ms) of starting date.
- `endAt`: Timestamp (in ms) of end date.
- `url`: (optional) Name of URL.
- `referrer`: (optional) Name of referrer.
- `title`: (optional) Name of page title.
- `query`: (optional) Name of query.
- `event`: (optional) Name of event.
- `host`: (optional) Name of hostname.
- `os`: (optional) Name of operating system.
- `browser`: (optional) Name of browser.
- `device`: (optional) Name of device (ex. Mobile)
- `country`: (optional) Name of country.
- `region`: (optional) Name of region/state/province.
- `city`: (optional) Name of city.

**Sample response**

URL: `https://umami.mydomain.com/api/websites/86d4095c-a2a8-4fc8-9521-103e858e2b41/stats?startAt=1656679719687&endAt=1656766119687`

```Code_code__NmYxN
{
  "pageviews": { "value": 3018, "prev": 3508 },
  "visitors": { "value": 847, "prev": 910 },
  "visits": { "value": 984, "prev": 1080 },
  "bounces": { "value": 537, "prev": 628 },
  "totaltime": { "value": 150492, "prev": 164713 }
}

```

- `pageviews`: Pages hits
- `visitors`: Number of unique visitors
- `visits`: Number of sessions
- `bounces`: Number of visitors who only visit a single page
- `totaltime`: Time spent on the website

---

## GET /api/websites/:websiteId/metrics

Gets metrics for a given time range.

**Parameters**

- `startAt`: Timestamp (in ms) of starting date.
- `endAt`: Timestamp (in ms) of end date.
- `type`: Metrics type (url \| referrer \| browser \| os \| device \| country \| event).
- `url`: (optional) Name of URL.
- `referrer`: (optional) Name of referrer.
- `title`: (optional) Name of page title.
- `query`: (optional) Name of query.
- `host`: (optional) Name of hostname.
- `os`: (optional) Name of operating system.
- `browser`: (optional) Name of browser.
- `device`: (optional) Name of device (ex. Mobile)
- `country`: (optional) Name of country.
- `region`: (optional) Name of region/state/province.
- `city`: (optional) Name of city.
- `language`: (optional) Name of language.
- `event`: (optional) Name of event.
- `limit`: (optional, default 500) Number of events returned.

**Sample response**

```Code_code__NmYxN
[\
  { "x": "/", "y": 46 },\
  { "x": "/docs", "y": 17 },\
  { "x": "/download", "y": 14 }\
]

```

- `x`: Unique value, depending on metric type (url \| referrer \| browser \| os \| device \| country \| event).
- `y`: Number of visitors.

[Websites](https://umami.is/docs/api/websites-api) [API client](https://umami.is/docs/api/api-client)

On this page [GET /api/websites/:websiteId/active](https://umami.is/docs/api/website-stats-api#get-apiwebsiteswebsiteidactive) [GET /api/websites/:websiteId/events/series](https://umami.is/docs/api/website-stats-api#get-apiwebsiteswebsiteideventsseries) [GET /api/websites/:websiteId/pageviews](https://umami.is/docs/api/website-stats-api#get-apiwebsiteswebsiteidpageviews) [GET /api/websites/:websiteId/stats](https://umami.is/docs/api/website-stats-api#get-apiwebsiteswebsiteidstats) [GET /api/websites/:websiteId/metrics](https://umami.is/docs/api/website-stats-api#get-apiwebsiteswebsiteidmetrics)
