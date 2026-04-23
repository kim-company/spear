# KurrentDB config

This directory contains KurrentDB configuration used for testing.
Certificates can be renewed using `openssl` with the existing CA key, or with the [es-gencert-cli](https://github.com/kurrent-io/es-gencert-cli) tool.

```bash
openssl genrsa -out kurrentdb/certs/node.key 2048
openssl req -new -key kurrentdb/certs/node.key -subj "/CN=eventstoredb-node" -out node.csr
openssl x509 -req -in node.csr -CA kurrentdb/certs/ca/ca.crt -CAkey kurrentdb/certs/ca/ca.key \
  -CAcreateserial -days 1825 \
  -extfile <(printf "subjectAltName=DNS:localhost,DNS:eventstore,IP:127.0.0.1\nextendedKeyUsage=serverAuth\n") \
  -out kurrentdb/certs/node.crt
rm node.csr
```

You can check the expiry date with `openssl`:

```
openssl x509 -in kurrentdb/certs/node.crt -noout -enddate
```

The current node certificate (`certs/node.crt`) is valid until April 2031.
The CA certificate (`certs/ca/ca.crt`) is valid until 2029 and does not need to be rotated.
