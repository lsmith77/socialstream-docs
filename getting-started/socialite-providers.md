# Socialite Providers

By default, Socialstream only supports the providers that are actively support by the Laravel Socialite package: Facebook, Twitter, LinkedIn, Google, GitHub, GitLab, and Bitbucket.

However, if you wish, you may provide a custom implementation inside your application for any of the providers supported by the community community driven [socialiteproviders](https://socialiteproviders.com/) package.

To implement support for a custom provider, you will first need to create an SVG icon or custom button component for the provider within your desired stack. You will then need to update the published components for your stack to cater for this provider:

**Livewire**

Icon location: `resources/views/components/socialstream-icons/`

* `resources/views/components/socialstream.blade.php`
* `resources/views/components/connected-account.blade.php`

**Inertia**

Icon location: `resource/js/Components/SocialstreamIcons/`

* `resources/js/Components/Socialstream.vue`
* `resources/js/Components/ConnectedAccount.vue`

{% hint style="warning" %}
Some providers will not return a token in the callback response. As such, you will need to modify the `connected_accounts` table migration to make the `token` field nullable:

```php
$table->string('token', 1000)->nullable();
```
{% endhint %}
