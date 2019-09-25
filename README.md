# ![rapidmail Logo](https://avatars0.githubusercontent.com/u/25850436?v=3&s=50 "rapidmail Logo") rapidmail APIv3 client

API client written in PHP providing access to the current version of the [rapidmail](https://www.rapidmail.de) API.

[![Build Status](https://travis-ci.org/rapidmail/rapidmail-apiv3-client-php.svg?branch=master)](https://travis-ci.org/rapidmail/rapidmail-apiv3-client-php)
[![Latest Stable Version](https://poser.pugx.org/rapidmail/rapidmail-apiv3-client-php/v/stable)](https://packagist.org/packages/rapidmail/rapidmail-apiv3-client-php)
## Installation

### With Composer

Preferred installation method is to use the [Composer](https://getcomposer.org) dependency manager. 

```bash
composer require rapidmail/rapidmail-apiv3-client-php
```

### From Zip File 

Download [rapidmail-apiv3-client-php.zip](/rapidmail/rapidmail-apiv3-client-php/releases/latest/download/rapidmail-apiv3-client-php.zip) from releases section and extract it into an appropriate subdirectory within your project.

Be aware that this method is most likely to cause problems when used in conjunction with [Composer](https://getcomposer.org) or an existing installation of [Guzzle](http://docs.guzzlephp.org), so it's strongly recommended to choose the preferred installation method above.

## Getting started

Create a new API client instance and provide your APIv3 credentials:

```php
require_once __DIR__ . '/vendor/autoload.php';

use Rapidmail\ApiClient\Client;

$client = new Client('api_username_hash', 'api_password_hash');
```

After that you can access various services encapsulated within the client:

```php
$mailingService = $client->mailings();

// Iterate all mailings 

foreach($mailingService->query() as $mailing) {
    var_dump($mailing);
}
```

## Examples

### Retrieve mailings

Get a list of your mailings with some filters applied:

```php
// Filter for sent mailings newer than a given date 

var_dump(
    $mailingService->query([
        'created_since' => '2019-09-01 10:22:00',
        'status' => 'sent'
    ])
);
```

### Retrieve recipient lists

```php
$listService = $client->recipientlists();

foreach ($listService->query() as $list) {
    var_dump($list);
}
```

### Retrieve recipients
```php
$recipientsService = $client->recipients();

$collection = $recipientsService->query(
    [
        'recipientlist_id' => 123456789 // Recipientlist ID MUST be provided
    ]
);

foreach ($collection as $recipient) {
    var_dump($recipient);
}
```

### Create a new recipient
```php
$recipientsService = $client->recipients();

var_dump(
    $recipientsService->create(
        // Dataset: Represents the recipient dataset you're creating
        [
            'recipientlist_id' => 123456789, // Required
            'email' => 'john@example.net', // Required
            'firstname' => 'John',
            'lastname' => 'Doe',
            'gender' => 'male'
        ],
        // Flags: Configures system behavior, like sending activationmails
        [
            'send_activationmail' => 'yes'
        ]
    )
);
```

## Error handling 

Always keep in mind to handle errors properly and catch exceptions that might occur: 

```php
use \Rapidmail\ApiClient\Exception\ApiClientException;

try {
    $mailingService->query(['status' => 'unknown']);
} catch (ApiClientException $e) {
    // Catch API client exceptions
    echo "Exception raised: " . $e->getMessage();
}
```

## Documentation

More information about using the API client can be found in the following subsections: 

* [Available services](/docs/available-services.md)
* [Typed parameters](/docs/typed-parameters.md)


Also refer to the [API documentation](https://developer.rapidmail.wiki/documentation.html) for a complete list of the available endpoints and their parameters.

## License

rapidmail APIv3 client is licensed under the terms of the [BSD 2-clause](LICENSE) license.