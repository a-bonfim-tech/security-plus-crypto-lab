# Security+ Crypto Lab

A local TLS/PKI lab with retained X.509 certificates and TLS 1.3 session evidence for technical inspection and independent verification.

## What this lab demonstrates

- A local X.509 PKI with a retained CA certificate and a `localhost` server certificate.
- A server certificate issued by the retained local CA, using RSA 2048 and SHA-256 with RSA.
- TLS 1.3 negotiation with `TLS_AES_256_GCM_SHA384`.
- A separate TLS 1.3 negotiation with `TLS_CHACHA20_POLY1305_SHA256`.
- Inspection of certificate metadata, X.509 extensions and detailed TLS handshake output.
- Independent verification of the retained server certificate against the retained local CA.

## Evidence snapshot

| Evidence | What it demonstrates |
| --- | --- |
| `evidence/01_server_certificate.txt` | X.509 server certificate metadata, issuer, RSA public key, extensions and SAN values |
| `evidence/02_tls13_handshake.txt` | TLS 1.3 negotiation with `TLS_AES_256_GCM_SHA384` and successful verification output |
| `evidence/03_tls13_chacha20_handshake.txt` | TLS 1.3 negotiation with `TLS_CHACHA20_POLY1305_SHA256` |
| `evidence/04_certificate_validation.txt` | Retained server certificate subject, issuer and validity metadata |
| `evidence/05_tls_handshake_details.txt` | Detailed TLS handshake trace, including handshake message inspection |

## Technical workflow

Local PKI artifacts
→ `localhost` TLS sessions
→ TLS 1.3 parameter negotiation
→ certificate and handshake inspection
→ retained evidence

## Key observations

- `certs/local-ca.crt` is a CA certificate with `CA:TRUE`.
- `certs/server.crt` is a non-CA server certificate issued by `SecurityPlusLocalCA`.
- The server certificate identifies `localhost` and includes SAN entries for `localhost` and `127.0.0.1`.
- Its Extended Key Usage includes TLS Web Server Authentication.
- Retained TLS sessions show negotiation of both AES-GCM and ChaCha20-Poly1305 TLS 1.3 cipher suites.
- Captured client output reports successful certificate verification.

## Reproduce / Verify

The retained public certificate artifacts can be independently inspected with OpenSSL.

Verify the server certificate against the retained local CA:

~~~sh
openssl verify -CAfile certs/local-ca.crt certs/server.crt
~~~

Inspect server certificate identity and validity:

~~~sh
openssl x509 \
  -in certs/server.crt \
  -noout \
  -subject \
  -issuer \
  -serial \
  -dates \
  -fingerprint \
  -sha256
~~~

Inspect certificate extensions:

~~~sh
openssl x509 -in certs/server.crt -noout -text
~~~

Reproduction covers the retained certificate artifacts; the original end-to-end execution sequence was not retained.

## Evidence scope

This repository documents a local TLS/PKI lab using retained certificates and TLS 1.3 session outputs. The evidence covers certificate inspection, trust validation and recorded AES-GCM / ChaCha20-Poly1305 negotiations; broader production PKI, revocation and incident-response scenarios remain outside scope.

## Repository contents

- `certs/` — retained CA/server certificates, CSR and server certificate extension configuration.
- `evidence/` — retained X.509 and TLS inspection outputs.
- `docs/` — supporting conceptual and interview notes.

Private key files are not versioned in the repository.
