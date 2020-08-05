# Delegated Credentials

As defined in the [draft](https://tools.ietf.org/html/draft-ietf-tls-subcerts-09).

## Strategy

There are two different implementations of Delegated Credentials over [TLS Tris](https://github.com/cloudflare/tls-tris):

* https://github.com/cloudflare/tls-tris/pull/32
* https://github.com/cloudflare/tls-tris/pull/95

While the simplicity of the first is nicer, the second one takes more thoughts
into account. A hybrid from both of them might be the best case to implement.

Either way, this will mean that we need to modify the `conn.go`, `common.go`,
`generate_cert.go`, `handshake_client.go`, `handshake_messages.go`,
`handshake_server.go`, and perhaps `key_agreement.go`.

On `generate_cert.go`, we should add a flag (bool) called `isDC` that determines
is DC will be used. This will mean that we need to add to x509.Certificate a
`isDC` bool, and add something like `KeyUsageDelegatedCredentials` to KeyUsage.
Let's see if this is possible.

## Critical things to take into account

* DC are only supported for TLS1.3, so the version should always be checked.
  The way the Golang code establishes the version is coupled with different
  things, so one should be careful with it.

## Open questions and to add

* Usage x509 extension identifier?
* The test vectors should be added: https://github.com/tlswg/tls-subcerts/pull/77

## Comparison


| Library     | Draft version implemented | Server auth support | Client auth support | Link                                           |
|-------------|---------------------------|---------------------|---------------------|------------------------------------------------|
| BoringSSL   | Seems like 02/03          |     Yes             |     No              |  https://bit.ly/3fymIdk                        |
| Go Golang   | None                      |     No              |     No              | https://github.com/golang/go/issues/35311      |
| CF Golang   | 09                        |     Yes             |     Yes             | https://github.com/cloudflare/go/issues/26     |
| CF TLS-tris | Seeems like 01/02         |     Yes             |     No              | https://github.com/cloudflare/tls-tris/pull/95 |
| Rust TLS    | None                      |     No              |     No              |                                                |
