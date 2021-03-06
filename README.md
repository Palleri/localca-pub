
<img alt="logo" src="https://i.imgur.com/wQv0uOx.png">

-----------------------------------------------------------------------
# ONLY FOR INTERNAL ACCESS
<b>DO NOT PUBLISH ON THE INTERNET.</b>

<b>DO NOT USE FOR PRODUCTION.</b>

<b>NOT CHECKED FOR BUGS OR VULNERABILITIES.</b>

-----------------------------------------------------------------------

### Aslong as the ca.pem remains in /var/www/html/files/ca the container will not install a new one
### To reinstall CA, delete folder ./data/files/ca

```yml
version: '3.2'
secrets:
  cakey:
    file: ./cakey.txt
services:
  app:
    container_name: localca
    hostname: ca
    domainname: local.local
    secrets:
      - cakey
    image: 'palleri/localca:latest'
    restart: unless-stopped
    ports:
      - '80:80'
    volumes:
      - ./data:/var/www/html
    environment:
      - ca=ca.local.local
      - cakey_FILE=/run/secrets/cakey
      - O=localca
      - C=SE
```

### ca = Name of the ROOTCA
### cakey = privatekey (Use docker secrets instead of plain-text inside docker-compose.yml)

---------------------
Access webgui: http://x.x.x.x/index.php
---------------------

Convert to .p12:
This create certificate with client authentication attributes instead of server authentication.

---------------------

For more security:
Create your client certificate .p12 and add it to your browser.
Install ca.pem in your browser and nginx and activate ssl_verify_client on; 

NGINX conf
```
ssl_client_certificate /etc/ssl/certs/ca.pem;
ssl_verify_client on;

```

<a href="https://github.com/palleri/localca-pub">Github Palleri/LocalCA</a>
