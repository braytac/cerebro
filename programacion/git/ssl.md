server certificate verification failed. CAfile: /etc/ssl/certs/ca-certificates.crt CRLfile: none

GIT_SSL_NO_VERIFY=1 git clone 


git config --global http.sslVerify "false"
desactivarlo de forma permanente
