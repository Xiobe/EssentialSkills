# Introduction
GPG stands for GNU Privacy Guard, the homepage of the project can be found at https://gnupg.org/.

The main idea of GPG is to use the tool to use cryptography for handling daily challenges.

## Claiming identity
To prove that you are you is a pretty hard thing to do. The joke goes that online everybody can claim they are a dog but it is a pretty hard thing to proof that you are the one and only you. Cryptography allows us to do this and we will use GPG to do this.

## Signing documents or software
Once people can check if you are you we come to the crucial part, namely trust. If you read information or run software you want it to come from a trustable source. The only way to check this is you have actually signed your document or software.

## Encryption
Encryption is the possibility to take an input and change it in such a way that only the destinee can be the one reading it. According to Kerckhoff's principle (https://en.wikipedia.org/wiki/Kerckhoffs%27_principle0) you should be able to intercept the message and know what the used alogrithm is but as long as you don't know the key, the message should be safe.

# Randomness Generation
One of the important steps in working with GPG is to generate enough randomness on a system. The common terminology for randomness is entropy. Entropy is required to generate a unique key. This has a very important consequence since if you have exactly the same system under the same circumstances you might generate twice the same key and thus create a potential issue. 

## RGN Tools
To generate enough randomness we install rgn-tools
### installation
```
sudo apt-get install rng-tools
```
### configuration
```
sudo vi /etc/default/rng-tools
```
Now we tell rgn-tools to use /dev/random to generate the entropy by adding the following line:
```
HRNGDEVICE=/dev/urandom
```
### restart
```
sudo service rng-tools restart
```

# Key Management
## Key Generation
```
gpg --gen-key
```
Choose a RSA/RSA key with a 4096 bits key size that expires yearly. This means every year you will need to do a key roll over.

To make this tutorial we have made a key that only exists on the local system and lives for one day.

## List Public Key Ring
```
gpg --list-keys
```
To check out which keys are known to the system you can list the public key ring. By default the public keyring is stored in ~/.gnupg/pubring.gpg.
Example:
```
/home/user/.gnupg/pubring.gpg
-----------------------------
pub 2048/08096706 2016-11-07 [expires 2016-11-08]
uid               tester test <tester@test.org>
sub 2048R/C779FA1A 2016-11-07 [expires 2016-11-08]
```

## List Private Key Ring
```
gpg --list-secret-keys
```
By default the private keyring is stored in ~/.gnupg/secring.gpg
Example:
```
/home/user/.gnupg/secring.gpg
-----------------------------
sec 2048R/08096706 2016-11-07 [expires: 2016-11-08]
uid                tester test <tester@test.org>
sub 2048R/C779FA1A 2016-11-07
```
The keyID in our example is 0896706 for the public ket and C779FA1A for the private key.

## Changing your password
```
gpg --edit-key KeyID
```
You will be greeted by the gpg prompt and type password
```
gpg> password
```
Now you will be prompted to enter the old password, then choose a new password and type quit to exit.

## Public Key Export
```
gpg --export -a KeyID > public.key
```
This exports a key to the file public.key.

## Private Key Export
```
gpg --export-secret-key -a KeyID > private.key
```
This exports your private key. The idea is to exchange your public key and never your private key. Make sure you store a copy of your private key in a safe location.

## Public Key Import
```
gpg --import public.key
```
## Private Key Import
```
gpg --allow-secret-key-import --import private.key
```
## Delete Public Key
```
gpg --delete-key "user"
```
## Delete Private Key
```
gpg --delete-secret-key "user"
```
## Upload a key to a keyserver
```
gpg --keyserver pgp.mit.edu --send-keys PublicKeyID
```
This will send your keys to the MIT keyserver pgp.mit.edu

