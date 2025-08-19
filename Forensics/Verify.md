# DISKO 1

- **Category:** [Forensics]
- **Difficulty:** Easy
- **Description:** People keep trying to trick my players with imitation flags. I want to make sure they get the real thing! I'm going to provide the SHA-256 hash and a decrypt script to help you know that my flags are legitimate.

## 1. Problem Analysis

In this challenge we are given a linux envriornment we have to SSH into and we need to locate the flag hidden within the environment that is also hidden behind a SHA-256 hash along with dozens of other hashes that are fake flags.

## 2. Approach

To solve this challenge, we first need to SSH into the provided Linux environment using the username and password given to us. After that we can use `ls` to see what's in our current directory. We will see `checksum.txt`, `decrypt.sh`, and `files`. If we go into the `files` directory, we see a bunch of different files that are all encrypted. We can use the `decrypt.sh` script to decrypt the files in `files`. We are also given a `checksum.txt` file that has the real flag's SHA-256 hash. We can use this to verify that we have found the correct flag. To solve this challenge, we first need to decrypt the files in the `files` directory; however, there are many encrypted files that are not the flag, so we need to first filter the one that is the same as the one in `checksum.txt`.

Since all the files are encrypted and one is the flag, we can use the `sha256sum` command to calculate the SHA-256 hash of each encrypted file and compare it with the hash in `checksum.txt`. To do this we can use the command `sha256sum files/* > hashes.txt` to create a file with all the hashes of the files in the `files` directory. We can then use `grep` to find the corresponding hash in `hashes.txt` that matches the one in `checksum.txt`.

We can use the command `grep b09c99c555e2b39a7e97849181e8996bc6a62501f0149c32447d8e65e205d6d2 hashes.txt`, `b09c99c555e2b39a7e97849181e8996bc6a62501f0149c32447d8e65e205d6d2` is the hash of the flag that we are given in `checksum.txt`. After running this command, we will see that it tells us that the file `451fd69b` is the one that contains the real flag. Now we can use the `decrypt.sh` script to decrypt the file. We run the command `./decrypt.sh files/451fd69b` and it will output the flag.

## 3. Vulnerability

The vulnerability is that the flag is hidden behind a simple SHA-256 hash, which can easily be decrypted using the same script that is provided. If an attacker is able to get access to the environment and sees that both the encryption method and decryption method are provided on the same workstation, they can easily decrypt the files and steal anything valuable like data or other hidden codes.

## 4. Flag

After decrypting the file `451fd69b`, we get the flag `picoCTF{*********************}`.

## 5. Key Takeaways

- **Remember to understand hashing and how it can be broken.**
- **Use the sha256sum command to calculate the SHA-256 hash of a file or string.**
- **Don't have the same encryption and decryption methods on the same system.**
