---
title: Chiffrement symétrique
draft: false
tags:
  - Cours
---
Cours: [TD1-crypto.pdf](TD1-crypto.pdf)
## Origine

La cryptographie a d'abord été utilisée dans un cadre militaire. Jusqu'à 1976, ces algorithmes étaient utilisés à des fins militaires (Code césar, Enigma...)

## Définition

Un chiffrement symétrique est un méthode consistant à rendre illisible une information en utilisant une clé. Cette même clé servira à déchiffrer l'information une fois réceptionnée.

## Chiffre de césar

Le code césar était utilisé pendant l'antiquité par les romains pour cacher leurs communications entre militaires.

Cette méthode consiste à "décaler" chaque lettre d'un nombre précis, où ce nombre `k` est notre clé.

Ainsi, le code césar s'exprimera de cette façon:
- Chiffrement: $c = (p + k) \% 26$
- Déchiffrement: $p = (c - k)\%26$
où:
- `c` est le message codé
- `p` est le message décodé
- `k` est la clé

On peut alors écrire le code python suivant:
```py
alphabet="abcdefghijklmnopqrstuvwxyz"

def numericChar(chain,car):

  car = car.lower()

  return chain.index(car)

def encryptShift(origMessage, key="d"):

  encryptedMessage = ""

  if key.isalpha():

    key = numericChar(alphabet, key)

  for car in origMessage:

    if car.lower() in alphabet:

      encryptedMessage += alphabet[(numericChar(alphabet,car) + key)%26]

    else:

      encryptedMessage += car

  return encryptedMessage
```

Ainsi, si on chiffre le message `It's been fun, Noah` avec la clé `s`, on obtiendra `al'k twwf xmf, fgsz`

> [!Summary|wide-3 min-0]+ Méthode de déchiffrement
> Réponse à la question "Ecrivez la fonction de déchiffrement correspondante"
> ```py
> def decryptShift(cryptedMessage, key = "d"):
> decryptedMessage = ""
>  if key.isalpha():
>     key = numericChar(alphabet, key)
>   for car in cryptedMessage:
>     if car.lower() in alphabet:
>       decryptedMessage += alphabet[(numericChar(alphabet,car) - key + 26)%26]
>     else:
>       decryptedMessage += car
>   return decryptedMessage;
> ```


Si cette méthode peut sembler utile, il est toutefois très facile de déchiffrer le message par brute force:

```py
def bruteForceShift(m):
  for k in alphabet:
    print(decryptShift(m, k))

bruteForceShift("al'k twwf xmf, fgsz")
```

On obtiendra alors:
```
al'k twwf xmf, fgsz
zk'j svve wle, efry
yj'i ruud vkd, deqx
xi'h qttc ujc, cdpw
wh'g pssb tib, bcov
vg'f orra sha, abnu
uf'e nqqz rgz, zamt
te'd mppy qfy, yzls
sd'c loox pex, xykr
rc'b knnw odw, wxjq
qb'a jmmv ncv, vwip
pa'z illu mbu, uvho
oz'y hkkt lat, tugn
ny'x gjjs kzs, stfm
mx'w fiir jyr, rsel
lw'v ehhq ixq, qrdk
kv'u dggp hwp, pqcj
ju't cffo gvo, opbi
it's been fun, noah   <===
hs'r addm etm, mnzg
gr'q zccl dsl, lmyf
fq'p ybbk crk, klxe
ep'o xaaj bqj, jkwd
do'n wzzi api, ijvc
cn'm vyyh zoh, hiub
bm'l uxxg yng, ghta
```

Aussi, si nous faisons:
```py
encryptShift("Get me a vanilla ice cream, make it a double.", "6")
```
Nous obtiendrons alors `mkz sk g bgtorrg oik ixkgs, sgqk oz g juahrk.`

Ce message peut être décodé par les clés 6, 32, 58, 84...
Pourquoi? Car ces 4 nombres ont un point commun, ils peuvent être exprimé par $6 + (26*x)$

Cette méthode peut être testée et validée pour toutes les clés et tout les messages possibles.

## Chiffrement Affine

