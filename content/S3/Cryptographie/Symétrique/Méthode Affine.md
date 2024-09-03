---
title: Méthode Affine
draft: false
tags: []
---
## Définition

Le chiffrement affine est une version améliorée du Code césar.
Il utilise deux clés, `a` et `b`

On retrouve ces deux équations:
- Chiffrement: $c = (a * p) + b\  mod \ n$
- Déchiffrement: $p = a$$^-$$^1$ $*\ (c * b)\ mod\ n$
Où:
- `c` est le message codé
- `p` est le message décodé
- `a` et `b` sont les coefficients (clé secrète) du chiffre affine.
- `a` doit avoir un inverse $a^-$$^1$ qui est l'inverse de $a\ mod\ n$
- `a` est inversible si `gcd(a, n) == 1` (voir méthode inverse)

**Méthode `inverse`:**
```py
def inverse(a, n):
  for it in range(1, n):
    if ((a * it) % n == 1):
      return it
  return -1
```
## Chiffrement

Afin de chiffre un message avec le chiffrement Affine, on peut utiliser le code ci-dessous

```py
def encryptAffine(m, a, b, n = 26):
  encryptedMessage = ""

  for car in m:
    if car.lower() in alphabet:
      c = ((a * numericChar(alphabet, car)) + b) % n
      encryptedMessage += alphabet[c]
    else:
      encryptedMessage += car
  return encryptedMessage
```

Ainsi, `encryptAffine("Sparks", 10, 2)` donneras `awcqya`

## Déchiffrement

De même, le code ci-dessous est un exemple d'implémentation du déchiffrement du chiffrement Affine:

```py
def decryptAffine(ctext, a, b, n=26):
    a_inv = inverse(a, n)
    if a_inv == -1:
        raise ValueError("a n'est pas inversible modulo n")
        
    plaintext = ""
    for char in ctext:
        if char.lower() in alphabet:
            c = numericChar(alphabet, char)
            p = (a_inv * (c - b)) % n
            plaintext += alphabet[p]
        else:
            plaintext += char
    return plaintext
```

