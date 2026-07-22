# Laravel Snowflake

**This package has been merged into https://github.com/bradietilley/php-snowflake**

---

A Laravel wrapper of [PHP Snowflake](https://github.com/bradietilley/php-snowflake) to provide a simple, opinionated implementation of Snowflake IDs for your eloquent models.

![Static Analysis](https://github.com/bradietilley/laravel-snowflake/actions/workflows/static.yml/badge.svg)
![Tests](https://github.com/bradietilley/laravel-snowflake/actions/workflows/tests.yml/badge.svg)
![Laravel Version](https://img.shields.io/badge/Laravel%20Version-11.x-F9322C)
![PHP Version](https://img.shields.io/badge/PHP%20Version-%E2%89%A58.4-4F5B93)

## Introduction

The laravel-snowflake package provides a seamless way to generate unique, time-ordered, distributed IDs within your Laravel application. Unlike traditional auto-incrementing IDs, Snowflake IDs offer several advantages: they eliminate ID collisions across distributed systems, enhance horizontal scalability, and prevent predictable sequential exposure of database records. Compared to UUIDs and ULIDs, Snowflake IDs are shorter, visually appealing, and easier to write out, making them ideal for public identifiers. This package integrates smoothly with Laravel, offering microsecond precision while ensuring high-performance, conflict-free ID generation across multiple clusters and workers. 🚀

## Installation

**This package has been merged into https://github.com/bradietilley/php-snowflake**

```
composer require bradietilley/laravel-snowflake
```

## Documentation

**Preparing your schema:**

You'll want to make sure that your model's primary key does not autoincrement. Autoincrement is automatically added when you use `$table->id();` so go ahead and switch this out:

```diff
-$table->id();
+$table->bigInteger('id')->unsigned()->primary();
```

**Integrating with you models:**

Next you'll want to add the `HasSnowflake` trait to your models. This trait this will handle all aspects of a snowflake ID including:

- Automatically setting the `id` to a Snowflake ID
- Configuring the cast for `id` to `string`
- Disabling `increments` on the model
- Configuring the `keyType` to `string`

This can be done as simple as:

```php
use BradieTilley\Snowflakes\Eloquent\HasSnowflake;

class SomeModel extends Model
{
    use HasSnowflake;
}
```

You're all set.

```php
$model = SomeModel::create();
$model->id; // 9348975348573485734
```

### Concurrency

The package includes a `LaravelSequenceResolver` for PHP Snowflake, which utilizes a cache repository to manage the generation of multiple concurrent IDs within the same microsecond.

You can configure various cache-related options, such as:

- The cache store (`snowflakes.sequencing.store`)
- Cache prefix (`snowflakes.sequencing.prefix`)
- Cache lock expiry (`snowflakes.sequencing.expiry`)
- Cache lock wait time (`snowflakes.sequencing.wait`)

These settings allow for proper handling of concurrent ID generation in distributed environments.

### Testing

In unit testing scenarios, you may want to generate sequential, predictable IDs, similar to traditional auto-incrementing IDs.

By enabling the `snowflakes.testing` configuration setting, the standard `SnowflakeIdentifierResolver` is automatically swapped with a `SequentialIdentifierResolver`. This generates IDs that are realistic in length and follow a standard auto-incrementing pattern.

When in testing mode, Snowflake IDs can be grouped using the `$group` argument in the `BradieTilley\Snowflakes\SnowflakeGenerator::make()->id($group)` or `BradieTilley\Snowflake\Snowflake::id($group)` methods. The `$group` is automatically set to the respective model class name.

For example, both `Product::create()` and `User::create()` generate an ID of `9000000000000000001`, then `9000000000000000002`, then `9000000000000000003` and so-on.


## Author

- [Bradie Tilley](https://github.com/bradietilley)
