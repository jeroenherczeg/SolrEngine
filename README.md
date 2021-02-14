# Solr Driver for Laravel Scout with support for filters and facets ('where' and 'select distinct row, count(*)')

<p align="center"><img src="http://lucene.apache.org/solr/assets/identity/Solr_Logo_on_white.png" width="200px"><br><br></p>

[![Latest Version on Packagist][ico-version]][link-packagist]
[![Software License][ico-license]](LICENSE.md)
[![Build Status][ico-travis]][link-travis]
[![Coverage Status][ico-scrutinizer]][link-scrutinizer]
[![Quality Score][ico-code-quality]][link-code-quality]
[![Total Downloads][ico-downloads]][link-downloads]
[![Laravel Scout][ico-laravel-scout]][link-laravel-scout]
[![Apache Solr][ico-solr]][link-solr]
[![PHP][ico-php]][link-php]

## Documentation

You can read the documentation [here](https://solr-driver-for-laravel-scout.readthedocs.io/en/latest/).

## Problems, questions or comments?

If you have **any** problems, questions or comments, feel free to submit an [issue](link-issue) and I will reply to you as soon as possible.


## Prerequisites

Install [Laravel Scout](https://laravel.com/docs/8.x/scout).

## Dependencies

We use konekt/concord, a package manager, to override the \Laravel\Scout\Builder in SolrProvider.php

## Install

Install via Composer

``` bash
$ composer require iateadonut/laravel-scout-solr
```

Set your SCOUT_DRIVER to solr:

```
// .env

...

SCOUT_DRIVER=solr
```


You must add the Scout service provider and the Solr engine service provider in your app.php config:

```
// config/app.php

'providers' => [
    ...
        /*
         * Package Service Providers...
         */
        Laravel\Scout\ScoutServiceProvider::class,
        ScoutEngines\Solr\SolrProvider::class,
],
```

Publish the config file:

```php artisan vendor:publish --provider="Laravel\Scout\ScoutServiceProvider"```


Add the Solr configuration to the scout config file:

```php
// config/scout.php

...

    /*
    |--------------------------------------------------------------------------
    | Solr Configuration
    |--------------------------------------------------------------------------
    |
    | Here you may configure your Solr settings. Solr is the popular, blazing
    | -fast, open source enterprise search platform built on Apache Lucene.
    | If necessary, you can override the configuration in your .env file.
    |
    */

    'solr' => [
        'host' => env('SOLR_HOST', '127.0.0.1'),
        'port' => env('SOLR_PORT', '8983'),
        'path' => env('SOLR_PATH', '/solr/'),
        'core' => env('SOLR_CORE', 'scout'),
    ],
```

## Usage

### to index

```php artisan scout:import "App\Users"```

Now you can use Laravel Scout as described in the [official documentation](https://laravel.com/docs/5.7/scout)

## Extended Usage

### filter([array])

the ->where() of solr

distinct filters are separated by AND while attributes within  a filter are separated by OR

e.g.

````->filter('category', [1,2])```` - this would yield where category in (1,2)

````->filter('category', [1])->filter('category', [2])```` - this would yield where category in (1) AND category in (2)

### facet()

facets will give you lists of a field name and the number of results for each distinct value.

e.g. if you have a table with the column 'fat' with the distinct values 1 and 0:

->facet('fat')

will calculate how many rows have a value of 1 and how many rows have a value of 0

### getFacets()

returns an array of values and the number of times they occur in the result set.

````$facets = $model->search('term')->facet('column')->getFacets();````

## Using Solr with Laravel Homestead

You can install Solr within your Homestead virtual machine.

Add the port forwarding to your Homestead.yaml

```
// ~/Homestead/Homestead.yaml

...

ports:
    - send: 18983
      to: 8983
      
...
```

Add the following install steps to your Homestead after.sh script.

```
// ~/Homestead/after.sh

#!/bin/sh

# If you would like to do some extra provisioning you may
# add any commands you wish to this file and they will
# be run after the Homestead machine is provisioned.
#
# If you have user-specific configurations you would like
# to apply, you may also create user-customizations.sh,
# which will be run after this script.

# Install Java Runtime Enviroment
sudo apt-get update
sudo apt-get install default-jre -y

# Install Solr 7.5
wget http://www-eu.apache.org/dist/lucene/solr/7.5.0/solr-7.5.0.tgz
tar zxf solr-7.5.0.tgz
cd solr-7.5.0
bin/solr create -c scout
bin/solr start

```

You will need to recreate your the virtual machine.

```
vagrant destroy && vagrant up
```

Once the virtual machine is installed and running, you can access Solr admin on http://127.0.0.1:18983/solr/#/ .

## Change log

Please see [CHANGELOG](CHANGELOG.md) for more information on what has changed recently.

## Testing

``` bash
$ composer test
```

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) and [CODE_OF_CONDUCT](CODE_OF_CONDUCT.md) for details.

## Security

If you discover any security related issues, please email jeroen@herczeg.be instead of using the issue tracker.

## Credits

- [Jeroen Herczeg][link-author]
- [solariumphp/solarium](https://github.com/solariumphp/solarium)
- [All Contributors][link-contributors]

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.

[ico-version]: https://img.shields.io/packagist/v/jeroenherczeg/laravel-scout-solr.svg?style=flat-square
[ico-license]: https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square
[ico-travis]: https://img.shields.io/travis/jeroenherczeg/laravel-scout-solr/master.svg?style=flat-square
[ico-scrutinizer]: https://img.shields.io/scrutinizer/coverage/g/jeroenherczeg/laravel-scout-solr.svg?style=flat-square
[ico-code-quality]: https://img.shields.io/scrutinizer/g/jeroenherczeg/laravel-scout-solr.svg?style=flat-square
[ico-downloads]: https://img.shields.io/packagist/dt/jeroenherczeg/laravel-scout-solr.svg?style=flat-square
[ico-laravel-scout]: https://img.shields.io/badge/laravel%20scout-v5-blue.svg?style=flat-square
[ico-solr]: https://img.shields.io/badge/apache%20solr-7.5-blue.svg?style=flat-square
[ico-php]: https://img.shields.io/badge/php-7-blue.svg?style=flat-square

[link-packagist]: https://packagist.org/packages/jeroenherczeg/laravel-scout-solr
[link-travis]: https://travis-ci.org/jeroenherczeg/laravel-scout-solr
[link-scrutinizer]: https://scrutinizer-ci.com/g/jeroenherczeg/laravel-scout-solr/code-structure
[link-code-quality]: https://scrutinizer-ci.com/g/jeroenherczeg/laravel-scout-solr
[link-downloads]: https://packagist.org/packages/jeroenherczeg/laravel-scout-solr
[link-author]: https://github.com/jeroenherczeg
[link-contributors]: ../../contributors
[link-laravel-scout]: https://laravel.com/docs/5.7/scout
[link-solr]: http://lucene.apache.org/solr/
[link-php]: http://php.net/
[link-issues]: https://github.com/jeroenherczeg/laravel-scout-solr/issues
