## Overview of Application
This application simulates a secure client and server that communicate through a VPN.

## Format of an Unsigned Certificate
The format of an unsigned certificate is [IP address]:[port]:[public key]. So for the IP address "127.0.0.1", port number "65432", and public key "(25243, 56533)", the unsigned certificate would be "127.0.0.1:65432:(25243, 56533)"

## Example Output
### Secure Server Output
```
Generated public key '(25243, 56533)' and private key '31290'
Connecting to the certificate authority at IP 127.0.0.1 and port 55553
Prepared the formatted unsigned certificate '127.0.0.1:65432:(25243, 56533)'
Connection established, sending certificate '127.0.0.1:65432:(25243, 56533)' to the certificate authority to be signed
Received signed certificate 'D_(2397, 56533)[127.0.0.1:65432:(25243, 56533)]' from the certificate authority
server starting - listening for connections at IP 127.0.0.1 and port 65432
Connected established with ('127.0.0.1', 57418)
sending certificate D_(2397, 56533)[127.0.0.1:65432:(25243, 56533)] to client
received encrypted symmetric key E_(25243, 56533)[31444] from client
TLS handshake complete: established symmetric key '31444', acknowledging to client
Received client message: 'b'HMAC_21519[symmetric_31444[Hello, world]]'' [41 bytes]
Decoded message 'Hello, world' from client
Responding 'Hello, world' to the client
Sending encoded response 'HMAC_21519[symmetric_31444[Hello, world]]' back to the client
server is done!
```

### VPN Output
```
VPN starting - listening for connections at IP 127.0.0.1 and port 55554
Connected established with ('127.0.0.1', 57417)
Received client message: 'b'127.0.0.1~IP~65432~port~request TLS'' [35 bytes]
connecting to server at IP 127.0.0.1 and port 65432
server connection established, sending message 'request TLS'
message sent to server, waiting for reply
Received server response: 'b'D_(2397, 56533)[127.0.0.1:65432:(25243, 56533)]'' [47 bytes], forwarding to client
Received client message: 'b'E_(25243, 56533)[31444]'' [23 bytes], forwarding to server
Received server response: 'b"symmetric_31444[Symmetric key '31444' received]"' [47 bytes], forwarding to client
Received client message: 'b'HMAC_21519[symmetric_31444[Hello, world]]'' [41 bytes], forwarding to server
Received server response: 'b'HMAC_21519[symmetric_31444[Hello, world]]'' [41 bytes], forwarding to client
VPN is done!
```

### Secure Client Output
```
Connecting to the certificate authority at IP 127.0.0.1 and port 55553
Connection established, requesting public key
Received public key (54136, 56533) from the certificate authority for verifying certificates
Client starting - connecting to VPN at IP 127.0.0.1 and port 55554
received certificate D_(2397, 56533)[127.0.0.1:65432:(25243, 56533)] from server
sending encrypted key E_(25243, 56533)[31444] to server
TLS handshake complete: sent symmetric key '31444', waiting for acknowledgement
Received acknowledgement 'Symmetric key '31444' received', preparing to send message
Sending message 'HMAC_21519[symmetric_31444[Hello, world]]' to the server
Message sent, waiting for reply
Received raw response: 'b'HMAC_21519[symmetric_31444[Hello, world]]'' [41 bytes]
Decoded message 'Hello, world' from server
client is done!
```

## TLS Handshake Walkthrough (the steps of a TLS handshake and what each step accomplishes

1. The client requests the TLS handshake from the server in order to initiate the process.
2. The server receives the request and sends back its signed certificate to the client. This tells the client what the server's public key is, and because the certificate is signed, it allows the client to verify the server's identity with the certificate authority.
3. The client verifies the certificate with the certificate authority's public key and verifies that they're communicating with the correct server as explained in the previous step.
4. The client generates a symmetric key, uses the server's public key to encrypt it, and sends it to the server. The client encrypts it with the server's public key so that no one else can read what their symmetric key is.
5. The server receives the encrypted symmetric key and decrypts it using their private key, and the server is the only person who has the private key corresponding to their public key. Now both the client and server have the symmetric key and are able to communicate with it. 

## Simulation Failures

### Asymmetric Key Generation
The public key is a tuple consisting of a random number between 1 and p and p. The private key is the second number of the public key, p, minus the first number, so if you know the public key then you can find the private key and your messages won't be secure. Also, in this simulation, p is the same number every time you run the program, making it easier to figure out how the private key is generated.

### Encryption Algorithm
The encryption algorithms just add a heading and brackets around the message, so a third party like the VPN is able to read all of the messages.

## Acknolwedgements
I did not collaborate with anyone on this assginment.