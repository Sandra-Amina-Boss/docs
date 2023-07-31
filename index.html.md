---
title: Upstash API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - python
  - go

toc_footers:
  - <a href='https://console.upstash.com'>Sign Up for a API Key</a>

includes:
  - codes

search: true
---

# Introduction

Welcome to the Upstash API! You can use the API to create and manage your databases.

We have language bindings in Shell, Python, and Go. You can view code examples in the dark area at the right, and you can switch the programming language of the examples with the tabs in the top right.


# Authentication

> To authorize, use this code:

```go
	client := &http.Client{}
	req, err := http.NewRequest("GET", "https://api.upstash.com/v2/redis/database", nil)
	if err != nil {
		log.Fatal(err)
	}
	req.SetBasicAuth("EMAIL", "API_KEY")
	resp, err := client.Do(req)
	if err != nil {
		log.Fatal(err)
	}
	bodyText, err := ioutil.ReadAll(resp.Body)
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("%s\n", bodyText)
```

```python
import requests

response = requests.get('https://api.upstash.com/v2/redis/database', auth=('EMAIL', 'API_KEY'))

```

```shell
# With shell, you can just pass the correct header with each request
curl "https://api.upstash.com/v2/redis/database"
  -u EMAIL:API_KEY
```

> Make sure to replace `EMAIL` and `API_KEY` with your email and API key.