## Generating a revocation certificate
A revocation certificate is a certificate that allows you to revoke a public keyon a server without knowing the password. It is thus very important that the certificate is backed up and stored offline.
```
gpg --output revocation-certificate.asc --gen-revoke keyID
```
## Revoking a key
```
gpg --import revocation-certificate.asc
```
This revokes the key locally but you still need to make the rest of the world aware.
```
gpg --keyserver pgp.mit.edu --send-keys keyID
```
## Key Roll Over
```
gpg --edit-key YourPublicKeyID
```
This give you a gpg prompt, when you type list you will see the public key and its sub key.
```
gpg>list
```
By default no subkey is selected and thus you will be working on the primary key. If you wish to switch to the subkey you can invoke the key command. You pass the key command the index of the key you want to edit. If you don't pass key an index the primary key is assumed.
```
gpg> key index
```
If a subkey is selected you will see a little * next to the sub. Next we need to set the expiration date for both keys separately. It is perfectly possible for a pub key and a sub key to have a different expiration date.

To set a new expiration date you use the command expire and set its new date.
```
gpg> expire
```

Once you have set both expiration dates you need to save your changes.
```
gpg> save
```

Finally you need to reupload the keys again
```
gpg --keyserver pgp.mit.edu --send-keys PublicKeyID
```

You might notice that when you list your private key your sub key actually doesn't have an expiration date. This makes sense since they are not meant to be distributed.

# Key Signing
A key is easily generated and only worth the trust you put into it. To increase the trust in a key it can be signed by a third party that did a check that the key you claim to own is actually yours.

## Generating a key fingerprint
Instead of checking a complete key, you can give out a fingerprint of your key.
```
gpg --fingerprint > fingerprint
```

## Identity Verification
Before you sign a key you need to verify the identity of the person. This is the procedure to check identities:
1. You look up the fingerprint for the public key on http://pgp.mit.edu/ and write it down on the paper you take along to the meeting.
2. The person that you will meet needs to do the same.
3. At the meeting both verify if the fingerprints of the public keys match up.
4. Check 2 pieces of identification with photo ID.

Understand that this system is not fool-proof and it is better to know at least 2 persons who can vouch for somebody.

## Import their key
Search by email on the public key server
```
gpg --search-keys email_address
```
Request by Public Key from a public key server the keys
```
gpg --recv-keys PublicKeyID
```
Import the key from file
```
gpg --import public.key
```

## Sign the key
```
gpg --sign-key --ask-cert-level PublicKeyID
```

## Provide the signed key back
Once you have signed they keym it needs to be provided back to the owner.
```
gpg -a --export TheirPublicKey > TheirPublicKey.key
```
Provide the TheirPublicKey.key file back to the owner.

# Encryption of Data
## Armoring the output
The --armor option we use is to produce ASCII armored (base64 encoded) text. This makes it easier to pass on if you want to sent email via data but there is not a reason if you send data over a protocol like scp, sftp or ftp because these protocols are designed to transfer data.

## Version
To figure out what compression algorithms and encryption ciphers and public key types are available you can do
```
gpg --version
```

## Compression
You can specify that the data needs to be compressed
```
--compress-algo zip
--compress-algo zlib
--compress-algo bzip2
```
If your dqtq is not compressible and you choose none, it can give you approximately a 50% performance increase during the encryption. It is thus recommendable to know this upfront. If the data is compressible you will gain in encryption performance since the amount of data to transfer is smaller.

## Symmetric Encryption
Symmetric encryption means that you have a pre-shared password. This can be used in cases where you need to exchange data. The password is then shared over an out-of-bad channel for example in person.

In this example we will use the AES256 algorithm:
```
gpg --symmetric --cipher-algo AES256 --armor -o file.txt.gpg file.txt
```

The advantage of symmetric encryption is that anybody can handle the file, the disadvatage is that this means that anybody who can intercept the password can also handle the file. History has shown that the interception of the data/message is trivial and thus the only thing left is the key.

## Asymmetric Encryption
Asymmetric encryption means that only the destinee can read the datam not even the person who encrypted it can if he/she hasn't access to the original data. The main advantage is that thee is no private key exchange required, only the public keys need to be exchanged.
```
gpg -e -u SenderPublicKeyID -r ReceiverPublicKeyID -o file.txt.gpg file.txt
```

# Decryption of Data
## Symmetric Decryption
```
gpg -o file.txt -d file.txt.gpg
```
This will produce the file file.txt when you introduce the right password.
## Asymmetric Decryption
```
gpg -o file.txt -d file.txt.gpg
```
