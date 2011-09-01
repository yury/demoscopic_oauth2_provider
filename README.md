# devise_oauth2_providable

Rails3 engine that brings OAuth2 Provider support to your application.

This version requires Mongoid.

Current OAuth2 Specification Draft:
http://tools.ietf.org/html/draft-ietf-oauth-v2-15

## Features

* integrates OAuth2 authentication with Devise authenthentication stack
* one-stop-shop includes all Models, Controllers and Views to get up and
  running quickly
* All server requests support authentication via bearer token included in
  the request.  http://tools.ietf.org/html/draft-ietf-oauth-v2-bearer-04

Scopes are now supported. For instance, make a non_expiring token by adding 
a scope to OAuth2::Scopes::Included like this:

```ruby
module OAuth2::Scopes::Included
  def non_expiring
    self.default_lifetime = 100.years
  end
end
```

## Installation

```ruby
# Bundler Gemfile
gem 'devise_mongoid_oauth2_providable'
```

No migration is necessary to get it running with Mongoid.


```ruby
class User
  # NOTE: include :database_authenticatable configuration
  # if supporting Resource Owner Password Credentials Grant Type
  devise :oauth2_providable, 
    :oauth2_password_grantable,
    :oauth2_refresh_token_grantable
end
```

## Models

### Client
registered OAuth2 client for storing the unique client_id and
client_secret.

### AccessToken
http://tools.ietf.org/html/draft-ietf-oauth-v2-15#section-1.3

Short lived token used by clients to perform subsequent requests (see
bearer token spec)

expires after 15min by default.

### RefreshToken
http://tools.ietf.org/html/draft-ietf-oauth-v2-15#section-1.5

Long lived token used by clients to request new access tokens without
requiring user intervention to re-authorize.

expires after 1 month by default.

### AuthorizationCode
http://tools.ietf.org/html/draft-ietf-oauth-v2-15#section-1.4.1

*Very* short lived token created to allow a client to request an access
token after a user has gone through the authorization flow.

expires after 1min by default.

## Routes

### /oauth2/authorize
http://tools.ietf.org/html/draft-ietf-oauth-v2-15#section-2.1

Endpoint to start client authorization flow.  Models, controllers and
views are included for out of the box deployment.

Supports the Authorization Code and Implicit grant types.

### /oauth2/token
http://tools.ietf.org/html/draft-ietf-oauth-v2-15#section-2.2

Endpoint to request access token.  See grant type documentation for
supported flows.

## Grant Types

### Resource Owner Password Credentials Grant Type
http://tools.ietf.org/html/draft-ietf-oauth-v2-15#section-4.3

in order to use the Resource Owner Password Credentials Grant Type, your
Devise model *must* be configured with the :database_authenticatable option

### Client Credentials Grant Type
http://tools.ietf.org/html/draft-ietf-oauth-v2-15#section-4.4

### Authorization Code Grant Type
http://tools.ietf.org/html/draft-ietf-oauth-v2-15#section-4.1

### Implicit Grant Type
http://tools.ietf.org/html/draft-ietf-oauth-v2-15#section-4.2

### Refresh Token Grant Type
http://tools.ietf.org/html/draft-ietf-oauth-v2-15#section-6

## Contributing
 
* Fork the project
* Fix the issue
* Add unit tests
* Submit pull request on github

See CONTRIBUTORS.txt for list of project contributors

## Copyright

Copyright (c) 2011 Socialcast, Inc. 
See LICENSE.txt for further details.

