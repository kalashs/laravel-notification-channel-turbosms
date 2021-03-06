# Turbo SMS notifications channel for Laravel 5.3+
Based on [github.com/laravel-notification-channels/smsc-ru](https://github.com/laravel-notification-channels/smsc-ru)

This package made for send notifications using [turbosms.ua](https://turbosms.ua/) with Laravel 5.3+.

## Contents

- [Installation](#installation)
    - [Setting up the TurboSms service](#setting-up-the-TurboSms-service)
- [Usage](#usage)
    - [Available Message methods](#available-message-methods)
- [Changelog](#changelog)
- [Security](#security)
- [Contributing](#contributing)
- [Credits](#credits)
- [License](#license)


## Installation

You can install the package via composer:
```composer require yakimka/laravel-notification-channel-turbosms```

For Laravel < 5.5 you must install the service provider:
```php
// config/app.php
'providers' => [
    ...
    NotificationChannels\TurboSms\TurboSmsServiceProvider::class,
],
```

### Setting up the TurboSms service

Add your TurboSms login, secret key (hashed password) and default sender name (or phone number) to your `config/services.php`:

```php
// config/services.php
...
'turbosms' => [
    'login' => env('TURBOSMS_LOGIN'),
    'secret' => env('TURBOSMS_SECRET'),
    'sender' => 'John Doe',
    'url' => 'http://turbosms.in.ua/api/wsdl.html',
],
...
```

## Usage

You can use the channel in your `via()` method inside the notification:

```php
use Illuminate\Notifications\Notification;
use NotificationChannels\TurboSms\TurboSmsMessage;
use NotificationChannels\TurboSms\TurboSmsChannel;

class AccountApproved extends Notification
{
    public function via($notifiable)
    {
        return [TurboSmsChannel::class];
    }

    public function toTurboSms($notifiable)
    {
        return TurboSmsMessage::create("Task #{$notifiable->id} is complete!");
    }
}
```

In your notifiable model, make sure to include a routeNotificationForTurboSms() method, which return the phone number.

```php
public function routeNotificationForTurboSms()
{
    return $this->phone;
}
```

### Available methods

`from()`: Sets the sender's name or phone number.

`content()`: Sets a content of the notification message.

## Security

If you discover any security related issues, please email ss.yakim@gmail.com instead of using the issue tracker.

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

## Credits

- [yakimka](https://github.com/yakimka)
- [JhaoDa](https://github.com/jhaoda)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
