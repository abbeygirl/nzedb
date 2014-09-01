# Prerequisites:
You require a certificate bundle to verify remote server certificates for authenticity.
You can download pre-made bundles or create your own bundle here: http://curl.haxx.se/docs/caextract.html

On linux you can put this bundle in the /etc/ssl/certs/ folder, make sure it's readable by all users.

An example of fetching the file on linux and giving it rwx permissions:

`sudo wget -P /etc/ssl/certs/ http://curl.haxx.se/ca/cacert.pem`  
`sudo chmod 744 /etc/ssl/certs/cacert.pem`

If the bundle does not contain the certificate, you will need to manually download it into your certs folder.

# For existing installations of nZEDb (prior to october 2014):
Open up www/config.php in a text editor.
Add the following in the file (before `require_once 'automated.config.php';`):

START COPYING UNDER THIS LINE:
***


`/* Location to CA bundle file on your system. You can download one here: http://curl.haxx.se/docs/caextract.html */`  
`define('nZEDb_SSL_CAFILE', '');`  
`/* Path where openssl cert files are stored on your system, this is a fall back if the CAFILE is not found. */`  
`define('nZEDb_SSL_CAPATH', '');`  
`/* Use the aforementioned CA bundle file to verify remote SSL certificates when connecting to a server using TLS/SSL. */`  
`define('nZEDb_SSL_VERIFY_PEER', '0');`  
`/* Verify the host is who they say they are. */`  
`define('nZEDb_SSL_VERIFY_HOST', '0');`  
`/* Allow self signed certificates. Note this does not work on CURL as CURL does not have this option. */`  
`define('nZEDb_SSL_ALLOW_SELF_SIGNED', '1');`

***
STOP COPYING ABOVE THIS LINE.


`nZEDb_SSL_CAFILE` would be the ca file you downloaded. ie '/etc/ssl/certs/ca-bundle.crt'

`nZEDb_SSL_CAPATH` would be the folder where all your certs are stored by default. ie '/etc/ssl/certs/'

`nZEDb_SSL_VERIFY_PEER` would be set to '1' if you want to use the ca file to verify remote certificates.

`nZEDb_SSL_VERIFY_HOST` would be set to '1' if you want to check if the hostname you are connecting to matches the CA name in the certificate. Note that some NNTP providers use the wrong CA name and you will need to set this to 0 to be able to connect to their servers. This is not ideal, you should consider getting a NNTP provider that has a proper certificate.

`nZEDb_SSL_ALLOW_SELF_SIGNED` would be set to '0' if you want to block self-signed certificates.

# For new nZEDb installations:
On step 3 of the install, you will be asked for the info above, so follow the guide above if you are confused.