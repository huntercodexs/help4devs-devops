# GPG HELPER

## GPG Keys

#### Install the gpg program

<code>

    $ gpg --version
    gpg (GnuPG) 2.2.19
    libgcrypt 1.8.5
    Copyright (C) 2019 Free Software Foundation, Inc.
    License GPLv3+: GNU GPL version 3 or later <https://gnu.org/licenses/gpl.html>
    This is free software: you are free to change and redistribute it.
    There is NO WARRANTY, to the extent permitted by law.
    
    Home: /home/{USER}/.gnupg
    Supported algorithms:
    Pubkey: RSA, ELG, DSA, ECDH, ECDSA, EDDSA
    Cipher: IDEA, 3DES, CAST5, BLOWFISH, AES, AES192, AES256, TWOFISH,
            CAMELLIA128, CAMELLIA192, CAMELLIA256
    Hash: SHA1, RIPEMD160, SHA256, SHA384, SHA512, SHA224
    Compression: Uncompressed, ZIP, ZLIB, BZIP2

</code>

#### Generating a Key Pair

<pre>
gpg --gen-key
</pre>

###### Example

<code>

    $ gpg --gen-key
    gpg (GnuPG) 2.2.19; Copyright (C) 2019 Free Software Foundation, Inc.
    This is free software: you are free to change and redistribute it.
    There is NO WARRANTY, to the extent permitted by law.
    
    Note: Use "gpg --full-generate-key" for a full featured key generation dialog.
    
    GnuPG needs to construct a user ID to identify your key.
    
    Real name: {REAL-NAME}
    Email address: {EMAIL-ADDRESS}
    You selected this USER-ID:
        "{REAL-NAME} {EMAIL-ADDRESS}"
    
    Change (N)ame, (E)mail, or (O)kay/(Q)uit? O
    We need to generate a lot of random bytes. It is a good idea to perform
    some other action (type on the keyboard, move the mouse, utilize the
    disks) during the prime generation; this gives the random number
    generator a better chance to gain enough entropy.
    We need to generate a lot of random bytes. It is a good idea to perform
    some other action (type on the keyboard, move the mouse, utilize the
    disks) during the prime generation; this gives the random number
    generator a better chance to gain enough entropy.
    gpg: key 8190C4130ABA0F98 marked as ultimately trusted
    gpg: revocation certificate stored as
    '/home/{USER}/.gnupg/openpgp-revocs.d/CA925CD6C9E8D064FF05B4728190C4130ABA0F98.rev'
    public and secret key created and signed.
    
    pub   rsa3072 2021-06-23 [SC] [expires: 2023-06-23]
          CA925CD6C9E8D064FF05B4728190C4130ABA0F98
    uid                      {REAL-NAME} {EMAIL-ADDRESS}
    sub   rsa3072 2021-06-23 [E] [expires: 2023-06-23]

</code>

#### Listing Keys

###### Example

<code>

    $ gpg --list-keys
    /home/{USER}/.gnupg/pubring.kbx
    ---------------------------------
    pub   rsa3072 2021-06-23 [SC] [expires: 2023-06-23]
          CA925CD6C9E8D064FF05B4728190C4130ABA0F98
    uid           [ultimate] {REAL-NAME} {EMAIL-ADDRESS}
    sub   rsa3072 2021-06-23 [E] [expires: 2023-06-23]

</code>

where

<pre>
CA925CD6C9E8D064FF05B4728190C4130ABA0F98 = Keyid
</pre>

and

<pre>
{CA925CD6C9E8D064FF05B4728190C413}{0ABA0F98} = ShortID (last 8 characters)

use: gpg --list-keys --keyid-format short
</pre>

#### Multiple Keys

> HINT: In case you have multiple keys, the local gpg will use the first listed signature key (gpg --list-signatures)
> for any publishing steps, if you need to use a specific key you could add the details of the gpg key inside a 
<configuration> section in your pom.xml and use local settings.xml to discover the passphrase via the signature
> keyname. You may need to use the signature keyid in hexadecimal format:

