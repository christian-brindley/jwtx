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

  -p|--properties filename (file with options)
  -w|--well-known uri  (OIDC wellknown URI)
  -j|--jwks-uri uri    (jwks_uri if --well-known not specified)
  -s|--sigverify       (perform signature verification - requires --well-known or --jwk options)
  -d|--debug           (debug output)
  -f|--file filename   (file containing JWT - if not supplied, JWT taken from stdin)
  -v|--version         (print jwtx version)
  -h|--help            (print this usage information)
  
## Operation
  
## Examples  

