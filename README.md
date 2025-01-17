# Laravel HTTP Tests Generator

[![Latest Version on Packagist][ico-version]][link-packagist]
[![Software License][ico-license]](LICENSE.md)
[![Total Downloads][ico-downloads]][link-downloads]

**Generate http tests by clicking through your Laravel app**

Just execute the command to record your actions as http tests

``` bash
php artisan autohttptest:create

```

![imagen](https://user-images.githubusercontent.com/4065733/31252701-a10f4580-a9e7-11e7-8b83-92cfc4b962f3.png)


The command will **intercept** your requests and translate the response as a test.


When finished, your test will be saved in tests/Feature/

## Demo in video

![autohttptests-gif](https://user-images.githubusercontent.com/4065733/88353656-fcf9cc00-cd23-11ea-86b3-6c096378f540.gif)

**What does it test?**

- Request acting as same user
- Make request using the same verb (GET,PUT,POST) with same arguments
- Assert http response code
- Assert errors
- Assert redirection

## Example code


```
<?php

namespace Tests\Feature;
use Tests\TestCase;

class SomethingTest extends TestCase
{
    public function testAutoHttpTest()
    {
        $this
        ->actingAs(\App\Models\User::find(1))
        ->post('home/something', [
            'name'         => 'a',
            'lastname'     => 'a',
            'city'         => '',
            'hobbies'   => '',
            'twitter_username' => 'a',
        ])
        ->assertStatus(302)
        ->assertSessionHasErrors([
            'name',
            'country_id',
            'twitter_username',
        ]);

        $this
        ->actingAs(\App\Models\User::find(1))
        ->post('home/something', [
            'name'              => 'asdfa',
            'lastname'          => 'asdfa',
            'country_id' => '1',
            'city'              => '',
            'hobbies'        => '',
            'twitter_username'      => 'asdfa',
        ])
            ->assertStatus(302)
            ->assertRedirect('home/something');
    }
}
```

**Note**

Here we capture an unsuccessful post, with errors.
Then, a **successful** post with redirection

## Install

Via Composer

``` bash
$ composer require eduardoarandah/autohttptests
```

If you are using laravel < 5.4 add to providers in config/app.php

```
EduardoArandaH\AutoHttpTests\AutoHttpTestsServiceProvider::class,
```

## Usage

``` bash
php artisan autohttptest:create

```

## How does it work?

when you run `php artisan autohttptest:create yourtest` it intercepts all requests and responses through a middleware. 

The request is then analyzed and transformed into a test in a file `storage/autohttptests.txt`

If you're curious, you can see the building of that file in real time with

```
tail -f storage/autohttptests.txt
```

## Credits

- [Eduardo Aranda Hernandez][link-author]

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.

[ico-version]: https://img.shields.io/packagist/v/eduardoarandah/autohttptests.svg?style=flat-square
[ico-license]: https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square
[ico-travis]: https://img.shields.io/travis/eduardoarandah/autohttptests/master.svg?style=flat-square
[ico-scrutinizer]: https://img.shields.io/scrutinizer/coverage/g/eduardoarandah/autohttptests.svg?style=flat-square
[ico-code-quality]: https://img.shields.io/scrutinizer/g/eduardoarandah/autohttptests.svg?style=flat-square
[ico-downloads]: https://img.shields.io/packagist/dt/eduardoarandah/autohttptests.svg?style=flat-square

[link-packagist]: https://packagist.org/packages/eduardoarandah/autohttptests
[link-travis]: https://travis-ci.org/eduardoarandah/autohttptests
[link-scrutinizer]: https://scrutinizer-ci.com/g/eduardoarandah/autohttptests/code-structure
[link-code-quality]: https://scrutinizer-ci.com/g/eduardoarandah/autohttptests
[link-downloads]: https://packagist.org/packages/eduardoarandah/autohttptests
[link-author]: https://github.com/eduardoarandah
[link-contributors]: ../../contributors
