---
title: Status Hero API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell


includes:
  - errors

search: true
---

# Introduction

Hi there! Thanks for checking out the Status Hero API. New to Status Hero? Go ahead and [create an account](https://statushero.com/signup) - you'll need one to access the API.

Thoughts or feedback about our API implementation? [Let us know](https://statushero.com/contact)!

# Format and Versioning

The Status Hero API is designed to be RESTful. All data is transferred in JSON.

API endpoints require HTTPS. You'll also notice the `v1` in each endpoint URL. This indicates that the latest API is currently version 1.0. If we ever have to break backwards compatibility the version number will be bumped.

# Authentication

The API uses header-based authentication. You'll need to provide two headers with each request:

* `X-Team-ID`
* `X-API-Key`

Your team ID and team API key can be found in your [Team Settings](https://statushero.com/teams/current/edit) in Status Hero, under the API tab. You'll need to be an account administrator or team manager to view these credentials.

All requests are scoped by Status Hero team. (Each Status Hero account can have multiple teams.)

> Here's an example call with the headers:

```shell
curl -i "https://service.statushero.com/api/v1/reports" \
  -H "Content-Type: application/json" \
  -H "X-TEAM-ID: your-team-id" \
  -H "X-API-KEY: your-team-api-key"
```

# Pagination

Each API call that produces a collection of objects will only return 30 at a time. To page through the list, simply add the `page` parameter to the call.

> Here's an example call with the page parameter:

```shell
curl -i "https://service.statushero.com/api/v1/reports?page=3" \
  -H "Content-Type: application/json" \
  -H "X-TEAM-ID: your-team-id" \
  -H "X-API-KEY: your-team-api-key"
```

# Rate Limiting

In general we allow applications that integrate with Status Hero to send no more than one message per second. We allow bursts over that limit for short periods. However, if your app continues to exceed the limit over a longer period of time it will be rate limited.

# Members

A `Member` is a user who belongs to your team.

## Get All Members

```shell
curl -i "https://service.statushero.com/api/v1/members" \
  -H "Content-Type: application/json" \
  -H "X-TEAM-ID: your-team-id" \
  -H "X-API-KEY: your-team-api-key"
```

> The above command returns JSON structured like this:

```json
{
  "members": [
    {
      "id": "5b73a1de-b26f-46fe-8f61-8355a2d9241b",
      "slug": "emma-garrett",
      "username": "EmmaGarrett",
      "role": "active",
      "manager": false,
      "user": {
        "id": "e9d25609-6cac-4286-99a7-5d08f29ecd8f",
        "email": "demo+emma@8012labs.com",
        "first_name": "Emma",
        "last_name": "Garrett",
        "time_zone": "Pacific Time (US & Canada)",
        "administrator": false,
        "slug": "emma-garrett",
        "full_name": "Emma Garrett",
        "initials": "eg",
        "avatar_url":
          "https://statushero.com/image/upload/c_fill/v1517855343/wssdbhfluhj2ipga0r8e"
      }
    },
    {
      "id": "a3bf073a-98eb-447d-abcd-7833760f334d",
      "slug": "hugh-oliver",
      "username": "HughOliver",
      "role": "active",
      "manager": false,
      "user": {
        "id": "fb74f792-e555-4b77-8a3b-a62f525d2c25",
        "email": "demo+hugh@8012labs.com",
        "first_name": "Hugh",
        "last_name": "Oliver",
        "time_zone": "London",
        "administrator": false,
        "slug": "hugh-oliver",
        "full_name": "Hugh Oliver",
        "initials": "ho",
        "avatar_url":
          "https://://statushero.com/image/upload/c_fill/v1517855347/qesrin6dwioevihqxanr"
      }
    }
  ]
}
```

This endpoint retrieves a collection of `Member` objects for each user on the team, ordered by last name. ()

### HTTP Request

`GET https://service.statushero.com/api/v1/members`

Or optionally:

`GET https://service.statushero.com/api/v1/members?q={search_term}`

| Query Parameter | Description                                                                                                   |
| --------------- | ------------------------------------------------------------------------------------------------------------- |
| `q`             | Narrow the returned list with a search by a whole or partial match of last name, first name, or email address |

## Get a Specific Team Member

```shell
curl -i "https://service.statushero.com/api/v1/members/5b73a1de-b26f-46fe-8f61-8355a2d9241b" \
  -H "Content-Type: application/json" \
  -H "X-TEAM-ID: your-team-id" \
  -H "X-API-KEY: your-team-api-key"
```

> The above command returns JSON structured like this:

```json
{
  "member": {
    "id": "5b73a1de-b26f-46fe-8f61-8355a2d9241b",
    "slug": "emma-garrett",
    "username": "EmmaGarrett",
    "role": "active",
    "manager": false,
    "user": {
      "id": "e9d25609-6cac-4286-99a7-5d08f29ecd8f",
      "email": "demo+emma@8012labs.com",
      "first_name": "Emma",
      "last_name": "Garrett",
      "time_zone": "Pacific Time (US & Canada)",
      "administrator": false,
      "slug": "emma-garrett",
      "full_name": "Emma Garrett",
      "initials": "eg",
      "avatar_url":
        "https://statushero.com/image/upload/c_fill/v1517855343/wssdbhfluhj2ipga0r8e"
    }
  }
}
```

This endpoint retrieves a specific team member.

### HTTP Request

`GET https://service.statushero.com/api/v1/members/{id}`

### URL Parameters

| Parameter | Description                                       |
| --------- | ------------------------------------------------- |
| `id`      | The ID or URL slug of the team member to retrieve |

# Reports

A `Report` is collection of status updates from each of your team members for a particular date.

## Get All Reports

```shell
curl -i "https://service.statushero.com/api/v1/reports" \
  -H "Content-Type: application/json" \
  -H "X-TEAM-ID: your-team-id" \
  -H "X-API-KEY: your-team-api-key"
```

> The above command returns JSON structured like this:

```json
{
  "reports": [
    {
      "id": "13855d03-0769-49a6-9198-9fd82226191e",
      "date": "2018-02-05",
      "team_id": "b9f52ce4-21d8-4a0d-8134-cda9d78da8fa",
      "questions": {
        "next": "What are your goals for today?",
        "blockers": "Any obstacles impeding your progress?",
        "previous": "What did you accomplish Friday?",
        "previous_completed": "Did you meet Friday's goals?"
      },
      "question_labels": {
        "next": "today",
        "blockers": "blockers",
        "previous": "Friday"
      },
      "metrics": {
        "updated_at": "2018-02-05 13:48:00",
        "blocked_count": 4,
        "participation_count": 8,
        "absent_statuses_count": 0,
        "present_statuses_count": 9,
        "previous_completed_count": 6
      },
      "url": "https://statushero.com/teams/alpha-team/reports/2018-02-05",
      "completed_status_ids": [
        "75dbb4af-c2a4-4fba-9dbf-d61234b16fee",
        "ccef1bea-661e-4857-8de1-1089f651ead9",
        "38ac9d7b-80c5-4986-b1d8-b1783249c7d2",
        "2ab4751c-c7ef-4b45-a79c-ac9822d708f6",
        "efcae075-21f5-45c2-92db-e2079b3e83f2",
        "b1b2da8b-67cc-40a7-9bed-e7fb59b36245",
        "fd8dbb91-38b3-4e18-b716-98e259713f68",
        "33579827-686f-43b7-8d84-fd2cc4b380d1"
      ]
    },
    {
      "id": "77390f38-1cf5-441c-a1a4-a60242f97129",
      "date": "2018-02-02",
      "team_id": "b9f52ce4-21d8-4a0d-8134-cda9d78da8fa",
      "questions": {
        "next": "What are your goals for today?",
        "blockers": "Any obstacles impeding your progress?",
        "previous": "What did you accomplish yesterday?",
        "previous_completed": "Did you meet yesterday's goals?"
      },
      "question_labels": {
        "next": "today",
        "blockers": "blockers",
        "previous": "yesterday"
      },
      "metrics": {
        "updated_at": "2018-02-02 13:48:00",
        "blocked_count": 3,
        "participation_count": 7,
        "absent_statuses_count": 0,
        "present_statuses_count": 9,
        "previous_completed_count": 4
      },
      "url": "https://statushero.com/teams/alpha-team/reports/2018-02-02",
      "completed_status_ids": [
        "75983cad-1ca7-4671-a264-92b5220fe5cd",
        "f1f621c0-9d6b-4b03-adb3-468862439b58",
        "6c2d67c4-6239-4489-813b-99a0e1b2a8e0",
        "9a9e21f7-7d83-42c3-889b-1aa0a9ed3164",
        "b8e969e5-df6d-4386-ae14-275d351c9ef7",
        "dcefef9b-8871-44e1-b484-5d5447759aef",
        "f53af99d-81ce-41e6-b3c2-50c861d0663a"
      ]
    }
  ]
}
```

This endpoint retrieves a collection of `Report` objects for your team, ordered by descending date.

### HTTP Request

`GET https://service.statushero.com/api/v1/reports`

## Get a Specific Report

```shell
curl -i "https://service.statushero.com/api/v1/reports/2018-02-05" \
  -H "Content-Type: application/json" \
  -H "X-TEAM-ID: your-team-id" \
  -H "X-API-KEY: your-team-api-key"
```

> The above command returns JSON structured like this:

```json
{
  "report": {
    "id": "13855d03-0769-49a6-9198-9fd82226191e",
    "date": "2018-02-05",
    "team_id": "b9f52ce4-21d8-4a0d-8134-cda9d78da8fa",
    "questions": {
      "next": "What are your goals for today?",
      "blockers": "Any obstacles impeding your progress?",
      "previous": "What did you accomplish Friday?",
      "previous_completed": "Did you meet Friday's goals?"
    },
    "question_labels": {
      "next": "today",
      "blockers": "blockers",
      "previous": "Friday"
    },
    "metrics": {
      "updated_at": "2018-02-05 13:48:00",
      "blocked_count": 4,
      "participation_count": 8,
      "absent_statuses_count": 0,
      "present_statuses_count": 9,
      "previous_completed_count": 6
    },
    "url": "https://statushero.com/teams/alpha-team/reports/2018-02-05",
    "completed_status_ids": [
      "75dbb4af-c2a4-4fba-9dbf-d61234b16fee",
      "ccef1bea-661e-4857-8de1-1089f651ead9",
      "38ac9d7b-80c5-4986-b1d8-b1783249c7d2",
      "2ab4751c-c7ef-4b45-a79c-ac9822d708f6",
      "efcae075-21f5-45c2-92db-e2079b3e83f2",
      "b1b2da8b-67cc-40a7-9bed-e7fb59b36245",
      "fd8dbb91-38b3-4e18-b716-98e259713f68",
      "33579827-686f-43b7-8d84-fd2cc4b380d1"
    ]
  }
}
```

This endpoint retrieves a specific `Report` for your team.

### HTTP Request

`GET https://service.statushero.com/api/v1/reports/{id}`

### URL Parameters

| Parameter | Description                                                              |
| --------- | ------------------------------------------------------------------------ |
| `id`      | The ID or report date (in `YYYY-MM-DD` format) of the report to retrieve |

# Statuses

Statuses are also know as "check-ins" in Status Hero. Each `Report` has a `Status` for each active team member (except for team Observers.)

## Get All Statuses

```shell
curl -i "https://service.statushero.com/api/v1/statuses" \
  -H "Content-Type: application/json" \
  -H "X-TEAM-ID: your-team-id" \
  -H "X-API-KEY: your-team-api-key"
```

> The above command returns JSON structured like this:

```json
{
  "statuses": [
    {
      "id": "ccef1bea-661e-4857-8de1-1089f651ead9",
      "answers": {
        "next":
          "Prep and host this month's Redis meetup. We may be scrambling to fill the second speaker slot. Start clearing outstanding OAuth connector tickets.",
        "blockers": "Waiting on facilities for room request",
        "previous":
          "Souped up the demo account for the #sales meeting. Ran throught the pitch a couple times with @IreneShaw.",
        "previous_completed": true
      },
      "blocked": true,
      "completed_at": "2018-02-05T15:00:00Z",
      "previous_completed": true,
      "report_id": "13855d03-0769-49a6-9198-9fd82226191e",
      "slug": "2018-02-05-emma-garrett",
      "attachments": [],
      "report_date": "2018-02-05",
      "title": "Check-in from Emma Garrett (Portland) for February 5, 2018",
      "url":
        "https://statushero.com/teams/alpha-team/statuses/2018-02-05-emma-garrett",
      "comment_ids": ["9b122169-0f78-4ff4-85b8-cb999acd61c9"],
      "reaction_ids": ["9b122169-0f78-4ff4-85b8-cb999acd61c9"],
      "status_activity_ids": ["f5238ebd-6f24-41c8-826d-815bb514ac2d"],
      "user": {
        "id": "e9d25609-6cac-4286-99a7-5d08f29ecd8f",
        "email": "demo+emma@8012labs.com",
        "first_name": "Emma",
        "last_name": "Garrett",
        "time_zone": "Pacific Time (US & Canada)",
        "administrator": false,
        "slug": "emma-garrett",
        "full_name": "Emma Garrett",
        "initials": "eg",
        "avatar_url":
          "https://statushero.com/image/upload/c_fill/v1517855343/wssdbhfluhj2ipga0r8e"
      }
    },
    {
      "id": "2ab4751c-c7ef-4b45-a79c-ac9822d708f6",
      "answers": {
        "next":
          "Get CI runs to be _much faster_, both in locally and in all branch runs. Lean on @GracePearson if necessary for SSDs.",
        "blockers": "",
        "previous":
          "* Got through 22 of the 34 outstanding defects for the OAuth connector\n* Reconfig'd Devops dashboard to poll more often #optimization",
        "previous_completed": true
      },
      "blocked": false,
      "completed_at": "2018-02-05T14:51:00Z",
      "previous_completed": true,
      "report_id": "13855d03-0769-49a6-9198-9fd82226191e",
      "slug": "2018-02-05-hugh-oliver",
      "attachments": [],
      "report_date": "2018-02-05",
      "title": "Check-in from Hugh Oliver (London) for February 5, 2018",
      "url":
        "https://statushero.com/teams/alpha-team/statuses/2018-02-05-hugh-oliver",
      "comment_ids": [],
      "reaction_ids": [],
      "status_activity_ids": [],
      "user": {
        "id": "fb74f792-e555-4b77-8a3b-a62f525d2c25",
        "email": "demo+hugh@8012labs.com",
        "first_name": "Hugh",
        "last_name": "Oliver",
        "time_zone": "London",
        "administrator": false,
        "slug": "hugh-oliver",
        "full_name": "Hugh Oliver",
        "initials": "ho",
        "avatar_url":
          "https://://statushero.com/image/upload/c_fill/v1517855347/qesrin6dwioevihqxanr"
      }
    }
  ]
}
```

This endpoint retrieves a collection of `Status` objects (check-ins) for your team, ordered by descending submission time.

### HTTP Request

`GET https://service.statushero.com/api/v1/statuses`

## Get a Specific Status

```shell
curl -i "https://service.statushero.com/api/v1/statuses/2ab4751c-c7ef-4b45-a79c-ac9822d708f6" \
  -H "Content-Type: application/json" \
  -H "X-TEAM-ID: your-team-id" \
  -H "X-API-KEY: your-team-api-key"
```

> The above command returns JSON structured like this:

```json
{
  "status": {
    "id": "ccef1bea-661e-4857-8de1-1089f651ead9",
    "answers": {
      "next":
        "Prep and host this month's Redis meetup. We may be scrambling to fill the second speaker slot. Start clearing outstanding OAuth connector tickets.",
      "blockers": "Waiting on facilities for room request",
      "previous":
        "Souped up the demo account for the #sales meeting. Ran throught the pitch a couple times with @IreneShaw.",
      "previous_completed": true
    },
    "blocked": true,
    "completed_at": "2018-02-05T15:00:00Z",
    "previous_completed": true,
    "report_id": "13855d03-0769-49a6-9198-9fd82226191e",
    "slug": "2018-02-05-emma-garrett",
    "attachments": [],
    "report_date": "2018-02-05",
    "title": "Check-in from Emma Garrett (Portland) for February 5, 2018",
    "url":
      "https://statushero.com/teams/alpha-team/statuses/2018-02-05-emma-garrett",
    "comment_ids": ["9b122169-0f78-4ff4-85b8-cb999acd61c9"],
    "reaction_ids": ["9b122169-0f78-4ff4-85b8-cb999acd61c9"],
    "status_activity_ids": ["f5238ebd-6f24-41c8-826d-815bb514ac2d"],
    "user": {
      "id": "e9d25609-6cac-4286-99a7-5d08f29ecd8f",
      "email": "demo+emma@8012labs.com",
      "first_name": "Emma",
      "last_name": "Garrett",
      "time_zone": "Pacific Time (US & Canada)",
      "administrator": false,
      "slug": "emma-garrett",
      "full_name": "Emma Garrett",
      "initials": "eg",
      "avatar_url":
        "https://statushero.com/image/upload/c_fill/v1517855343/wssdbhfluhj2ipga0r8e"
    }
  }
}
```

This endpoint retrieves a specific `Status` (check-in).

### HTTP Request

`GET https://service.statushero.com/api/v1/statuses/{id}`

### URL Parameters

| Parameter | Description                              |
| --------- | ---------------------------------------- |
| `id`      | The ID or slug of the status to retrieve |

# Status Activities

Status Activities consist of data, often from other tools, that are associated with individual status updates (check-ins). For example, Jira issue updates or GitHub pushes. A `StatusActivity` belongs to a `Status`.

## Get a Specific Status Activity

```shell
curl -i "https://service.statushero.com/api/v1/statuses_activities/f5238ebd-6f24-41c8-826d-815bb514ac2d" \
  -H "Content-Type: application/json" \
  -H "X-TEAM-ID: your-team-id" \
  -H "X-API-KEY: your-team-api-key"
```

> The above command returns JSON structured like this:

```json
{
  "status_activity": {
    "id": "f5238ebd-6f24-41c8-826d-815bb514ac2d",
    "status_id": "ccef1bea-661e-4857-8de1-1089f651ead9",
    "kind": "trello",
    "created_at": "2018-02-05T14:20:00Z",
    "description":
      "CA Board: moved \"Index wireless hard drive\" from \"Dev\" to \"Stage\"",
    "html_description":
      "<a href='https://trello.com/b/vee7/ca-board'>CA Board</a>: moved \"Index wireless hard drive\" from \"Dev\" to \"Stage\"",
    "url":
      "https://statushero.com/teams/alpha-team/statuses/2018-02-05-emma-garrett"
  }
}
```

This endpoint retrieves a specific `StatusActivity` for a `Status` (check-in). A call to the statuses endpoint will return an array of `StatusActivity` ids for each `Status`)

### HTTP Request

`GET https://service.statushero.com/api/v1/status_activities/{id}`

| Parameter | Description                                      |
| --------- | ------------------------------------------------ |
| `id`      | The ID of the status activity record to retrieve |

## Add a Status Activity

```shell
curl -i "https://service.statushero.com/api/v1/status_activities" \
  -X POST \
  -H "Content-Type: application/json" \
  -H "X-TEAM-ID: your-team-id" \
  -H "X-API-KEY: your-team-api-key" \
  -d '{"email":"adam@example.com", "source":"Kanban Board", "description": "Moved user story Reporting from Doing to QA"}' \
```

Use this endpoint to push activities from your external tools to Status Hero. You'll need to include the fields below that Status Hero can match up the activity with the appropriate team member and check-in. All fields are required unless noted. Returns a `201 Created` when successful.

### HTTP Request

`POST https://service.statushero.com/api/v1/status_activities`

| Field              | Description                                                                                                                                                                  | Example                                                                                      |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| `email`            | Email address for the associated team member. Must match what's in Status Hero.                                                                                              | `adam@example.com`                                                                           |
| `source`           | Source of the activity                                                                                                                                                       | `Kanban Board`                                                                               |
| `source_url`       | Optional. An external URL for the source of the activity                                                                                                                     | `https://our-kanban-board.example.com`                                                       |
| `description`      | A brief description of the activity (You can use simple HTML markup here, like links or bold tags, but we don't recommend it.)                                               | `Moved user story "Reporting" from "Doing" to "QA"`                                          |

# Tags

Tags (#hashtags) can be used by your team to categorize check-ins.

## Get a list of Tags

```shell
curl -i "https://service.statushero.com/api/v1/tags" \
  -H "Content-Type: application/json" \
  -H "X-TEAM-ID: your-team-id" \
  -H "X-API-KEY: your-team-api-key"
```

> The above command returns JSON structured like this:

```json
{
    "tags": [
        "bravo",
        "design",
        "development",
        "devops",
        "highpriority",
        "hotfix",
        "marketing",
        "mobile",
        "optimization",
        "performance",
        "planning",
        "review",
        "sales",
        "security"
    ]
}
```

This endpoint retrieves an array of tags that are configured for your team.

### HTTP Request

`GET https://service.statushero.com/api/v1/tags`

## Get Statuses from a Tag

```shell
curl -i "https://service.statushero.com/api/v1/tags/design" \
  -H "Content-Type: application/json" \
  -H "X-TEAM-ID: your-team-id" \
  -H "X-API-KEY: your-team-api-key"
```

> The above command returns JSON structured like this:

```json
{
  "statuses": [
    {
      "id": "4eb7f57f-4ddf-419e-a435-44717fd82eaa",
      "answers": {
        "next": "Break up the onboarding worklow epic. Amend the master style guide with #design changes from @dave",
        "blockers": "",
        "previous": "Repository maintenance complete - everyone should have a little boost in their git fetch and merge speeds, especially from Europe. Closed 7 DO issues. #performance",
        "previous_completed": "1"
      },
      "blocked": false,
      "completed_at": "2018-06-18T16:21:00Z",
      "previous_completed": true,
      "report_id": "be29a5f2-ffe1-4ee9-b554-72f95e2f308a",
      "slug": "2018-06-18-grace-pearson",
      "attachments": [],
      "report_date": "2018-06-18",
      "title": "Check-in from Grace Pearson (San Francisco) for June 18, 2018",
      "url": "https://statushero.com/teams/alpha-team/statuses/2018-06-18-grace-pearson",
      "comment_ids": [],
      "reaction_ids": [
        "77afc770-095b-4615-b7be-16246e9a22af",
        "624539f2-aae2-4169-b25a-8964c08ca921"
      ],
      "status_activity_ids": [],
      "user": {
        "id": "b07ce317-34f2-46f4-98b3-17473b0cf69f",
        "email": "demo+grace@8012labs.com",
        "first_name": "Grace",
        "last_name": "Pearson",
        "time_zone": "Pacific Time (US & Canada)",
        "administrator": false,
        "slug": "grace-pearson",
        "full_name": "Grace Pearson",
        "initials": "gp",
        "avatar_url": "https://res-5.cloudinary.com/status-hero/image/upload/c_fill/v1518389164/qfeksi1247eosiuvyujx"
      }
    },
    {
      "id": "f19f6ac7-e30e-48ba-b8ce-419a1e28ff3f",
      "answers": {
        "next": "Break up the onboarding worklow epic. Amend the master style guide with #design changes from @IreneShaw",
        "blockers": "",
        "previous": "Repository maintenance complete - everyone should have a little boost in their git fetch and merge speeds, especially from Europe. Closed 7 DO issues. #performance",
        "previous_completed": "1"
      },
      "blocked": false,
      "completed_at": "2018-06-15T14:21:00Z",
      "previous_completed": true,
      "report_id": "00b3421b-4311-4df4-9ff9-9120764542f4",
      "slug": "2018-06-15-dave-taylor",
      "attachments": [],
      "report_date": "2018-06-15",
      "title": "Check-in from Dave Taylor (Boston) for June 15, 2018",
      "url": "https://statushero.com/teams/alpha-team/statuses/2018-06-15-dave-taylor",
      "comment_ids": [
        "555c3990-9a83-4076-9a7e-1d4becec168a"
      ],
      "reaction_ids": [
        "da13fe8c-1974-4394-a338-71d27f01900f"
      ],
      "status_activity_ids": [
        "28b3cdb8-0d66-451f-b6f7-93b89176a5ea"
      ],
      "user": {
        "id": "7e564e63-a9a3-4cbf-a015-5c91b1e720d0",
        "email": "demo+dave@8012labs.com",
        "first_name": "Dave",
        "last_name": "Taylor",
        "time_zone": "Eastern Time (US & Canada)",
        "administrator": false,
        "slug": "dave-taylor",
        "full_name": "Dave Taylor",
        "initials": "dt",
        "avatar_url": "https://res-5.cloudinary.com/status-hero/image/upload/c_fill/v1518389162/jiyofof8szxy27emehyt"
      }
    },
  ]
}
```


This endpoint retrieves a collection of `Status` objects, ordered by descending date completed, where the designated tag is present.

### HTTP Request

`GET https://service.statushero.com/api/v1/tags/{tag}`

| Parameter | Description                              |
| --------- | ---------------------------------------- |
| `tag`      | The tag to use to retrieve the collection. E.g, "sales" or "devops" |

# Comments

A `Comment` is a message you or your team members can add to another check-in. A `Comment` belongs to a `Status` record.

## Get a Specific Comment

```shell
curl -i "https://service.statushero.com/api/v1/comments/{id}" \
  -H "Content-Type: application/json" \
  -H "X-TEAM-ID: your-team-id" \
  -H "X-API-KEY: your-team-api-key"
```

> The above command returns JSON structured like this:

```json
{
  "comment": {
    "id": "9b122169-0f78-4ff4-85b8-cb999acd61c9",
    "body":
      "You can't generate the capacitor without synthesizing the neural USB matrix",
    "created_at": "2018-02-05T11:00:00Z",
    "status_id": "ccef1bea-661e-4857-8de1-1089f651ead9",
    "title": "Emma added a comment to their February 5 check-in",
    "url":
      "https://statushero.com/teams/alpha-team/statuses/2018-02-05-emma-garrett",
    "user": {
      "id": "e9d25609-6cac-4286-99a7-5d08f29ecd8f",
      "email": "demo+emma@8012labs.com",
      "first_name": "Emma",
      "last_name": "Garrett",
      "time_zone": "Pacific Time (US & Canada)",
      "administrator": false,
      "slug": "emma-garrett",
      "full_name": "Emma Garrett",
      "initials": "eg",
      "avatar_url":
        "https://statushero.com/image/upload/c_fill/v1517855343/wssdbhfluhj2ipga0r8e"
    }
  }
}
```

This endpoint retrieves a `Comment` for a status update (check-in). (A call to the statuses endpoint will return an array of `Comment` ids for each `Status`.)

### HTTP Request

`GET https://service.statushero.com/api/v1/comments/{id}`

| Parameter | Description                              |
| --------- | ---------------------------------------- |
| `id`      | The ID of the comment record to retrieve |

# Reactions

A `Reaction` is quick way for team members to add a signal of feedback to another check-in. In the Status Hero UI, a reaction consists of an emoji, like a thumbs-up or a smiley face. A `Reaction` belongs to a `Status` record.

## Get a Specific Reaction

```shell
curl -i "https://service.statushero.com/api/v1/reactions/{id}" \
  -H "Content-Type: application/json" \
  -H "X-TEAM-ID: your-team-id" \
  -H "X-API-KEY: your-team-api-key"
```

> The above command returns JSON structured like this:

```json
{
  "reaction": {
    "id": "b75edfc0-89a6-40d6-9ed6-c4139464eac7",
    "body": "thumbsup",
    "created_at": "2018-02-05T18:30:13Z",
    "status_id": "ccef1bea-661e-4857-8de1-1089f651ead9",
    "title": "Adam reacted to a February 5 check-in from Emma with :thumbsup:",
    "url":
      "https://statushero.com/teams/alpha-team/statuses/2018-02-05-emma-garrett",
    "user": {
      "id": "aaca3009-4ed7-4e3e-b746-673faaf4dd75",
      "email": "demo+adam@8012labs.com",
      "first_name": "Adam",
      "last_name": "Young",
      "time_zone": "Eastern Time (US & Canada)",
      "administrator": true,
      "slug": "adam-young",
      "full_name": "Adam Young",
      "initials": "ay",
      "avatar_url":
        "https://://statushero.com/image/upload/c_fill/v1517855426/fphsvutenjphg82yv49q"
    }
  }
}
```

This endpoint retrieves a `Reaction` for a status update (check-in). (A call to the statuses endpoint will return an array of `Reaction` ids for each `Status`.)

### HTTP Request

`GET https://service.statushero.com/api/v1/reactions/{id}`

| Parameter | Description                               |
| --------- | ----------------------------------------- |
| `id`      | The ID of the reaction record to retrieve |

# Member Absences

Add or remove future vacation or leave dates for individual team members.

## Add Member Absence

```shell
curl -i "https://service.statushero.com/api/v1/member_absences/adam-young" \
  -X POST \
  -H "Content-Type: application/json" \
  -H "X-TEAM-ID: your-team-id" \
  -H "X-API-KEY: your-team-api-key"
  -d '{"date": "2018-04-28"}'
```

This idempotent endpoint creates a vacation or leave day for an individual team `Member`. You can get the IDs of team members with a call to the members endpoint. Returns a `201 Created` when successful, or `202 Accepted` if the date is already set as an absent date.

### HTTP Request

`POST https://service.statushero.com/api/v1/member_absences/{id}`

| Parameter | Description                       |
| --------- | --------------------------------- |
| `id`      | The ID or slug of the team member |

| Field  | Description                                                |
| ------ | ---------------------------------------------------------- |
| `date` | The date of the absence to create (in `YYYY-MM-DD` format) |

## Remove Member Absence

```shell
curl -i "https://service.statushero.com/api/v1/member_absences/adam-young/2018-04-28" \
  -X DELETE \
  -H "Content-Type: application/json" \
  -H "X-TEAM-ID: your-team-id" \
  -H "X-API-KEY: your-team-api-key"
```

This endpoint removes a vacation or leave day for an individual team `Member`. You can get the IDs of team members with a call to the members endpoint. Returns a `204 No Content` if successful.

### HTTP Request

`DELETE https://service.statushero.com/api/v1/member_absences/{id}/{date}`

| Parameter | Description                                                |
| --------- | ---------------------------------------------------------- |
| `id`      | The ID or slug of the team member                          |
| `date`    | The date of the absence to create (in `YYYY-MM-DD` format) |

# Team Absences

Add or remove future team-wide holidays.

## Add Team Absence

```shell
curl -i "https://service.statushero.com/api/v1/team_absences" \
  -X POST \
  -H "Content-Type: application/json" \
  -H "X-TEAM-ID: your-team-id" \
  -H "X-API-KEY: your-team-api-key"
  -d '{"date": "2018-04-28"}'
```

This idempotent endpoint creates a team-wide holiday. Returns a `201 Created` when successful, or `202 Accepted` if the date is already set as a holiday.

### HTTP Request

`POST https://service.statushero.com/api/v1/team_absences`

| Field  | Description                                                |
| ------ | ---------------------------------------------------------- |
| `date` | The date of the absence to create (in `YYYY-MM-DD` format) |

## Remove Team Absence

```shell
curl -i "https://service.statushero.com/api/v1/team_absences/2018-04-28" \
  -X DELETE \
  -H "Content-Type: application/json" \
  -H "X-TEAM-ID: your-team-id" \
  -H "X-API-KEY: your-team-api-key"
```

This endpoint removes a team-wide holiday for the specified date. Returns a `204 No Content` if successful.

### HTTP Request

`DELETE https://service.statushero.com/api/v1/team_absences/{date}`

| Parameter | Description                                                |
| --------- | ---------------------------------------------------------- |
| `date`    | The date of the absence to create (in `YYYY-MM-DD` format) |
