---
title: Chiffrement asymétrique
draft: false
tags: []
---
## Définition

Un **algorithme de chiffrement asymétrique** (ou à clé publique) est un système cryptographique qui utilise une paire de clés mathématiques liées entre elles : une **clé publique** et une **clé privée**.

- **Clé publique:** Elle peut être librement distribuée et sert à chiffrer les messages.
- **Clé privée:** Elle est tenue secrète par le destinataire et sert à déchiffrer les messages chiffrés avec la clé publique correspondante.

**Contrairement aux algorithmes symétriques**, qui utilisent une seule clé pour le chiffrement et le déchiffrement, les algorithmes asymétriques offrent un avantage majeur : ils permettent de **sécuriser la communication sans avoir à échanger préalablement une clé secrète**.

## Caractéristiques et utilisations

## Sécurité

La sécurité des algorithmes asymétriques repose sur des problèmes mathématiques complexes, comme la factorisation de grands nombres premiers (utilisés dans RSA).

### Flexibilité

Ils sont utilisés pour diverses applications :
- **Chiffrement:** Protéger la confidentialité des données.
- **Signatures numériques:** Authentifier l'origine d'un message et garantir son intégrité.
- **Échange de clés:** Établir des clés de session pour des algorithmes symétriques plus rapides.
## Algorithmes

Plusieurs algorithmes:
- [[AES]]
- [[RSA]]

## Sous-catégories

On retrouvera deux catégories d'algorithmes:
- Public Key Signature
- Digital Signature

## Types de chiffrements

### Flux

Dans un chiffrement par flux, les données sont traitées bit par bit ou octet par octet, de manière séquentielle. Un générateur de clé de flux produit un flux de bits pseudo-aléatoires qui est combiné avec les données en clair pour produire le texte chiffré.

**Avantages:**
- Vitesse élevée : idéal pour les communications en temps réel.
- Simplicité d'implémentation.
**Inconvénients:**
- Moins sûr que le chiffrement par blocs en cas d'erreurs de transmission ou de modifications des données.
- Sensibilité à la synchronisation entre l'émetteur et le récepteur.

**Exemples d'algorithmes de chiffrement par flux:** RC4, Salsa20, ChaCha20.
### Blocs

Au contraire, le chiffrement par blocs divise les données en blocs de taille fixe (par exemple, 128 bits pour AES) et chaque bloc est chiffré indépendamment.

**Avantages:**
- Sécurité renforcée : les erreurs de transmission affectent généralement un seul bloc.
- Flexibilité : peut être utilisé dans de nombreux modes de fonctionnement (ECB, CBC, CTR, etc.) pour répondre à différents besoins.

**Inconvénients:**
- Peut être moins performant que le chiffrement par flux pour de grands volumes de données.
- Nécessite souvent des modes de fonctionnement supplémentaires pour gérer les données de taille variable.

**Exemples d'algorithmes de chiffrement par blocs:** AES, DES, 3DES.

### Comparaison

| Caractéristique        | Chiffrement par flux       | Chiffrement par blocs                                |
| ---------------------- | -------------------------- | ---------------------------------------------------- |
| Traitement des données | Bit à bit ou octet à octet | Blocs de taille fixe                                 |
| Vitesse                | Généralement plus rapide   | Peut être plus lent pour de petits blocs             |
| Sécurité               | Moins sûr en cas d'erreur  | Plus sûr, mais nécessite des modes de fonctionnement |
| Flexibilité            | Moins flexible             | Plus flexible                                        |
Le choix entre un chiffrement par flux et un chiffrement par blocs dépend de l'application :
- **Chiffrement par flux:** Idéal pour les communications en temps réel (comme les protocoles de streaming), les applications nécessitant une vitesse élevée et une faible latence.
- **Chiffrement par blocs:** Préféré pour le chiffrement de fichiers, les communications sécurisées où la sécurité est primordiale, et les applications nécessitant une grande flexibilité.

