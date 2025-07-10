# DISKO 1

- **Category:** [Forensics]
- **Difficulty:** Easy
- **Description:** Can you find the flag in this disk image?

## 1. Problem Analysis

In this challenge we are given a disk image that we can download and we are meant to find the flag that's hidden somewhere within the image.

## 2. Approach

In this challenge we can download the disk image using `wget` into our browser terminal that Pico provides for us. After doing so, we have the disk image in our folder:

```
RamizHHH-picoctf@webshell:~/DISKO_1$ ls
disko-1.dd.gz
```

After that we have to use the `gunzip` command to unzip the image since it is zipped. We can unzip it using the command `gunzip disko-1.dd.gz ` and after that we are left with `disko-1.dd`. Since this is a disk image we cannot use regular methods of looking through a file like `nano` or `cat`. Since we are dealing with a disk image, these are low-level disk partitions and have a variety of commands that we can use specifically on them. We can assume that the flag we want is a string of text within the image somewhere. Knowing this, we can use the `strings` command, which extracts all readable ASCII text from a binary file such as a .dd file. Knowing this, we can also use the `grep` command to search for the word "pico" within all the texts and once we do that, it gives us:

```
RamizHHH-picoctf@webshell:~/DISKO_1$ strings disko-1.dd | grep pico
:/icons/appicon
# $Id: piconv,v 2.8 2016/08/04 03:15:58 dankogai Exp $
piconv -- iconv(1), reinvented in perl
  piconv [-f from_encoding] [-t to_encoding]
  piconv -l
  piconv -r encoding_alias
  piconv -h
B<piconv> is perl version of B<iconv>, a character encoding converter
a technology demonstrator for Perl 5.8.0, but you can use piconv in the
piconv converts the character encoding of either STDIN or files
Therefore, when both -f and -t are omitted, B<piconv> just acts
picoCTF{***************************************}
```

This gives us the flag.

## 3. Vulnerability

The main vulnerability in a disk image is that it contains a lot of the storage device's partition and any files, system logs or information regarding the system that was used to create the file. With disk images like the one in this challenge, if an attacker were to gain access to one of these files, they could then use commands like `strings` to identify classified information about the system that was used to create the image.

## 4. Flag

After using `strings disko-1.dd | grep pico` we get the flag `picoCTF{************************}`.

## 5. Key Takeaways

- **Remember the commands that can be used with disk images.**
- **Remember what they are and what they can contain.**
- **Try to remember what the reasons are someone would make a disk image.**
