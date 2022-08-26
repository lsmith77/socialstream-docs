# Configuration



## Configuring Socialstream

On installation, Socialstream will publish a new `socialstream.php` configuration file. This config file can be found in your applications `config` directory:

```
~/Sites/blog/config/socialstream.php
```

Inside this configuration file you may define the application middleware Socialstreams authentication routes exist behind and specify which Socialite Providers you wish to enable in your application.

{% hint style="success" %}
You may also enable various Socialstream features inside this config file, such as the remember sessions, provider avatars and generating missing emails. You can find out more by navigating to each feature using the navigation to the left of this page.
{% endhint %}

### Middleware

By default only the `web` middleware route is needed. This should suffice for the majority of use-cases, but if you find yourself needing to add specific middleware logic to the exposed routes (such as headers, rate limiting etc.), you may do so by first defining the middleware in your applications Http Kernel, then adding it here.

### Providers

The `providers` config option allows you to specify the OAuth providers your Socialstream application should support. These will be the providers your users use to authenticate with your application. By default, the `github` provider is enabled, but you are free to add and remove as many providers as your application requires.&#x20;

To enable a provider, simply add the name of the provider as a string (e.g. `google`) to the `providers` array. Alternatively, you may use the static methods available for each provider within the `JoelButcher\Socialstream\Providers` class:

```
JoelButcher\Socialstream\Providers:google();
```

{% hint style="info" %}
#### Twitter OAuth 2.0

Since [v5.4.0](https://github.com/laravel/socialite/releases/tag/v5.4.0), Laravel Socialte supports both Twitter OAuth 1.0 and OAuth 2.0. To enable support for the OAuth 2.0 version of the Twitter provider, simply add the `Providers::twitterOAuth2()` method to the `providers` array in your config file. Alternatively you may add `twitter-oauth-2` as the string variant.
{% endhint %}

#### Configuring a Provider

Once you have specified a provider in your `socialstream.php` config file, you will need to add that provider to your `services.php` config file and provide a `client_id`, `client_secret` and `redirect` value for each provider you are supporting. For example, if you wish to suppor the GitHub provider, you should add the following to your `services.php` config file:

```php
'github' => [
    'client_id' => env('{PROVIDER}_CLIENT_ID'),
    'client_secret' => env('{PROVIDER}_CLIENT_SECRET'),
    'redirect' => 'https://my-app.com/oauth/{provider}/callback',
],
```

{% hint style="warning" %}
#### Twitter

There is currently a [known bug in Laravel Socialite](https://github.com/laravel/socialite/issues/604) that doesn't allow you to define separate configs for both Twitter OAuth 1.0 and Twitter OAuth 2.0.&#x20;
{% endhint %}

