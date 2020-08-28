# jwtx

Command line tool for examining JSON Web Tokens.

Decodes JWT header and payload, and optionally validates the JWS signature.

## Requirements

The script is a bash shell script, with the following dependencies

- jq (https://stedolan.github.io/jq)
- openssl (https://www.openssl.org)
- awk
- sed

## Usage

jwtx can be used to examine an encoded JWT either by reading from a local file, or by piping through stdin. Commmand line options are as follows
```
  -p|--properties filename (file with options)
  -w|--well-known uri  (OIDC wellknown URI)
  -j|--jwks-uri uri    (jwks_uri if --well-known not specified)
  -s|--sigverify       (perform signature verification - requires --well-known or --jwk options)
  -d|--debug           (debug output)
  -f|--file filename   (file containing JWT - if not supplied, JWT taken from stdin)
  -v|--version         (print jwtx version)
  -h|--help            (print this usage information)
```

## Operation

The JWT is passed to jwkx either via stdin, or via the -f option.

jwtx displays a summary of the token contents, including

- JOSE header
- JOSE payload

When the --sigverify option is specified, jwtx also validates the JWT signature. The public key is retrieved from the issuer JWK: this JWK is retrieved either from the URI specified by the --jwks-uri option, or from the jwks_uri value discovered via the URI specified by the --well-known option.
  
## Examples  

1. Decode a token with no signature verification

```
echo 'eyJ0eXAiOiJKV1QiLCJraWQiOiJFRjcxaVNhb3NiQzVDNHRDNlN5cTFHbTY0N00iLCJhbGciOiJQUzI1NiJ9.eyJhdF9oYXNoIjoibEVqbFlqWVgtYTF6djl1VktsODlWUSIsInN1YiI6ImphbmUuZG9lIiwiYXVkaXRUcmFja2luZ0lkIjoiY2IxNThlYjgtOGM4Zi00MGI1LWE0MGUtYTU0NjQyOTI1ZTJhLTI1MiIsImlzcyI6Imh0dHBzOi8vYW0uYXV0aGRlbW8ub3JnL29hdXRoMi9yZWFsbXMvcm9vdC9yZWFsbXMvdGVzdCIsInRva2VuTmFtZSI6ImlkX3Rva2VuIiwiYXVkIjoidGVzdGNsaWVudCIsImF6cCI6InRlc3RjbGllbnQiLCJhdXRoX3RpbWUiOjE1OTgyODg4OTAsInJlYWxtIjoiL3Rlc3QiLCJleHAiOjE1OTgyODk0OTMsInRva2VuVHlwZSI6IkpXVFRva2VuIiwiaWF0IjoxNTk4Mjg4ODkzfQ.NNKNdsOD2h0Y1kz75Ljqluu3QWzgVZyqrOxmBnMI9I6nPAqhd4rkxo3HsQ_E1e_0dpa_jp-xB4-FXk0RLI2xqFp7fEehW9NdaMZm2nT75Id2O_IAoNhqV_iski6HlKSwB3qJ5MwjBS2R2EG_3Co3KDn2NuyIuqpu1RS6Ut1TnYH8P4-jse4AIIRr9kM-Id52-TU1NlKkSAcHvjqyoPhXt6L_6nA60ZtduXWVwkWCuvhH32myG5K8UEQxNU-lfO8L7VAWQPRPDPo1fDqlyMKeWQHlGA8TrgXRbdry1p0JvETFFXE_GlxkOO5MFeOB3HgwftW6Mhf-N9g3Wewx3HMhgQ' \
  | ./jwtx
{
  "typ": "JWT",
  "kid": "EF71iSaosbC5C4tC6Syq1Gm647M",
  "alg": "PS256"
}
{
  "at_hash": "lEjlYjYX-a1zv9uVKl89VQ",
  "sub": "jane.doe",
  "auditTrackingId": "cb158eb8-8c8f-40b5-a40e-a54642925e2a-252",
  "iss": "https://am.authdemo.org/oauth2/realms/root/realms/test",
  "tokenName": "id_token",
  "aud": "testclient",
  "azp": "testclient",
  "auth_time": 1598288890,
  "realm": "/test",
  "exp": 1598289493,
  "tokenType": "JWTToken",
  "iat": 1598288893
}
```


2. Decode a token and verify the signature. Use OpenID Connect Discovery to find the public key

```
./jwtx -f jwt.txt -s -w https://am.authdemo.org/oauth2/realms/root/realms/test/.well-known/openid-configuration
{
  "typ": "JWT",
  "kid": "EF71iSaosbC5C4tC6Syq1Gm647M",
  "alg": "PS256"
}
{
  "at_hash": "lEjlYjYX-a1zv9uVKl89VQ",
  "sub": "jane.doe",
  "auditTrackingId": "cb158eb8-8c8f-40b5-a40e-a54642925e2a-252",
  "iss": "https://am.authdemo.org/oauth2/realms/root/realms/test",
  "tokenName": "id_token",
  "aud": "testclient",
  "azp": "testclient",
  "auth_time": 1598288890,
  "realm": "/test",
  "exp": 1598289493,
  "tokenType": "JWTToken",
  "iat": 1598288893
}
Verified OK
```

