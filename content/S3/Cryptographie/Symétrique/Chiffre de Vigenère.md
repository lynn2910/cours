---
title: Chiffre de Vigenère
draft: false
tags:
---
## Table de Vigenère (Carré)

La table de Vigenère (ou carré de Vigenère) est une table avec 26 lignes et 26 colonnes où, à chaque colonne, les lettres vont être décalées d'un indice.

On peut représenter la table de Vigenère de cette façon:![[Vigenere_Table_with_Numbers.png]]
On peut remarquer qu'il y a de nombreuses ressemblances avec les structures de données **matrices**.

La matrice peut être générée de la façon suivante:

```py
vigenere_matrix = []

for a in range(0, 26):
  arr = []
  for b in range(0, 26):
    arr.append((a + b) % 26)
  vigenere_matrix.append(arr)
```

Il suffit alors, pour obtenir la lettre selon le caractère et la clé, le code suivant:
```py
def vigenere(c, k):
  return vigenere_matrix[c][k]
```

Cette matrice sera utilisée par les méthodes de chiffrement et déchiffrement.
## Chiffrement

le chiffrement s'effectue avec l'équation suivante: $c_i = (c_i + k_i) \%26$

Où `c` est le message codé, `k` est la clé et `i` le ieme caractère.

On peut alors utiliser cette méthode en python:
```py
def encryptVigenere(m, key):
  encryptedMessage = ""
  for i in range(0, len(m)):
    encryptedMessage += alphabet[vigenere(
        numericChar(alphabet, m[i]),
        numericChar(alphabet, key[i % len(key)])
    )]
  return encryptedMessage
```


## Déchiffrement

Le déchiffrement s'exprime quand à lui avec l'équation $d_i = (c_i - k_i)\% 26$

Où `d` est le message déchiffré. Les autres variables sont **identiques** à la méthode de chiffrement.

Ainsi, on peut déchiffrer des messages en utilisant cette méthode:
```py
def decryptVigenere(cm, key):
  decryptedMessage = ""
  for i in range(0, len(cm)):
    decryptedMessage += alphabet[vigenere(
        numericChar(alphabet, cm[i]),
        26 - numericChar(alphabet, key[i % len(key)])
    )]
  return decryptedMessage
```

