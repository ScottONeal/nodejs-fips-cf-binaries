# nodejs-fips-cf-binaries

A collection of Node.js OpenSSL Fips Compliant Binaries used for a [custom Node.js buildpack](https://github.com/ScottONeal/nodejs-buildpack)

## Creating Binary Release

Pull CF ubuntu docker file for building. Note: it is important to understand the process when installing the FIPS module, please read this: https://github.com/nodejs/node#building-nodejs-with-fips-compliant-openssl

```bash
# Fetch CF ubuntu image
docker pull cloudfoundry/cflinuxfs2

# Attach term to image
docker run -it cloudfoundry/cflinuxfs2 bash

# Download and install OpenSSL Fips Module, will install module to /usr/local/ssl/fips-2.0/
curl --remote-name https://www.openssl.org/source/openssl-fips-2.0.12.tar.gz
cd openssl-fips-2.0.12.tar.gz
./config
make
make install

# Download Desired Node.js Version (example is 4.3.0)
cd
curl --remote-name https://nodejs.org/dist/v4.3.0/node-v4.3.0.tar.gz
cd node-v4.3.0

# Get ready to build!
./configure --openssl-fips=/usr/local/ssl/fips-2.0
make
# Go get some coffe
DESTDIR=../ PREFIX="node-release" make install

# Files will be installed to ~/node-release || ~/usr/local (depends on source building from)
# Create .tgz file to be used for CF buildpack
tar -cvzf node-fips-v4.3.0.tgz ~/node-release/
```
