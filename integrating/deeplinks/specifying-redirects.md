# Specifying Redirects

All [Methods](provider-methods/) support a `redirect_link=` param that lets Phantom know how to get back to the original app. The URI specified by this param should be URL encoded. The following is an example for a `mydapp://onPhantomConnected` redirect URI:

```
redirect_link%3Dmydapp%3A%2F%2FonPhantomConnected
```

If the deeplink request to Phantom comes with a response, Phantom will append the results as query parameters in the `redirect_link=` upon redirecting.

```
redirect_link=mydapp://onPhantomConnected?data=...
```
