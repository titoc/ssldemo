# Troubleshooting SSL handshake issue


 3CE’s ssl connection supports:  TLSv1.2 and the below cipher suites as listed from the nmap command.
 In order for a client to connect, the client & server should agree on the TLS version and
 the cipher to be used during handshake. The handshake will fail if there is no agreement.
 Since 3CE uses high strength ciphers, the client should enable unlimited strength policy.
 In the most recent versions of JDK, high strength ciphers are enabled by default.
 If you are getting the below error, its because the client & server cannot negotiate ssl.
 
```
javax.net.ssl.SSLHandshakeException: Received fatal alert: handshake_failure
```

 To fix this via code, just add the below snippet right after the main method in Java.
 
```
     Security.setProperty("crypto.policy", "unlimited");
```

 If you are still having handshake errors after adding the below line,
 then your JDK/JRE version may not be enabled for high strength ciphers by default.

 Follow the instructions in the link below in such cases to enable
 Unlimited Strength Jurisdiction Policy Files

 http://opensourceforgeeks.blogspot.com/2014/09/how-to-install-java-cryptography.html


```
 nmap --script ssl-enum-ciphers -p 443 auth.3ce.com

 Starting Nmap 7.01 ( https://nmap.org ) at 2018-12-11 12:08 EST
 Nmap scan report for auth.3ce.com (192.99.46.133)
 Host is up (0.0025s latency).
 rDNS record for 192.99.46.133: ns500552.ip-192-99-46.net
 PORT    STATE SERVICE
 443/tcp open  https
 | ssl-enum-ciphers:
 |   TLSv1.2:
 |     ciphers:
 |       TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (secp384r1) - A
 |       TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384 (secp384r1) - A
 |       TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA (secp384r1) - A
 |       TLS_DHE_RSA_WITH_AES_256_GCM_SHA384 (dh 4096) - A
 |       TLS_DHE_RSA_WITH_AES_256_CBC_SHA256 (dh 4096) - A
 |       TLS_DHE_RSA_WITH_AES_256_CBC_SHA (dh 4096) - A
 |     compressors:
 |       NULL
 |     cipher preference: server
 |_  least strength: A

 Nmap done: 1 IP address (1 host up) scanned in 10.94 seconds

```
