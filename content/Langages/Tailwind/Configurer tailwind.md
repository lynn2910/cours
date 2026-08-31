---
title: Configurer tailwind
draft: false
tags:
---
Pour rappel, notre fichier `tailwind.config.js` ressemble à ceci :
```js
module.exports = {
  purge: [], // ou la liste 
  darkMode: 'media',
  theme: {
    extend: {},
  },
  variants: {
    extend: {},
  },
  plugins: [],
}
```

## Écrans

Par défaut, on retrouvera :
```js
module.exports = {
  ...,
  theme: {
    screens: {
      'sm': '640px',
      'md': '768px',
      'lg': '1024px',
      'xl': '1280px',
      '2xl': '1536px',
    }
  },
  ...
}
```

Cela permettra de configurer des styles pour être affichés selon la taille de l'écran (comme le `@media` en CSS), on pourra alors faire, par exemple :
```html
<div class="flex flex-row sm:flex-col">
	<p>Mai Hua</p>
	<p>麦华</p>
</div>
```

On verra alors :
```
Mai Hua 麦华
``` 
Mais si notre écran fait **640 px** ou moins, on verra :
```
Mai Hua
麦华
```
Car la classe `flex-col` sera appliquée (équivalent de `flex-direction: column;`)

Pour ajouter des tailles d'écrans, il suffit d'utiliser la clé `extend`:
```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  ...,
  theme: {
    extend: {
      screens: {
        '3xl': '1600px',
      },
    },
  },
  ...
}
```

Il sera alors possible, dans ce cas, d'utiliser `3xl:...` dans le style des pages.

---
## Couleurs

La clé `coulors` permet de customiser les couleurs globales du projet:
```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  ...,
  theme: {
    colors: {
      transparent: 'transparent',
      black: '#000',
      white: '#fff',
      gray: {
        100: '#f7fafc',
        // ...
        900: '#1a202c',
      },

      // ...
    }
  }
  ...
}
```

*Pour voir toutes les couleurs disponibles: [Customizing Colors - tailwindcss.com](https://tailwindcss.com/docs/customizing-colors)*

Pour ajouter des couleurs, il suffit d'ajouter `colors` dans `theme.extend`:
```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  ...,
  theme: {
    extend: {
      colors: {
        brown: {
          50: '#fdf8f6',
          100: '#f2e8e5',
          200: '#eaddd7',
          300: '#e0cec7',
          400: '#d2bab0',
          500: '#bfa094',
          600: '#a18072',
          700: '#977669',
          800: '#846358',
          900: '#43302b',
        },
      }
    },
  },
  ...
}
```

Il est également possible d'ajouter des variations à des couleurs de bases :
```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  theme: {
    extend: {
      colors: {
        blue: {
          950: '#17275c',
        },
      }
    },
  },
}
```

> [!info] La palette bleu de base
> ![[tailwind-blue-palette.png]]

---
## Espacements

Par défaut, l'espacement dans tailwind:
```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  theme: {
    spacing: {
      px: '1px',
      0: '0',
      0.5: '0.125rem',
      1: '0.25rem',
      1.5: '0.375rem',
      2: '0.5rem',
      2.5: '0.625rem',
      3: '0.75rem',
      3.5: '0.875rem',
      4: '1rem',
      5: '1.25rem',
      6: '1.5rem',
      7: '1.75rem',
      8: '2rem',
      9: '2.25rem',
      10: '2.5rem',
      11: '2.75rem',
      12: '3rem',
      14: '3.5rem',
      16: '4rem',
      20: '5rem',
      24: '6rem',
      28: '7rem',
      32: '8rem',
      36: '9rem',
      40: '10rem',
      44: '11rem',
      48: '12rem',
      52: '13rem',
      56: '14rem',
      60: '15rem',
      64: '16rem',
      72: '18rem',
      80: '20rem',
      96: '24rem',
    },
  }
}
```

Il peut alors être utilisé dans la `width`, `height`, `margin`, `padding`, etc.

Comme pour tout le reste, pour ajouter des espacements supplémentaires, il suffit d'utiliser :
```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  theme: {
    extend: {
      spacing: {
        '13': '3.25rem',
        '15': '3.75rem',
        '128': '32rem',
        '144': '36rem',
      }
    }
  }
}
```

*Visualiser les différentes tailles: [Default spacing scale - tailwindcss.com](https://tailwindcss.com/docs/customizing-spacing#default-spacing-scale)*

---
## Polices

Par défaut, les polices sont :
```js
module.exports = {
  theme: {
    fontFamily: {
      'sans': ['ui-sans-serif', 'system-ui', ...],
      'serif': ['ui-serif', 'Georgia', ...],
      'mono': ['ui-monospace', 'SFMono-Regular', ...]
    }
  }
}
```

On peut ajouter des polices de cette manière :
```diff
const defaultTheme = require('tailwindcss/defaultTheme')

module.exports = {
  theme: {
    extend: {
      fontFamily: {
+       'sans': ['"Proxima Nova"', ...defaultTheme.fontFamily.sans],
      },
    }
  }
}
```

*Voir [[Rappels Javascript#Déstructurer un Array]] pour `...defaultTheme.fontFamily.sans`*

> [!danger]+ Il faut OBLIGATOIREMENT ajouter les `@import` et `@fontface` nécessaires quand on utilise une police custom.
> Dans notre CSS avec les trois directives, on doit rajouter:
> ```css
> @layer base {
>   @font-face {
>     font-family: 'Roboto';
>     font-style: normal;
>     font-weight: 400;
>     font-display: swap;
>     src: url(/fonts/Roboto.woff2) format('woff2');
>   }
> }
> ```
> Mais on peut également, plus simplement, 
> ```css
> @import url('https://fonts.googleapis.com/css2?family=Jost:ital,wght@0,100..900;1,100..900&family=Noto+Sans:ital,wght@0,100..900;1,100..900&display=swap');
> }
> ```
> (La police sera automatiquement importée)

> [!tip]- Importer les polices
> On peut utiliser un import, tel que:
> ```css
> @import url('https://fonts.googleapis.com/css2?family=Jost:ital,wght@0,100..900;1,100..900&family=Noto+Sans:ital,wght@0,100..900;1,100..900&display=swap');
> }
> ```
> Ou utiliser:
> ```html
> <link rel="preconnect" href="https://fonts.googleapis.com">
> <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
> <link href="https://fonts.googleapis.com/css2?family=Jost:ital,wght@0,100..900;1,100..900&family=Noto+Sans:ital,wght@0,100..900;1,100..900&display=swap" rel="stylesheet">
> ```

