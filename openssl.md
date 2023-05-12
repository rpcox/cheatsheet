## openssl
---
### Generate default RSA key

    openssl genrsa -out key.pem                    # legacy - default 1024 bit
    openssl genpkey -algorithm rsa -out key.pem    # default 2048 bit

### Generate 4096 bit RSA key

    openssl genrsa -out key.pem 4096               # legacy
    openssl genpkey -algorithm rsa -out key.pem -pkeyopt rsa_keygen_bits:4096

### Generate AES256 encrypted 4096 bit password protected RSA key

    openssl genrsa -aes256 -passout pass:foobar -out key.pem 4096        # legacy
    openssl genpkey -algorithm rsa -aes256 -pass pass:foobar -out key.pem -pkeyopt rsa_keygen_bits:4096

### List available EC curves

    openssl ecparam --list_curves

### Genernate EC key (curve=brainpoolP160r1)

    openssl ecparam -genkey -name brainpoolP160r1 -noout -out eckey.pem
    openssl genpkey -algorithm ec -pkeyopt ec_paramgen_curve:brainpoolP160r1 -out eckey.pem

### Convert and encrypt EC key

    openssl pkcs8 -topk8 -in eckey.pem -passout pass:foobar -out private.pem

### Extract public key

    openssl rsa -in key.pem -pubout -out public_key.pem   # rsa
    openssl ec  -in key.pem -pubout -out public_key.pem   # ec

### Pass phrase options using -passin and -passout

    pass:password  - literal password on cmd line
    env:VAR        - read from environment variable named 'VAR'
    file:path2file - read from file
    fd:number      - read from file descripter 'number'
    stdin          - read from stdin

### Generate a CSR from an existing private key

    openssl req -key private.key -new -out site.csr

### Generate a CSR from an existing certificate and private key

    openssl x509 in domain.crt-signkey domain.key -x509toreq -out domain.csr

### View CSR contents

    openssl x509 -text -noout -in site.csr

### Print the certificate contents

     openssl x509 -in cert.pem -noout -text

### Convert a certificate from PEM to DER

     openssl x509 -in cert.pem -inform PEM -out cert.der -outform DER

### Print the certificate subject

     openssl x509 -subject -noout -in cert.pem

### Print the certificate SAN

     openssl x509 -noout -text -in cert.pem | grep -A 1 Alt | grep DN | sed -r 's/^ +//'

### Print the certificate start/begin date

     openssl x509 -startdate -noout -in cert.pem | cut -d '=' -f2

### Print the certificate end date

     openssl x509 -enddate -noout -in cert.pem | cud -d '=' -f 2

### Print the certificate serial number

     openssl x509 -serial -noout -in cert.pem | cut =d '=' -f 2

### Print the certificate issuer

     openssl x509 -issuer -noout -in cert.pem

