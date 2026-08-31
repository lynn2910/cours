---
title: Micro contrôleurs
draft: false
tags:
---
# Le hardware

## 1. Le package

Le package est l'enveloppe plastique d'un microcontrôleur qui expose des pins de connexion, eux-mêmes utilisés pour se connecter aux autres composants d'un circuit.

## 2. Les pins (GPIO)

Les pins sont appelées **GPIO** : **G**lobal **P**urpose **I**nput/**O**utput.

Certains pins ont une fonction bien précise, quand certains peuvent être librement utilisés par un utilisateur pour être connectés à des circuits tels que des sensors, des LEDs, des interrupteurs, ...

> [!example] L'esp8266
> L'**esp8266** a 33 pins et 17 GPIOs, mais seules **11** sont réellement utilisables puisque *6 sont toujours utilisés pour connecter le µC à une mémoire flash, avec le micro-code à exécuter*.

### 2.1 Pins digitaux

Un pin *digital* permet de lire / écrire un signal numérique, soit un signal à deux états:
- `HIGH` : correspond au voltage le plus haut (*3.3V sur un esp8266, 5V sur un ATmega328*)
- `LOW` : correspond au voltage le plus bas (*0V*)

> [!tip] Un GPIO digital peut être utilisé en lecture et en écriture.

### 2.2 Pins analogiques

Un pin *analogique* permet de lire / écrire un signal qui varie dans le temps, entre les niveaux hauts et bas.

Même si le signal physique varie de façon continue, ce n'est pas le cas du point de vue du µC, puisque qu'il discrétise le signal afin d'obtenir une valeur numérique.

La plage de variation, le nombre de pas, la vitesse d'acquisition, etc... dépendent du µC.

> [!example] Avec l'esp8266
> L'esp8266 accepte des signaux variants entre 0 et 1V, et discrétise sur 1024 pas.
> 

> [!danger] Un GPIO analogique peut être utilisé en lecture OU en écriture.

# Les bases du software

## 1. Un sketch

### 1.1 Structure et cycle de vie

Un sketch, c'est un programme **arduino** qui contient 2 fonctions primordiales :
- `void setup()`
- `void loop()`

La méthode `setup()` initialise les variables globales, les communications en protocole [[#2. Le protocole Serie (Serial)|série]], les capteurs, la connexion wifi... Cette méthode n'est appelée qu'une seule fois après le démarrage.

La méthode `loop()` est le point d'entrée d'exécution, Cette méthode est appelée en boucle jusqu'à un démarrage, un arrêt ou un plantage du µC.

> [!tip] La méthode `loop` équivaut au `main` en C, bien que le main ne soit pas cyclique.

On peut représenter le cycle de vie de cette manière :

![[Pasted image 20251007103735.png]]

### 1.2 Variables globales

Les variables globales peuvent être définies après les `#include` et `#define`, avec :
```c
volatile <type> <name>;
```

> [!example]- Exemple
> ```c
> volatile byte stateButton;
> ```
> Voir [[#2. La base des interruptions]] pour l'exemple complet.

> [!tip] Conseil de nommage
> Il est recommandé de définir des variables globales pour les noms de pin. Plutôt que d'utiliser `D1`, `D2`, ..., utiliser `#define <name> <pin>` afin de ne pas devoir refactor tout le code si on change le pin.

---
## 2. Lire / écrire sur les pins GPIO


Voir [[#2. Les pins (GPIO)]] pour la définition et les modes de GPIO.

### 2.1 Nommage des pins

Les sketchs n'utilisent pas le nom technique ou le numéro de pin physique du package du µC.

> [!example] La GPIO5 de l'esp3266 est le pin n°24.

À la place, on utilise des surnoms / numéros qui sont les mêmes quel que soit le constructeur de la carte de développement.

> [!danger] Attention
> Ces surnoms n'ont pas forcément de rapport avec le nom officiel du GPIO. Par exemple, le GPIO5 de l'esp8266 est nommé `D1`.
> 
> Il arrive que ca corresponde, comme le GPIO12 de l'esp32 est renommée `12`

### 2.2 Sens d'utilisation d'un GPIO digital

Pour définir le sens d'utilisation, on utilise la méthode `pinMode()`.

Normalement, ce sens ne varie pas au cours de l'exécution, donc cela est fait pendant l'initialisation dans `setup()`. Pour changer le sens, il suffit de réutiliser `pinMode()`.

> [!example] Sur l'esp8266
> ```c
> void setup() {
> 	pinMode(D3, OUTPUT);
> 	pinMode(D2, INPUT);
> 	// ...
> }
> ```

### 2.3 Lire / écrire sur un GPIO

Sur un esp8266 :
```c
int val = analogRead(A0); // Lis une valeur de 0 à 1023 (= 0 à 3.3V)
byte b = digitalRead(D2);

digitalWrite(D3, HIGH); // D3 a désormais 3.3V
digitalWrite(D3, LOW); // D3 a maintenant 0V
```

Sur un esp32 :
```c
int val = analogRead(33); // Lis une valeur entre 0 et 4095 (= 0 à 3.3V)
byte b = digitalRead(8);

analogWrite(25, 1234); // Ecris 1234 sur le ping 25 de l'esp32
digitalWrite(4, HIGH); // Le pin 4 donne désormais 3.3V
digitalWrite(4, LOW); // Le ping 4 donne maintenant 0V
```

La valeur `1234` permet d'appliquer le voltage `0.9942V`, qui peut être calculé avec cette équation : 
$$
\frac{3.3*1234}{4096} = 0.9942V
$$

---
## 3. Le protocole de communication série (serial)

La *plupart* des cartes de développement ont un circuit qui permet d'envoyer un programme dans la mémoire flash liée au µC, grâce à un câble USB.

> [!tip] L'utilité du circuit "série"
> Ce circuit permet de convertir les communications utilisant un protocole série en un protocole USB.

Ce circuit est relié à deux GPIO du µC, qui sont reliés à une partie du µC capable d'envoyer et recevoir des données via le protocole série.

> [!tip] Utilité
> Le protocole série est utile pour faire du débogage en envoyant des messages depuis le µC.

### 3.1 Initialisation

L'initialisation du protocole se fait en général dans la méthode `setup()`, en donnant une vitesse en bauds (voir [[#3.2 Vitesse de communication]]).

On peut ensuite utiliser la méthode `println(<string>)` afin d'envoyer des lignes de texte. La méthode `read()` permet de lire **un octet**.

```c
void setup(){
	Serial.begin(115200);
	Serial.println("hello");
}
```

### 3.2 Vitesse de communication

La vitesse de communication dépend du µC.

> [!example] ATmega328
> L'ATmega328 autorise seulement 9600 bauds.

Certains modules, comme par exemple les GPS, utilisent également un protocole série pour communiquer avec le µC. Cela pose problème sur un esp8266 puisque le circuit série est déjà utilisé pour communiquer avec le PC.

Heureusement, on peut faire la communication série avec n'importe quels GPIOs digitales. Mais dans ce cas, il faut émuler le protocole série par du logiciel, qui est moins performant que de passer par l'électronique interne du µC.

---
## 4. Interruptions

> [!todo]

---
## 5. Pulse Width Modulation (PWM)

> [!todo]

---
## 6. Conso. électrique et mise en veille

> [!todo]

---
# Logiciel utilisé

Blink Arduino

> [!todo]

## Méthodes utiles à connaître

| Nom              | Indépendant de la carte | Description                                                                             |
| ---------------- | ----------------------- | --------------------------------------------------------------------------------------- |
| `digitalRead`    | O                       |                                                                                         |
| `digitalWrite`   | O                       |                                                                                         |
| `delay`          | O                       | Attend un moment indiqué, en millisecondes. Le microcontrôleur ne fait **rien d'autre** |
| `Serial.begin`   | O                       | Permet de définir la vitesse d'échange entre l'ordinateur et la carte.                  |
| `Serial.println` | O                       | Envoie un message à l'ordinateur.                                                       |

# Sketchs

## 1. Setup Minimal et Cycle E/S

```c
void setup() {
	Serial.begin(115200);
	Serial.println("hello");
	pinMode(D1, OUTPUT);
	pinMode(D2, INPUT);
	delay(100);
}

void loop(){
	byte state = digitalRead(D2); // 
	Serial.println(state);
	digitalWrite("high");
	delay(500);
	digitalWrite(D1, LOW);
	Serial.println("low");
	delay(500);
}
```

## 2. La base des interruptions

```c
#define PIN_LEN D1
#define PIN_BUTTON D2

volatile byte stateButton;

void ICACHE_RAM_ATTR getState(){
	stateButton = digitalRead(PIN_BUTTON);
	if (stateButton == 1) {
		digitalWrite(PIN_LED, HIGH);
	} else {
		digitalWrite(PIN_LED, LOW);
	}
}

void setup(){
	Serial.begin(115200);
	pinMode(PIN_LED, OUTPUT);
	pinMode(PIN_BUTTON, INPUT);
	delay(50);
	attachInterrupt(digitalPinToInterrupt(PIN_BUTTON), getState, CHANGE);
}

void loop(){
	if (stateButton == 1) {
		Serial.println("high");
	} else {
		Serial.println("low");
	}
	
	delay(2000);
}
```

## 3. Interruption Externe et Contrôle de Fréquence PWM

```c
#define PIN_LED D1
#define PIN_BUTTON D2

volatile int freq;

void ICACHE_RAM_ATTR getState() {
	freq += 1;
	analogWriteFreq(freq);
}

void setup(){
	Serial.begin(115200);
	while(!Serial);
	
	pinMode(PIN_LED, OUTPUT);
	pinMode(PIN_BUTTON, INPUT);
	delay(50);
	
	freq += 1;
	analogWrite(freq);
	analogWrite(freq);
	analogWrite(PIN_LED, 50);
	
	attachInterrupt(digitalPinToInterrupt(D2), getState, RISING);
}

void loop(){}
```

## 4. Cycles de Sommeil Profond ESP32

```c
RTC_DATA_ATTR int bootCount = 0;

void print_wakeup_reason(){
  esp_sleep_wakeup_cause_t wakeup_reason;
  wakeup_reason = esp_sleep_get_wakeup_cause();
  switch(wakeup_reason)  {
    case ESP_SLEEP_WAKEUP_EXT0 : 
	    Serial.println("Wakeup caused by external signal using RTC_IO"); 
	    break;
    case ESP_SLEEP_WAKEUP_EXT1 :
	    Serial.println("Wakeup caused by external signal using RTC_CNTL"); 
	    break;
    case ESP_SLEEP_WAKEUP_TIMER :
	    Serial.println("Wakeup caused by timer");
	    break;
    case ESP_SLEEP_WAKEUP_TOUCHPAD :
	    Serial.println("Wakeup caused by touchpad");
	    break;
    case ESP_SLEEP_WAKEUP_ULP :
	    Serial.println("Wakeup caused by ULP program");
	    break;
    default :
	    Serial.printf("Wakeup was not caused by deep sleep: %d\n",wakeup_reason);
	    break;
  }
}

void setup() {
  Serial.begin(115200);
  ++bootCount;
  Serial.println(bootCount);
  print_wakeup_reason();

  esp_sleep_enable_ext0_wakeup(GPIO_NUM_33,1);
  esp_sleep_enable_timer_wakeup(5000000);
}

void loop() {
  delay(1000);
  Serial.println("Going to sleep now");
  esp_deep_sleep_start();
  Serial.println("This will never be printed");
}
```

