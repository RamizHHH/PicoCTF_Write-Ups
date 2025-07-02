# Hashcrack

- **Category:** [Cryptography]
- **Difficulty:** Easy
- **Description:** A company stored a secret message on a server which got breached due to the admin using weakly hashed passwords. Can you gain access to the secret stored within the server?

## 1. Problem Analysis

In this challenge we are meant to access a remote server using netcat and break into a server to retrive the secrets hidden by hashed passwords. My inital thought was to jump immediately to cyberchef to try and decode these hashes but cyberchef can only decode and not decrypt and so we need to figure out what type of hashing algorithm each hash used and to use online tools to decrypt them.

## 2. Inital Approach

We first need to use a terminal to gain access to the server using netcat. PicoCTF has a built in webshell but I prefer Kali Linux's terminal, after opening the terminal use the netcat command provided `nc verbal-sleep.picoctf.net 57356` and you are greeted with the following prompt:

```
Welcome!! Looking For the Secret?

We have identified a hash: 482c811da5d5b4bc6d497ffa98491e38
Enter the password for identified hash:
```
