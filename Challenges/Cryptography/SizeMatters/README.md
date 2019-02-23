# SizeMatters
In this challenge, the participant has to decrypt a RSA encrypted file which holds the flag. The directory *keys* holds all relevant keys.

**Description of the Challenge:**
I got an encrypted message but I can't read it... Can you help me?

## Setup
Encrypt a text using the RSA algorithm, but with very small key.

## Solution
```$ xxd cipher.txt```
This gives us ciphertext(ct):
ct(16) = 771f 4d59 05ed 6952 8a65 7c78 a8c1 a30c 50d3 00b9 6cf3 4084 0dfd 8545 b97e 9afb
ct(10) = 53880535074945013230435758976480263950284747331505425222992943706506535017211

Decrypt the public key with openssl to get the content:
```$ openssl rsa -in public_key.pub -pubin -text -out decrypted_key.pub```

This transforms to: 
```
Public-Key: (256 bit)
Modulus:
    00:d1:0c:b4:f8:e0:e2:47:35:31:1b:2e:ea:51:c4:
    92:7c:25:64:e7:be:a2:df:fb:07:aa:a8:04:47:7c:
    17:8b:37
Exponent: 65537 (0x10001)
-----BEGIN PUBLIC KEY-----
MDwwDQYJKoZIhvcNAQEBBQADKwAwKAIhANEMtPjg4kc1MRsu6lHEknwlZOe+ot/7
B6qoBEd8F4s3AgMBAAE=
-----END PUBLIC KEY-----
```

Our modulus is:
```00D10CB4F8E0E24735311B2EEA51C4927C2564E7BEA2DFFB07AAA804477C178B37```

We convert that to get our modulus n:
```$ echo 'ibase=16;00D10CB4F8E0E24735311B2EEA51C4927C2564E7BEA2DFFB07AAA804477C178B37' | bc```

We now have e and n. With `e = 65537` and `n = 94555836542772250304557228852117561016715118593147833737951475151898626460471`.

Next, find the factors p and q with a factorisation tool like yafu (or any other):
```
$ wine yafu-x64
>>factor(94555836542772250304557228852117561016715118593147833737951475151898626460471)
```

Now we have p and q:
```
p = 287707519340024534819672541734682755279
q = 328652642654856331261136192361290324249
```

With this 256 bit key on i5 8th gen energy saving mode it took 1m45s to factorise n. Now we have p, q, n, e. We need d to decrypt the cipher message. Because d is the inverse of e, we need to calculate the greatest common denominator of e and phi. `(phi = (p-1) * (q-1))`. At this time we have p, q, n, e, d and ct. With this, we can calculate plaintext `(pt) = ct^d % n`

We now have pt.
```pt = 3649853882500813017912310397880930091864139235441126989515418053276564746```

This numerical representation of our flag can now be converted to a string and we get a string which contains our flag.

The python script *solve_all.py* shows one way of solving the challenge.

## Flag
THICTF{CHuCK_noRRis}
