Pour voir les bases : [[Introduction Agile|Introduction Agile]]

## Socket

Sous Unix, les communications se font à l'aide d'un objet appelé **socket**.

Un **socket** est un [[Entrées sorties (I.O)#Descripteurs de fichiers|descripteur de fichiers]].

Celles-ci sont créées par la fonction **socket**
```c
int socket(int domaine, int type, int protocole);
```

Les paramètres définissent le type de socket souhaitent (en fonction du type de réseau utilisé).

**Pour les communications en TCP/IP :**
- **Domaine :** `AF_INET` (pour IPv4)
- **Type :** `SOCK_STREAM` (Pour TCP)
- **Protocole :** `0`

---
## Fonctions coté client

Une fois le **socket** créée, il est possible de le connecter grâce à la fonction `connect`:
```c
int connect(int sockfd, const struct sockaddr *addr, socklen_t addrlen)
```

Avec:
- `sockfd`: socket à connecter
- `addr`: adresse à laquelle à connecter
- `addrlen`: Taille de la structure pointée par `addr`

Les adresses IP sont codées dans une structure de type `sockaddr_in`:
```c
struct sockaddr_in {
	sa_family_t sin_family; // Famille d'adresses (AF_INET pour IPv4)
	in_port_t sin_port; // Numéro de port (en ordre des octets réseau) 
	struct in_addr sin_addr; // Adresse IP
	unsigned char sin_zero[8]; // Bourrage pour compatibilité avec sockaddr
}
```

---
## Fonctions coté serveur

