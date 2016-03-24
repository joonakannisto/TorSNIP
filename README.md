TorSNIP
=======

SNI proxying for onion addresses.

Problem description
===================
Current tor2web implementation is not end-to-end secure. The tor2web server decrypts the HTTPS connection and forms the onion circuit, and relays that to the target hidden service. It would be better if the HTTPS connection would be made directly to the onion site.

Possible Solution
=================
It is possible to know the server name from TLS's Server Name Indication (SNI).
https://github.com/dlundquist/sniproxy
The SNI proxy can multiplex the same ip address for multiple target servers behind a NAT.

How it should work
==================
Client connects to msydqstlz2kzerdg.tornips.org using HTTPS to port 443.
Server at tornips.org sees that the client is trying to reach msydqstlz2kzerdg.onion through its proxy server.
Server creates a hidden service circuit to msydqstlz2kzerdg.onion and sends all the TLS handshake first packet and all subsequent traffic to that circuit.
The target hidden service needs to listen to port 443, and have HTTPS service there with a name msydqstlz2kzerdg.tornips.org, and a valid certificate. The service can get a valid certificate using letsencrypt.

Techical details
================
First it is maybe easier to do a static mapping to a few onion addresses as a proof of concept. Making proxy services that represent a certain address in some port in the localhost. After that the dlundquist code needs to be forked most probably. The current code could also do name lookups'n shit. That would then require doing an internal network plus a name server for the initial proof of concept. 
