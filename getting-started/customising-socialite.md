# Customising Socialite

## Generating Redirect URLs

In some cases, you may need to customise how Socialite handles the generation of the redirect to a provider. To help you with this, Socialstream publishes a `GenerateRedirectForProvider.php` which can be found in your applications `app/Actions/Socialstream` folder.&#x20;

For example, you may wish to append additional scopes to your request, tyically scopes are defined under the provider in your `services.php` configuration file. However, you may wish to dynamically alter this at runtime:

```php
/**
 * Generates the redirect for a given provider.
 * 
 * @param  string  $provider
 * @return \Symfony\Component\HttpFoundation\RedirectResponse
 */
public function generate(string $provider)
{
    $scopes = ['*'];
    
    if ($provider = 'github' && config('services.github.manage_repos') {
        $scopes = array_merge($scopes, [
            'repos.manage',
        ]);
    }
    
    return Socialite::driver($provider)
        ->scopes($scopes)
        ->with(['response_type' => 'token'])
        ->redirect();
}
```

## Resolving users

By default, Socialstream will resolve user information from Socialite using the following logic:

```php
Socialite::driver($provider)->user();
```

However, you may decide you want to customise this functionality. For example, you may decide to use the `stateless` method to [disable state verification](https://laravel.com/docs/9.x/socialite#stateless-authentication). Socialstream makes this easy for you by publishing a `ResolveSocialiteUser` action to you applications `app/Actions/Socialstream` directory:

```php
<?php

namespace App\Actions\Socialstream;

use JoelButcher\Socialstream\Contracts\ResolvesSocialiteUsers;
use JoelButcher\Socialstream\Socialstream;
use Laravel\Socialite\Facades\Socialite;

class ResolveSocialiteUser implements ResolvesSocialiteUsers
{
    /**
     * Resolve the user for a given provider.
     *
     * @param  string  $provider
     * @return \Laravel\Socialite\Contracts\User
     */
    public function resolve($provider)
    {
        $user = Socialite::driver($provider)->user();

        if (Socialstream::generatesMissingEmails()) {
            $user->email = $user->getEmail() ?? ("{$user->id}@{$provider}".config('app.domain'));
        }

        return $user;
    }
}
```

You may alter this logic as you see fit.

