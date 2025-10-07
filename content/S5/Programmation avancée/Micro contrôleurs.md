---
title: Micro contrôleurs
draft: false
tags:
---
# Les bases

## 1. Un sketch

### 1.1 Structure et cycle de vie

Un sketch, c'est un programme arduino qui contient 2 fonctions primordiales :
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

> [!todo]

---
## 3. Le protocole Serie (Serial)

> [!todo]

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
	attachInterrupt(digitalPinToInterrup(PIN_BUTTON), getState, CHANGE);
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

