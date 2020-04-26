# SMIC 2

### SUBJECT

> La sécurité du cryptosystème RSA repose sur un problème calculatoire bien connu.
> 
> On vous demande de déchiffrer le "message" chiffré `c` ci-dessous pour retrouver le "message" en clair `m` associé à partir de la clé publique `(n, e)`.
> 
> Valeurs :
> 
> - `e = 65537`
> - `n = 632459103267572196107100983820469021721602147490918660274601`
> - `c = 63775417045544543594281416329767355155835033510382720735973`
>
>  Le flag est `FCSC{xxxx}` où `xxxx` est remplacé par la valeur de `m` en écriture décimale.  

In this challenge, we must do the opposite of the SMIC 1, decrypt the message by finding his private key.  

### RSA DECRYPTION, A QUESTION OF FACTORIZATION

As a reminder, here is how an RSA decryption works.  
Decryption requires knowing the private key `d` and the public key `n`.  
For any encrypted message `c`, the plain message `m` is computed :
`m = (c ** d) % n`  

As a hacker, I must be able to realize the prime factor decomposition of the number `n` to find its 2 factors `p` and `q`, needed to compute the private key.  
This attack is based on the fact that the prime numbers used to generate the keys must be long enough to make the encryption more powerful. The shorter `n`, the faster it will be to go the opposite way with the calculation algorithms we currently have.  

### RESTORE THE PRIVATE KEY

Thanks to https://factordb.com, I was able to find `p` and `q` from `n` :
![factordb](/images/factordb.png)

Now, I am able to reconstitute my private key and decrypt the message.

```python3
#!/usr/bin/env python

from Crypto.Util.number import inverse

e = 65537
n = 632459103267572196107100983820469021721602147490918660274601
c = 63775417045544543594281416329767355155835033510382720735973

p = 650655447295098801102272374367
q = 972033825117160941379425504503

phi = (p - 1) * (q - 1)

d = inverse(e, phi)

m = pow(c, d, n)

print("FCSC{%s}" % m)
```

Our result is :  
`FCSC{563694726501963824567957403529535003815080102246078401707923}`
