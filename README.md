# consul-vault-offline-ca-root
## Steps to validate
1. Configure you current Kubernetes context.
1. Run `sh install.sh` from the root of this directory.
1. When prompted for a password for the certificate generation, leave it empty.  Confirm.
1. Once process says it has succeeded, copy `./certs/root/ca.crt` to your local trust store (login items on Mac).
1. Click on the Cert and open the Trust section. Select Always Trust.  Save.
1. Portforward to the server port 8501: 
    ``` bash
    kubectl port-forward service/consul-server 8501:8501
    ```
1. Verify the cert from the Consul UI has a trust chain back to the Offline Root using the following command
    ``` bash
    openssl s_client -connect localhost:8501 -CAfile ./certs/root/ca.crt
    ```
1. Your output should look like this with the `Verify return code: 0 (ok)` at the bottom:
``` bash
CONNECTED(00000005)
depth=2 C = US, ST = California, L = San Francisco, O = HashiConf, OU = HashiConf EU, CN = Consul CA Root
verify return:1
depth=1 C = US, ST = California, L = San Francisco, O = HashiConf, OU = HashiConf Europe, CN = Consul CA1 v1
verify return:1
depth=0 CN = server.dc1.consul
verify return:1
---
Certificate chain
 0 s:/CN=server.dc1.consul
   i:/C=US/ST=California/L=San Francisco/O=HashiConf/OU=HashiConf Europe/CN=Consul CA1 v1
 1 s:/C=US/ST=California/L=San Francisco/O=HashiConf/OU=HashiConf Europe/CN=Consul CA1 v1
   i:/C=US/ST=California/L=San Francisco/O=HashiConf/OU=HashiConf EU/CN=Consul CA Root
---
Server certificate
-----BEGIN CERTIFICATE-----
MIIEcDCCA1igAwIBAgIUUE0kGuN6gc2k8tLzW03lvf0PKJIwDQYJKoZIhvcNAQEL
BQAwgYExCzAJBgNVBAYTAlVTMRMwEQYDVQQIEwpDYWxpZm9ybmlhMRYwFAYDVQQH
Ew1TYW4gRnJhbmNpc2NvMRIwEAYDVQQKEwlIYXNoaUNvbmYxGTAXBgNVBAsTEEhh
c2hpQ29uZiBFdXJvcGUxFjAUBgNVBAMTDUNvbnN1bCBDQTEgdjEwHhcNMjIwNjAy
MDEyMzEyWhcNMjIwNzAyMDEyMzQyWjAcMRowGAYDVQQDExFzZXJ2ZXIuZGMxLmNv
bnN1bDCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBANznzH0vNBlDXQ0k
BIGaQ8A832zu8RgLfbXc4v0QFltQTUxUlhIgJ+Czm+X4KoYITMoIehthK0Ek+PiH
SBimAY2xj7di/IraVOgaqQ68naFvKDqEEJdPYRg4nKkdowktaJzbUsfZEUxQ88J2
V7nR7J92UKM7XyJhZBdgN9byPwprgZjCVxl2HtCmhvuGNBJEA1Qo/zEB9MFjK3a3
LxfIkJGdcQRsyBSinonz7wav2S6V8SCgQDOM1ReueHjKLbmb2b8zzyRwRfcRzp+P
aQmDX+mi/KyRxTByuA8nqShr+XhTE7xKnU1XXn0q+h4doZXDrQghz3CInoDq/vK0
zuTavqMCAwEAAaOCAUIwggE+MA4GA1UdDwEB/wQEAwIDqDAdBgNVHSUEFjAUBggr
BgEFBQcDAQYIKwYBBQUHAwIwHQYDVR0OBBYEFG4zC8DJAZp+shdCo6HG6DC3P+Z/
MB8GA1UdIwQYMBaAFN8JIeWBQkWU0aLC4sulnr0AfkIxMIHMBgNVHREEgcQwgcGC
DyouY29uc3VsLXNlcnZlcoIXKi5jb25zdWwtc2VydmVyLmRlZmF1bHSCGyouY29u
c3VsLXNlcnZlci5kZWZhdWx0LnN2Y4ITKi5zZXJ2ZXIuZGMxLmNvbnN1bIINY29u
c3VsLXNlcnZlcoIVY29uc3VsLXNlcnZlci5kZWZhdWx0ghljb25zdWwtc2VydmVy
LmRlZmF1bHQuc3Zjgglsb2NhbGhvc3SCEXNlcnZlci5kYzEuY29uc3VshwR/AAAB
MA0GCSqGSIb3DQEBCwUAA4IBAQA9/l6IveBMF4Byozv7UV2y7HqoIAC1+0x0SbT6
DQ1+Ac+syPNrzarfIGlGwtxnpXk8u5YXiEqKXGPytRr/sl5xAENReRRCzS08VtlC
C6eesU/w4MaPiA3pdB65GFCj7B1wLRhPO5bPpeop0dznUkFWNz1jGSUt7qm2M0m1
ED0rx/hyFerIfG10v3tzcLXyF77/XAfCp6Xyq/0nl2vdvcJLu+6hXYBqtBYlp7lW
Rz8FoWVlKtEL1jrYBP5mgeqp1mL0hiLGSkRb2k9JjITBZdzgc3cL0Gl+9c+Z7nga
QILYxymjNL6FA6MlQCj273YgxHV/s5+6hDmv0qbWOj8DTin/
-----END CERTIFICATE-----
subject=/CN=server.dc1.consul
issuer=/C=US/ST=California/L=San Francisco/O=HashiConf/OU=HashiConf Europe/CN=Consul CA1 v1
---
No client certificate CA names sent
Server Temp Key: ECDH, X25519, 253 bits
---
SSL handshake has read 2989 bytes and written 281 bytes
---
New, TLSv1/SSLv3, Cipher is ECDHE-RSA-CHACHA20-POLY1305
Server public key is 2048 bit
Secure Renegotiation IS supported
Compression: NONE
Expansion: NONE
No ALPN negotiated
SSL-Session:
    Protocol  : TLSv1.2
    Cipher    : ECDHE-RSA-CHACHA20-POLY1305
    Session-ID: EE3DCB4F4E12F373E80F2BB277A74D032B292AF8261783FC2020DE184037128F
    Session-ID-ctx:
    Master-Key: 19072E980101C4ABA49790AF3A03C07600CF2F09285CEAF0EEAC66E281BA9D8CE5C6EE7B756334CC0061F2009AE92522
    TLS session ticket:
    0000 - 1b ba 46 f7 9c 9a 7a bc-5f bd 18 03 af 67 99 70   ..F...z._....g.p
    0010 - 2c 13 e4 c5 71 b9 88 dd-19 b7 c0 40 05 cf da 49   ,...q......@...I
    0020 - d2 86 11 89 7c 67 a0 29-e9 7f 12 6f 4c 9a ae d5   ....|g.)...oL...
    0030 - 1c d7 92 ae 5a ad 19 88-9e 2c fd de 04 2d 08 97   ....Z....,...-..
    0040 - 98 2e 6d a0 3b 32 40 d2-0d e2 02 ba 19 6b 88 86   ..m.;2@......k..
    0050 - e2 fa 72 1c e9 cc ef 5d-52 41 df 64 f2 07 05 db   ..r....]RA.d....
    0060 - af 00 c3 73 b3 02 fa 43-1c f5 6c 35 fb b4 08 cd   ...s...C..l5....
    0070 - 34 8c a2 df d2 ee 66 56-69 2c 67 4a be 90 69 a9   4.....fVi,gJ..i.
    0081 - <SPACES/NULS>

    Start Time: 1654133799
    Timeout   : 7200 (sec)
    Verify return code: 0 (ok)
---
```

*Note:  Trying to confirm in the browser on a macOS leads to a `certificate is not standards compliant` which is related I think to the restrictions on macOS.  I tried troubleshooting this by making the end date of the cert 1 year instead of 3 (mac imposes an 825 maximum for non-public certs).  There still seems to be something else related to the browser and my self signed cert, but I believe to be more of an issue using a dev cert than related to the logic of displaying the Trust Chain, since the browser does properlydisplay that.
