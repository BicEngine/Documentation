# Image

_The images component is used to object-represent arbitrary image data_

### Installation <a href="#installation" id="installation"></a>

{% code fullWidth="false" %}
```bash
composer require bic-engine/image
```
{% endcode %}

_If you install this component outside of an application, you must require the `vendor/autoload.php` file in your code to enable the class autoloading mechanism provided by Composer. Read_ [_this article_](https://getcomposer.org/doc/00-intro.md) _for more details._

### Usage

The Image class provides abstraction over arbitrary image data.

```php
use Bic\Image\Compression;
use Bic\Image\Image;
use Bic\Image\PixelFormat;

$image = new Image(
    format: PixelFormat::R8G8B8,        // RGB, 8 bits per channel
    width: 64,                          // 64 pixels width
    height: 64,                         // 64 pixels height
    data: '<arbitrary_binary_data>',    // binary data payload
    compression: Compression::NONE,     // (optional) data compression
    metadata: null,                     // (optional) additional metadata info
);
```

### Pixel Formats

By default, the image component contains several preinstalled popular image formats.

```php
use Bic\Image\PixelFormat;

$rgb24  = PixelFormat::R8G8B8;   // RGB order (no alpha), 8 bits per channel
$bgr24  = PixelFormat::B8G8R8;   // BGR order (no alpha), 8 bits per channel
$rgba32 = PixelFormat::R8G8B8A8; // RGBA order, 8 bits per channel
$bgra32 = PixelFormat::B8G8R8A8; // BGRA order, 8 bits per channel
$abgr32 = PixelFormat::A8B8G8R8; // ABGR order, 8 bits per channel
```

#### Custom Pixel Formats

If you have any specific custom pixel packing format (similar to `VK_FORMAT_R4G4B4A4_UNORM_PACK16` Vulkan format), then you can create a custom format.

```php
use Bic\Image\PixelFormat\ColorInfo;
use Bic\Image\UserPixelFormat;

//
// Simple R4G4B4A4 format 
//
// Red:   4-bits, in interval [0 ... 4]
// Green: 4-bits, in interval [4 ... 8]
// Blue:  4-bits, in interval [8 ...12]
// Alpha: 4-bits, in interval [12...16]
//
$rgba16 = new UserPixelFormat(
    bytes: 2,
    red:   new ColorInfo(mask: 0b11110000_00000000),
    green: new ColorInfo(mask: 0b00001111_00000000),
    blue:  new ColorInfo(mask: 0b00000000_11110000),
    alpha: new ColorInfo(mask: 0b00000000_00001111),
);
```

Using color masks, you can specify which bits are responsible for a specific color. For example, on the [Khronos Vulkan](https://registry.khronos.org/vulkan/site/spec/latest/chapters/formats.html) page there are many more formats supported by the video card and which can be expressed through a similar bit mask.

Please note that the pixel format only applies to unpacked (non-compressed) image data. In particular, if the source image is packed using the DXT and/or BCx (DX10) algorithm (DDS image format), then the pixel format is responsible for the pixel format after decomressing the data into a bitmap.
