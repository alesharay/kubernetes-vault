** This section is an over simplification of the TLS process and PKI. Enough info is given solely to pass the exam. Read [https://smallstep.com/blog/everything-pki/](https://smallstep.com/blog/everything-pki/) for more information

- A certificate is used to guarantee trust between two parties during a transaction

- When a user tries to access a webserver, TLS certificates ensure that the communication between the user and the server is encrypted and the server is who it says it is

- Symmetric Encryption is a secure way of encrypting data

- Since symmetric encryption uses the same key to encrypt and decrypt data and since the key has to be exchanged between the sender and receiver, there is risk of a hacker gaining access to the key and decrypting the data

- This is where asymmetric encryption comes in

- With asymmetric encryption, instead of using a single key to encrypt and decrypt data, asymmetric encryption uses a pair of keys; a private key and a public key

- If you encrypt (lock) the data with your public key, you can only decrypt (unlock) it with the associated private key

- The private key must always be secure with you and not be shared with anyone else

- The public key may be shared with whoever you want

- To generate a public and private key, use the <span style="color:red">ssh-keygen</span> command

- The <span style="color:red">ssh-keygen</span> command creates two files

- By default, the filenames (if not given manually) are id_rsa and id_rsa.pub (if creating RSA keys)

![[tlsb-1.png]]

- The file ending in " .pub " is the public key

- Secure your server by locking down all access to it, except through a door that is locked using your public key

- This is done by adding an entry with your public key into the servers

- As long as no one gets their hands on the private key, which should be safe with you, no one can gain access to the server

- When you try to SSH into a server, you specify the location of your private key

![[tlsb-2.png]]

- To secure more than one server with your key-pair, create copies of your public key and place them on all of the servers you want to secure

- You can use the same private key to SSH into all of your servers securely

- If other users want access to your servers, they can then create their own key pairs and place the public keys on the servers

- To securely transfer a symmetric key from the client to the server, we use asymmetric encryption

- We generate a public and private key pair on the server
- We use openssl for this

- To use asymmetric encryption (openssl) to generate a private key, use the <span style="color:red">openssl genrsa -out</span> command

- Then use the <span style="color:red">openssl rsa -in</span> command to generate the accompanying public key

![[tlsb-3.png]]

- The user's browser encrypts the symmetric key by using the asymmetric public key provided by the server

- The symmetric key is now secure

- Since the hacker does not have the private key, it cannot decrypt the message

- It is possible to view the public key sent from the server to confirm it is the correct public key for your private key

- When a server sends the public key, it does not send it alone, it sends a TLS certificate with the public key in it

- TLS stands for transport layer security

- The TLS certificate has information about:

- Who (subject) it was issued to

- This field helps to validate the identity of the user

- The public key of the server
- The location of the server
- Etc…

![[tlsb-4.png]]

- If the certificate is for a webserver, the subject should match the url that the user types into their browser

- There is also a subject alternative names (SAN) section that allows additional names and aliases where you are able to access the site

- Hackers can fake certificates because anyone can generate them

- To confirm a certificate is legit, look at who signed and issued the certificate

- Certificate Authorities or CAs authorize certificates so that they are trusted

- These are well known organizations that can sign and validate certificates for you

- Some popular public Certificate Authorities include:

- Symantic
- Digicert
- Comodo
- GlobalSign
- Let's Encrypt

- To get a certificate signed, take the following steps

1. Generate a certificate signing request (CSR) using the private key you generated and the domain name of your website

1. This is done using the openssl command
2. This generates a CSR file which should then be sent to the CA for signing

![[tlsb-5.png]]

![[tlsb-6.png]]

1. Once the CA signs and validates your certificate, they send it back

- CAs use different techniques to confirm that you are the owner of that domain

- CAs themselves have a set of public and private key pairs

- The purpose of the CAs public and private keys is to sign certificates

- The public keys of all public CAs are built into browsers

- This is how the browser knows that the certificate was signed by the real CA and not a fake signature mimicking the CA

- To validate sites hosted privately, you can host your own private CAs

Summary

- To encrypt messages, we use asymmetric encryption with a pair of public and private keys
- An admin uses a pair of keys to secure SSH connectivity to the servers
- A server uses a pair of keys to secure HTTPS traffic
- The server first sends a certificate signing request (CSR) to the certificate authority (CA)
- The CA uses its private key to sign the CSR

- All browsers have a copy of the public CAs public key

- The signed certificate is sent back to the server
- The server configures the webapp with the signed certificate
- Whenever the user accesses the webapp, the server sends the certificate with its public key
- The user's browser reads the certificate and uses the CAs public key to validate and retrieve the server's public key
- The browser generates a symmetric key that it wishes to use going forward
- The symmetric key is encrypted using the server's public key and sent back to the server
- The server uses its private key to decrypt the message and retrieve the symmetric key

- The symmetric key is used for communication going forward,

- Once the user (user's browser) establishes trust with the webapp, they use a username and password to authenticate to the server
- For the server to validate that the client (browser) is who they say they are, the server can request a certificate from the client
- The client must generate a pair of keys and a signed certificate from a valid CA
- The client then sends the certificate to the server for it to verify that the client is who they say they are

- TLS client certificates are not generally implemented on websites

- When they are, it's done under the hood

- The entire infrastructure, including the CA servers, the people, and the process of generating, distributing, and maintaining digital certificates is known as public key infrastructure (PKI)

- Note that with the pair of public and private keys, you can both encrypt and decrypt data with either one of them

- But they only work with each other
- If data is encrypted with one key, it has to be decrypted with the other

- Using asymmetric encryption, you cannot both encrypt and decrypt data with the same key

- So if you encrypt your data with your private key, that means that anyone with your public key would be able to decrypt the data and read the message
- So be careful with your encryption

- Usually certificates with public keys have .pem or .crt extensions

- Usually private keys have .key or -key.pem extensions

- So private keys have the word "key" in them

![[tlsb-7.png]]