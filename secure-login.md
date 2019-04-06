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

#### Benchmark and configuration for scrypt
https://github.com/kingswisdom/SteemMessenger/tree/main/view/js/tests/scrypt_benchmark
