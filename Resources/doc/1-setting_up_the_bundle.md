Step 1: Setting up the bundle
=============================
### A) Add HWIOAuthBundle to your project

```bash
composer require hwi/oauth-bundle symfony/http-client nyholm/psr7 guzzlehttp/promises php-http/httplug-bundle
```

Why `symfony/http-client` and its dependencies? We are decoupled from any HTTP messaging client thanks to [HTTPlug](http://httplug.io/).

Why `php-http/httplug-bundle`? This is the official [Symfony integration](https://packagist.org/packages/php-http/httplug-bundle) of HTTPlug.
This makes it possible to provide the required HTTP client and message factory with ease.
The dependency is optional but you will have to [provide your own services](internals/configuring_the_http_client.md) if you don't set it up.

If you use a recent version of Symfony supporting [Symfony Flex](https://symfony.com/doc/current/quick_tour/flex_recipes.html), when prompted, accept to execute the recipes coming from the contrib repository.
You'll see an error at the end of the process, it's intended. Continue straight to the second step: [configuring resource owners (Facebook, GitHub, Google, Windows Live and others](2-configuring_resource_owners.md) to fix it.

If you use an old version of Symfony, follow the instructions provided in the next sections.

### B) Enable the bundle

Enable the bundle in the kernel:

```php
// app/AppKernel.php

public function registerBundles()
{
    $bundles = [
        // ...
        new Http\HttplugBundle\HttplugBundle(), // If you require the php-http/httplug-bundle package.
        new HWI\Bundle\OAuthBundle\HWIOAuthBundle(),
    ];
}
```

If you don't use `HttplugBundle`, you will have to
[configure your own client service](internals/configuring_the_http_client.md).

### C) Import the routing

Import the `redirect.xml` and `login.xml` routing files in your own routing file.

```yaml
# app/config/routing.yml
hwi_oauth_redirect:
    resource: "@HWIOAuthBundle/Resources/config/routing/redirect.xml"
    prefix:   /connect

hwi_oauth_connect:
    resource: "@HWIOAuthBundle/Resources/config/routing/connect.xml"
    prefix:   /connect

hwi_oauth_login:
    resource: "@HWIOAuthBundle/Resources/config/routing/login.xml"
    prefix:   /login
```

**Note:**

> To prevent strange issues, this route should be imported before your custom ones.

### Continue to the next step!
When you're done. Continue by configuring the resource owners you want to use
in your application!


[Step 2: Configuring resource owners (Facebook, GitHub, Google, Windows Live and others](2-configuring_resource_owners.md)
