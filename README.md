# Favro PHP Api

<p align="center">
    <img src="logo.png" alt="Favro PHP Api">
</p>

<p align="center">
<a href="https://travis-ci.org/seregazhuk/php-favro-api"><img src="https://travis-ci.org/seregazhuk/php-favro-api.svg?branch=master"></a>
<a href="https://scrutinizer-ci.com/g/seregazhuk/php-favro-api/?branch=master"><img src="https://scrutinizer-ci.com/g/seregazhuk/php-favro-api/badges/quality-score.png?b=master"></a>
<a href="https://codeclimate.com/github/seregazhuk/php-favro-api"><img src="https://codeclimate.com/github/seregazhuk/php-favro-api/badges/gpa.svg" /></a>
<a href="https://codeclimate.com/github/seregazhuk/php-favro-api/coverage"><img src="https://codeclimate.com/github/seregazhuk/php-favro-api/badges/coverage.svg" /></a>
<a href="https://packagist.org/packages/seregazhuk/favro-api"><img src="https://poser.pugx.org/seregazhuk/favro-api/v/stable"></a>
<a href="https://packagist.org/packages/seregazhuk/favro-api"><img src="https://poser.pugx.org/seregazhuk/favro-api/downloads"></a>
</p>

- [Dependencies](#dependencies)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Users](#users)
- [Organizations](#organizations)
- [Collections](#collections)
- [Widgets](#widgets)

## Dependencies
Library requires CURL extension and PHP 5.5.9 or above.

## Installation
Via composer:
```
composer require "seregazhuk/favro-api:*"
```

## Quick Start

```php 
// You may need to amend this path to locate composer's autoloader
require './vendor/autoload.php';

use seregazhuk\Favro\Favro;

$favro = Favro::create('seregazhuk88@gmail.com', 'Eikah1ei');

// get your organizations
$result = $favro->organizations->getAll();
$organizations = $result['entities'];

// select first organization
$favro->setOrganization($organizations[0]['organizationId']);

// get all collections
$result = $favro->collections->getAll();
```

## Users

[Get all users](https://favro.com/developer/#get-all-users):
```php
$result = $favro->users->getAll();
```

The response will be an array of users:

```php
[
    "limit": 100,
    "page": 0,
    "pages": 1,
    "requestId": "8cc57b1d8a218fa639c8a0fa",
    "entities": [
        [
            "userId": "67973f72db34592d8fc96c48",
            "name": "Favro user",
            "email": "user@favro.com"
        ]
    ]
]
```

[Get a user](https://favro.com/developer/#get-a-user):

Arguments: 

| Argument | Type | Description |
| --- | --- | --- |
|userId|string|The id of the user to be retrieved.|

```php
$result = $favro->users->getById($userId);
```
The response returns a user object:

```php
[
    "userId": "67973f72db34592d8fc96c48",
    "name": "Favro user",
    "email": "user@favro.com"
]
```

## Organizations

[Get all organizations](https://favro.com/developer/#get-all-organizations):
```php
$result = $favro->organizations->getAll();
```
The response will be an array of organizations:

```php
[
    "limit": 100,
    "page": 0,
    "pages": 1,
    "requestId": "8cc57b1d8a218fa639c8a0fa",
    "entities": [
        [
            "organizationId" : "67973f72db34592d8fc96c48",
            "name" : "My organization",
            "sharedToUsers": [
                [
                    "userId" : "fB6bJr5TbaKLiofns",
                    "role" : "administrator",
                    "joinDate" : "2016-02-10T14:25:58.745Z"
                ]
            ]
        ]
    ]
]
```

[Get an organization](https://favro.com/developer/#get-an-organization):

Arguments: 

| Argument | Type | Description |
| --- | --- | --- |
|organizationId|string|The id of the organization to be retrieved.|

```php
$result = $favro->organizations->getById($ogranizationId);
```

The response returns an organization object:

```php
[
    "organizationId" : "67973f72db34592d8fc96c48",
    "name" : "My organization",
    "sharedToUsers": [
        [
            "userId" : "fB6bJr5TbaKLiofns",
            "role" : "administrator",
            "joinDate" : "2016-02-10T14:25:58.745Z"
        ]
    ]
]
```

[Create an organization](https://favro.com/developer/#create-an-organization):

Argument `$attributes` is an array and contains the following values:

| Index | Type | Description |
| --- | --- | --- |
|name|string|The name of the organization.|
|shareToUsers|array|The users who will be invited to the organization. See below for a description of a user share object.|

`shareToUsers` is also an array with the following structure:

| Index | Type | Description |
| --- | --- | --- |
|userId|string|The userId of the existing user. Required if email is not provided.|
|email|string|Email. Required if userId is not provided.|
|role|string|The role of the user in the organization. Refer to [organization roles](https://favro.com/developer/#organization-roles). Required.|
|delete|boolean|Removes user from the organization if value equals to true. Optional.|

```php
$result = $favro->organizations->create($attributes);
```

The response will be the created organization:

```php
[
    "organizationId" : "67973f72db34592d8fc96c48",
    "name" : "My organization",
    "sharedToUsers": [
        [
            "userId" : "fB6bJr5TbaKLiofns",
            "role" : "administrator",
            "joinDate" : "2016-02-10T14:25:58.745Z"
        ]
    ]
]
```

[Update an organization](https://favro.com/developer/#update-an-organization):

Arguments:

| Argument | Type | Description |
| --- | --- | --- |
|organizationId|string|The id of the organization to update.|
|attributes|array|Array of attributes to be updated.|


`attributes` is an array with the following structure:

| Index | Type | Description |
| --- | --- | --- |
|name|string|The name of the organization.|
|members|array|Update user roles in the organization. See below for a description of a user share object.|
|shareToUsers|array|The users who will be invited to the organization. See below for a description of a user share object.|

`shareToUsers` is also an array with the following structure:

| Index | Type | Description |
| --- | --- | --- |
|userId|string|The userId of the existing user. Required if email is not provided.|
|email|string|Email. Required if userId is not provided.|
|role|string|The role of the user in the organization. Refer to [organization roles](https://favro.com/developer/#organization-roles). Required.|
|delete|boolean|Removes user from the organization if value equals to true. Optional.|

```php
$result = $favro->organizations->update($organizationId, $attributes);
```

The response will be the updated organization:

```php
[
        "organizationId" : "67973f72db34592d8fc96c48",
        "name" : "My organization",
        "sharedToUsers": [
            [
                "userId" : "fB6bJr5TbaKLiofns",
                "role" : "administrator",
                "joinDate" : "2016-02-10T14:25:58.745Z"
            ]
        ]
]
```

## Collections

[Get all collections](https://favro.com/developer/#get-all-collections):
```php
$result = $favro->collections->getAll();
```

[Get an collection](https://favro.com/developer/#get-a-collection):
```php
$result = $favro->collections->getById($collectionId);
```

[Create a collection](https://favro.com/developer/#create-a-collection):
```php
$result = $favro->collections->create($attributes);
```

[Update a collection](https://favro.com/developer/#update-a-collection):
```php
$result = $favro->collections->update($collectionId, $attributes);
```

[Delete a collection](https://favro.com/developer/#delete-a-collection):
```php
$result = $favro->collections->delete($collectionId);
```

## Widgets
[Get all widgets](https://favro.com/developer/#get-all-widgets):
```php
$result = $favro->widgets->getAll();
```

[Get a widget](https://favro.com/developer/#get-a-widget):
```php
$result = $favro->widgets->getById($widgetId);
```

[Create a widget](https://favro.com/developer/#create-a-widget):
```php
$result = $favro->widgets->create($attributes); 
```

[Update a widget](https://favro.com/developer/#update-a-widget):
```php
$result = $favro->widgets->update(
```

## How can I thank you?
Why not star the github repo? I'd love the attention!

Thanks! 