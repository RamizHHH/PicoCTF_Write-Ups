# Hashcrack

- **Category:** [Cryptography]
- **Difficulty:** Easy
- **Description:** A company stored a secret message on a server, which got breached due to the admin using weakly hashed passwords. Can you gain access to the secret stored within the server?

## 1. Problem Analysis

In this challenge we are meant to access a remote server using netcat and break into a server to retrieve the secrets hidden by hashed passwords. To solve this we first need to find what kind of hashing algorithm was used to make the hashes and how we can decode them.

## 2. Approach

We first need to use a terminal to gain access to the server using netcat. PicoCTF has a built-in web shell, but I prefer Kali Linux's terminal, after opening the terminal, use the netcat command provided `nc verbal-sleep.picoctf.net 57356` and you are greeted with the following prompt:

```
Welcome!! Looking For the Secret?

We have identified a hash: 482c811da5d5b4bc6d497ffa98491e38
Enter the password for identified hash:
```

We can see the hash given to us and we need to decrypt it to get the password that's hidden. Firstly, we can decipher what hashing algorithm is being used by using a website called hashes.com, we can simply paste the hash `482c811da5d5b4bc6d497ffa98491e38` into the website and it will tell us it is MD5. We can then use a tool to decrypt it and we can use one called decode.fr, we can search for MD5 and use the decoder tool to decode this hash. The hash gives us `password123` and once we insert the password into the terminal, we get through the first block and then there is a second block requiring us to decrypt the password again. We can repeat the same steps as before, use hashes.com to find the hash type and decode.fr to decode the hash.

## 3. Vulnerability

The core vulnerability in this challenge is the weak hashing of passwords. These passwords were easy to decrypt and that makes it a serious vulnerability if a company or person were to use passwords and hashes like these. Using just two online tools, we were able to decrypt 3 passwords in less than 5 minutes, and that exploit can be very serious for organizations.

## 4. Flag

Once you get through the first, second and third block it reveals the flag as `picoCTF{************************}`.

## 5. Key Takeaways

- **Use better hashing methods**
- **Remember that tools like hashes.com and decode.fr exist and make sure to use them**
