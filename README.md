# node-gtoken

> Node.js Google Authentication Service Account Tokens

## Installation

``` sh
npm install gtoken
```

## Usage

``` js
var gtoken = require('gtoken')({
  email: 'my_service_account_email@developer.gserviceaccount.com',
  scope: ['scope1 url', 'scope2 url'], // or space-delimited string of scopes
  keyFile: 'path/to/key.pem' // or .p12 key file
});

gtoken.getToken(function(err, token) {
  if (err) {
    // handle error
    return;
  }
  console.log(token);
});
```

Another option is to pass the private key as a string:

``` js
var key = '-----BEGIN RSA PRIVATE KEY-----\nXXXXXXXXXXX...';
var gtoken = require('gtoken')({
  email: 'my_service_account_email@developer.gserviceaccount.com',
  scope: ['scope1 url', 'scope2 url'], // or space-delimited string of scopes
  key: key
});
```

## Options

> Various options that can be set when creating `gtoken` object.

- `options.email or options.iss`: The service account email address.
- `options.scope`: An array of scope strings or space-delimited string of scopes.
- `options.sub`: The email address of the user requesting delegated access.
- `options.keyFile`: The filename of `.pem` key or `.p12` key.
- `options.key`: The raw RSA private key value, in place of using `options.keyFile`.

### .getToken(callback)

> Returns the cached token or requests a new one.

``` js
gtoken.getToken(function(err, token) {
  console.log(err || token);
  // gtoken.token value is also set
});
```

### .hasExpired()

> Returns true if the token has expired, or token does not exist.

``` js
gtoken.getToken(function(err, token) {
  if(token) {
    gtoken.hasExpired(); // false
  }
});
```

### Properties

> Various properties set on the gtoken object after `.getToken()`.

- `gtoken.token`: The access token.
- `gtoken.expires_at`: The expiry date as milliseconds since 1970/01/01
- `gtoken.key`: The raw key value.
- `gtoken.raw_token`: Most recent raw token data received from Google.

## Downloading your private `.p12` key from Google

1. Open the [Google Developer Console][gdevconsole].
2. Open your project and under "APIs & auth", click Credentials.
3. Generate a new `.p12` key and download it into your project.

## Converting your `.p12` key to a `.pem` key

You can just specify your `.p12` file (with `.p12` extension) as the `keyFile` and it will automatically be converted to a `.pem` on the fly, however this results in a slight performance hit. If you'd like to convert to a `.pem` for use later, use OpenSSL if you have it installed.

``` sh
$ openssl pkcs12 -in key.p12 -nodes -nocerts > key.pem
```

Don't forget, the passphrase when converting these files is the string `'notasecret'`

## License

MIT

[gdevconsole]: https://console.developers.google.com
