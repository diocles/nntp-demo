# Golang NNTP server with TLS

This code is mostly copy/pasted from github.com/dustin/go-nntp, but I've
changed the socket setup.

Certificates can be generated using the code at:
http://golang.org/src/crypto/tls/generate_cert.go

```
    go run generate_cert.go --host localhost --ca
```

My gnus config for testing looks a bit like this:

```elisp
(setq-default gnus-secondary-select-methods
              '((nntp "localhost"
       	              (nntp-port-number 1119)
                      (nntp-open-connection-function nntp-open-tls-stream))))
```

At the time of writing, you need to use my fork of go-nntp, available at
github.com/diocles/go-nntp.