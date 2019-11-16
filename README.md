# laravel-nested-attributes

[![Latest Version on Packagist][ico-version]][link-packagist]
[![Software License][ico-license]](LICENSE.md)
[![Build Status][ico-travis]][link-travis]
[![Total Downloads][ico-downloads]][link-downloads]

Nested attributes allow you to save attributes on associated records through the parent. By default nested attribute updating is turned off and you can enable it using the $nested attribute. When you enable nested attributes an attribute writer is defined on the model.

## Structure

If any of the following are applicable to your project, then the directory structure should follow industry best practises by being named the following.

```
src/
tests/
vendor/
```


## Install

Via Composer

``` bash
$ composer require thomisticus/laravel-nested-attributes
```

## Usage

``` php
namespace App;

use Thomisticus\NestedAttributes\Model;

class Post extends Model
{
    protected $fillable = ['title'];
    
    protected $nested = ['option', 'comments'];

    public function option() {
        //it can be also morphOne
        return $this->hasOne('App\Option');
    }

    public function comments() {
        //it can be also morphMany
        return $this->hasMany('App\Comment');
    }
}
```

or

``` php
namespace App;

use Illuminate\Database\Eloquent\Model;
use Thomisticus\NestedAttributes\Traits\HasNestedAttributesTrait;

class Post extends Model
{
    use HasNestedAttributesTrait;

    ...
}
```

usage:

``` php
\App\Post::create([
    'title' => 'Some text',

    'option' => [
        'info' => 'some info'
    ],

    'comments' => [
        [
            'text' => 'Comment 1'
        ], [
            'text' => 'Comment 2'
        ],
    ]
]);


\App\Post::findOrFail(1)->update([
    'title' => 'Better text',

    'option' => [
        'info' => 'better info'
    ],

    'comments' => [
        [
            'id' => 2,
            'text' => 'Comment 2'
        ],
    ]
]);
```

to delete nested row you should pass `_destroy` attribute:

``` php
\App\Post::findOrFail(1)->update([
    'title' => 'Better text',

    'option' => [
        'info' => 'better info'
    ],

    'comments' => [
        [
            'id' => 2,
            '_destroy' => true
        ],
    ]
]);
```


## Change log

Please see [CHANGELOG](CHANGELOG.md) for more information on what has changed recently.

## Testing

``` bash
$ composer test
```

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) and [CODE_OF_CONDUCT](CODE_OF_CONDUCT.md) for details.

## Security

If you discover any security related issues, please email igor.sgm@gmail.com instead of using the issue tracker.

## Credits

- [Piotr Krajewski][link-author]
- [All Contributors][link-contributors]

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.

[ico-version]: https://img.shields.io/packagist/v/thomisticus/laravel-nested-attributes.svg?style=flat-square
[ico-license]: https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square
[ico-travis]: https://img.shields.io/travis/thomisticus/laravel-nested-attributes/master.svg?style=flat-square
[ico-downloads]: https://img.shields.io/packagist/dt/thomisticus/laravel-nested-attributes.svg?style=flat-square

[link-packagist]: https://packagist.org/packages/thomisticus/laravel-nested-attributes
[link-travis]: https://travis-ci.org/thomisticus/laravel-nested-attributes
[link-downloads]: https://packagist.org/packages/thomisticus/laravel-nested-attributes
[link-author]: https://github.com/thomisticus
[link-contributors]: ../../contributors
