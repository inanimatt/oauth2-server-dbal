# Doctrine DBAL storage for league/oauth2-server

This is a straight port of the ezcomponents PDO implementation offered with league/oauth2-server 2.x. Implementations were removed in 3.x so I'm offering this here as a stopgap until storage is reimplemented in the upcoming 4.x

## Installation

Add this repository to your `composer.json`:

```json
"repositories": [
    {
        "type": "vcs",
        "url": "https://github.com/inanimatt/oauth2-server-dbal"
    }
],
```

Then require the package:

```json
"require": {
    "inanimatt/oauth2-server-dbal": "~1.0"
}
```

Finally run `composer update inanimatt/oauth2-server-dbal` to download and install it.

If you're not using the composer autoloader, make sure your autoloader supports PSR-4.

## Usage

Make a DBAL connection as usual (you probably already have one to hand):

```php
require __DIR__.'/vendor/autoload.php';

$config = new \Doctrine\DBAL\Configuration();
$connectionParams = [
    'dbname'   => 'mydb',
    'user'     => 'myusername',
    'password' => 'lovesexsecret',
    'host'     => 'localhost',
    'driver'   => 'pdo_mysql',
    'encoding' => 'utf8',
];
$connection = \Doctrine\DBAL\DriverManager::getConnection($connectionParams, $config);
```

Now when you create your Authorization Server or Resource Server, pass in these storage classes with the database connection as an argument:

```php
$resource_server = new \League\OAuth2\Server\Resource(
    new \Inanimatt\OAuth2\Server\Storage\DBAL\Session($connection)
);

$authorization_server = new \League\OAuth2\Server\Authorization(
    new \Inanimatt\OAuth2\Server\Storage\DBAL\Client($connection),
    new \Inanimatt\OAuth2\Server\Storage\DBAL\Session($connection),
    new \Inanimatt\OAuth2\Server\Storage\DBAL\Scope($connection)
);
```

