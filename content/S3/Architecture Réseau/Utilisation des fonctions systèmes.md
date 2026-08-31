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
- **Protocole :** `0` (protocole par défaut)

Cette méthode renvoie un descripteur de fichier pour le socket créé, ou `-1` en cas d'erreur.

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
}

struct in_addr {
	uint32_t s_addr; // adresse avec les octets dans l'ordre réseau
}
```

**Exemple**

Nous allons nous connecter au port `4242` de l'adresse `1.2.3.4`:
```c
int sock;
struct sockaddr_in addr;

sock = socket(AF_INET, SOCK_STREAM, 0);

addr.sin_family = AF_INET;
addr.sin_port = htons(4242);
inet_aton("1.2.3.4", &addr.sin_addr);

connect(sock, (struct sockaddr*)&addr, sizeof addr);
```

Maintenant que la connexion a été établie, on pourra communiquer de façon classique avec les opérations de lecture et d'écriture sur le **socket**.

Pour fermer le socket, il faut appeler la méthode `close(socket)`

---
## Fonctions coté serveur

Afin de créer un serveur qui écoute sur un port donné, on utilisera :
```c
int bind(int sockfd, const struct sockaddr *addr, socklen_t addrlen);
```

Où :
- `sockfd`: le socket à associer
- `addr`: l'adresse à laquelle l'associer
- `addrlen`: taille de la structure pointée par `addr`

Avec le protocole TCP/IP, on utilisera la structure `sockaddr_in`, remplie avec les valeurs suivantes :
- `sin_family`: `AF_INET`
- `sin_port`: Le numéro de port souhaité
- `sin_addr`: l'adresse souhaitée ou `INADDR_ANY` pour toutes les interfaces locales.

**Par exemple :**
```c
addr.sin_addr = hton(INADDR_ANY);
```

Une fois le socket associé à une adresse, on peut la mettre en écoute afin d'obtenir les connexions entrantes avec cette fonction :
```c
int listen(int sockfd, int backlog)
```

Où :
- `sockfd`: le socket
- `backlog`: Le nombre maximal de connexions entrantes simultanées

Et quand on reçoit une connexion entrante, on peut utiliser la fonction `accept`:
```c
int accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen);
```

Où :
- `sockfd`: Le socket en écoute
- `addr`: Si elle n'est pas `NULL`, elle permet de passer un pointeur vers une structure qui sera renseignée avec l'adresse du client qui s'est connecté.
- `addrlen`: sert à passer la taille de la structure pointée.

La fonction `accept` renvoie un `socket` pour la connexion. Il est alors possible de lire les informations avec les méthodes classiques de lecture et d'écriture (voir [[Entrées sorties (I.O)]])

Comme pour le client, la méthode `close(conn_socket)` permet de fermer la connexion avec le client.