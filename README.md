# Signing and Verification

Signing and Verification helps with both the integrity and authenticity.

**What is digest?**<br>
The digest is a kind of "verifier" calculated from the data blob by using a hash algorithm like sha256.<br>
The digest is actually the data which will be used to generate the signature.

**What is signature?**<br>
Shortly, the signature is protected version of the digest. At this points, keys join the process.

**What is verification?**<br>
Verification process is briefly comparing the calculated hash with the decrypted signature using the public key.


**Asymmetric Signing and Verification using OpenSSL Terminal**<br>
1. *Calculation of the digest with sha256:*<br>
To have binary formatted digest, use -binary option as below:<br>
openssl dgst -sha256 -binary input.data > digest <br>
To have a hex formatted digest, use -hex option as below:<br>
openssl dgst -sha256 -hex input.data > digest <br><br>
2. *Generation of the key pair based on using the RSA-4096:*<br>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;To generate 4096 bit length private key:<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;openssl genrsa -out priv_rsa_key.pem 4096<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Extract pub-key from it:<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;openssl rsa -in priv_rsa_key.pem -pubout > pub_rsa_key.pem<br>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;RSA priv key and pub key structure:<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-----BEGIN RSA PRIVATE KEY-----<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;RSAPrivateKey ::= SEQUENCE {<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;version           Version,<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modulus           INTEGER,  -- n<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;publicExponent    INTEGER,  -- e<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;privateExponent   INTEGER,  -- d<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;prime1            INTEGER,  -- p<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;prime2            INTEGER,  -- q<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;exponent1         INTEGER,  -- d mod (p-1)<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;exponent2         INTEGER,  -- d mod (q-1)<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;coefficient       INTEGER,  -- (inverse of q) mod p<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;otherPrimeInfos   OtherPrimeInfos OPTIONAL<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-----END RSA PRIVATE KEY-----<br><br>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-----BEGIN RSA PUBLIC KEY-----<br>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;RSAPublicKey ::= SEQUENCE {<br>
      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;modulus           INTEGER,  -- n<br>
      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;publicExponent    INTEGER   -- e<br>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}<br>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-----END RSA PUBLIC KEY-----<br>

  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;To see the all components of the priv key, execute the command below:<br>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;openssl rsa -in priv_rsa_key.pem -noout -text<br>

3. *Signing and verification:*<br>
openssl dgst -binary -sign priv-key.pem -keyform PEM -sha256 -out input.data.sign  input.data<br>
dgst: To calculate the digest of the data.<br>
-binary: To have binary formatted digest.<br>
-sign priv-key.pem : To sign the calculated digest using the priv-key.pm<br>
-keyform PEM : Key format of the private key.<br>
-sha256: The hash function used to calculate the digest.<br>
-out input.data.sign: The file to store the signature.<br>
input.data: The name of the input file. <br><br>

 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;openssl dgst -binary -verify key.pub -keyform PEM -sha256 -signature data.zip.sign data.zip<br>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;dgst: To calculate the digest of the data.<br>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-binary: To have binary formatted digest.<br>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-verify key.pub : To verify the calculated digest using the priv-key.pm<br>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-keyform PEM : Key format of the private key.<br>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-sha256: The hash function used to calculate the digest.<br>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-out input.data.sign: The name of the output file.<br>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;input.data: The name of the input file. <br><br>
