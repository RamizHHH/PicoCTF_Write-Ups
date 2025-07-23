# EVEN RSA CAN BE BROKEN???

- **Category:** [Cryptography]
- **Difficulty:** Easy
- **Description:** This service provides you an encrypted flag. Can you decrypt it with just N & e?

## 1. Problem Analysis

In this challenge we are given a Python script that outputs some encrypted text we need to decrypt. To complete this challenge, we need to analyze the script and figure out how to decrypt the text it's giving us.

## 2. Approach

In this challenge we are given a python script to analyze and figure out how it is encrypting the flag so we can decrypt it and actually get the flag we need. Here is the script given to us:

```
from sys import exit
from Crypto.Util.number import bytes_to_long, inverse
from setup import get_primes

e = 65537

def gen_key(k):
    """
    Generates RSA key with k bits
    """
    p,q = get_primes(k//2)
    N = p*q
    d = inverse(e, (p-1)*(q-1))

    return ((N,e), d)

def encrypt(pubkey, m):
    N,e = pubkey
    return pow(bytes_to_long(m.encode('utf-8')), e, N)

def main(flag):
    pubkey, _privkey = gen_key(1024)
    encrypted = encrypt(pubkey, flag)
    return (pubkey[0], encrypted)

if __name__ == "__main__":
    flag = open('flag.txt', 'r').read()
    flag = flag.strip()
    N, cypher  = main(flag)
    print("N:", N)
    print("e:", e)
    print("cyphertext:", cypher)
    exit()

```

From this script we can see that the main function we need to look at is `gen_key`, this function generates the RSA key, which in turn encrypts the flag we need. Note that the challenge is using RSA which is a cryptosystem that is used for secure data transmission; you can read more about it [here](https://en.wikipedia.org/wiki/RSA_cryptosystem#Decryption). To decypt RSA we need six things: `p`, `q`, `N`, `e`, `d`, and the cypher `C`. we are either given or we can calculate all of them except for `p` and `q`, which are two prime numbers that make `N`.

Looking at `N`, we can see that it is an even number and every time we run the script, `N` is an even number so we can conclude that it is divisible by 2 and so we can conclude that one of the prime numbers that makes N is 2 and so we can get the other number by dividing `N` by 2.

To complete the rest of the challenge, we need a python script to compute these large numbers and to decode the final sequence of numbers into a string. This is the script I made below:

```
#Replace each variable with the ones given to you when you run the script
N = 14074657677452860262391970494798472523784906086795359766001745701253491070978783174209611318699878880181205790000308591947721823093266057442540885447169826
e = 65537
C = 10522371897414269361495290222351876068035765625697028140035965077849586326213132444606170014116573888271858455653475238697348642002666339877470383393792273

#Since all the N's the script gives us are even, we can factor out 2 as one of the prime factors.
p = 2

# Now we can find the other prime number q by dividing N by p
q = N // p

# Now we can calculate the private exponent d using the formula d = e^(-1) mod ((p-1)*(q-1))
d = pow(e, -1, (p - 1) * (q - 1))

# Now we can decrypt the ciphertext C using the private key (d, N)
m = pow(C, d, N)

# Convert the decrypted message m to bytes and decode it to a string
try:
    flag = m.to_bytes((m.bit_length() + 7) // 8, 'big').decode('utf-8')
    print("Flag:", flag)
except UnicodeDecodeError:
    print("The decrypted message is not a valid UTF-8 string.")
```

The formula for d is given in the original script and the formula for `m` can be found online or on the wiki I linked above. Note that the wiki has a different method for finding d but we have to use the method the script provides.

After running the script it will print the flag and then you can solve the challenge.

## 3. Vulnerability

The vulnerability in this challenge is that `p` and `q` are not random, since every `N` we generate is even, it can be determined that one of the numbers is 2 and that makes it very dangerous since a hacker can exploit this and determine the encrypted text. RSA requires that p and q be large, prime and have a large difference between them and if one of them isn't very large, a hacker could even brute-force it to decipher the key.

## 4. Flag

After running the script I provided or a similar one you could make, you can get the flag `picoCTF{**************}`

## 5. Key Takeaways

- **Always make sure that if you are using RSA, ensure `p` and `q` are very large and have a very large difference.**
- **When dealing with cryptography, try to look at patterns between outputs.**
- **Don't try to solve this on paper lol; always use a script.**
