### Transport Layer Security (TLS)

TLS support provides two additional channel types: TLSServer and TLSClient.

These overloaded methods on `DNP3Manager` take an additional configuration structure called `TLSConfig`.

The opendnp3 TLS implementation only allows mutually authenticated connections using client certificates. Each side
must provide 3 items:

* A certificate (either self-signed or root certificate) that will be used to authenticate the peer certificate.
* A local certificate to send to the peer.
* A private key corresponding to the public key in the local certificate used to authenticate.

All keys and certificates should be provided in the PEM format.
