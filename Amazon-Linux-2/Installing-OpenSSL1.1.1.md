# Installing OpenSSL1.1.1 on Amazon-Linux-2

    Amazon doesnt have the latest version of OpenSSL in its repositories. Installing from source is a viable option for fixing this issue. In this example I will be using the 5.10.112-108.499.amzn2.x86_64 kernel. Follow AWS documentation to upgrade your kernal.

## Bash commands

- Build and Install
'''
sudo -y yum update
sudo yum install -y make gcc perl-core pcre-devel wget zlib-devel
cd /usr/local/src
git clone <https://github.com/openssl/openssl.git>
cd openssl
sudo /bin/bash ./config --prefix=/usr --openssldir=/etc/ssl --libdir=lib no-shared zlib-dynamic
make
make test
make install
'''
- Configure link libraries
Add the openssl library path '''/usr/local/ssl/lib64''' to a conf file in /etc/ls.so.conf.d
'''
cd /etc/ld.so.conf.d/
vim openssl-1.1.1.conf
sudo ldconfig -v
'''
- Configure the OpenSSL binary files
Backup the original bin file and add the following to the openssl.sh script
'''
#Set OPENSSL_PATH
OPENSSL_PATH="/usr/local/ssl/bin"
export OPENSSL_PATH
PATH=$PATH:$OPENSSL_PATH
export PATH
'''
'''
mv /bin/openssl /bin/openssl.BAK
vim /etc/profile.d/openssl.sh

chmod +x /etc/profile.d/openssl.sh
source /etc/profile.d/openssl.sh
echo $PATH
which openssl
'''

