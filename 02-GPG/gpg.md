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

# Signing of Data
Signing of data is used to prove that you made some piece of work. For example the software packages you produce can be signed. This gives you a way to proof to your users that the software comes from you and isn't altered by another party and for example packed with a piece of malware.
```
gpg -s file.txt
```

The output file is data.gpg, which isn't human readable. IF you want a human readable output you can clearsign.
```
gpg --clearsign file.txt
```

It is absolutely possible to encrypt symmetrically and sign the data
```
gpg --symmetric --cipher-algo AES256 --armor --sign -o file.txt.gpg file.txt
```

It is also possible to encrypt asymmetrically and sign the data
```
gpg -e -u SenderPublicKeyID -r ReceiverPublicKeyID --sign -o file.txt.gpg file.txt
```

# Daily Use
## Checking a Linux Distro
Linux distributions need to be provided with a signature. In the past it was the case that they were provided with an MD5 hash but this only provided a guarantee that the ISO was completely downloaded. It has happened that the doenload servers got hacked and an altered versio was offered for download together with an altered MD5 hash.

By signing the ISO file the problem is different, the threat agent need the private key and the password besides access to the server. The person that downloads the ISO only needs the public key, the signature file and the ISO file.

The example underneath is an example for Tails, a distro that specializes in TOR communication.

Download the ISO, the signature file and the key:
```
wget http://ftp.free.fr/mirrors/tails.boum.org/tails/stable/tails-i386-2.5/tails-i386-2.5.iso
wget https://tails.boum.org/torrents/files/tails-i386-2.5.iso.sig
wget https://tails.boum.org/tails-signing.key
```

The key only needs to be imported once, if you have it already you don't need to do this a second time
```
gpg --import tails-signing.key
```

Now it is time to verify the download
```
gpg --keyid-format 0xlong --verify tails-i386-2.5.iso.sig  tails-i386-2.5.iso
```

It should output this:
```
gpg: Signature made Sun 31 Jul 2016 08:21:52 PM CEST
gpg: using RSA key 0x3C83DCB52F699C56
gpg: Good signature from "Tails developers (offline long-term identity key) <tails@boum.org>" [full]
gpg: aka "Tails developers <tails@boum.org>" [full]
Primary key fingerprint: A490 D0F4 D311 A415 3E2B  B7CA DBB8 02B2 58AC D84F
Subkey fingerprint: A509 1F72 C746 BA6B 163D  1C18 3C83 DCB5 2F69 9C56
```

## Linux
You can set your key as default in your ~/.bash_profile by doing
```
export GPGKEY=YourPublicKeyID
```

Next you will have to restart the gpg-agent
```
killall -q gpg-agent
eval $(gpg-agent --daemon)
export GPGKEY=YourPublicKeyID
```

## Keybase
Keybase is a solution to present ifentity as a series of statements. These statements are like:
* i own the twitter account @evanderhasselt
* i own this laptop
* i own this phone
* i own this tablet
* i don't own this phone anymore, it got stolen

The idea is to havea public repository of your public accounts. You have a public profile that allows you to tell which accounts are yours and so when somebody creates an account to impersonate you it is quite easy to tell that that account doesn't belong to you.

You can thus publish your gpg key easily.

https://keybase.io

## Mailvelope
Mailvelope is a browser plugin for Chrome and Firefox which allows you to use gpg encryption for Gmai, Yahoo Mail, Outlook.com and GMX.

https://www.mailvelope.com

## Emailing Attachments
It is important to realize that attachments need to be signed seperately and uploaded to the web client seperately. The previously discussed mailvelope can do this for Gmail, Yahoo Mail, Outlook.com and GMX.
