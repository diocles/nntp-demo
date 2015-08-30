# Golang NNTP server with TLS

This code is mostly copy/pasted from github.com/dustin/go-nntp, but I've
changed the socket setup.

At the time of writing, you need to use my fork of go-nntp, available at
github.com/diocles/go-nntp.

Certificates can be generated using the code at:
http://golang.org/src/crypto/tls/generate_cert.go

```
    go run generate_cert.go --host localhost --ca
```

## Testing with Gnus

My gnus config for testing looks a bit like this:

```elisp
(setq-default gnus-secondary-select-methods
              '((nntp "localhost"
       	              (nntp-port-number 1119)
                      (nntp-open-connection-function nntp-open-tls-stream))))
```

To actually verify the certificate, you can use gnutls-cli like this:

```elisp
(if (fboundp 'gnutls-available-p)
    (fmakunbound 'gnutls-available-p))
(setq-default tls-program '("gnutls-cli --strict-tofu -p %p %h")
	      starttls-extra-arguments '("--strict-tofu"))
```

and run:

```
gnutls-cli --tofu -p 1119 localhost
```

Or alternatively, if you use a CA-signed TLS certificate somehow:

```
(setq gnutls-verify-error t)
```
