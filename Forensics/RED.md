# DISKO 1

- **Category:** [Forensics]
- **Difficulty:** Easy
- **Description:** RED, RED, RED, RED

## 1. Problem Analysis

In this challenge, we are given a simple red square image, and we need to find the flag hidden within the image and its data.

## 2. Approach

In this problem we are given a red square image that we can download and analyze. We first use `wget` to download the image into the picoCTF browser terminal. After that we have our image and our goal is to find the flag hidden within it. To solve this challenge we need to use two different Linux terminal commands: `exiftool` and `zsteg`.

We use `exiftool` to look at the metadata of the image. This includes information such as the file type, size, dimensions and more. We use the command `exiftool red.png` and we get the following output:

```
ExifTool Version Number         : 12.40
File Name                       : red.png
Directory                       : .
File Size                       : 796 bytes
File Modification Date/Time     : 2025:03:06 03:34:15+00:00
File Access Date/Time           : 2025:08:08 19:12:33+00:00
File Inode Change Date/Time     : 2025:08:08 19:12:28+00:00
File Permissions                : -rw-rw-r--
File Type                       : PNG
File Type Extension             : png
MIME Type                       : image/png
Image Width                     : 128
Image Height                    : 128
Bit Depth                       : 8
Color Type                      : RGB with Alpha
Compression                     : Deflate/Inflate
Filter                          : Adaptive
Interlace                       : Noninterlaced
Poem                            : Crimson heart, vibrant and bold,.Hearts flutter at your sight..Evenings glow softly red,.Cherries burst with sweet life..Kisses linger with your warmth..Love deep as merlot..Scarlet leaves falling softly,.Bold in every stroke.
Image Size                      : 128x128
Megapixels                      : 0.016
```

We can see from the output above that it gives us a lot of information about the image. The most important and interesting part is the `Poem` field. Which we could not have seen by using a regular image viewer.

The poem reads as follows:

```
Crimson heart, vibrant and bold,.Hearts flutter at your sight..Evenings glow softly red,.Cherries burst with sweet life..Kisses linger with your warmth..Love deep as merlot..Scarlet leaves falling softly,.Bold in every stroke.
```

Looking at this poem, lets look at each capital letter and see if we can find a pattern:

```
C rimson
H earts
E venings
C herries
K isses
L ove
S carlet
B old
```

As we can see, each capital letter spells out the word `CHECKLSB`. This is a hint that we need to look at the least significant bits of the image to get the flag.

We can then use the `zsteg` command; the `zsteg` tool allows us to look at the least significant bits or LSB of the image. We use the command `zsteg red.png` and we get the following output:

```
meta Poem           .. text: "Crimson heart, vibrant and bold,\nHearts flutter at your sight.\nEvenings glow softly red,\nCherries burst with sweet life.\nKisses linger with your warmth.\nLove deep as merlot.\nScarlet leaves falling softly,\nBold in every stroke."
b1,rgba,lsb,xy      .. text: "cGljb0NURntyM2RfMXNfdGgzX3VsdDFtNHQzX2N1cjNfZjByXzU0ZG4zNTVffQ==cGljb0NURntyM2RfMXNfdGgzX3VsdDFtNHQzX2N1cjNfZjByXzU0ZG4zNTVffQ==cGljb0NURntyM2RfMXNfdGgzX3VsdDFtNHQzX2N1cjNfZjByXzU0ZG4zNTVffQ==cGljb0NURntyM2RfMXNfdGgzX3VsdDFtNHQzX2N1cjNfZjByXzU0ZG4zNTVffQ=="
b1,rgba,msb,xy      .. file: OpenPGP Public Key
b2,g,lsb,xy         .. text: "ET@UETPETUUT@TUUTD@PDUDDDPE"
b2,rgb,lsb,xy       .. file: OpenPGP Secret Key
b2,bgr,msb,xy       .. file: OpenPGP Public Key
b2,rgba,lsb,xy      .. file: OpenPGP Secret Key
b2,rgba,msb,xy      .. text: "CIkiiiII"
b2,abgr,lsb,xy      .. file: OpenPGP Secret Key
b2,abgr,msb,xy      .. text: "iiiaakikk"
b3,rgba,msb,xy      .. text: "#wb#wp#7p"
b3,abgr,msb,xy      .. text: "7r'wb#7p"
b4,b,lsb,xy         .. file: 0421 Alliant compact executable not stripped
```

From the output above we can see that the second line `b1,rgb,lsb,xy` gives us a very long string of text. Looking at the string, we can see that it is base64 encoded. Since it's encoded, we can only assume that it is the flag. Using the popular tool [cyberchef](https://gchq.github.io/CyberChef/) we can decode the base64 output and we get the flag.

## 3. Vulnerability

The main vulnerability in this challenge is that the image given to us contains metadata that we can exploit once we know what to look for. Metadata like the one in this challenge can contain hidden information that is not visible to the naked eye. If an attacker were to gain the knowledge of there being hidden information in the metadata of an image, they could use the tools we used in this challenge to extract the information and gain access to sensitive data. The idea of hidden information in images is known as steganography.

## 4. Flag

After using the command `zsteg red.png` and decoding the base64 output using cyberchef, we get the flag `picoCTF{******************}`.

## 5. Key Takeaways

- **Remember the tools that can be used to analyze images.**
- **Remember that hiding information in images is a common technique and can be exploited.**
- **Understand how we can use metadata to find hidden information.**
- **Remember steganography for future challenges.**
