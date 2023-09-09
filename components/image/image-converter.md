# Image Converter

> _The image converter component is used to convert image pixel formats._

## Installation

```bash
composer require bic-engine/image-converter
```

{% hint style="info" %}
If you install this component outside an application, you must require the `vendor/autoload.php` file in your code to enable the class autoloading mechanism provided by Composer[^1].

Read [this article](https://getcomposer.org/doc/00-intro.md) for more details.
{% endhint %}

## Usage

The converter object provides method `convert(ImageInterface, PixelFormatInterface): ImageInterface` for converting images.

To create a converter, you must create a corresponding object. The package provides the following converter drivers:

* `Bic\Image\Converter\SoftwareConverter` — software converter implementation that works on any device.

```php
use Bic\Image\Converter\SoftwareConverter;
use Bic\Image\Image;
use Bic\Image\PixelFormat;

$converter = new SoftwareConverter();

$input = new Image( ... );
$output = $converter->convert($input, PixelFormat::B8G8R8);

var_dump($input->getFormat()); // Input Image Format
var_dump($image->getFormat()); // Output Image Format
```

{% hint style="info" %}
Please note that any image conversion that reduces the bit depth of the color format irreversibly loses color data info.

For example, this way the conversion from `B8G8R8A8` to `R8G8B8` loses information about the alpha channel.
{% endhint %}

[^1]: A Dependency Manager for PHP
