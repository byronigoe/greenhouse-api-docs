# Departments

## The department object

An organization's departments.

> With `render_as=list` (default)

```json
{
  "id": 12345,
  "name": "Technology",
  "parent_id": null,
  "child_ids": [
      34065,
      25908
  ],
  "external_id": "89076"
}
```

> With `render_as=tree`

```json
{
  "id": 12345,
  "name": "Technology",
  "children": [
    {
      "id": 34065,
      "name": "Design",
      "children": [],
      "external_id": "32526"
    },
    {
      "id": 25908,
      "name": "Engineering",
      "children": [               
        {
            "id": 14510,
            "name": "Third-Level Department",
            "children": [
              {
                  "id": 14502,
                  "name": "Strategy",
                  "children": [],
                  "external_id": null
              }
            ],
            "external_id": "56735"
        }
      ],
      "external_id": "47658"
    }
  ],
  "external_id": "26758"
}
```

### Noteworthy attributes

| Attribute | Description |
|-----------|-------------|
| id | The department's unique identifier |
| name | The department's name
| external_id | An arbitrary ID provided by an external source; does not map to another entity in Greenhouse.

## GET: List Departments

List all of an organization's departments.

```shell
curl 'https://harvest.greenhouse.io/v1/departments' 
-H "Authorization: Basic MGQwMzFkODIyN2VhZmE2MWRjMzc1YTZjMmUwNjdlMjQ6"
```

> The above command returns JSON structured like this, with `render_as=list` (default)

```json
[
  {
    "id": 12345,
    "name": "Technology",
    "parent_id": null,
    "child_ids": [
        34065,
        25908
    ],
    "external_id": "89076"
  }
  {
    "id": 67890,
    "name": "Administration",
    "parent_id": 54647,
    "child_ids": [],
    "external_id": null
  }
]
```

> With `render_as=tree`

```json
[
  {
    "id": 12345,
    "name": "Technology",
    "children": [
      {
        "id": 34065,
        "name": "Design",
        "children": [],
        "external_id": "32526"
      },
      {
        "id": 25908,
        "name": "Engineering",
        "children": [               
          {
              "id": 14510,
              "name": "Third-Level Department",
              "children": [],
              "external_id": "56735"
          }
        ],
        "external_id": "47658"
      }
    ],
    "external_id": "26758"
  }
  {
    "id": 67890,
    "name": "Administration",
    "parent_id": 54647,
    "child_ids": [],
    "external_id": null
  }
]
```


### HTTP Request

`GET https://harvest.greenhouse.io/v1/departments`

### Querystring parameters

| Parameter | Description |
|-----------|-------------|
| per_page | Return up to this number of objects per response. Must be an integer between 1 and 500. Defaults to 100.
| page | A cursor for use in pagination.  Returns the n-th chunk of `per_page` objects.
| render_as | This parameter defines how to represent the list of departments. The default value is 'list', which returns a flat list of departments.  If this is set to 'tree', departments are represented in a tree-like structure where they may include sub-departments as `children`.

<br>
[See noteworthy response attributes.](#the-department-object)

## GET: Retrieve Department

```shell
curl 'https://harvest.greenhouse.io/v1/departments/{id}' 
-H "Authorization: Basic MGQwMzFkODIyN2VhZmE2MWRjMzc1YTZjMmUwNjdlMjQ6"
```

> The above command returns JSON structured like this:

> With `render_as=list` (default)

```json
{
  "id": 12345,
  "name": "Technology",
  "parent_id": null,
  "child_ids": [
      34065,
      25908
  ],
  "external_id": "89076"
}
```

> With `render_as=tree`

```json
{
  "id": 12345,
  "name": "Technology",
  "children": [
    {
      "id": 34065,
      "name": "Design",
      "children": [],
      "external_id": "32526"
    },
    {
      "id": 25908,
      "name": "Engineering",
      "children": [               
        {
            "id": 14510,
            "name": "Third-Level Department",
            "children": [],
            "external_id": "56735"
        }
      ],
      "external_id": "47658"
    }
  ],
  "external_id": "26758"
}
{
  "id": 67890,
  "name": "Administration",
  "parent_id": 54647,
  "child_ids": [],
  "external_id": null
}
```

Retrieve a department by its `id`.

### HTTP Request

`GET https://harvest.greenhouse.io/v1/departments/{id}`

### URL Parameters

Parameter | Description
--------- | -----------
id | The ID of the department to retrieve

### Querystring parameters

| Parameter | Description |
|-----------|-------------|
| render_as | This parameter defines how to represent the list of departments. The default value is 'list', which returns a flat list of departments.  If this is set to 'tree', departments are represented in a tree-like structure where they may include sub-departments as `children`.

<br>
[See noteworthy response attributes.](#the-department-object)

## PATCH: Edit Department

```shell
curl -X PATCH 'https://harvest.greenhouse.io/v1/departments/{id}'
-H "On-Behalf-Of: {greenhouse user ID}"
-H "Authorization: Basic MGQwMzFkODIyN2VhZmE2MWRjMzc1YTZjMmUwNjdlMjQ6"
```

> The above command takes a JSON request, structured like this:

```
{
   "name": "Engineering",
   "external_id": "EXTERNAL_ID_1234"
}
```

> The above command returns a JSON response, structured like this:

```json
{
  "id": 45644,
  "name": "Engineering",
  "parent_id": null,
  "child_ids": [
      34065,
      25908
  ],
  "external_id": "EXTERNAL_ID_1234"
}
```

Edit a department's basic information.

### HTTP Request

`PATCH https://harvest.greenhouse.io/v1/departments/{id}`

### Headers

Header | Description
--------- | -----------
On-Behalf-Of | ID of the user issuing this request. Required for auditing purposes.

### JSON Body Parameters

Parameter | Required | Type | Description
--------- | ----------- | ----------- | -----------
name | Yes | string | The department's name. If included, this cannot be blank.
external_id* | No | string | The department's external ID. If included, this must be unique to this department within the organization.

\* - If the external id feature is not enabled for your organization, attempting to edit this field will raise an API Error.


## POST: Add Department

```shell
curl -X POST 'https://harvest.greenhouse.io/v1/departments
-H "On-Behalf-Of: {greenhouse user ID}"
-H "Authorization: Basic MGQwMzFkODIyN2VhZmE2MWRjMzc1YTZjMmUwNjdlMjQ6"
```

> The above command takes a JSON request, structured like this:

```
{
  "name": "A New Department",
  "parent_id": 12345,
  "external_id": "456454"
}
```

> The above command returns a JSON response, structured like this:

```json
{
  "id": 34535,
  "name": "A New Department",
  "parent_id": 12345,
  "child_ids": [],
  "external_id": "456454"
}
```

Create a new department

### HTTP Request

`POST https://harvest.greenhouse.io/v1/departments`

### Headers

Header | Description
--------- | -----------
On-Behalf-Of | ID of the user issuing this request. Required for auditing purposes.  Must be a user who can create departments.

### JSON Body Parameters

Parameter | Required | Type | Description
--------- | ----------- | ----------- | ----------- | -----------
name |  yes | string | The name of your new department.  Must be less than 255 characters and unique within your organization.
parent_id* | no | number | The department id for the new department to be nested under.  If this isn't included, the department will be created at the top level.
external_id** | no | string | The external_id for the office.

\* - The tiered department feature is available only for customers with Standard or Premium Greenhouse Recruiting. Use of this field will return an error for Basic Greenhouse Recruiting customers.

\** - The external_id feature is available only for customers with Standard or Premium Greenhouse Recruiting. Use of this field will return an error for Basic Greenhouse Recruiting customers.