<code>
    
    $ gpg --list-signatures --keyid-format 0xshort
    /home/mylocaluser/.gnupg/pubring.kbx
    ---------------------------------
    pub   rsa3072/0x3ABDEC12 2021-01-27 [SC] [expires: 2023-01-27]
    74524542545300A398653AB5242798823ABDEC12
    uid           [ultimate] Other Name <otheremail@example.com>
    sig 3        0x3ABDEC12 2021-01-27  Other Name <alarconj@gmail.com>
    sub   rsa3072 2021-01-27 [E] [expires: 2023-01-27]
    sig          0x3ABDEC12 2021-01-27  Julian Alarcon <alarconj@gmail.com>
    
    pub   rsa3072/0x0ABA0F98 2021-06-23 [SC] [expires: 2022-03-21]
    CA925CD6C9E8D064FF05B4728190C4130ABA0F98
    uid           [ultimate] Central Repo Test <central@example.com>
    sig 3        0x0ABA0F98 2021-06-24  Central Repo Test <central@example.com>
    sub   rsa3072/0x7C17C93B 2021-06-23 [E] [expires: 2023-06-23]
    sig          0x0ABA0F98 2021-06-23  Central Repo Test <central@example.com>
    
    You will find in the line that starts with sig 3 that 0x3ABDEC12 is the signature 
    keyid that you can use in your pom.xml.

</code>

#### Signing a File

<pre>
gpg -ab myfile.java
</pre>

Output should be something like

<pre>
myfile.java.asc
</pre>

#### Distributing Your Public Key

> NOTE: If you see the warning message about wrong permissions, for example: gpg:
> 
> WARNING: unsafe permissions on homedir '/home/{USER}/.gnupg'
> 
> Fix it using the instructions below:

<pre>
chown -R $(whoami) ~/.gnupg/
chmod 600 ~/.gnupg/*
chmod 700 ~/.gnupg
</pre>

###### Syntax

<pre>
gpg --keyserver {SERVER} --send-keys {KEY-ID}
</pre>

###### Example

<pre>
gpg --keyserver keyserver.ubuntu.com --send-keys CA925CD6C9E8D064FF05B4728190C4130ABA0F98
</pre>

#### GPG Key Servers List

<pre>
keyserver.ubuntu.com
keys.openpgp.org
pgp.mit.edu
</pre>

#### Import the gpg key

###### Syntax

<pre>
gpg --keyserver {SERVER} --recv-keys {KEY-ID}
</pre>

###### Example

<pre>
gpg --keyserver keyserver.ubuntu.com --recv-keys CA925CD6C9E8D064FF05B4728190C4130ABA0F98
</pre>

#### Edit the GPG Key

###### Syntax

<pre>
gpg --edit-key {KEY-ID}
</pre>

###### Example - Update Expired Date

<code>

    $ gpg --edit-key CA925CD6C9E8D064FF05B4728190C4130ABA0F98
    gpg (GnuPG) 2.2.19; Copyright (C) 2019 Free Software Foundation, Inc.
    This is free software: you are free to change and redistribute it.
    There is NO WARRANTY, to the extent permitted by law.
    
    Secret key is available.
    
    sec  rsa3072/8190C4130ABA0F98
         created: 2021-06-23  expires: 2023-06-23  usage: SC  
         trust: ultimate      validity: ultimate
    ssb  rsa3072/4656B4857C17C93B
         created: 2021-06-23  expires: 2023-06-23  usage: E   
    [ultimate] (1). Central Repo Test <central@example.com>
    
    gpg> 1
    sec  rsa3072/8190C4130ABA0F98
         created: 2021-06-23  expires: 2023-06-23  usage: SC  
         trust: ultimate      validity: ultimate
    ssb  rsa3072/4656B4857C17C93B
         created: 2021-06-23  expires: 2023-06-23  usage: E   
    [ultimate] (1)* Central Repo Test <central@example.com>
    gpg> expire
    Changing expiration time for the primary key.
    Please specify how long the key should be valid.
             0 = key does not expire
          <n>  = key expires in n days
          <n>w = key expires in n weeks
          <n>m = key expires in n months
          <n>y = key expires in n years
    Key is valid for? (0) 
    gpg> save

    $ gpg --keyserver keyserver.ubuntu.com --send-keys CA925CD6C9E8D064FF05B4728190C4130ABA0F98

</code>

#### Delete a Sub Key

Some PGP tools generates sub keys and use them for signing by default, but to make Maven tools recognize the 
signature, you must use the primary key to sign your artifacts.

<pre>
$ gpg --edit-key CA925CD6C9E8D064FF05B4728190C4130ABA0F98
gpg> key 2
gpg> delkey
gpg> save
</pre>
