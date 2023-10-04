# Example on how to generate the required certificates for keycloak

# Generation of Root CA
* ```openssl req -x509 -sha256 -days 3650 -newkey rsa:4096 -keyout rootCA.key -out rootCA.crt```

# Generation of certificate for keycloak server
* ```openssl req -new -newkey rsa:4096 -keyout localhost.key -out localhost.csr -nodes```
* ```openssl x509 -req -CA rootCA.crt -CAkey rootCA.key -in localhost.csr -out localhost.crt -days 365 -CAcreateserial -extfile localhost.ext```

# Generation of user certificate
* ```openssl req -new -newkey rsa:4096 -nodes -keyout fredFlintstone.key -out fredFlintstone.csr```
* ```openssl x509 -req -CA rootCA.crt -CAkey rootCA.key -in fredFlintstone.csr -out fredFlintstone.crt -days 365 -CAcreateserial```

# Export certificate and private keys to p12
* ```openssl pkcs12 -export -out fredFlintstone.p12 -name "fredFlintstone" -inkey fredFlintstone.key -in fredFlintstone.crt```
* ```openssl pkcs12 -export -in localhost.crt -inkey localhost.key -out localhost.p12```

# Create the keystore for keycloak
* ```keytool -import -file rootCA.crt -alias rootCA -keystore truststore.jks```
