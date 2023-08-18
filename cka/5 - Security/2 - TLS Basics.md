** This section is an over simplification of the TLS process and PKI. Enough info is given solely to pass the exam. Read [https://smallstep.com/blog/everything-pki/](https://smallstep.com/blog/everything-pki/) for more information

- A [[3.1 - Certification Creation|certificate]] is used to guarantee trust between two parties during a transaction

- When a <i><span style="color:#477bbe">user</span></i> tries to access a <i><span style="color:#477bbe">servers</span></i>, <b><i><span style="color:#d46644">TLS certificates</span></i></b> ensure that the communication between the <i><span style="color:#477bbe">user</span></i> and the <i><span style="color:#477bbe">server</span></i> is <b><i><span style="color:#d46644">encrypted</span></i></b> and the <i><span style="color:#477bbe">server</span></i> is who it says it is

- <b><i><span style="color:#d46644">Symmetric Encryption</span></i></b> is a <i><span style="color:#477bbe">Secure</span></i> way of <b><i><span style="color:#d46644">encrypting</span></i></b> data

- Since <b><i><span style="color:#d46644">symmetric encryption</span></i></b> uses the same <b><i><span style="color:#d46644">key</span></i></b> to <b><i><span style="color:#d46644">encrypt</span></i></b> and <b><i><span style="color:#d46644">decrypt</span></i></b> data and since the <b><i><span style="color:#d46644">key</span></i></b> has to be exchanged between the sender and receiver, there is risk of a hacker gaining access to the <b><i><span style="color:#d46644">key</span></i></b> and <b><i><span style="color:#d46644">decrypting</span></i></b> the data
	- This is where <b><i><span style="color:#d46644">asymmetric encryption</span></i></b> comes in

- With <b><i><span style="color:#d46644">asymmetric encryption</span></i></b>, instead of using a single <b><i><span style="color:#d46644"><b><i><span style="color:#d46644">key</span></i></b></span></i></b> to encrypt and <b><i><span style="color:#d46644">decrypt</span></i></b> data, <b><i><span style="color:#d46644">asymmetric encryption</span></i></b> uses a pair of <b><i><span style="color:#d46644">keys</span></i></b>; a <b><i><span style="color:#d46644">private key</span></i></b> and a <b><i><span style="color:#d46644">public key</span></i></b>

- If you encrypt (lock) the data with your <b><i><span style="color:#d46644">public key</span></i></b>, you can only <b><i><span style="color:#d46644">decrypt</span></i></b> (unlock) it with the associated <b><i><span style="color:#d46644">private key</span></i></b>

- The <b><i><span style="color:#d46644">private key</span></i></b> must always be <i><span style="color:#477bbe">secure</span></i> with you and not be shared with anyone else

- The <b><i><span style="color:#d46644">public key</span></i></b> may be shared with whoever you want

- To generate a <b><i><span style="color:#d46644">public</span></i></b> and <b><i><span style="color:#d46644">private key</span></i></b>, use the <span style="color:red">ssh-keygen</span> command

- The <span style="color:red">ssh-keygen</span> command creates two files
	- By default, the filenames (if not given manually) are <span style="color:red">id_rsa</span> and <span style="color:red">id_rsa.pub</span> (if creating <b><i><span style="color:#d46644">RSA keys</span></i></b>)

![[tlsb-1.png]]

- The file ending in <span style="color:red">.pub</span> is the <b><i><span style="color:#d46644">public key</span></i></b>

- <i><span style="color:#477bbe">Secure</span></i> your <i><span style="color:#477bbe">server</span></i> by locking down all access to it, except through a door that is locked using your <b><i><span style="color:#d46644">public key</span></i></b>
	- This is done by adding an entry with your <b><i><span style="color:#d46644">public key</span></i></b> into the <i><span style="color:#477bbe">servers</span></i>

- As long as no one gets their hands on the <b><i><span style="color:#d46644">private key</span></i></b>, which should be safe with you, no one can gain access to the <i><span style="color:#477bbe">server</span></i>

- When you try to <i><span style="color:#477bbe">SSH</span></i> into a <i><span style="color:#477bbe">server</span></i>, you specify the location of your <b><i><span style="color:#d46644">private key</span></i></b>

![[tlsb-2.png]]

- To <i><span style="color:#477bbe">secure</span></i> more than one <i><span style="color:#477bbe">server</span></i> with your <b><i><span style="color:#d46644">key pair</span></i></b>, create copies of your <b><i><span style="color:#d46644">public key</span></i></b> and place them on all of the <i><span style="color:#477bbe">servers</span></i> you want to <i><span style="color:#477bbe">secure</span></i>
	- You can use the same <b><i><span style="color:#d46644">private key</span></i></b> to <i><span style="color:#477bbe">SSH</span></i> into all of your <i><span style="color:#477bbe">servers securely</span></i>

- If other <i><span style="color:#477bbe">users</span></i> want access to your <i><span style="color:#477bbe">servers</span></i>, they can then create their own <b><i><span style="color:#d46644">key pairs</span></i></b> and place the <b><i><span style="color:#d46644">public keys</span></i></b> on the <i><span style="color:#477bbe">servers</span></i>

- To <i><span style="color:#477bbe">securely</span></i> transfer a <b><i><span style="color:#d46644">symmetric key</span></i></b> from the <i><span style="color:#477bbe">client</span></i> to the <i><span style="color:#477bbe">server</span></i>, we use <b><i><span style="color:#d46644">asymmetric encryption</span></i></b>
	- We generate a <b><i><span style="color:#d46644">public</span></i></b> and <b><i><span style="color:#d46644">private key</span></i></b> pair on the <i><span style="color:#477bbe">server</span></i>
	- We use <b><i><span style="color:#d46644">openssl</span></i></b> for this

- To use <b><i><span style="color:#d46644">asymmetric encryption</span></i></b> (<b><i><span style="color:#d46644">openssl</span></i></b>) to generate a <b><i><span style="color:#d46644">private key</span></i></b>, use the <span style="color:red">openssl genrsa -out</span> command
	- Then use the <span style="color:red">openssl rsa -in</span> command to generate the accompanying <b><i><span style="color:#d46644">public key</span></i></b>

![[tlsb-3.png]]

- The <i><span style="color:#477bbe">user's</span></i> browser <b><i><span style="color:#d46644">encrypts</span></i></b> the <b><i><span style="color:#d46644">symmetric key</span></i></b> by using the asymmetric <b><i><span style="color:#d46644">public key</span></i></b> provided by the <i><span style="color:#477bbe">server</span></i>
	- The <b><i><span style="color:#d46644">symmetric key</span></i></b> is now <i><span style="color:#477bbe">secure</span></i>

- Since the hacker does not have the <b><i><span style="color:#d46644">private key</span></i></b>, it cannot <b><i><span style="color:#d46644">decrypt</span></i></b> the message

- It is possible to view the <b><i><span style="color:#d46644">public key</span></i></b> sent from the <i><span style="color:#477bbe">server</span></i> to confirm it is the correct <b><i><span style="color:#d46644">public key</span></i></b> for your <b><i><span style="color:#d46644">private key</span></i></b>

- When a <i><span style="color:#477bbe">server</span></i> sends the <b><i><span style="color:#d46644">public key</span></i></b>, it does not send it alone, it sends a <b><i><span style="color:#d46644">TLS certificate</span></i></b> with the <b><i><span style="color:#d46644">public key</span></i></b> in it

- <b><i><span style="color:#d46644">TLS</span></i></b> stands for <b><i><span style="color:#d46644">transport layer security</span></i></b>

- The <b><i><span style="color:#d46644">TLS</span></i></b> has information about:
	- Who (subject) it was issued to
		- This field helps to validate the identity of the <i><span style="color:#477bbe">user</span></i>
	- The <b><i><span style="color:#d46644">public key</span></i></b> of the <i><span style="color:#477bbe">server</span></i>
	- The location of the <i><span style="color:#477bbe">server</span></i>
	- Etc…

![[tlsb-4.png]]

- If the [[3.1 - Certification Creation|certificate]] is for a <i><span style="color:#477bbe">webserver</span></i>, the <span style="color:#5c7e3e">subject</span> should match the url that the <i><span style="color:#477bbe">user</span></i> types into their browser
	- There is also a <span style="color:#5c7e3e">subject alternative names</span> (<span style="color:#5c7e3e">SAN</span>) section that allows additional names and aliases where you are able to access the site

- Hackers can fake [[3.1 - Certification Creation|certificates]] because anyone can generate them

- To confirm a [[3.1 - Certification Creation|certificate]] is legit, look at who signed and issued the [[3.1 - Certification Creation|certificate]]

- <b><i><span style="color:#d46644">Certificate Authorities</span></i></b> or <b><i><span style="color:#d46644">CAs</span></i></b> authorize [[3.1 - Certification Creation|certificates]] so that they are trusted
	- These are well known organizations that can sign and validate [[3.1 - Certification Creation|certificates]] for you

- Some popular <b><i><span style="color:#d46644">public Certificate Authorities</span></i></b> include:
	- Symantic
	- Digicert
	- Comodo
	- GlobalSign
	- Let's Encrypt

- To get a [[3.1 - Certification Creation|certificate]] signed, take the following steps
	1. Generate a <b><i><span style="color:#d46644">certificate signing request</span></i></b> (<b><i><span style="color:#d46644">CSR</span></i></b>) using the <b><i><span style="color:#d46644">private key</span></i></b> you generated and the domain name of your website
		1. This is done using the <b><i><span style="color:#d46644">openssl</span></i></b> command
		2. This generates a <b><i><span style="color:#d46644">CSR</span></i></b> file which should then be sent to the <b><i><span style="color:#d46644">CA</span></i></b> for signing

	![[tlsb-5.png]]

	![[tlsb-6.png]]

	2. Once the <b><i><span style="color:#d46644">CA</span></i></b> signs and validates your [[3.1 - Certification Creation|certificate]], they send it back

- <b><i><span style="color:#d46644">CAs</span></i></b> use different techniques to confirm that you are the owner of that domain

- <b><i><span style="color:#d46644">CAs</span></i></b> themselves have a set of <b><i><span style="color:#d46644">public</span></i></b> and <b><i><span style="color:#d46644">private key pairs</span></i></b>
	- The purpose of the <b><i><span style="color:#d46644">CAs public</span></i></b> and <b><i><span style="color:#d46644">private keys</span></i></b> is to sign [[3.1 - Certification Creation|certificates]]

- The <b><i><span style="color:#d46644">public keys</span></i></b> of all <b><i><span style="color:#d46644">public CAs</span></i></b> are built into browsers
	- This is how the browser knows that the [[3.1 - Certification Creation|certificate]] was signed by the real <b><i><span style="color:#d46644">CA</span></i></b> and not a fake signature mimicking the <b><i><span style="color:#d46644">CA</span></i></b>

- To validate sites hosted privately, you can host your own <b><i><span style="color:#d46644">Private CAs</span></i></b>

### Summary

- To <b><i><span style="color:#d46644">encrypt</span></i></b> messages, we use <b><i><span style="color:#d46644">asymmetric encryption</span></i></b> with a pair of <b><i><span style="color:#d46644">public</span></i></b> and <b><i><span style="color:#d46644">private keys</span></i></b>
- An <i><span style="color:#477bbe">admin</span></i> uses a pair of <b><i><span style="color:#d46644">keys</span></i></b> to secure <i><span style="color:#477bbe">SSH</span></i> connectivity to the <i><span style="color:#477bbe">servers</span></i>
- A <i><span style="color:#477bbe">server</span></i> uses a pair of <b><i><span style="color:#d46644">keys</span></i></b> to <i><span style="color:#477bbe">secure HTTPS traffic</span></i>
- The <i><span style="color:#477bbe">server</span></i> first sends a <b><i><span style="color:#d46644">certificate signing request</span></i></b> (<b><i><span style="color:#d46644">CSR</span></i></b>) to the [[3.1 - Certification Creation|certificate]] authority (<b><i><span style="color:#d46644">CA</span></i></b>)
- The <b><i><span style="color:#d46644">CA</span></i></b> uses its <b><i><span style="color:#d46644">private key</span></i></b> to sign the <b><i><span style="color:#d46644">CSR</span></i></b>
	- All browsers have a copy of the <b><i><span style="color:#d46644">public CAs public key</span></i></b>
- The signed [[3.1 - Certification Creation|certificate]] is sent back to the <i><span style="color:#477bbe">server</span></i>
- The <i><span style="color:#477bbe">server</span></i> configures the webapp with the signed [[3.1 - Certification Creation|certificate]]
- Whenever the <i><span style="color:#477bbe">user</span></i> accesses the webapp, the <i><span style="color:#477bbe">server</span></i> sends the [[3.1 - Certification Creation|certificate]] with its <b><i><span style="color:#d46644">public key</span></i></b>
- The <i><span style="color:#477bbe"><i><span style="color:#477bbe">user's</span></i></span></i> browser reads the [[3.1 - Certification Creation|certificate]] and uses the <b><i><span style="color:#d46644">CAs public key</span></i></b> to validate and retrieve the <i><span style="color:#477bbe">server</span></i> <b><i><span style="color:#d46644">public key</span></i></b>
- The browser generates a <b><i><span style="color:#d46644">symmetric key</span></i></b> that it wishes to use going forward
- The <b><i><span style="color:#d46644">symmetric key</span></i></b> is <b><i><span style="color:#d46644">encrypted</span></i></b> using the server's <b><i><span style="color:#d46644">public key</span></i></b> and sent back to the <i><span style="color:#477bbe">server</span></i>
- The <i><span style="color:#477bbe">server</span></i> uses its <b><i><span style="color:#d46644">private key</span></i></b> to <b><i><span style="color:#d46644">decrypt</span></i></b> the message and retrieve the <b><i><span style="color:#d46644">symmetric key</span></i></b>
	- The <b><i><span style="color:#d46644">symmetric key</span></i></b> is used for communication going forward,
- Once the <i><span style="color:#477bbe">user</span></i> (<i><span style="color:#477bbe">user's</span></i> browser) establishes trust with the webapp, they use a username and password to [[1 - Authentication|authenticate]] to the <i><span style="color:#477bbe">server</span></i>
- For the <i><span style="color:#477bbe">server</span></i> to validate that the <i><span style="color:#477bbe">client</span></i> (browser) is who they say they are, the <i><span style="color:#477bbe">server</span></i> can request a [[3.1 - Certification Creation|certificate]] from the <i><span style="color:#477bbe">client</span></i>
- The <i><span style="color:#477bbe">client</span></i> must generate a pair of <b><i><span style="color:#d46644">keys</span></i></b> and a signed [[3.1 - Certification Creation|certificate]] from a valid <b><i><span style="color:#d46644">CA</span></i></b>
- The <i><span style="color:#477bbe">client</span></i> then sends the [[3.1 - Certification Creation|certificate]] to the <i><span style="color:#477bbe">server</span></i> for it to verify that the <i><span style="color:#477bbe">client</span></i> is who they say they are

- <b><i><span style="color:#d46644">TLS client certificate</span></i></b> are not generally implemented on websites
	- When they are, it's done under the hood

- The entire infrastructure, including the <i><span style="color:#477bbe">CA servers</span></i>, the people, and the process of generating, distributing, and maintaining digital [[3.1 - Certification Creation|certificates]] is known as <b><i><span style="color:#d46644">Public Key Infrastructure</span></i></b> (<b><i><span style="color:#d46644">PKI</span></i></b>)

- Note that with the pair of <b><i><span style="color:#d46644">public</span></i></b> and <b><i><span style="color:#d46644">private keys</span></i></b>, you can both <b><i><span style="color:#d46644">encrypt</span></i></b> and <b><i><span style="color:#d46644">decrypt</span></i></b> data with either one of them
	- But they only work with each other
	- If data is <b><i><span style="color:#d46644">encrypted</span></i></b> with one <b><i><span style="color:#d46644">key</span></i></b>, it has to be <b><i><span style="color:#d46644">decrypted</span></i></b> with the other

- Using <b><i><span style="color:#d46644">asymmetric encryption</span></i></b>, you cannot both <b><i><span style="color:#d46644">encrypt</span></i></b> and <b><i><span style="color:#d46644">decrypt</span></i></b> data with the same <b><i><span style="color:#d46644">key</span></i></b>
	- So if you <b><i><span style="color:#d46644">encrypt</span></i></b> your data with your <b><i><span style="color:#d46644">private key</span></i></b>, that means that anyone with your <b><i><span style="color:#d46644">public key</span></i></b> would be able to <b><i><span style="color:#d46644">decrypt</span></i></b> the data and read the message
	- So be careful with your <b><i><span style="color:#d46644">encryption</span></i></b>

- Usually [[3.1 - Certification Creation|certificates]] with <b><i><span style="color:#d46644">public keys</span></i></b> have <span style="color:red">.pem</span> or <span style="color:red">.crt</span> extensions

- Usually <b><i><span style="color:#d46644">private keys</span></i></b> have <span style="color:red">.key</span> or <span style="color:red">-key.pem</span> extensions
	- So <b><i><span style="color:#d46644">private keys</span></i></b> have the word "<b><i><span style="color:#d46644">key</span></i></b>" in them

![[tlsb-7.png]]