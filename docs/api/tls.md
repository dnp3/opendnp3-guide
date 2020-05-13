### Transport Layer Security (TLS)

Opendnp3 provides support for TLS with mutual authentication using ASIO's C++ wrapper around openssl.

Bulding the library with TLS support provides two additional channel types: `TLSServer` and `TLSClient`.

`DNP3Manager` has provides methods for creating these channel types which take an additional configuration structure
called `TLSConfig`.

Each side of the TLS channel must provide 3 items:

* A certificate (either self-signed or root certificate) that will be used to authenticate the peer certificate.
* A local certificate, or certificate chain, to send to the peer.
* A private key corresponding to the public key in the local certificate used to authenticate.

Refer to the documentation for `TLSConfig` for details.

There are example certificates and private keys in the`cpp\tests\asiotests\certs` directory. These certificates are
examples only and used in component tests to ensure openssl can perform a sucessful handshake.

!!! important
    All keys and certificates must be provided in the **PEM format**
