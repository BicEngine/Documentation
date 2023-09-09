# Image Factory

> _The image factory component is used to creating images from arbitrary source payload._

## Installation

```bash
composer require bic-engine/image-factory
```

{% hint style="info" %}
If you install this component outside an application, you must require the `vendor/autoload.php` file in your code to enable the class autoloading mechanism provided by Composer[^1].

Read [this article](https://getcomposer.org/doc/00-intro.md) for more details.
{% endhint %}

## Usage

The `Bic\Image\Factory\Factory` object allows you to create images from various sources. To correctly read such information, you must add at least one image format decoder.

```php
use Bic\Image\Factory\Factory;

$factory = new Factory([ new ExampleImageDecoder() ]);
```

Besides specifying the decoder explicitly in the constructor, you can add them later.

```php
$factory->extend(new ExampleImageDecoder());
```

After creating the factory, you can use the appropriate methods to create images.

{% hint style="warning" %}
Please note that each method returns a collection of images (`iterable<ImageInterface>`), because source can contain several images, for example, `*.ico`, `*.dds`, `*.gif`, etc. formats.
{% endhint %}

### File Source

One of the most obvious options is to create an images object from an existing file. To do this, simply call the  `createImageFromFile` method on the factory object, passing there the full path to the file.

```php
$images = $factory->createImageFromFile(__DIR__ . '/path/to/file.jpg');

foreach ($images as $image) {
    var_dump($image);
}
```

In addition to the result, this method can return the following exceptions:

* `Bic\Image\Factory\Exception\FileNonReadableException` — In case of file cannot be read (for example, there is not enough permission to read the file).
* `Bic\Image\Factory\Exception\FileNotFoundException` — In case of file not found.
* `Bic\Image\Factory\Exception\NotDecodableException` — In case of image cannot be decoded (for example, if there is no corresponding decoder that can read a given file format).

{% hint style="danger" %}
Please note that some low-level decorders may not support [PHP Protocols](https://www.php.net/manual/en/wrappers.php) such as `http://` or `memory://`, so their use is **not recommended**.
{% endhint %}

### Streaming Source

If you are using streaming data, you should use the `createImageFromResource` method.

<pre class="language-php"><code class="lang-php"><strong>$stream = \fopen(__DIR__ . '/path/to/file.jpg', 'rb');
</strong>
$images = $factory->createImageFromResource($stream);

foreach ($images as $image) {
    var_dump($image);
}

\fclose($stream);
</code></pre>

{% hint style="danger" %}
This method does **not close** and **not rewind** the stream after reading is completed. You will need to use the manual `rewind()` and `fclose()` functions.
{% endhint %}

{% hint style="warning" %}
This method changes the state of the passed argument
{% endhint %}

In addition to the result, this method can return the following exceptions:

* `Bic\Image\Factory\Exception\NonReadableException` — In case of stream cannot be read.
* `Bic\Image\Factory\Exception\NonSeekableException` — In case of offset of the stream cannot be received.
* `Bic\Image\Factory\Exception\NotDecodableException` — In case of image cannot be decoded (for example, if there is no corresponding decoder that can read a given format).

### Binary String Source

If you are using builtin string data, you should use the `createImageFromString` method.

```php
$images = $factory->createImageFromString(<<<'DATA'
    \153\154\155\052\251\124\153\154\155\052\251\124
    DATA);

foreach ($images as $image) {
    var_dump($image);
}
```

In addition to the result, this method can return the following exceptions:

* `Bic\Image\Factory\Exception\NotDecodableException` — In case of image cannot be decoded (for example, if there is no corresponding decoder that can read a given format).

[^1]: A Dependency Manager for PHP
