# CodeIgniter RESTful Server

[![StyleCI](https://github.styleci.io/repos/230589/shield?branch=master)](https://github.styleci.io/repos/230589)

RESTful server implementation for CodeIgniter

## Requirements

- PHP 7.2 or greater
- CodeIgniter 3.1.11+

## Installation

```sh
composer require mlabate/restful-codeigniter
```

## Usage

Note that you will need to copy `rest.php` to `config` directory (e.g. `application/config`)

Step 1: Add this to your controller (should be before any of your code)

```php
use mlabate\RestServer\RestController;
```

Step 2: Extend your controller

```php
class Example extends RestController
```

## Basic GET example

The following sample controller (saved as `Api.php`) can be called in two ways:
<>
* `http://my-domain/api/users/` will return the list of all users
* `http://my-domain/api/users/id/1` will only return information about the user with id = 1

```php
<?php
defined('BASEPATH') OR exit('No direct script access allowed');

use mlabate\RestServer\RestController;

class Api extends RestController {

    function __construct()
    {
        // Construct the parent class
        parent::__construct();
    }

    public function users_get()
    {
        // Users from a data store e.g. database
        $users = [
            ['id' => 0, 'name' => 'John', 'email' => 'john@example.com'],
            ['id' => 1, 'name' => 'Jim', 'email' => 'jim@example.com'],
        ];

        $id = $this->get( 'id' );

        if ( $id === null )
        {
            // Check if the users data store contains users
            if ( $users )
            {
                // Set the response and exit
                $this->response( $users, 200 );
            }
            else
            {
                // Set the response and exit
                $this->response( [
                    'status' => false,
                    'message' => 'No users were found'
                ], 404 );
            }
        }
        else
        {
            if ( array_key_exists( $id, $users ) )
            {
                $this->response( $users[$id], 200 );
            }
            else
            {
                $this->response( [
                    'status' => false,
                    'message' => 'No such user found'
                ], 404 );
            }
        }
    }
}
```
