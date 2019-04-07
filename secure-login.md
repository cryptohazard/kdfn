# Secure Client Login

I summarize here the state of art to implement a Client login based on ~~passwords~~ passphrases. This is the most widely used form of login and unfortunately, not the most secure.

### 2FA
The most used is the phone number based 2FA. We also see 2FA with email validation at each login or with some hardware dongle or card.
For mobile phone 2FA, we need to pay attention: is the second factor on the phone itself or the phone number(sim card)? If the security relies on the mobile phone itself, the client has to be wary of hack and theft. While on the phone number, the client also has to be wary of Sim card swap: an attacker bypass the security, if any, of the mobile phone carrier and get a new sim card with the client number. 

### OTP
One Time Password.

------------------
We have the following steps to secure clients login:
* client server communications
* passphrase generation
* passphrase storage

## client server communications
There should be  a secure canal of communication between the client and the server. This is usually an HTTPS connexion. For other settings, SSH is the other option.
## passphrase generation
You need to check the passphrase strengh with something like zxcvbn:
https://github.com/dropbox/zxcvbn

You can also recommend your users to adopt a password manager like Lastpass, (cite other)

## passphrase storage
A good reference: https://crackstation.net/hashing-security.htm
```

User: email@address               -------------->            :Server

User:                            <-------------- User_Salt   :Server

User: H(passphrase||User_Salt)    -------------->            :Server
```

**Note 1**: The *User_Salt* should be generated, with a secure random generator, by the Server. It does not need to be secret. The server stores in a database: (email, User_Salt, H(passphrase||User_Salt).

**Note 2**: We are leaking here which email is in the registered in the system. If such privacy is important, the salt should be computed from the user id. For example User_Salt= sha256(user_id). Also using directly email(but not pseudo) as salt should be fine.

**Note 3**: *H* is a a slow secure cryptographic hash function that slows down an attacker. We recommend using scrypt and present below a benchmark and choice of parameters. Some other functions: argon2, bcrypt, PBKDF2.

#### Benchmark and configuration for scrypt
We can recommend the following parameters for scrypt: ```N=2^16, r=8, p=1```.
Here is a benchmark, in JS, to time the login of a client:
https://github.com/kingswisdom/SteemMessenger/tree/main/view/js/tests/scrypt_benchmark

Please note that while a client might log in with JS, an attacker would use faster programs(like in C/C++), parallelization(notably with GPU) to attack the system. 
