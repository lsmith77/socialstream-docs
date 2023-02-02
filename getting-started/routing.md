# Routing

When using Socialstream, two new routes are registered with your application, these are:

```
http(s)://mydomain.test/oauth/{provider}
http(s)://mydomain.test/oauth/{provider}/callback
```

The first of these URLs is intended to be used for obtaining and redirecting your users to the given `provider`'s OAuth flow.

The second is the return URL, which you should set in the application settings within a providers developer console (for example, the "Authorization callback URL" when configuring a new OAuth application in GitHub).\
\
