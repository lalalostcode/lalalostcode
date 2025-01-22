# Interencdec

GIven the file called enc\_flag

```
YidkM0JxZGtwQlRYdHFhR3g2YUhsZmF6TnFlVGwzWVROclh6ZzJhMnd6TW1zeWZRPT0nCg==
```

I try to encode from the Cyber Chef and here's the result:

<figure><img src="../../../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

#### Decoding Process:

1. **Base64 Decoding**:
   * The `b'...'` format suggests it's a byte string in Python, and the content inside can be Base64-decoded.
   * Let's remove it

```
d3BqdkpBTXtqaGx6aHlfazNqeTl3YTNrXzg2a2wzMmsyfQ==
```

I attempted to decode from Base64 again, because it still has weird format if we insist to decode in caesar.

```
wpjvJAM{jhlzhy_k3jy9wa3k_86kl32k2}
```

The challenge gives clue the strings was encoded many time and base64, caesar mentioned on the description. So, let's move to Caesar Chiper for brute force possible shift

<figure><img src="../../../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

<details>

<summary>Flag</summary>

picoCTF{caesar\_d3cr9pt3d\_86de32d2}

</details>

