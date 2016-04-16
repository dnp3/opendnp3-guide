### Transport Layer Security (TLS)

TLS support as provided by added two additional channel types, TLSServer and TLSClient.

These overloaded methods on _DNP3Manager_ take an additional configuration parameter [_TLSConfig_](https://github.com/automatak/dnp3/blob/2.0.x/cpp/libs/include/asiopal/tls/TLSConfig.h).

```
class TLSConfig
{
public:

	TLSConfig(
	    const std::string& peerCertFilePath,
	    const std::string& localCertFilePath,
	    const std::string& privateKeyFilePath,
	    const std::string& cipherList = ""
	);


	std::string peerCertFilePath;
	std::string localCertFilePath;
	std::string privateKeyFilePath;
	std::string cipherList;

	/// Allow TLS version 1.0 (default true)
	bool allowTLSv10;

	/// Allow TLS version 1.1 (default true)
	bool allowTLSv11;

	/// Allow TLS version 1.2 (default true)
	bool allowTLSv12;

};
```

The opendnp3 TLS implementation only allows mutually authenticated connections using client certificates. Each side must provide 3 items:

* A certificate (either self-signed or root certificate) that will be used to authenticate the peer certificate.
* A local certificate to send to the peer.
* A private key corresponding to the public key in the local certificate used to authenticate.

All keys and certificates should be provided in the PEM format.

**Note that in the next release (2.2.0), only TLS 1.2 will be enabled by default**
