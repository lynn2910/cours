---
title: Evenements
draft: false
tags:
---
# Recevoir un évènement

Avec VueJS, recevoir un évènement d'un composant ou d'un objet HTML est très simple, il existe deux façons de le faire :
```vue
<button v-on:click="saveContent()">Sauvegarder</button>
```
Ou :
```vue
<button @click="saveContent()">Sauvegarder</button>
```

---
# Émettre un évènement

De la même manière, émettre un évènement est tout aussi simple, il faudra utiliser `this.$emit`

## Exemple

```vue
<template>
	...
	<button @click="saveContent()">Sauvegarder</button>
</template>

<script>
export default {
	name: "PrestataireNameEdit",
	methods: {
		saveContent(){
			// On va émettre l'évènement
			this.$emit('save', {
				content: this.content,
				prestataire: this.prestataire
			});
		}
	}
}
</script>
```

On pourra alors recevoir l'évènement dans le composant parent:
```vue
<template>
	<PrestataireNameEdit @save="saveName"></PrestataireNameEdit>
</template>

<script>
export default {
	methods: {
		saveName({ content, prestataire }){
			console.log("Save Name event:");
			console.log(`Content: '${content}'`);
			console.log(`Prestataire: ${prestataire}`);
		}
	}
}
</script>
```
*Voir [[Rappels Javascript#Manipulation de string|Manipulation de strings | JS]] et [[Rappels Javascript#Déstructurer un Array|Déstructurer un Array]]*