Upstash API requires API keys to authenticate requests. You can view and manage API keys at the [Upstash Console](https://console.upstash.com).

Upstash API uses [HTTP Basic authentication](https://en.wikipedia.org/wiki/Basic_access_authentication). You should pass `EMAIL` and `API_KEY` as basic authentication username and password respectively.

With a client such as `curl`, you can pass your credentials with the `-u` option, as the following example shows:

`curl https://api.upstash.com/v2/redis/database -u EMAIL:API_KEY`
<aside class="notice">
Replace <code>EMAIL</code> and <code>API_KEY</code> with your email and API key.
</aside>

# Redis

Endpoints to create, manage and monitor databases.

All endpoints return responses encoded as JSON.

Unit for size parameters: `bytes`.

## Create Database

```python
import requests

data = '{"name":"myredis","region":"eu-west-1","tls":true}'

response = requests.post('https://api.upstash.com/v2/redis/database', data=data, auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X POST \
  https://api.upstash.com/v2/redis/database \
  -u 'EMAIL:API_KEY' \
  -d '{"name":"myredis","region":"eu-west-1","tls": true}'
```

```go
client := &http.Client{}
var data = strings.NewReader(`{
    "name":"myredis",
    "region":"eu-west-1",
    "tls": true
}`)
req, err := http.NewRequest("POST", "https://api.upstash.com/v2/redis/database", data)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```
> Make sure to replace `EMAIL` and `API_KEY` with your email and API key.
> The above command returns a JSON structured like this:

```json
{
  "database_id": "96ad0856-03b1-4ee7-9666-e81abd0349e1",
  "database_name": "MyRedis",
  "database_type": "Pay as You Go",
  "region": "eu-central-1",
  "port": 30143,
  "creation_time": 1658909671,
  "state": "active",
  "password": "038a8e27c45e43068d5f186085399884",
  "user_email": "example@upstash.com",
  "endpoint": "eu2-sought-mollusk-30143.upstash.io",
  "tls": true,
  "rest_token": "AXW_ASQgOTZhZDA4NTYtMDNiMS00ZWU3LTk2NjYtZTgxYWJkMDM0OWUxMDM4YThlMjdjNDVlNDMwNjhkNWYxODYwODUzOTk4ODQ=",
  "read_only_rest_token": "AnW_ASQgOTZhZDA4NTYtMDNiMS00ZWU3LTk2NjYtZTgxYWJkMDM0OWUx8sbmiEcMm9u7Ks5Qx-kHNiWr_f-iUXSIH8MlziKMnpY="
}
```

This endpoint creates a new Redis database.

Request parameters should be encoded as JSON.

### HTTP Request

`POST https://api.upstash.com/v2/redis/database`

### Request Parameters

Parameter  | Description
---------  | -----------
name  | Name of the database
region  | Region of the database. (eu-west-1, us-east-1, us-west-1, ap-northeast-1 or us-central1)
tls  | Set true to enable tls.

### Response Parameters

Parameter  | Description
---------  | -----------
database_id  | ID of the created database
database_name  | Name of the database
database_type  | Type of the database in terms of pricing model(Free, Pay as You Go or Enterprise)
region  | The region where database is hosted
port  | Database port for clients to connect
creation_time  | Creation time of the database as [Unix time](https://en.wikipedia.org/wiki/Unix_time)
state  | State of database (active or deleted)
password  | Password of the database
user_email  | Email or team id of the owner of the database
endpoint  | Endpoint URL of the database
tls  | TLS/SSL is enabled or not

## Create Database (Global)

```python
import requests

data = '{"name":"myredis", "region":"global", "primary_region":"us-east-1", "read_regions":["us-west-1","us-west-2"], "tls":true}'

response = requests.post('https://api.upstash.com/v2/redis/database', data=data, auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X POST \
  https://api.upstash.com/v2/redis/database \
  -u 'EMAIL:API_KEY' \
  -d '{"name":"myredis", "region":"global", "primary_region":"us-east-1", "read_regions":["us-west-1","us-west-2"], "tls": true}'
```

```go
client := &http.Client{}
var data = strings.NewReader(`{
    "name":"myredis",
    "region":"global",
    "primary_region"":"us-east-1",
    "read_regions":["us-west-1","us-west-2"],
    "tls": true
}`)
req, err := http.NewRequest("POST", "https://api.upstash.com/v2/redis/database", data)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```
> Make sure to replace `EMAIL` and `API_KEY` with your email and API key.
> The above command returns a JSON structured like this:

```json
{
  "database_id": "93e3a3e-342c-4683-ba75-344c08ae143b",
  "database_name": "global-test",
  "database_type": "Pay as You Go",
  "region": "global",
  "type": "paid",
  "port": 32559,
  "creation_time": 1674596896,
  "state": "active",
  "password": "dd1803832a2746309e118373549e574d",
  "user_email": "support@upstash.com",
  "endpoint": "steady-stud-32559.upstash.io",
  "tls": false,
  "rest_token": "AX8vACQgOTMyY2UyYy00NjgzLWJhNzUtMzQ0YzA4YWUxNDNiZMyYTI3NDYzMDllMTE4MzczNTQ5ZTU3NGQ=",
  "read_only_rest_token": "An8vACQg2UtMzQyYy00NjgzLWJhNzUtMzQ0YzA4YBVsUsyn19xDnTAvjbsiq79GRDrURNLzIYIOk="
}
```

This endpoint creates a new Redis database.

Request parameters should be encoded as JSON.

### HTTP Request

`POST https://api.upstash.com/v2/redis/database`

### Request Parameters

Parameter  | Description
---------  | -----------
name  | Name of the database
region  | Region of the database. Only valid option is global.
primary_region | Primary Region of the Global Database. <br> Options:<br> `us-east-1,us-west-1`<br>`us-west-2,eu-west-1,eu-central-1`<br>`ap-southeast-1,ap-southeast-2,sa-east-1`
read_regions |  Array of Read Regions of the Database. <br> Options:<br> `us-east-1,us-west-1`<br>`us-west-2,eu-west-1,eu-central-1`<br>`ap-southeast-1,ap-southeast-2,sa-east-1`
tls  | Set true to enable tls.

### Response Parameters

Parameter  | Description
---------  | -----------
database_id  | ID of the created database
database_name  | Name of the database
database_type  | Type of the database in terms of pricing model(Free, Pay as You Go or Enterprise)
region  | The region where database is hosted
port  | Database port for clients to connect
creation_time  | Creation time of the database as [Unix time](https://en.wikipedia.org/wiki/Unix_time)
state  | State of database (active or deleted)
password  | Password of the database
user_email  | Email or team id of the owner of the database
endpoint  | Endpoint URL of the database


## Update Regions (Global)

```python
import requests

data = '{"read_regions":["eu-west-1"]}'

response = requests.post('https://api.upstash.com/v2/redis/database', data=data, auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X POST \
  https://api.upstash.com/v2/redis/update-regions/:id \
  -u 'EMAIL:API_KEY' \
  -d '{ "read_regions":["us-west-1"] }'
```

```go
client := &http.Client{}
var data = strings.NewReader(`{,
    "read_regions":["us-west-1"]
}`)
req, err := http.NewRequest("POST", "https://api.upstash.com/v2/redis/read-regions/:id", data)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```
> Make sure to replace `EMAIL` and `API_KEY` with your email and API key.
> The above command returns a JSON structured like this:

```json
"OK"
```

This endpoint updates read regions of Global Redis database.

Request parameters should be encoded as JSON.

### HTTP Request

`POST https://api.upstash.com/v2/redis/update-regions/:id`

### Request Parameters

Parameter  | Description
---------  | -----------
read_regions |  Array of Read Regions of the Database. <br> Options:<br> `us-east-1,us-west-1`<br>`us-west-2,eu-west-1,eu-central-1`<br>`ap-southeast-1,ap-southeast-2,sa-east-1`


## Reset Password

```python
import requests

response = requests.post('https://api.upstash.com/v2/redis/reset-password/:id', auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X POST \
  https://api.upstash.com/v2/redis/reset-password/:id \
  -u 'EMAIL:API_KEY'
```

```go
client := &http.Client{}
req, err := http.NewRequest("POST", "https://api.upstash.com/v2/redis/reset-password/:id", nil)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```
> Make sure to replace `EMAIL` and `API_KEY` with your email and API key.
> The above command returns a JSON structured like this:

```json
{
  "database_id": "96ad0856-03b1-4ee7-9666-e81abd0349e1",
  "cluster_id": "dea1f974",
  "database_name": "MyRedis",
  "database_type": "Pay as You Go",
  "region": "eu-central-1",
  "port": 30143,
  "creation_time": 1658909671,
  "state": "active",
  "password": "49665a1710f3434d8be008aab50f38d2",
  "user_email": "example@upstash.com",
  "endpoint": "eu2-sought-mollusk-30143.upstash.io",
  "tls": true,
  "consistent": false,
  "pool_id": "f886c7f3",
  "rest_token": "AXW_ASQgOTZhZDA4NTYtMDNiMS00ZWU3LTk2NjYtZTgxYWJkMDM0OWUxNDk2NjVhMTcxMGYzNDM0ZDhiZTAwOGFhYjUwZjM4ZDI=",
  "read_only_rest_token": "AnW_ASQgOTZhZDA4NTYtMDNiMS00ZWU3LTk2NjYtZTgxYWJkMDM0OWUxB5sRhCROkPsxozFcDzDgVGRAxUI7UUr0Y6uFB7jMIOI="
}
```

This endpoint updates the password of a database.

Request parameters should be encoded as JSON.

### HTTP Request

`POST https://api.upstash.com/v2/redis/reset-password/:id`

### URL Parameters

Parameter  | Description
---------  | -----------
ID | The ID of the database to reset password

### Response Parameters

Parameter  | Description
---------  | -----------
database_id  | ID of the created database
database_name  | Name of the database
database_type  | Type of the database in terms of pricing model(Free, Pay as You Go or Enterprise)
region  | The region where database is hosted
port  | Database port for clients to connect
creation_time  | Creation time of the database as [Unix time](https://en.wikipedia.org/wiki/Unix_time)
state  | State of database (active or deleted)
password  | Password of the database
user_email  | Email or team id of the owner of the database
endpoint  | Endpoint URL of the database
tls  | TLS/SSL is enabled or not

## Rename Database

```python
import requests

data = '{"name":"myredis"}'

response = requests.post('https://api.upstash.com/v2/redis/rename/:id', data=data, auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X POST \
  https://api.upstash.com/v2/redis/rename/:id \
  -u 'EMAIL:API_KEY' \
  -d '{"name":"myredis"}'
```

```go
client := &http.Client{}
var data = strings.NewReader(`{
    "name":"myredis"
}`)
req, err := http.NewRequest("POST", "https://api.upstash.com/v2/redis/rename/:id", data)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```
> Make sure to replace `EMAIL` and `API_KEY` with your email and API key.
> The above command returns a JSON structured like this:

```json
{
  "database_id": "67b6af16-acb2-4f00-9e38-f6cb9bee800d",
  "database_name": "myredis",
  "database_type": "Free",
  "region": "eu-west-1",
  "port": 33992,
  "creation_time": 1643381812,
  "state": "active",
  "password": "f65e75d6b64448f783f24143f4cf91f6",
  "user_email": "example@upstash.com",
  "endpoint": "eu1-selected-goat-30138.upstash.io",
  "tls": true,
}
```

This endpoint renames a database.

Request parameters should be encoded as JSON.

### HTTP Request

`POST https://api.upstash.com/v2/redis/rename/:id`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the database to rename

### Request Parameters

Parameter  | Description
---------  | -----------
name  | The new name of the database

### Response Parameters

Parameter  | Description
---------  | -----------
database_id  | ID of the created database
database_name  | Name of the database
database_type  | Type of the database in terms of pricing model(Free, Pay as You Go or Enterprise)
region  | The region where database is hosted
port  | Database port for clients to connect
creation_time  | Creation time of the database as [Unix time](https://en.wikipedia.org/wiki/Unix_time)
state  | State of database (active or deleted)
password  | Password of the database
user_email  | Email or team id of the owner of the database
endpoint  | Endpoint URL of the database
tls  | TLS/SSL is enabled or not

## Enable TLS

```python
import requests

response = requests.post('https://api.upstash.com/v2/redis/enable-tls/:id', auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X POST \
  https://api.upstash.com/v2/redis/enable-tls/:id \
  -u 'EMAIL:API_KEY'
```

```go
client := &http.Client{}
req, err := http.NewRequest("POST", "https://api.upstash.com/v2/redis/enable-tls/:id", nil)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```
> Make sure to replace `EMAIL` and `API_KEY` with your email and API key.
> The above command returns a JSON structured like this:

```json
{
  "database_id": "67b6af16-acb2-4f00-9e38-f6cb9bee800d",
  "database_name": "myredis",
  "database_type": "Free",
  "region": "eu-west-1",
  "port": 33992,
  "creation_time": 1643381812,
  "state": "active",
  "password": "f65e75d6b64448f783f24143f4cf91f6",
  "user_email": "example@upstash.com",
  "endpoint": "eu1-selected-goat-30138.upstash.io",
  "tls": true,
}
```

This endpoint enables tls on a database.

Request parameters should be encoded as JSON.

### HTTP Request

`POST https://api.upstash.com/v2/redis/enable-tls/:id`

### URL Parameters

Parameter  | Description
---------  | -----------
ID | The ID of the database to enable tls

### Response Parameters

Parameter  | Description
---------  | -----------
database_id  | ID of the created database
database_name  | Name of the database
database_type  | Type of the database in terms of pricing model(Free, Pay as You Go or Enterprise)
region  | The region where database is hosted
port  | Database port for clients to connect
creation_time  | Creation time of the database as [Unix time](https://en.wikipedia.org/wiki/Unix_time)
state  | State of database (active or deleted)
password  | Password of the database
user_email  | Email or team id of the owner of the database
endpoint  | Endpoint URL of the database
tls  | TLS/SSL is enabled or not

## Enable OR Disable Eviction

```python
import requests
# ENABLE
response = requests.post('https://api.upstash.com/v2/redis/enable-eviction/:id', auth=('EMAIL', 'API_KEY'))
response.content

# DISABLE
response = requests.post('https://api.upstash.com/v2/redis/disable-eviction/:id', auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
# ENABLE
curl -X POST \
  https://api.upstash.com/v2/redis/enable-eviction/:id \
  -u 'EMAIL:API_KEY'
  
# DISABLE
curl -X POST \
  https://api.upstash.com/v2/redis/disable-eviction/:id \
  -u 'EMAIL:API_KEY'
```

```go
client := &http.Client{}
// ENABLE
req, err := http.NewRequest("POST", "https://api.upstash.com/v2/redis/enable-eviction/:id", nil)

// DISABLE
req, err := http.NewRequest("POST", "https://api.upstash.com/v2/redis/disable-eviction/:id", nil)

if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```
> Make sure to replace `EMAIL` and `API_KEY` with your email and API key.
> The above command returns a JSON structured like this:

```json
"OK"
```

This endpoint enables or disables eviction for given database.

### HTTP Request

Enable: `POST https://api.upstash.com/v2/redis/enable-eviction/:id`


Disable: `POST https://api.upstash.com/v2/redis/disable-eviction/:id`

### URL Parameters

Parameter  | Description
---------  | -----------
ID | The ID of the database to enable or disable eviction

### Response Parameters

"OK"

## Enable OR Disable Auto Scaling

```python
import requests
# ENABLE
response = requests.post('https://api.upstash.com/v2/redis/enable-autoupgrade/:id', auth=('EMAIL', 'API_KEY'))
response.content

# DISABLE
response = requests.post('https://api.upstash.com/v2/redis/disable-autoupgrade/:id', auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
# ENABLE
curl -X POST \
  https://api.upstash.com/v2/redis/enable-autoupgrade/:id \
  -u 'EMAIL:API_KEY'

# DISABLE
curl -X POST \
  https://api.upstash.com/v2/redis/disable-autoupgrade/:id \
  -u 'EMAIL:API_KEY'
```

```go
client := &http.Client{}
// ENABLE
req, err := http.NewRequest("POST", "https://api.upstash.com/v2/redis/enable-autoupgrade/:id", nil)

// DISABLE
req, err := http.NewRequest("POST", "https://api.upstash.com/v2/redis/disable-autoupgrade/:id", nil)

if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```
> Make sure to replace `EMAIL` and `API_KEY` with your email and API key.
> The above command returns a JSON structured like this:

```json
"OK"
```

This endpoint enables or disables auto scaling for given database.

### HTTP Request

Enable: `POST https://api.upstash.com/v2/redis/enable-autoupgrade/:id`


Disable: `POST https://api.upstash.com/v2/redis/disable-autoupgrade/:id`

### URL Parameters

Parameter  | Description
---------  | -----------
ID | The ID of the database to enable or disable autoupgrade

### Response Parameters

"OK"


## Move To Team

```python
import requests

data = '{"team_id": "6cc32556-0718-4de5-b69c-b927693f9282","database_id": "67b6af16-acb2-4f00-9e38-f6cb9bee800d"}'

response = requests.post('https://api.upstash.com/v2/redis/move-to-team', data=data, auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X POST \
  https://api.upstash.com/v2/redis/move-to-team \
  -u 'EMAIL:API_KEY' \
  -d '{"team_id": "6cc32556-0718-4de5-b69c-b927693f9282","database_id": "67b6af16-acb2-4f00-9e38-f6cb9bee800d"}'
```

```go
client := &http.Client{}
var data = strings.NewReader(`{
    "team_id": "6cc32556-0718-4de5-b69c-b927693f9282",
    "database_id": "67b6af16-acb2-4f00-9e38-f6cb9bee800d"
}`)
req, err := http.NewRequest("POST", "https://api.upstash.com/v2/redis/move-to-team", data)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```

This endpoint moves database under a target team

Request parameters should be encoded as JSON.

### HTTP Request

`POST https://api.upstash.com/v2/redis/move-to-team`

### Request Parameters

Parameter  | Description
---------  | -----------
team_id  | The id of the target team
database_id  | The id of the database to move

## List Databases

```python
import requests

response = requests.get('https://api.upstash.com/v2/redis/databases', auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X GET \
  https://api.upstash.com/v2/redis/databases \
  -u 'EMAIL:API_KEY'
```

```go
client := &http.Client{}
req, err := http.NewRequest("GET", "https://api.upstash.com/v2/redis/databases", nil)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```
> Make sure to replace `EMAIL` and `API_KEY` with your email and API key.
> The above command returns a JSON structured like this:

```json
[
    {
        "database_id": "96ad0856-03b1-4ee7-9666-e81abd0349e1",
        "database_name": "MyRedis",
        "database_type": "Pay as You Go",
        "region": "eu-central-1",
        "port": 30143,
        "creation_time": 1658909671,
        "state": "active",
        "password": "038a8e27c45e43068d5f186085399884",
        "user_email": "example@upstash.com",
        "endpoint": "eu2-sought-mollusk-30143.upstash.io",
        "tls": true,
        "rest_token": "AXW_ASQgOTZhZDA4NTYtMDNiMS00ZWU3LTk2NjYtZTgxYWJkMDM0OWUxMDM4YThlMjdjNDVlNDMwNjhkNWYxODYwODUzOTk4ODQ=",
        "read_only_rest_token": "AnW_ASQgOTZhZDA4NTYtMDNiMS00ZWU3LTk2NjYtZTgxYWJkMDM0OWUx8sbmiEcMm9u7Ks5Qx-kHNiWr_f-iUXSIH8MlziKMnpY="
    }
]
```

This endpoint list all databases of user.

Request parameters should be encoded as JSON.

### HTTP Request

`GET https://api.upstash.com/v2/redis/databases`

### Response Parameters

Parameter  | Description
---------  | -----------
database_id  | ID of the created database
database_name  | Name of the database
database_type  | Type of the database in terms of pricing model(Free, Pay as You Go or Enterprise)
region  | The region where database is hosted
creation_time  | Creation time of the database as [Unix time](https://en.wikipedia.org/wiki/Unix_time
tls  | TLS/SSL is enabled or not

## Delete Database

```python
import requests

response = requests.delete('https://api.upstash.com/v2/redis/database/:id' auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X DELETE \
  https://api.upstash.com/v2/redis/database/:id \
  -u 'EMAIL:API_KEY'
```

```go
client := &http.Client{}
req, err := http.NewRequest("DELETE", "https://api.upstash.com/v2/redis/database/:id", nil)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```

This endpoint deletes a database.

Request parameters should be encoded as JSON.

### HTTP Request

`DELETE https://api.upstash.com/v2/redis/database/:id`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the database to delete

## Get Database Stats

```python
import requests

response = requests.get('https://api.upstash.com/v2/redis/stats/:id', auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X GET \
  https://api.upstash.com/v2/redis/stats/:id \
  -u 'EMAIL:API_KEY'
```

```go
client := &http.Client{}
req, err := http.NewRequest("GET", "https://api.upstash.com/v2/redis/stats/:id", nil)
if err != nil {
log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```
> Make sure to replace `EMAIL` and `API_KEY` with your email and API key.
> The above command returns a JSON structured like this:

```json
{
  "connection_count": [
    {
      "x": "2023-05-22 10:59:23.426 +0000 UTC",
      "y": 320
    },
    ...
  ],
  "keyspace": [
    {
      "x": "2023-05-22 10:59:23.426 +0000 UTC",
      "y": 344725564
    },
    ...
  ],
  "throughput": [
    {
      "x": "2023-05-22 11:00:23.426 +0000 UTC",
      "y": 181.88333333333333
    },
    ...
  ],
  "produce_throughput": null,
  "consume_throughput": null,
  "diskusage": [
    {
      "x": "2023-05-22 10:59:23.426 +0000 UTC",
      "y": 532362818323
    },
    ...
  ],
  "latencymean": [
    {
      "x": "2023-05-22 10:59:23.426 +0000 UTC",
      "y": 0.176289
    },
    ...
  ],
  "read_latency_mean": [
    {
      "x": "2023-05-22 11:00:23.426 +0000 UTC",
      "y": 0
    },
    ...
  ],
  "read_latency_99": [
    {
      "x": "2023-05-22 11:00:23.426 +0000 UTC",
      "y": 0
    },
    ...
  ],
  "write_latency_mean": [
    {
      "x": "2023-05-22 11:00:23.426 +0000 UTC",
      "y": 0
    },
    ...
  ],
  "write_latency_99": [
    {
      "x": "2023-05-22 11:00:23.426 +0000 UTC",
      "y": 0
    },
    ...
  ],
  "hits": [
    {
      "x": "2023-05-22 11:00:23.426 +0000 UTC",
      "y": 0
    },
    ...
  ],
  "misses": [
    {
      "x": "2023-05-22 11:00:23.426 +0000 UTC",
      "y": 0
    },
    ...
  ],
  "read": [
    {
      "x": "2023-05-22 11:00:23.426 +0000 UTC",
      "y": 82.53333333333333
    },
    ...
  ],
  "write": [
    {
      "x": "2023-05-22 11:00:23.426 +0000 UTC",
      "y": 99.35
    },
    ...
  ],
  "dailyrequests": [
    {
      "x": "2023-05-18 11:58:23.534505371 +0000 UTC",
      "y": 68844080
    },
    ...
  ],
  "days": [
    "Thursday",
    "Friday",
    "Saturday",
    "Sunday",
    "Monday"
  ],
  "dailybilling": [
    {
      "x": "2023-05-18 11:58:23.534505371 +0000 UTC",
      "y": 145.72694911244588
    },
    ...
  ],
  "dailybandwidth": 50444740913,
  "bandwidths": [
    {
      "x": "2023-05-18 11:58:23.534505371 +0000 UTC",
      "y": 125391861729
    },
    ...
  ],
  "dailyproduce": null,
  "dailyconsume": null,
  "total_monthly_requests": 1283856937,
  "total_monthly_read_requests": 1034567002,
  "total_monthly_write_requests": 249289935,
  "total_monthly_storage": 445942383672,
  "total_monthly_billing": 222.33902763855485,
  "total_monthly_produce": 0,
  "total_monthly_consume": 0
}
```

This endpoint gets detailed stats of a database.

Request parameters should be encoded as JSON.

### HTTP Request

`GET https://api.upstash.com/v2/redis/stats/:id`

### URL Parameters

Parameter  | Description
---------  | -----------
ID | The ID of the database

### Response Parameters

Parameter  | Description
---------  | -----------
connection_count  | Total number of connections momentarily
keyspace  | Total number keys exists in the database
throughput  | Throughput seen on the database connections
produce_throughput  | Throughput seen on the database connections for write requests
consume_throughput  | Throughput seen on the database connections for read requests
diskusage  | Total amount of this usage of the database
latencymax  | Maximum server latency observed in the last hour
latencymean  | Mean server latency observed in the last hour
latency99  | 99 percentile server latency observed in the last hour
hits  | Total number requests made to the database that are hit
misses  | Total number requests made to the database that are miss
read  | Total number read requests made to the database
write  | Total number write requests made to the database
dailyrequests  | Total number requests made to the database in the corresponding day
days  | String representation of last 5 days of the week starting from the current day
dailybilling  | Last 5 days of daily billing for the database
dailyproduce  | Total number of daily produce commands
dailyconsume  | Total number of daily consume commands
total_monthly_requests  | Total number of request made in current month
total_monthly_read_requests  | Total number of read request made in current month
total_monthly_write_requests  | Total number of write request made in current month
total_monthly_storage  | Total storage used in current month
total_monthly_billing  | Total cost of the database in the current month
total_monthly_produce  | Total number of produce commands in the current month
total_monthly_consume  | Total number of consume commands in the current month

## Get Database

```python
import requests

response = requests.get('https://api.upstash.com/v2/redis/database/:id', auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X GET \
  https://api.upstash.com/v2/redis/database/:id \
  -u 'EMAIL:API_KEY'
```

```go
client := &http.Client{}
req, err := http.NewRequest("GET", "https://api.upstash.com/v2/redis/database/:id", nil)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```
> Make sure to replace `EMAIL` and `API_KEY` with your email and API key.
> The above command returns a JSON structured like this:

```json
{
  "database_id": "96ad0856-03b1-4ee7-9666-e81abd0349e1",
  "database_name": "MyRedis",
  "database_type": "Pay as You Go",
  "region": "eu-central-1",
  "port": 30143,
  "creation_time": 1658909671,
  "state": "active",
  "password": "038a8e27c45e43068d5f186085399884",
  "user_email": "example@upstash.com",
  "endpoint": "eu2-sought-mollusk-30143.upstash.io",
  "tls": true,
  "rest_token": "AXW_ASQgOTZhZDA4NTYtMDNiMS00ZWU3LTk2NjYtZTgxYWJkMDM0OWUxMDM4YThlMjdjNDVlNDMwNjhkNWYxODYwODUzOTk4ODQ=",
  "read_only_rest_token": "AnW_ASQgOTZhZDA4NTYtMDNiMS00ZWU3LTk2NjYtZTgxYWJkMDM0OWUx8sbmiEcMm9u7Ks5Qx-kHNiWr_f-iUXSIH8MlziKMnpY=",
  "db_max_clients": 1000,
  "db_max_request_size": 1048576,
  "db_disk_threshold": 107374182400,
  "db_max_entry_size": 104857600,
  "db_memory_threshold": 1073741824,
  "db_daily_bandwidth_limit": 53687091200,
  "db_max_commands_per_second": 1000,
  "db_request_limit": 9223372036854775808
}
```

This endpoint gets details of a database.

Request parameters should be encoded as JSON.

### HTTP Request

`GET https://api.upstash.com/v2/redis/database/:id`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the database to retrieve

### Response Parameters

Parameter  | Description
---------  | -----------
database_id  | ID of the created database
database_name  | Name of the database
database_type  | Type of the database in terms of pricing model(Free, Pay as You Go or Enterprise)
port | Port of the database to connect to
state | The current state of the database whether it is active or deleted.
password | Password to connect to the database
user_email | Email of the user or team the database belongs to
endpoint | Endpoint of the database to connect to
region  | The region where database is hosted
creation_time  | Creation time of the database as [Unix time](https://en.wikipedia.org/wiki/Unix_time
tls  | TLS/SSL is enabled or not
rest_token | Token for rest based communication with the database
read_only_rest_token | Read only token for rest based communication with the database
db_max_clients | Max number of concurrent clients can be opened on this database currently
db_max_request_size | Max size of a request that will be accepted by the database currently(in bytes)
db_disk_threshold | Total disk size limit that can be used for the database currently(in bytes)
db_max_entry_size | Max size of an entry that will be accepted by the database currently(in bytes)
db_memory_threshold | Max size of a memory the database can use(in bytes)
db_daily_bandwidth_limit | Max daily bandwidth can be used by the database(in bytes)
db_max_commands_per_second | Max number of commands can be sent to the database per second

# Kafka

Endpoints to create, manage Upstash Kafka.

All endpoints return responses encoded as JSON.

Unit for time parameters: `milliseconds - ms`.

Unit for size parameters: `bytes`.

## Create Kafka Cluster

```python
import requests

data = '{"name":"mykafkacluster","region":"eu-west-1","multizone":true}'

response = requests.post('https://api.upstash.com/v2/kafka/cluster', data=data, auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X POST \
  https://api.upstash.com/v2/kafka/cluster \
  -u 'EMAIL:API_KEY' \
  -d '{"name":"mykafkacluster","region":"eu-west-1","multizone":true}'
```

```go
client := &http.Client{}
var data = strings.NewReader(`{
    "name": "test_kafka_cluster_4",
    "region": "eu-west-1",
    "multizone": true
}`)
req, err := http.NewRequest("POST", "https://api.upstash.com/v2/kafka/cluster", data)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```
> Make sure to replace `EMAIL` and `API_KEY` with your email and API key.
> The above command returns a JSON structured like this:

```json
{
    "cluster_id": "9bc0e897-cbd3-4997-895a-fd77ad00aec9",
    "name": "mykafkacluster",
    "region": "eu-west-1",
    "type": "paid",
    "multizone": true,
    "tcp_endpoint": "sharing-mastodon-12819-eu1-kafka.upstashdev.com",
    "rest_endpoint": "sharing-mastodon-12819-eu1-rest-kafka.upstashdev.com",
    "state": "active",
    "username": "c2hhcmluZy1tYXN0b2Rvbi0xMjgxOSRV1ipriSBOwd0PHzw2KAs_cDrTXzvUKIs",
    "password": "zlQgc0nbgcqF6MxOqnh7tKjJsGnSgLFS89uS-FXzMVqhL2dgFbmHwB-IXAAsOYXzUYj40g==",
    "max_retention_size": 1073741824000,
    "max_retention_time": 2592000000,
    "max_messages_per_second": 1000,
    "creation_time": 1643978975,
    "max_message_size": 1048576,
    "max_partitions": 100
}
```

This endpoint creates a new kafka cluster.

Request parameters should be encoded as JSON.

### HTTP Request

`POST https://api.upstash.com/v2/kafka/cluster`

### Request Parameters

Parameter  | Description
---------  | -----------
name  | Name of the new kafka cluster
region  | The region the cluster will be deployed in(eu-west-1, us-east-1)
multizone  | Set true to enable multi-zone replication

### Response Parameters

Parameter  | Description
---------  | -----------
cluster_id  | ID of the created kafka cluster
name | Name of the kafka cluster
region | The region the kafka cluster is deployed in
type | Shows whether the cluster is free or paid
multizone | Whether the multizone replication is enabled for the cluster or not
tcp_endpoint | TCP endpoint to connect to the kafka cluster
rest_endpoint | REST endpoint to connect to the kafka cluster
state | Current state of the cluster(active, deleted)
username | Username to be used in authenticating to the cluster
password | Password to be used in authenticating to the cluster
max_retention_size | Max retention size will be allowed to topics in the cluster
max_retention_time | Max retention time will be allowed to topics in the cluster
max_messages_per_second | Max messages allowed to be produced per second
creation_time | Cluster creation timestamp
max_message_size | Max message size will be allowed in topics in the cluster
max_partitions | Max total number of partitions allowed in the cluster

## List Kafka Clusters

```python
import requests

response = requests.get('https://api.upstash.com/v2/kafka/clusters', auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X GET \
  https://api.upstash.com/v2/kafka/clusters \
  -u 'EMAIL:API_KEY'
```

```go
client := &http.Client{}
req, err := http.NewRequest("GET", "https://api.upstash.com/v2/kafka/clusters", nil)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```
> Make sure to replace `EMAIL` and `API_KEY` with your email and API key.
> The above command returns a JSON structured like this:

```json
[
    {
        "cluster_id": "9bc0e897-cbd3-4997-895a-fd77ad00aec9",
        "name": "test_kafka_cluster",
        "region": "eu-west-1",
        "type": "paid",
        "multizone": true,
        "tcp_endpoint": "sharing-mastodon-12819-eu1-kafka.upstashdev.com",
        "rest_endpoint": "sharing-mastodon-12819-eu1-rest-kafka.upstashdev.com",
        "state": "active",
        "username": "c2hhcmluZy1tYXN0b2Rvbi0xMjgxOSRV1ipriSBOwd0PHzw2KAs_cDrTXzvUKIs",
        "password": "zlQgc0nbgcqF6MxOqnh7tKjJsGnSgLFS89uS-FXzMVqhL2dgFbmHwB-IXAAsOYXzUYj40g==",
        "max_retention_size": 1073741824000,
        "max_retention_time": 2592000000,
        "max_messages_per_second": 1000,
        "creation_time": 1643978975,
        "max_message_size": 1048576,
        "max_partitions": 100
    }
]
```

This endpoint list all kafka clusters of user.

Request parameters should be encoded as JSON.

### HTTP Request

`GET https://api.upstash.com/v2/kafka/clusters`

### Response Parameters

Parameter  | Description
---------  | -----------
cluster_id  | ID of the created kafka cluster
name | Name of the kafka cluster
region | The region the kafka cluster is deployed in
type | Shows whether the cluster is free or paid
multizone | Whether the multizone replication is enabled for the cluster or not
tcp_endpoint | TCP endpoint to connect to the kafka cluster
rest_endpoint | REST endpoint to connect to the kafka cluster
state | Current state of the cluster(active, deleted)
username | Username to be used in authenticating to the cluster
password | Password to be used in authenticating to the cluster
max_retention_size | Max retention size will be allowed to topics in the cluster
max_retention_time | Max retention time will be allowed to topics in the cluster
max_messages_per_second | Max messages allowed to be produced per second
creation_time | Cluster creation timestamp
max_message_size | Max message size will be allowed in topics in the cluster
max_partitions | Max total number of partitions allowed in the cluster

## Get Kafka Cluster

```python
import requests

response = requests.get('https://api.upstash.com/v2/kafka/cluster/:id', auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X GET \
  https://api.upstash.com/v2/kafka/cluster/:id \
  -u 'EMAIL:API_KEY'
```

```go
client := &http.Client{}
req, err := http.NewRequest("GET", "https://api.upstash.com/v2/kafka/cluster/:id", nil)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```
> Make sure to replace `EMAIL` and `API_KEY` with your email and API key.
> The above command returns a JSON structured like this:

```json
{
    "cluster_id": "9bc0e897-cbd3-4997-895a-fd77ad00aec9",
    "name": "test_kafka_cluster",
    "region": "eu-west-1",
    "type": "paid",
    "multizone": true,
    "tcp_endpoint": "sharing-mastodon-12819-eu1-kafka.upstashdev.com",
    "rest_endpoint": "sharing-mastodon-12819-eu1-rest-kafka.upstashdev.com",
    "state": "active",
    "username": "c2hhcmluZy1tYXN0b2Rvbi0xMjgxOSRV1ipriSBOwd0PHzw2KAs_cDrTXzvUKIs",
    "password": "zlQgc0nbgcqF6MxOqnh7tKjJsGnSgLFS89uS-FXzMVqhL2dgFbmHwB-IXAAsOYXzUYj40g==",
    "max_retention_size": 1073741824000,
    "max_retention_time": 2592000000,
    "max_messages_per_second": 1000,
    "creation_time": 1643978975,
    "max_message_size": 1048576,
    "max_partitions": 100
}
```

This endpoint gets details of a kafka cluster.

Request parameters should be encoded as JSON.

### HTTP Request

`GET https://api.upstash.com/v2/kafka/cluster/:id`

### URL Parameters

Parameter  | Description
---------  | -----------
ID | The ID of the kafka cluster

### Response Parameters

Parameter  | Description
---------  | -----------
cluster_id  | ID of the created kafka cluster
name | Name of the kafka cluster
region | The region the kafka cluster is deployed in
type | Shows whether the cluster is free or paid
multizone | Whether the multizone replication is enabled for the cluster or not
tcp_endpoint | TCP endpoint to connect to the kafka cluster
rest_endpoint | REST endpoint to connect to the kafka cluster
state | Current state of the cluster(active, deleted)
username | Username to be used in authenticating to the cluster
password | Password to be used in authenticating to the cluster
max_retention_size | Max retention size will be allowed to topics in the cluster
max_retention_time | Max retention time will be allowed to topics in the cluster
max_messages_per_second | Max messages allowed to be produced per second
creation_time | Cluster creation timestamp
max_message_size | Max message size will be allowed in topics in the cluster
max_partitions | Max total number of partitions allowed in the cluster

## Delete Kafka Cluster

```python
import requests

response = requests.delete('https://api.upstash.com/v2/kafka/cluster/:id' auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X DELETE \
  https://api.upstash.com/v2/kafka/cluster/:id \
  -u 'EMAIL:API_KEY'
```

```go
client := &http.Client{}
req, err := http.NewRequest("DELETE", "https://api.upstash.com/v2/kafka/cluster/:id", nil)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```

This endpoint deletes a kafka cluster.

Request parameters should be encoded as JSON.

### HTTP Request

`POST https://api.upstash.com/v2/kafka/cluster/:id`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kafka cluster to delete

## Rename Kafka Cluster

```python
import requests

data = '{"name":"mykafkacluster-2"}'

response = requests.post('https://api.upstash.com/v2/kafka/rename-cluster/:id', data=data, auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X POST \
  https://api.upstash.com/v2/kafka/rename-cluster/:id \
  -u 'EMAIL:API_KEY' \
  -d '{"name":"mykafkacluster-2"}'
```

```go
client := &http.Client{}
var data = strings.NewReader(`{
    "name":"mykafkacluster-2"
}`)
req, err := http.NewRequest("POST", "https://api.upstash.com/v2/kafka/rename-cluster/:id", data)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```
> Make sure to replace `EMAIL` and `API_KEY` with your email and API key.
> The above command returns a JSON structured like this:

```json
{
    "cluster_id": "9bc0e897-cbd3-4997-895a-fd77ad00aec9",
    "name": "mykafkacluster-2",
    "region": "eu-west-1",
    "type": "paid",
    "multizone": true,
    "tcp_endpoint": "sharing-mastodon-12819-eu1-kafka.upstashdev.com",
    "rest_endpoint": "sharing-mastodon-12819-eu1-rest-kafka.upstashdev.com",
    "state": "active",
    "username": "c2hhcmluZy1tYXN0b2Rvbi0xMjgxOSRV1ipriSBOwd0PHzw2KAs_cDrTXzvUKIs",
    "password": "zlQgc0nbgcqF6MxOqnh7tKjJsGnSgLFS89uS-FXzMVqhL2dgFbmHwB-IXAAsOYXzUYj40g==",
    "max_retention_size": 1073741824000,
    "max_retention_time": 2592000000,
    "max_messages_per_second": 1000,
    "creation_time": 1643978975,
    "max_message_size": 1048576,
    "max_partitions": 100
}
```

This endpoint renames a kafka cluster.

Request parameters should be encoded as JSON.

### HTTP Request

`POST https://api.upstash.com/v2/kafka/rename-cluster/:id`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kafka cluster to rename

### Request Parameters

Parameter  | Description
---------  | -----------
name  | The new name of the kafka cluster

### Response Parameters

Parameter  | Description
---------  | -----------
cluster_id  | ID of the created kafka cluster
name | Name of the kafka cluster
region | The region the kafka cluster is deployed in
type | Shows whether the cluster is free or paid
multizone | Whether the multizone replication is enabled for the cluster or not
tcp_endpoint | TCP endpoint to connect to the kafka cluster
rest_endpoint | REST endpoint to connect to the kafka cluster
state | Current state of the cluster(active, deleted)
username | Username to be used in authenticating to the cluster
password | Password to be used in authenticating to the cluster
max_retention_size | Max retention size will be allowed to topics in the cluster
max_retention_time | Max retention time will be allowed to topics in the cluster
max_messages_per_second | Max messages allowed to be produced per second
creation_time | Cluster creation timestamp
max_message_size | Max message size will be allowed in topics in the cluster
max_partitions | Max total number of partitions allowed in the cluster

## Reset Kafka Cluster Password

```python
import requests

response = requests.post('https://api.upstash.com/v2/kafka/reset-password/:id', auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X POST \
  https://api.upstash.com/v2/kafka/reset-password/:id \
  -u 'EMAIL:API_KEY'
```

```go
client := &http.Client{}
req, err := http.NewRequest("POST", "https://api.upstash.com/v2/kafka/reset-password/:id", nil)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```
> Make sure to replace `EMAIL` and `API_KEY` with your email and API key.
> The above command returns a JSON structured like this:

```json
{
    "cluster_id": "9bc0e897-cbd3-4997-895a-fd77ad00aec9",
    "name": "mykafkacluster-2",
    "region": "eu-west-1",
    "type": "paid",
    "multizone": true,
    "tcp_endpoint": "sharing-mastodon-12819-eu1-kafka.upstashdev.com",
    "rest_endpoint": "sharing-mastodon-12819-eu1-rest-kafka.upstashdev.com",
    "state": "active",
    "username": "c2hhcmluZy1tYXN0b2Rvbi0xMjgxOSRV1ipriSBOwd0PHzw2KAs_cDrTXzvUKIs",
    "password": "7ea02715ceeb4fd3ba1542a5f3bf758e",
    "max_retention_size": 1073741824000,
    "max_retention_time": 2592000000,
    "max_messages_per_second": 1000,
    "creation_time": 1643978975,
    "max_message_size": 1048576,
    "max_partitions": 100
}
```

This endpoint updates the password of a kafka cluster.

Request parameters should be encoded as JSON.

### HTTP Request

`POST https://api.upstash.com/v2/kafka/reset-password/:id`

### URL Parameters

Parameter  | Description
---------  | -----------
ID | The ID of the kafka cluster to reset password

### Response Parameters

Parameter  | Description
---------  | -----------
cluster_id  | ID of the created kafka cluster
name | Name of the kafka cluster
region | The region the kafka cluster is deployed in
type | Shows whether the cluster is free or paid
multizone | Whether the multizone replication is enabled for the cluster or not
tcp_endpoint | TCP endpoint to connect to the kafka cluster
rest_endpoint | REST endpoint to connect to the kafka cluster
state | Current state of the cluster(active, deleted)
username | Username to be used in authenticating to the cluster
password | Password to be used in authenticating to the cluster
max_retention_size | Max retention size will be allowed to topics in the cluster
max_retention_time | Max retention time will be allowed to topics in the cluster
max_messages_per_second | Max messages allowed to be produced per second
creation_time | Cluster creation timestamp
max_message_size | Max message size will be allowed in topics in the cluster
max_partitions | Max total number of partitions allowed in the cluster

## Create Kafka Topic

```python
import requests

data = '{"name":"test-kafka-topic","partitions":1,"retention_time":1234,"retention_size":4567,"max_message_size":8912,"cleanup_policy":"delete","cluster_id":"9bc0e897-cbd3-4997-895a-fd77ad00aec9"}'

response = requests.post('https://api.upstash.com/v2/kafka/topic', data=data, auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X POST \
  https://api.upstash.com/v2/kafka/topic \
  -u 'EMAIL:API_KEY' \
  -d '{"name":"test-kafka-topic","partitions":1,"retention_time":1234,"retention_size":4567,"max_message_size":8912,"cleanup_policy":"delete","cluster_id":"9bc0e897-cbd3-4997-895a-fd77ad00aec9"}'
```

```go
client := &http.Client{}
var data = strings.NewReader(`{
    "name": "test-kafka-topic",
    "partitions": 1,
    "retention_time": 1234,
    "retention_size": 4567,
    "max_message_size": 8912,
    "cleanup_policy": "delete",
    "cluster_id": "9bc0e897-cbd3-4997-895a-fd77ad00aec9"
}`)
req, err := http.NewRequest("POST", "https://api.upstash.com/v2/kafka/topic", data)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```
> Make sure to replace `EMAIL` and `API_KEY` with your email and API key.
> The above command returns a JSON structured like this:

```json
{
    "topic_id": "0f458c88-2dc6-4f69-97bb-05060e0be934",
    "topic_name": "test-kafka-topic",
    "cluster_id": "9bc0e897-cbd3-4997-895a-fd77ad00aec9",
    "region": "eu-west-1",
    "creation_time": 1643981720,
    "state": "active",
    "partitions": 1,
    "multizone": true,
    "tcp_endpoint": "sharing-mastodon-12819-eu1-kafka.upstashdev.com",
    "rest_endpoint": "sharing-mastodon-12819-eu1-rest-kafka.upstashdev.com",
    "username": "c2hhcmluZy1tYXN0b2Rvbi0xMjgxOSRV1ipriSBOwd0PHzw2KAs_cDrTXzvUKIs",
    "password": "eu8K3rYRS-ma0AsINDo7MMemmHjjRSldHJcG3c1LUMZkFfdSf9u_Kd4xCWO9_oQc",
    "cleanup_policy": "delete",
    "retention_size": 4567,
    "retention_time": 1234,
    "max_message_size": 8912
}
```

This endpoint creates a new kafka topic in a cluster.

Request parameters should be encoded as JSON.

### HTTP Request

`POST https://api.upstash.com/v2/kafka/topic`

### Request Parameters

Parameter  | Description
---------  | -----------
name  | Name of the new kafka topic
partitions  | The number of partitions the topic will have
retention_time | Retention time of messsages in the topic (-1 for highest possible value)
retention_size | Retention size of the messages in the topic (-1 for highest possible value)
max_message_size | Max message size in the topic (-1 for highest possible value)
cleanup_policy | Cleanup policy will be used in the topic(compact or delete)
cluster_id | ID of the cluster the topic will be deployed in

### Response Parameters

Parameter  | Description
---------  | -----------
topic_id  | ID of the new kafka topic
topic_name  | Name of the new kafka topic
cluster_id  | ID of the created kafka cluster
region  | The region the topic is in
creation_time | Creation time of the topic
state | State of the topic(active or deleted)
partitions | Number of partitions the topic has
multizone | Whether the topic is deployed in a multizone kafka cluster or not
tcp_endpoint | TCP endpoint to connect to the topic
rest_endpoint | REST endpoint to connect to the topic
username | Username to be used in authenticating to the cluster
password | Password to be used in authenticating to the cluster
cleanup_policy | Cleanup policy will be used in the topic(compact or delete)
max_retention_size | Max retention size will be allowed to topics in the cluster
max_retention_time | Max retention time will be allowed to topics in the cluster
max_message_size | Max message size will be allowed in topics in the cluster

## Reconfigure Kafka Topic

```python
import requests

data = '{"retention_time":1235,"retention_size":4568,"max_message_size":8913}'

response = requests.post('https://api.upstash.com/v2/kafka/update-topic/:id', data=data, auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X POST \
  https://api.upstash.com/v2/kafka/update-topic/:id \
  -u 'EMAIL:API_KEY' \
  -d '{"retention_time":1235,"retention_size":4568,"max_message_size":8913}'
```

```go
client := &http.Client{}
var data = strings.NewReader(`{
    "retention_time": 1235,
    "retention_size": 4568,
    "max_message_size": 8913
}`)
req, err := http.NewRequest("POST", "https://api.upstash.com/v2/kafka/update-topic/:id", data)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```
> Make sure to replace `EMAIL` and `API_KEY` with your email and API key.
> The above command returns a JSON structured like this:

```json
{
    "topic_id": "0f458c88-2dc6-4f69-97bb-05060e0be934",
    "topic_name": "test-kafka-topic",
    "cluster_id": "9bc0e897-cbd3-4997-895a-fd77ad00aec9",
    "region": "eu-west-1",
    "creation_time": 1643981720,
    "state": "active",
    "partitions": 1,
    "multizone": true,
    "tcp_endpoint": "sharing-mastodon-12819-eu1-kafka.upstashdev.com",
    "rest_endpoint": "sharing-mastodon-12819-eu1-rest-kafka.upstashdev.com",
    "username": "c2hhcmluZy1tYXN0b2Rvbi0xMjgxOSRV1ipriSBOwd0PHzw2KAs_cDrTXzvUKIs",
    "password": "eu8K3rYRS-ma0AsINDo7MMemmHjjRSldHJcG3c1LUMZkFfdSf9u_Kd4xCWO9_oQc",
    "cleanup_policy": "delete",
    "retention_size": 4568,
    "retention_time": 1235,
    "max_message_size": 8913
}
```

This endpoint reconfigures an existing kafka topic.

Request parameters should be encoded as JSON.

### HTTP Request

`POST https://api.upstash.com/v2/kafka/update-topic/:id`

### Request Parameters

Parameter  | Description
---------  | -----------
retention_time | Retention time of messsages in the topic. This parameter is optional.
retention_size | Retention size of the messages in the topic. This parameter is optional.
max_message_size | Max message size in the topic. This parameter is optional.

### Response Parameters

Parameter  | Description
---------  | -----------
topic_id  | ID of the new kafka topic
topic_name  | Name of the new kafka topic
cluster_id  | ID of the created kafka cluster
region  | The region the topic is in
creation_time | Creation time of the topic
state | State of the topic(active or deleted)
partitions | Number of partitions the topic has
multizone | Whether the topic is deployed in a multizone kafka cluster or not
tcp_endpoint | TCP endpoint to connect to the topic
rest_endpoint | REST endpoint to connect to the topic
username | Username to be used in authenticating to the cluster
password | Password to be used in authenticating to the cluster
cleanup_policy | Cleanup policy will be used in the topic(compact or delete)
max_retention_size | Max retention size will be allowed to topics in the cluster
max_retention_time | Max retention time will be allowed to topics in the cluster
max_message_size | Max message size will be allowed in topics in the cluster

## Delete Kafka Topic

```python
import requests

response = requests.delete('https://api.upstash.com/v2/kafka/topic/:id' auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X DELETE \
  https://api.upstash.com/v2/kafka/topic/:id \
  -u 'EMAIL:API_KEY'
```

```go
client := &http.Client{}
req, err := http.NewRequest("DELETE", "https://api.upstash.com/v2/kafka/topic/:id", nil)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```

This endpoint deletes a kafka topic.

Request parameters should be encoded as JSON.

### HTTP Request

`POST https://api.upstash.com/v2/kafka/topic/:id`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kafka topic to delete

## Get Kafka Topic

```python
import requests

response = requests.get('https://api.upstash.com/v2/kafka/topic/:id', auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X GET \
  https://api.upstash.com/v2/kafka/topic/:id \
  -u 'EMAIL:API_KEY'
```

```go
client := &http.Client{}
req, err := http.NewRequest("GET", "https://api.upstash.com/v2/kafka/topic/:id", nil)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```
> Make sure to replace `EMAIL` and `API_KEY` with your email and API key.
> The above command returns a JSON structured like this:

```json
{
    "topic_id": "0f458c88-2dc6-4f69-97bb-05060e0be934",
    "topic_name": "test-kafka-topic",
    "cluster_id": "9bc0e897-cbd3-4997-895a-fd77ad00aec9",
    "region": "eu-west-1",
    "creation_time": 1643981720,
    "state": "active",
    "partitions": 1,
    "multizone": true,
    "tcp_endpoint": "sharing-mastodon-12819-eu1-kafka.upstashdev.com",
    "rest_endpoint": "sharing-mastodon-12819-eu1-rest-kafka.upstashdev.com",
    "username": "c2hhcmluZy1tYXN0b2Rvbi0xMjgxOSRV1ipriSBOwd0PHzw2KAs_cDrTXzvUKIs",
    "password": "eu8K3rYRS-ma0AsINDo7MMemmHjjRSldHJcG3c1LUMZkFfdSf9u_Kd4xCWO9_oQc",
    "cleanup_policy": "delete",
    "retention_size": 4568,
    "retention_time": 1235,
    "max_message_size": 8913
}
```

This endpoint gets details of a kafka topic.

Request parameters should be encoded as JSON.

### HTTP Request

`GET https://api.upstash.com/v2/kafka/topic/:id`

### URL Parameters

Parameter  | Description
---------  | -----------
ID | The ID of the kafka topic

### Response Parameters

Parameter  | Description
---------  | -----------
topic_id  | ID of the kafka topic
topic_name  | Name of the kafka topic
cluster_id  | ID of the created kafka cluster
region  | The region the topic is in
creation_time | Creation time of the topic
state | State of the topic(active or deleted)
partitions | Number of partitions the topic has
multizone | Whether the topic is deployed in a multizone kafka cluster or not
tcp_endpoint | TCP endpoint to connect to the topic
rest_endpoint | REST endpoint to connect to the topic
username | Username to be used in authenticating to the cluster
password | Password to be used in authenticating to the cluster
cleanup_policy | Cleanup policy will be used in the topic(compact or delete)
max_retention_size | Max retention size will be allowed to topics in the cluster
max_retention_time | Max retention time will be allowed to topics in the cluster
max_message_size | Max message size will be allowed in topics in the cluster

## List Kafka Credentials

```python
import requests

response = requests.get('https://api.upstash.com/v2/kafka/credentials', auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X GET \
  https://api.upstash.com/v2/kafka/credentials \
  -u 'EMAIL:API_KEY'
```

```go
client := &http.Client{}
req, err := http.NewRequest("GET", "https://api.upstash.com/v2/kafka/credentials", nil)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```
> Make sure to replace `EMAIL` and `API_KEY` with your email and API key.
> The above command returns a JSON structured like this:

```json
[
  {
    "credential_id": "27172269-da05-471b-9e8e-8fe4195871bc",
    "credential_name": "mycreds",
    "topic": "testopic",
    "permissions": "ALL",
    "cluster_id": "1793bfa1-d96e-46de-99ed-8f91f083209d",
    "cluster_slug":"noted-hamster-9151",
    "username":"bm90ZWQtaGFtc3Rlci05MTUxJPGKdKDkmwdObf8yMzmJ8jUqhmN1UQ7VmDe1xkk",
    "creation_time": 1655886853,
    "password": "xE1ypRHMq50jAhpbzu8qBb8jHNAxzezn6bkuRUvc2RZr7X1sznbhampm9p-feT61jnz6ewHJjUd5N6cQHhs84zCjQiP5somCY17FTQ7t6n0uPhWeyf-Fcw==",
    "state": "active"
  }
]
```

This endpoint lists created kafka credentials other than the default one.

Request parameters should be encoded as JSON.

### HTTP Request

`GET https://api.upstash.com/v2/kafka/credentials`

### Response Parameters

Parameter  | Description
---------  | ----------- 
credential_id  | ID of the kafka credential
credential_name  | Name of the kafka credential
topic  | Name of the kafka topic
permissions | Permission scope given to the kafka credential
cluster_id  | ID of the kafka cluster
cluster_slug  | Cluster Name that the credential is created for
username | Username to be used for the kafka credential
creation_time | Creation time of the credential
state | State of the credential(active or deleted)
password | Password to be used in authenticating to the cluster

## Create Kafka Credential

```python
import requests

data = '{"credential_name": "mycreds", "cluster_id":"1793bfa1-d96e-46de-99ed-8f91f083209d", "topic": "testtopic", "permissions": "ALL"}'

response = requests.post('https://api.upstash.com/v2/credential', data=data, auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X POST \
  https://api.upstash.com/v2/kafka/credential \
  -u 'EMAIL:API_KEY' \
  -d '{"credential_name": "mycreds", "cluster_id":"1793bfa1-d96e-46de-99ed-8f91f083209d", "topic": "testtopic", "permissions": "ALL"}'
```

```go
client := &http.Client{}
var data = strings.NewReader(`{
    "credential_name": "mycreds",
    "cluster_id":"1793bfa1-d96e-46de-99ed-8f91f083209d",
    "topic": "testopic",
    "permissions": "ALL"
}`)
req, err := http.NewRequest("POST", "https://api.upstash.com/v2/kafka/credential", data)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```
> Make sure to replace `EMAIL` and `API_KEY` with your email and API key.
> The above command returns a JSON structured like this:

```json
{
  "credential_id": "27172269-da05-471b-9e8e-8fe4195871bc",
  "credential_name": "mycreds",
  "topic": "testtopic",
  "permissions": "ALL",
  "cluster_id": "1793bfa1-d96e-46de-99ed-8f91f083209d",
  "cluster_slug":"easy-haddock-7753",
  "username":"ZWFzeS1oYWRkb2NrLTc3NTMkeeOs0FG4DZ3GxK99cArT0slAC37KLJgbe0fs7dA",
  "creation_time": 1655886853,
  "password": "xE1ypRHMq50jAhpbzu8qBb8jHNAxzezn6bkuRUvc2RZr7X1sznbhampm9p-feT61jnz6ewHJjUd5N6cQHhs84zCjQiP5somCY17FTQ7t6n0uPhWeyf-Fcw==",
  "state": "active"
}
```

This endpoint creates a kafka credential.

Request parameters should be encoded as JSON.

### HTTP Request

`POST https://api.upstash.com/v2/kafka/credential`

### Request Parameters

Parameter  | Description
---------  | -----------
credential_name  | Name of the new credential
cluster_id  | ID of the kafka cluster
topic  | Name of the kafka topic the credential will be used for
permissions  | Permission scope of the credential(ALL, PRODUCE or CONSUME)

### Response Parameters

Parameter  | Description
---------  | ----------- 
credential_id  | ID of the kafka credential
credential_name  | Name of the kafka credential
topic  | Name of the kafka topic
permissions | Permission scope given to the kafka credential
cluster_id  | ID of the kafka cluster
username | Username to be used for the kafka credential
creation_time | Creation time of the credential
state | State of the credential(active or deleted)
password | Password to be used in authenticating to the cluster

## Delete Kafka Credential

```python
import requests

response = requests.delete('https://api.upstash.com/v2/kafka/credential/:id' auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X DELETE \
  https://api.upstash.com/v2/kafka/credential/:id \
  -u 'EMAIL:API_KEY'
```

```go
client := &http.Client{}
req, err := http.NewRequest("DELETE", "https://api.upstash.com/v2/kafka/credential/:id", nil)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```

This endpoint deletes a kafka credential.

Request parameters should be encoded as JSON.

### HTTP Request

`POST https://api.upstash.com/v2/kafka/credential/:id`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kafka credential to delete

## List Kafka Topics in Cluster

```python
import requests

response = requests.get('https://api.upstash.com/v2/kafka/topics/:id', auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X GET \
  https://api.upstash.com/v2/kafka/topics/:id \
  -u 'EMAIL:API_KEY'
```

```go
client := &http.Client{}
req, err := http.NewRequest("GET", "https://api.upstash.com/v2/kafka/topics/:id", nil)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```
> Make sure to replace `EMAIL` and `API_KEY` with your email and API key.
> The above command returns a JSON structured like this:

```json
[
    {
        "topic_id": "0f458c88-2dc6-4f69-97bb-05060e0be934",
        "topic_name": "test-kafka-topic",
        "cluster_id": "9bc0e897-cbd3-4997-895a-fd77ad00aec9",
        "region": "eu-west-1",
        "creation_time": 1643981720,
        "state": "active",
        "partitions": 1,
        "multizone": true,
        "tcp_endpoint": "sharing-mastodon-12819-eu1-kafka.upstashdev.com",
        "rest_endpoint": "sharing-mastodon-12819-eu1-rest-kafka.upstashdev.com",
        "username": "c2hhcmluZy1tYXN0b2Rvbi0xMjgxOSRV1ipriSBOwd0PHzw2KAs_cDrTXzvUKIs",
        "password": "eu8K3rYRS-ma0AsINDo7MMemmHjjRSldHJcG3c1LUMZkFfdSf9u_Kd4xCWO9_oQc",
        "cleanup_policy": "delete",
        "retention_size": 4568,
        "retention_time": 1235,
        "max_message_size": 8913
    }
]
```

This endpoint list kafka topics in a cluster.

### HTTP Request

`GET https://api.upstash.com/v2/kafka/topics/:id`

### URL Parameters

Parameter  | Description
---------  | -----------
ID | The ID of the kafka cluster

### Response Parameters

Parameter  | Description
---------  | -----------
topic_id  | ID of the kafka topic
topic_name  | Name of the kafka topic
cluster_id  | ID of the created kafka cluster
region  | The region the topic is in
creation_time | Creation time of the topic
state | State of the topic(active or deleted)
partitions | Number of partitions the topic has
multizone | Whether the topic is deployed in a multizone kafka cluster or not
tcp_endpoint | TCP endpoint to connect to the topic
rest_endpoint | REST endpoint to connect to the topic
username | Username to be used in authenticating to the cluster
password | Password to be used in authenticating to the cluster
cleanup_policy | Cleanup policy will be used in the topic(compact or delete)
max_retention_size | Max retention size will be allowed to topics in the cluster
max_retention_time | Max retention time will be allowed to topics in the cluster
max_message_size | Max message size will be allowed in topics in the cluster

## Get Kafka Cluster Stats

```python
import requests

response = requests.get('https://api.upstash.com/v2/kafka/stats/cluster/:id', auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X GET \
  https://api.upstash.com/v2/kafka/stats/cluster/:id \
  -u 'EMAIL:API_KEY'
```

```go
client := &http.Client{}
req, err := http.NewRequest("GET", "https://api.upstash.com/v2/kafka/stats/cluster/:id", nil)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```
> Make sure to replace `EMAIL` and `API_KEY` with your email and API key.
> The above command returns a JSON structured like this:

```json
{
  "throughput": [
    {
      "x": "2022-02-07 11:30:28",
      "y": 0
    }
    ...
  ],
  "produce_throughput": [
    {
      "x": "2022-02-07 11:30:28",
      "y": 0
    }
    ...
  ],
  "consume_throughput": [
    {
      "x": "2022-02-07 11:30:28",
      "y": 0
    }
    ...
  ],
  "diskusage": [
    {
      "x": "2022-02-07 11:45:28",
      "y": 0
    }
    ...
  ],
  "days": [
    "Thursday",
    "Friday",
    "Saturday",
    "Sunday",
    "Monday"
  ],
  "dailyproduce": [
    {
      "x": "2022-02-07 11:30:28.937259962 +0000 UTC",
      "y": 0
    }
    ...
  ],
  "dailyconsume": [
    {
      "x": "2022-02-07 11:30:28.937256776 +0000 UTC",
      "y": 0
    }
    ...
  ],
  "total_monthly_storage": 0,
  "total_monthly_billing": 0,
  "total_monthly_produce": 0,
  "total_monthly_consume": 0
}
```

This endpoint gets detailed stats of a kafka cluster.

Request parameters should be encoded as JSON.

### HTTP Request

`GET https://api.upstash.com/v2/kafka/stats/cluster/:id`

### URL Parameters

Parameter  | Description
---------  | -----------
ID | The ID of the kafka cluster

### Response Parameters

Parameter  | Description
---------  | -----------
throughput  | Number of monthly messages in kafka cluster
produce_throughput  | Number of monthly messages produced in kafka cluster
consume_throughput  | Number of monthly messages consumed in kafka cluster
diskusage  | Total disk usage of the kafka cluster
days  | String representation of last 5 days of the week starting from the current day
dailyproduce  | Last 5 days daily produced message count in kafka cluster
dailyconsume  | Last 5 days daily consumed message count in kafka cluster
total_monthly_storage  | Average storage size of the kafka cluster in the current month
total_monthly_billing  | Total cost of the kafka cluster in current month
total_monthly_produce  | Total number of produced message in current month
total_monthly_consume  | Total number of consumed message in current month

## Get Kafka Topic Stats

```python
import requests

response = requests.get('https://api.upstash.com/v2/kafka/stats/topic/:id', auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X GET \
  https://api.upstash.com/v2/kafka/stats/topic/:id \
  -u 'EMAIL:API_KEY'
```

```go
client := &http.Client{}
req, err := http.NewRequest("GET", "https://api.upstash.com/v2/kafka/stats/topic/:id", nil)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```
> Make sure to replace `EMAIL` and `API_KEY` with your email and API key.
> The above command returns a JSON structured like this:

```json
{
  "throughput": [
    {
      "x": "2022-02-07 12:05:11",
      "y": 0
    }
    ...
  ],
  "produce_throughput": [
    {
      "x": "2022-02-07 12:05:11",
      "y": 0
    }
    ...
  ],
  "consume_throughput": [
    {
      "x": "2022-02-07 12:05:11",
      "y": 0
    }
    ...
  ],
  "diskusage": [
    {
      "x": "2022-02-07 12:20:11",
      "y": 0
    }
    ...
  ],
  "total_monthly_storage": 0,
  "total_monthly_produce": 0,
  "total_monthly_consume": 0
}
```

This endpoint gets detailed stats of a kafka cluster.

Request parameters should be encoded as JSON.

### HTTP Request

`GET https://api.upstash.com/v2/kafka/stats/topic/:id`

### URL Parameters

Parameter  | Description
---------  | -----------
ID | The ID of the kafka topic

### Response Parameters

Parameter  | Description
---------  | -----------
throughput | Number of monthly messages in kafka topic
produce_throughput | Number of monthly produced messages in kafka topic
consume_throughput | Number of monthly consumed messages in kafka topic
diskusage | The disk usage of the kafka topic
total_monthly_storage | Total monthly storage used by the kafka topic
total_monthly_produce | Total monthly produced number of messages to the kafka topic
total_monthly_consume | Total monthly consumed number of messages from the kafka topic





## Create Kafka Connector

```python
import requests

data = '{"name":"connectorName","cluster_id":"7568431c-88d5-4409-a808-2167f22a7133", "properties":{"connector.class": "com.mongodb.kafka.connect.MongoSourceConnector","connection.uri": "connection-uri"}}'

response = requests.post('https://api.upstash.com/v2/kafka/connector', data=data, auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X POST \
  https://api.upstash.com/v2/kafka/connector \
  -u 'EMAIL:API_KEY' \
  -d '{"name":"connectorName","cluster_id":"7568431c-88d5-4409-a808-2167f22a7133", "properties":{"connector.class": "com.mongodb.kafka.connect.MongoSourceConnector","connection.uri": "connection-uri"}}'
```

```go
client := &http.Client{}
var data = strings.NewReader(`{
    "name": "connectorName",
    "cluster_id": "7568431c-88d5-4409-a808-2167f22a7133",
    "properties":{"connector.class": "com.mongodb.kafka.connect.MongoSourceConnector","connection.uri": "connection-uri"}
}`)
req, err := http.NewRequest("POST", "https://api.upstash.com/v2/kafka/connector", data)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```
> Make sure to replace `EMAIL` and `API_KEY` with your email and API key.
> The above command returns a JSON structured like this:

```json
{
  "connector_id":"431ec970-b59d-4b00-95fe-5f3abcc52c2f",
  "name":"connectorName",
  "customer_id":"EMAIL",
  "cluster_id":"7568431c-88d5-4409-a808-2167f22a7133",
  "creation_time":1684369147,
  "deletion_time":0,
  "state":"pending",
  "state_error_message":"",
  "connector_state":"",
  "tasks":[],
  "topics":[],
  "connector_class":"com.mongodb.kafka.connect.MongoSourceConnector",
  "encoded_username":"YXBwYXJlbnQta2l0ZS0xMTMwMiTIqFhTItzgDdE56au6LgnnbtlN7ITzh4QATDw",
  "TTL":1684370947
}

```

This endpoint creates a new kafka connector in a cluster.

Request parameters should be encoded as JSON.

### HTTP Request

`POST https://api.upstash.com/v2/kafka/connector`

### Request Parameters

Parameter  | Description
---------  | -----------
name  | Name of the new kafka topic
cluster_id | ID of the cluster the topic will be deployed in
properties  | Properties of the connector. Custom config for different types of connectors.
### Response Parameters

Parameter  | Description
---------  | -----------
connector_id  | ID of the new kafka connector
name  | Name of the new kafka connector
cluster_id  | ID of the kafka cluster of the connector
creation_time | Creation time of the topic
customer_id | Owner of the connector
state | State of the connector
state_error_message | Error message, if the connector failed
connector_state | State of the connector
tasks | Tasks for the connector
topics | Topics that are given with properties config
connector_class | Class of the created connector
encoded_username | Encoded username for the connector
TTL | time to live for connector

## Reconfigure Kafka Connector

```python
import requests

data = '{"connector.class": "com.mongodb.kafka.connect.MongoSourceConnector","connection.uri": "connection-uri-update"}'

response = requests.post('https://api.upstash.com/v2/kafka/update-connector/:id', data=data, auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X POST \
  https://api.upstash.com/v2/kafka/update-connector/:id \
  -u 'EMAIL:API_KEY' \
  -d '{"connector.class": "com.mongodb.kafka.connect.MongoSourceConnector","connection.uri": "connection-uri-update"}'
```

```go
client := &http.Client{}
var data = strings.NewReader(`{
    "connector.class": "com.mongodb.kafka.connect.MongoSourceConnector",
    "connection.uri": "connection-uri-update"
}`)
req, err := http.NewRequest("POST", "https://api.upstash.com/v2/kafka/update-connector/:id", data)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```
> Make sure to replace `EMAIL` and `API_KEY` with your email and API key.
> The above command returns a JSON structured like this:

```json
{
  "connector_id":"431ec970-b59d-4b00-95fe-5f3abcc52c2f",
  "name":"connectorName",
  "customer_id":"EMAIL",
  "cluster_id":"7568431c-88d5-4409-a808-2167f22a7133",
  "creation_time":1684369147,
  "deletion_time":0,
  "state":"pending",
  "state_error_message":"",
  "connector_state":"",
  "tasks":[],
  "topics":[],
  "connector_class":"com.mongodb.kafka.connect.MongoSourceConnector",
  "encoded_username":"YXBwYXJlbnQta2l0ZS0xMTMwMiTIqFhTItzgDdE56au6LgnnbtlN7ITzh4QATDw"
}
```

This endpoint reconfigures an existing kafka connector.

Request parameters should be encoded as JSON.

### HTTP Request

`POST https://api.upstash.com/v2/kafka/update-connector/:id`

### Request Parameters

Parameter  | Description
---------  | -----------
properties | Custom property values, depending on the connector type. Given values will be changed on the connector.

### Response Parameters
Parameter  | Description
---------  | -----------
connector_id  | ID of the new kafka connector
name  | Name of the new kafka connector
cluster_id  | ID of the kafka cluster of the connector
creation_time | Creation time of the topic
customer_id | Owner of the connector
state | State of the connector
state_error_message | Error message, if the connector failed
connector_state | State of the connector
tasks | Tasks for the connector
topics | Topics that are given with properties config
connector_class | Class of the created connector
encoded_username | Encoded username for the connector
TTL | time to live for connector

## List Kafka Connectors in Cluster

```python
import requests

response = requests.get('https://api.upstash.com/v2/kafka/connectors/:id', auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X GET \
  https://api.upstash.com/v2/kafka/connectors/:id \
  -u 'EMAIL:API_KEY'
```

```go
client := &http.Client{}
req, err := http.NewRequest("GET", "https://api.upstash.com/v2/kafka/connectors/:id", nil)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```
> Make sure to replace `EMAIL` and `API_KEY` with your email and API key.
> The above command returns a JSON structured like this:

```json
[
  {
    "connector_id":"431ec970-b59d-4b00-95fe-5f3abcc52c2f",
    "name":"connectorName",
    "customer_id":"EMAIL",
    "cluster_id":"7568431c-88d5-4409-a808-2167f22a7133",
    "creation_time":1684369147,
    "deletion_time":0,
    "state":"failed",
    "state_error_message":"Connector configuration is invalid and contains the following 1 error(s):\nInvalid value connection-uri-update for configuration connection.uri: The connection string is invalid. Connection strings must start with either 'mongodb://' or 'mongodb+srv://\n",
    "connector_state":"",
    "tasks":[],
    "topics":[],
    "connector_class":"com.mongodb.kafka.connect.MongoSourceConnector",
    "properties": {
        "connection.uri":"connection-uri-update",
        "connector.class":"com.mongodb.kafka.connect.MongoSourceConnector"
      },
    "encoded_username":"YXBwYXJlbnQta2l0ZS0xMTMwMiTIqFhTItzgDdE56au6LgnnbtlN7ITzh4QATDw"
  }
]
```

This endpoint lists kafka connectors in a cluster.

### HTTP Request

`GET https://api.upstash.com/v2/kafka/connectors/:id`

### URL Parameters

Parameter  | Description
---------  | -----------
ID | The ID of the kafka cluster

### Response Parameters

Parameter  | Description
---------  | -----------
connector_id  | ID of the new kafka connector
name  | Name of the new kafka connector
cluster_id  | ID of the kafka cluster of the connector
creation_time | Creation time of the topic
customer_id | Owner of the connector
state | State of the connector
state_error_message | Error message, if the connector failed
connector_state | State of the connector
tasks | Tasks for the connector
topics | Topics that are given with properties config
connector_class | Class of the created connector
properties | Properties that the connector was configured with
encoded_username | Encoded username for the connector
TTL | time to live for connector

## Get Kafka Connector

```python
import requests

response = requests.get('https://api.upstash.com/v2/kafka/connector/:id', auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X GET \
  https://api.upstash.com/v2/kafka/connector/:id \
  -u 'EMAIL:API_KEY'
```

```go
client := &http.Client{}
req, err := http.NewRequest("GET", "https://api.upstash.com/v2/kafka/connector/:id", nil)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```
> Make sure to replace `EMAIL` and `API_KEY` with your email and API key.
> The above command returns a JSON structured like this:

```json
{
  "connector_id":"431ec970-b59d-4b00-95fe-5f3abcc52c2f",
  "name":"connectorName",
  "customer_id":"EMAIL",
  "cluster_id":"7568431c-88d5-4409-a808-2167f22a7133",
  "creation_time":1684369147,
  "deletion_time":0,
  "state":"failed",
  "state_error_message":"Connector configuration is invalid and contains the following 1 error(s):\nInvalid value connection-uri-update for configuration connection.uri: The connection string is invalid. Connection strings must start with either 'mongodb://' or 'mongodb+srv://\n",
  "connector_state":"",
  "tasks":[],
  "topics":[],
  "connector_class":"com.mongodb.kafka.connect.MongoSourceConnector",
  "properties": {
      "connection.uri":"connection-uri-update",
      "connector.class":"com.mongodb.kafka.connect.MongoSourceConnector"
    },
  "encoded_username":"YXBwYXJlbnQta2l0ZS0xMTMwMiTIqFhTItzgDdE56au6LgnnbtlN7ITzh4QATDw"
}
```

This endpoint gets details of a kafka connector.

### HTTP Request

`GET https://api.upstash.com/v2/kafka/connector/:id`

### URL Parameters

Parameter  | Description
---------  | -----------
ID | The ID of the kafka connector

### Response Parameters

Parameter  | Description
---------  | -----------
connector_id  | ID of the new kafka connector
name  | Name of the new kafka connector
cluster_id  | ID of the kafka cluster of the connector
creation_time | Creation time of the topic
customer_id | Owner of the connector
state | State of the connector
state_error_message | Error message, if the connector failed
connector_state | State of the connector
tasks | Tasks for the connector
topics | Topics that are given with properties config
connector_class | Class of the created connector
properties | Properties that the connector was configured with
encoded_username | Encoded username for the connector

## Delete Kafka Connector

```python
import requests

response = requests.delete('https://api.upstash.com/v2/kafka/connector/:id' auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X DELETE \
  https://api.upstash.com/v2/kafka/connector/:id \
  -u 'EMAIL:API_KEY'
```

```go
client := &http.Client{}
req, err := http.NewRequest("DELETE", "https://api.upstash.com/v2/kafka/connector/:id", nil)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```

This endpoint deletes a kafka topic.

### HTTP Request

`POST https://api.upstash.com/v2/kafka/connector/:id`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kafka connector to delete


## Restart Kafka Connector

```python
import requests

response = requests.post('https://api.upstash.com/v2/kafka/connector/:id/restart', auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X POST \
  https://api.upstash.com/v2/kafka/connector/:id/restart \
  -u 'EMAIL:API_KEY'
```

```go
client := &http.Client{}
req, err := http.NewRequest("POST", "https://api.upstash.com/v2/kafka/connector/:id/restart", nil)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```
> Make sure to replace `EMAIL` and `API_KEY` with your email and API key.
> The above command returns a JSON structured like this:

```json
"OK"
```

This endpoint restarts an existing connector.

### HTTP Request

`POST https://api.upstash.com/v2/kafka/connector/:id/restart`

### Request Parameters

Parameter  | Description
---------  | -----------
id  | ID of the connector to restart

### Response Parameters

"OK"


## Start Kafka Connector

```python
import requests

response = requests.post('https://api.upstash.com/v2/kafka/connector/:id/start', auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X POST \
  https://api.upstash.com/v2/kafka/connector/:id/start \
  -u 'EMAIL:API_KEY'
```

```go
client := &http.Client{}
req, err := http.NewRequest("POST", "https://api.upstash.com/v2/kafka/connector/:id/start", nil)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```
> Make sure to replace `EMAIL` and `API_KEY` with your email and API key.
> The above command returns a JSON structured like this:

```json
"OK"
```

This endpoint starts an existing paused connector.

### HTTP Request

`POST https://api.upstash.com/v2/kafka/connector/:id/start`

### Request Parameters

Parameter  | Description
---------  | -----------
id  | ID of the connector to start

### Response Parameters

"OK"

## Pause Kafka Connector

```python
import requests

response = requests.post('https://api.upstash.com/v2/kafka/connector/:id/pause', auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X POST \
  https://api.upstash.com/v2/kafka/connector/:id/pause \
  -u 'EMAIL:API_KEY'
```

```go
client := &http.Client{}
req, err := http.NewRequest("POST", "https://api.upstash.com/v2/kafka/connector/:id/pause", nil)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```
> Make sure to replace `EMAIL` and `API_KEY` with your email and API key.
> The above command returns a JSON structured like this:

```json
"OK"
```

This endpoint pauses an existing connector.

### HTTP Request

`POST https://api.upstash.com/v2/kafka/connector/:id/pause`

### Request Parameters

Parameter  | Description
---------  | -----------
id  | ID of the connector to pause

### Response Parameters

"OK"

# QStash

Endpoints to get QStash user info and rotating tokens.

All endpoints return responses encoded as JSON.

## Get QStash User

```python
import requests

response = requests.get('https://api.upstash.com/v2/qstash/user', auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X GET \
  https://api.upstash.com/v2/qstash/user \
  -u 'EMAIL:API_KEY'
```

```go
client := &http.Client{}
req, err := http.NewRequest("GET", "https://api.upstash.com/v2/qstash/user", nil)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```
> Make sure to replace `EMAIL` and `API_KEY` with your email and API key.
> The above command returns a JSON structured like this:



```json
{
  "customer_id":"<customer_id as string>",
  "id":"<id as string>",
  "password":"<password as string>",
  "token":"<token as string>",
  "active":true,
  "state":"active",
  "max_message_size":1048576,
  "max_requests_per_day":500000,
  "max_endpoints_per_topic":100,
  "max_retries":5,
  "max_topics":20,
  "max_schedules":100,
  "retention":2592000,
  "timeout":30,
  "creation_time":1658150385
}
```


This endpoint gets details of a QStash user.

Request parameters should be encoded as JSON.

### HTTP Request

`GET https://api.upstash.com/v2/qstash/user`

### Response Parameters

Parameter  | Description
---------  | -----------
customer_id  | Customer ID of the QStash user
id | ID of the QStash user
password | Password of the QStash user
token | Encoded token for the QStash user 
active | Activeness of the QStash user
state | State of the QStash user
max_message_size | Max message size allowed for requests
max_requests_per_day | Max number of requests allowed
max_endpoints_per_topic | Max number of endpoints allowed in topics
max_retries | Max allowed retries for failing requests 
max_topics | Max number of topics allowed to be created
max_schedules | Max number of schedules allowed to be created
retention | Retention for topics
timeout | Timeout for the requests
creation_time | Creation time of the QStash user


## Rotate QStash Token

```python
import requests

response = requests.post('https://api.upstash.com/v2/qstash/rotate-token', auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X POST \
  https://api.upstash.com/v2/qstash/rotate-token \
  -u 'EMAIL:API_KEY' \
```

```go
client := &http.Client{}

req, err := http.NewRequest("POST", "https://api.upstash.com/v2/qstash/rotate-token", nil)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```
> Make sure to replace `EMAIL` and `API_KEY` with your email and API key.
> The above command returns a JSON structured like this:

```json
{
  "customer_id":"<customer_id as string>",
  "id":"<id as string>",
  "password":"<password as string>",
  "token":"<new token as string>",
  "active":true,
  "state":"active",
  "max_message_size":1048576,
  "max_requests_per_day":500000,
  "max_endpoints_per_topic":100,
  "max_retries":5,
  "max_topics":20,
  "max_schedules":100,
  "retention":2592000,
  "timeout":30,
  "creation_time":1658150385
}
```

This endpoint rotates the QStash token and returns the QStash user.

### HTTP Request

`POST https://api.upstash.com/v2/qstash/rotate-token`

### Response Parameters

Parameter  | Description
---------  | -----------
customer_id  | Customer ID of the QStash user
id | ID of the QStash user
password | New Password of the QStash user
token | New Encoded token for the QStash user 
active | Activeness of the QStash user
state | State of the QStash user
max_message_size | Max message size allowed for requests
max_requests_per_day | Max number of requests allowed
max_endpoints_per_topic | Max number of endpoints allowed in topics
max_retries | Max allowed retries for failing requests 
max_topics | Max number of topics allowed to be created
max_schedules | Max number of schedules allowed to be created
retention | Retention for topics
timeout | Timeout for the requests
creation_time | Creation time of the QStash user

# Teams

Endpoints to create, manage Upstash teams.

All endpoints return responses encoded as JSON.

## Create Team

```python
import requests

data = '{"team_name":"myteam","copy_cc":true}'

response = requests.post('https://api.upstash.com/v2/team', data=data, auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X POST \
  https://api.upstash.com/v2/team \
  -u 'EMAIL:API_KEY' \
  -d '{"team_name":"myteam","copy_cc":true}'
```

```go
client := &http.Client{}
var data = strings.NewReader(`{
    "team_name":"myteam",
    "copy_cc":true
}`)
req, err := http.NewRequest("POST", "https://api.upstash.com/v2/team", data)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```
> Make sure to replace `EMAIL` and `API_KEY` with your email and API key.
> The above command returns a JSON structured like this:

```json
{
    "team_id": "75b471f2-15a1-47b0-8ce5-12a57682bfc9",
    "team_name": "test_team_name_2",
    "copy_cc": true
}
```

This endpoint creates a new team.

Request parameters should be encoded as JSON.

### HTTP Request

`POST https://api.upstash.com/v2/team`

### Request Parameters

Parameter  | Description
---------  | -----------
team_name  | Name of the new team
copy_cc  | Whether to copy existing credit card information to team or not(true, false)

### Response Parameters

Parameter  | Description
---------  | -----------
team_id  | ID of the created team
team_name  | Name of the created team
copy_cc | Whether creditcard information added to team during creation or not

## Delete Team

```python
import requests

response = requests.delete('https://api.upstash.com/v2/team/:id' auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X DELETE \
  https://api.upstash.com/v2/team/:id \
  -u 'EMAIL:API_KEY'
```

```go
client := &http.Client{}
req, err := http.NewRequest("DELETE", "https://api.upstash.com/v2/team/:id", nil)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```

This endpoint deletes a team.

Request parameters should be encoded as JSON.

### HTTP Request

`POST https://api.upstash.com/v2/team/:id`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the team to delete


## List Teams

```python
import requests

response = requests.get('https://api.upstash.com/v2/teams', auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X GET \
  https://api.upstash.com/v2/teams \
  -u 'EMAIL:API_KEY'
```

```go
client := &http.Client{}
req, err := http.NewRequest("GET", "https://api.upstash.com/v2/teams", nil)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```
> Make sure to replace `EMAIL` and `API_KEY` with your email and API key.
> The above command returns a JSON structured like this:

```json
[
    {
        "team_id": "95849b27-40d0-4532-8695-d2028847f823",
        "team_name": "test_team_name",
        "member_role": "owner",
        "copy_cc": true
    }
]
```

This endpoint list all teams of user.

Request parameters should be encoded as JSON.

### HTTP Request

`GET https://api.upstash.com/v2/teams`

### Response Parameters

Parameter  | Description
---------  | -----------
team_id  | ID of the team
member_role  | Role of the user in this team
team_name  | Name of the team
copy_cc | Whether creditcard information added to team during creation or not

## Add Team Member

```python
import requests

data = '{"team_id":"95849b27-40d0-4532-8695-d2028847f823","member_email":"example@upstash.com","member_role":"dev"}'

response = requests.post('https://api.upstash.com/v2/teams/member', data=data, auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X POST \
  https://api.upstash.com/v2/teams/member \
  -u 'EMAIL:API_KEY' \
  -d '{"team_id":"95849b27-40d0-4532-8695-d2028847f823","member_email":"example@upstash.com","member_role":"dev"}'
```

```go
client := &http.Client{}
var data = strings.NewReader(`{
    "team_id":"95849b27-40d0-4532-8695-d2028847f823",
    "member_email":"example@upstash.com",
    "member_role":"dev"
}`)
req, err := http.NewRequest("POST", "https://api.upstash.com/v2/teams/member", data)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```
> Make sure to replace `EMAIL` and `API_KEY` with your email and API key.
> The above command returns a JSON structured like this:

```json
{
    "team_id": "95849b27-40d0-4532-8695-d2028847f823",
    "team_name": "test_team_name",
    "member_email": "example@upstash.com",
    "member_role": "dev"
}
```

This endpoint adds a new team member to the specified team.

Request parameters should be encoded as JSON.

### HTTP Request

`POST https://api.upstash.com/v2/teams/member`

### Request Parameters

Parameter  | Description
---------  | -----------
team_id  | Id of the team to add the member to
member_email  | Email of the new team member
member_role | Role of the new team member(admin, dev or finance)

### Response Parameters

Parameter  | Description
---------  | -----------
team_id  | ID of the team
team_name  | Name of the team
member_email | Email of the new team member
member_role | Role of the new team member

## Delete Team Member

```python
import requests

data = '{"team_id":"95849b27-40d0-4532-8695-d2028847f823","member_email":"example@upstash.com"}'

response = requests.delete('https://api.upstash.com/v2/teams/member', data=data, auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X DELETE \
  https://api.upstash.com/v2/teams/member \
  -u 'EMAIL:API_KEY' \
  -d '{"team_id":"95849b27-40d0-4532-8695-d2028847f823","member_email":"example@upstash.com"}'
```

```go
client := &http.Client{}
var data = strings.NewReader(`{
    "team_id":"95849b27-40d0-4532-8695-d2028847f823",
    "member_email":"example@upstash.com"
}`)
req, err := http.NewRequest("DELETE", "https://api.upstash.com/v2/teams/member", data)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```


This endpoint deletes a team member from the specified team.

Request parameters should be encoded as JSON.

### HTTP Request

`DELETE https://api.upstash.com/v2/teams/member`

### Request Parameters

Parameter  | Description
---------  | -----------
team_id  | Id of the team to delete the member from
member_email  | Email of the member to be deleted

## Get Team Members

```python
import requests

response = requests.get('https://api.upstash.com/v2/teams/:id', auth=('EMAIL', 'API_KEY'))
response.content
```

```shell
curl -X GET \
  https://api.upstash.com/v2/teams/:id \
  -u 'EMAIL:API_KEY'
```

```go
client := &http.Client{}
req, err := http.NewRequest("GET", "https://api.upstash.com/v2/teams/:id", nil)
if err != nil {
    log.Fatal(err)
}
req.SetBasicAuth("email", "api_key")
resp, err := client.Do(req)
if err != nil {
    log.Fatal(err)
}
bodyText, err := ioutil.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%s\n", bodyText);
```
> Make sure to replace `EMAIL` and `API_KEY` with your email and API key.
> The above command returns a JSON structured like this:

```json
[
    {
        "team_id": "3423cb72-e50d-43ec-a9c0-f0f359941223",
        "team_name": "test_team_name_2",
        "member_email": "example@upstash.com",
        "member_role": "dev"
    },
    {
        "team_id": "3423cb72-e50d-43ec-a9c0-f0f359941223",
        "team_name": "test_team_name_2",
        "member_email": "example_2@upstash.com",
        "member_role": "owner"
    }
]
```

This endpoint list all teams of user.

Request parameters should be encoded as JSON.

### HTTP Request

`GET https://api.upstash.com/v2/teams/:id`

### URL Parameters

Parameter  | Description
---------  | -----------
ID | The ID of the team

### Response Parameters

Parameter  | Description
---------  | -----------
team_id  | ID of the team
member_role  | Role of the user in this team
team_name  | Name of the team
member_email | Email of the team member