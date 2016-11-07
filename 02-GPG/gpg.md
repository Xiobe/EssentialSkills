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

