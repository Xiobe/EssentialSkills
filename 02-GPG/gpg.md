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

