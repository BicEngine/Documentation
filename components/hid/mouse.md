# Mouse

> _The mouse component implements an abstraction over the mouse._

## Installation

```bash
composer require bic-engine/mouse
```

{% hint style="info" %}
If you install this component outside an application, you must require the `vendor/autoload.php` file in your code to enable the class autoloading mechanism provided by Composer[^1].

Read [this article](https://getcomposer.org/doc/00-intro.md) for more details.
{% endhint %}

## Usage

The component provides an `Bic\Mouse\ButtonInterface` interface and two implementations: An `Bic\Mouse\Button`enumeration with a predefined set of mouse buttons, and an `Bic\Mouse\UserButton` object, which allows you to implement custom buttons if the button implemented by the driver is not a standard one.

[^1]: A Dependency Manager for PHP
