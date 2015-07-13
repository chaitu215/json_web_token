# JSON Web Token [![travis][ci_img]][travis] [![code_climate][cc_img]][code_climate]

## A JSON Web Token implementation for Ruby

### Description
A Ruby implementation of the JSON Web Token (JWT) Standards Track [RFC 7519][rfc7519]

## Installation
    gem install json_web_token

### Philosophy & Design Goals
* Minimal API surface area
* Clear separation and conformance to underlying standards
  - JSON Web Signature (JWS) Standards Track [RFC 7515][rfc7515]
  - JSON Web Algorithms (JWA) Standards Track [RFC 7518][rfc7518]
* Thorough test coverage
* Modularity for comprehension and extensibility
* Fail fast and hard, with maximally strict validation
  - Inspired by [The Harmful Consequences of Postel's Maxim][thomson-postel]
* Implement only the REQUIRED elements of the JWT standard (initially)

### Intended Audience
Token authentication of API requests to Rails via these prominent gems:

- [Devise][devise]
- [Doorkeeper][doorkeeper]
- [OAuth2][oauth2]

Secure Cross-Origin Resource Sharing ([CORS][cors]) using the [rack-cors][rack-cors] gem

## Usage

### JsonWebToken.create(claims, options)

Returns a JSON Web Token string

`claims` (required) string or hash

`options` (optional) hash

* **alg** (optional, default: `HS256`)
* **key** (required unless alg is 'none')

Example

```ruby
require 'json_web_token'

# sign with default algorithm, HMAC SHA256
jwt = JsonWebToken.create({foo: 'bar'}, key: 'gZH75aKtMN3Yj0iPS4hcgUuTwjAzZr9C')

# sign with RSA SHA256 algorithm
opts = {
  alg: 'RSA256',
  key: < RSA private key >
}

jwt = JsonWebToken.create({foo: 'bar'}, opts)

# unsecured token (algorithm is 'none')
jwt = JsonWebToken.create({foo: 'bar'}, alg: 'none')

```

### JsonWebToken.validate(jwt, options)

Returns either:
* a JWT claims set string or hash, if the Message Authentication Code (MAC), or signature, is verified
* a string, 'Invalid', otherwise

`jwt` (required) is a JSON web token string

`options` (optional) hash

* **alg** (optional, default: `HS256`)
* **key** (required unless alg is 'none')

Example

```ruby
require 'json_web_token'

secure_jwt_example = 'eyJ0eXAiOiJKV1QiLA0KICJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJqb2UiLA0KICJleHAiOjEzMDA4MTkzODAsDQogImh0dHA6Ly9leGFt.cGxlLmNvbS9pc19yb290Ijp0cnVlfQ.dBjftJeZ4CVP-mB92K27uhbUJU1p1r_wW1gFWFOEjXk'

# verify with default algorithm, HMAC SHA256
claims = JsonWebToken.validate(secure_jwt_example, key: 'gZH75aKtMN3Yj0iPS4hcgUuTwjAzZr9C')

# verify with RSA SHA256 algorithm
opts = {
  alg: 'RSA256',
  key: < RSA public key >
}

claims = JsonWebToken.validate(jwt, opts)

# unsecured token (algorithm is 'none')
unsecured_jwt_example = 'eyJ0eXAiOiJKV1QiLA0KICJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJqb2UiLA0KICJleHAiOjEzMDA4MTkzODAsDQogImh0dHA6Ly9leGFt.'

claims = JsonWebToken.validate(unsecured_jwt_example, alg: 'none')

```
### Supported encryption algorithms

alg Param Value | Digital Signature or MAC Algorithm
------|------
HS256 | HMAC using SHA-256 per [RFC 2104][rfc2104]
HS384 | HMAC using SHA-384
HS512 | HMAC using SHA-512
RS256 | RSASSA-PKCS-v1_5 using SHA-256 per [RFC3447][rfc3447]
RS384 | RSASSA-PKCS-v1_5 using SHA-384
RS512 | RSASSA-PKCS-v1_5 using SHA-512
ES256 | ECDSA using P-256 and SHA-256 per [DSS][dss]
ES384 | ECDSA using P-384 and SHA-384
ES512 | ECDSA using P-521 and SHA-512
none | No digital signature or MAC performed (unsecured)

### Supported Ruby Versions
Ruby 2.0 and up

### Limitations
Future implementation may include these features:

- processing of OPTIONAL JWT registered claim names (e.g. 'exp')
- representation of a JWT as a JSON Web Encryption (JWE) [RFC 7516][rfc7516]
- OPTIONAL nested JWTs

[rfc2104]: http://tools.ietf.org/html/rfc2104
[rfc3447]: http://tools.ietf.org/html/rfc3447
[rfc7515]: http://tools.ietf.org/html/rfc7515
[rfc7516]: http://tools.ietf.org/html/rfc7516
[rfc7518]: http://tools.ietf.org/html/rfc7518
[rfc7519]: http://tools.ietf.org/html/rfc7519
[dss]: http://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.186-4.pdf

[thomson-postel]: https://tools.ietf.org/html/draft-thomson-postel-was-wrong-00
[cors]: http://www.w3.org/TR/cors/
[devise]: https://github.com/plataformatec/devise
[doorkeeper]: https://github.com/doorkeeper-gem/doorkeeper
[oauth2]: https://github.com/intridea/oauth2
[rack-cors]: https://github.com/cyu/rack-cors

[travis]: https://travis-ci.org/garyf/json_web_token
[ci_img]: https://travis-ci.org/garyf/json_web_token.svg?branch=master
[code_climate]: https://codeclimate.com/github/garyf/json_web_token
[cc_img]: https://codeclimate.com/github/garyf/json_web_token/badges/gpa.svg
