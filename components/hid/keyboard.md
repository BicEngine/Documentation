# Keyboard

> _The keyboard component implements an abstraction over the keyboard._

## Installation

```bash
composer require bic-engine/keyboard
```

{% hint style="info" %}
If you install this component outside an application, you must require the `vendor/autoload.php` file in your code to enable the class autoloading mechanism provided by Composer[^1].

Read [this article](https://getcomposer.org/doc/00-intro.md) for more details.
{% endhint %}

## Usage

The component provides an `Bic\Keyboard\KeyInterface` interface and two implementations: An `Bic\Keyboard\Key` enumeration with a predefined set of keys, and an `Bic\Keyboard\UserKey` object, which allows you to implement custom keys if the key implemented by the driver is not a standard one.

### Preset Keys

Below is a list of preset keys:

* `Key::SPACE`
* `Key::APOSTROPHE`
* `Key::COMMA`
* `Key::MINUS`
* `Key::PERIOD`
* `Key::SLASH`
* `Key::KEY_0`
* `Key::KEY_1`
* `Key::KEY_2`
* `Key::KEY_3`
* `Key::KEY_4`
* `Key::KEY_5`
* `Key::KEY_6`
* `Key::KEY_7`
* `Key::KEY_8`
* `Key::KEY_9`
* `Key::SEMICOLON`
* `Key::EQUAL`
* `Key::A`
* `Key::B`
* `Key::C`
* `Key::D`
* `Key::E`
* `Key::F`
* `Key::G`
* `Key::H`
* `Key::I`
* `Key::J`
* `Key::K`
* `Key::L`
* `Key::M`
* `Key::N`
* `Key::O`
* `Key::P`
* `Key::Q`
* `Key::R`
* `Key::S`
* `Key::T`
* `Key::U`
* `Key::V`
* `Key::W`
* `Key::X`
* `Key::Y`
* `Key::Z`
* `Key::LEFT_BRACKET`
* `Key::BACKSLASH`
* `Key::RIGHT_BRACKET`
* `Key::GRAVE_ACCENT`
* `Key::ESCAPE`
* `Key::ENTER`
* `Key::TAB`
* `Key::BACKSPACE`
* `Key::INSERT`
* `Key::DELETE`
* `Key::RIGHT`
* `Key::LEFT`
* `Key::DOWN`
* `Key::UP`
* `Key::PAGE_UP`
* `Key::PAGE_DOWN`
* `Key::HOME`
* `Key::END`
* `Key::CAPS_LOCK`
* `Key::SCROLL_LOCK`
* `Key::NUM_LOCK`
* `Key::PRINT_SCREEN`
* `Key::PAUSE`
* `Key::F1`
* `Key::F2`
* `Key::F3`
* `Key::F4`
* `Key::F5`
* `Key::F6`
* `Key::F7`
* `Key::F8`
* `Key::F9`
* `Key::F10`
* `Key::F11`
* `Key::F12`
* `Key::F13`
* `Key::F14`
* `Key::F15`
* `Key::KP_0`
* `Key::KP_1`
* `Key::KP_2`
* `Key::KP_3`
* `Key::KP_4`
* `Key::KP_5`
* `Key::KP_6`
* `Key::KP_7`
* `Key::KP_8`
* `Key::KP_9`
* `Key::KP_DECIMAL`
* `Key::KP_DIVIDE`
* `Key::KP_MULTIPLY`
* `Key::KP_SUBTRACT`
* `Key::KP_ADD`
* `Key::KP_ENTER`
* `Key::KP_EQUAL`
* `Key::LEFT_SHIFT`
* `Key::LEFT_CONTROL`
* `Key::LEFT_ALT`
* `Key::LEFT_SUPER`
* `Key::RIGHT_SHIFT`
* `Key::RIGHT_CONTROL`
* `Key::RIGHT_ALT`
* `Key::RIGHT_SUPER`
* `Key::MENU`

### Custom Keys

To create a custom key object you should use the `create()` method.

```php
use Bic\Keyboard\Key;

$key = Key::create(0xDEAD_BEEF);
```

{% hint style="info" %}
Every object created using this method will return the same instance every time it is called.

```php
use Bic\Keyboard\Key;

$key1 = Key::create(0xDEAD_BEEF);
$key2 = Key::create(0xDEAD_BEEF);

var_dump($key1 === $key2);
// bool(true)
```
{% endhint %}

### Key Modifiers

Below is a list of key modifiers (`Bic\Keyboard\Modifier`) that can be expressed as a bitmask.

* `Modifier::NONE`
* `Modifier::NUM_LOCK`
* `Modifier::SHIFT`
* `Modifier::CONTROL`
* `Modifier::ALT`
* `Modifier::SUPER`
* `Modifier::CAPS_LOCK`
* `Modifier::MODE`
* `Modifier::SCROLL_LOCK`

Thus, for example, the keyboard shortcut `Ctrl + Alt + Del` will be expressed as:

```php
use Bic\Keyboard\Key;
use Bic\Keyboard\Modifier;

$key = Key::DELETE;
$modifiers = Modifier::CONTROL | Modifier::ALT;
```

## Events

The keyboard component provides the following set of events.

```php
use Bic\Keyboard\Key;
use Bic\Keyboard\Modifier;
use Bic\Keyboard\Event\KeyDownEvent;

$down = new KeyDownEvent(
    target: $context,
    key: Key::DELETE,
    modifiers: Modifier::CONTROL | Modifier::ALT,
);
```

```php
use Bic\Keyboard\Key;
use Bic\Keyboard\Modifier;
use Bic\Keyboard\Event\KeyUpEvent;

$up = new KeyUpEvent(
    target: $context,
    key: Key::DELETE,
    modifiers: Modifier::CONTROL | Modifier::ALT,
);
```

### Attributes

If you want to subscribe to events, then you can use the corresponding [event component](keyboard.md#events).

To delegate events to methods, you can use the attributes provided by this package.

```php
use Bic\Keyboard\Key;
use Bic\Keyboard\Event\KeyDownEvent;
use Bic\Keyboard\Attribute\OnKeyDownEvent;

#[OnKeyDownEvent(key: Key::LEFT)]
public function handle(KeyDownEvent $key): void
{
    echo 'Key "left" has been pressed';
}
```

Below is a list of attributes:

* `Bic\Keyboard\Attribute\OnKeyDownEvent` — subscribes an "annotated" handler to a key down event.
  * Contains an optional `key: Bic\Keyboard\KeyInterface` argument if a specific key should be subscribed to.
* `Bic\Keyboard\Attribute\OnKeyUpEvent` — subscribes an "annotated" handler to a key up event.
  * Contains an optional `key: Bic\Keyboard\KeyInterface` argument if a specific key should be subscribed to.

[^1]: A Dependency Manager for PHP
