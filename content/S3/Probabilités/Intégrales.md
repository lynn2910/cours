---
title: Intégrales
draft: false
tags: 
aliases:
---

## Vocabulaire et définition

> [!todo] todo


## Droites

Prenons cette courbe :
![[Pasted image 20241106153140.png]]

Tel que `f(x)`: $\begin{equation}f(x) = 2x\end{equation}$

> [!tip] Observations
> 1. La fonction `f` est positive sur l'intervalle $[0; 1]$ car sa courbe est au-dessus de l'axe des abscisses
> 2. `f` est continue sur $[0; 1]$

Une intégrale **permet de calculer l'air sous la courbe**. 

Nous allons chercher ensuite l'intégrale suivante :

$$
\begin{equation}
(\int_{0}^{1}
f(x)dx= \frac{2*1}{2})
\end{equation}
=1
$$
Notre fonction `f` a donc bien une **densité de probabilité** sur $[0; 1]$ 

> [!warning] Quand une intégrale est une densité de probabilité
> Pour être une densité de probabilité, elle doit être strictement égale à 1



On peut alors effectuer des probabilités dessus.

Si nous voulons connaitre $P(X<0.5)$:
$$
P(X<0.5) = \frac{0.5*1}{2} = 0.25
$$
Et $P(X>0.75)$:
$P(X>0.75) = \frac{0.75*1,5}{2} = \frac{7}{16}$
Alors :
	$P(X=0.25) = 0$
	$P(0.25<X<0.75) = 0.5*0.5*\frac{0.5*1}{2}$
		$= 0.25 + 0.25$
		$= 0.5$

## Courbes

Prenons maintenant cette fonction :
![[Pasted image 20241106154843.png]]

Où `f` tel que $f(x)= \frac{1}{9}x^2$

Notre intégrale sera :
$$
\begin{equation}
\int_{a}^{b}f(x)dx=[F(x)]_{b}^{a}
\end{equation}
$$
Où `F(x)` est une primitive de `f` sur $[a;b]$

> [!success] Observations
> 1. `f` est positive sur $[0;3]$ car, pour tout réel $x$, $x^2 >= 0$ et $\frac{1}{9} > 0$, donc $\frac{1}{9}x^2 >=0$
> 2. `f` est continue $[0;3]$ car `f` est une fonction **polynôme**


$\int_{3}^{0}\frac{1}{9}x^2dx$
$= [\frac{1}{9}*\frac{x^3}{3}]_{0}^{3}$
$=[\frac{1}{27}x^3]_{0}^{3}$
$= \frac{1}{27}*3^3 - \frac{1}{27}*27 = 1$

Donc `f` est bien une **densité de probabilité** sur $[0; 3]$

### Calculer `P(1 <= X <= 2)`

$P(1 <= X <= 2) = \int_{1}^{2}f(x)dx$
	$= \int_{1}^{2}\frac{1}{9}x^2dx$
	$= [\frac{1}{27}x^3]^2$
	$= \frac{1}{27}*2^3 - \frac{1}{27}*1^3$
	$= \frac{1}{27}*8 - \frac{1}{27}*1$
	$= \frac{8}{27} - \frac{1}{27} = \frac{7}{27}$

### Calculer l'espérance

$E(X) = \int_{0}^{3}xf(x)dx$
	$= \int_{0}^{3}x*\frac{1}{9}x^2dx$
	$=\int_{0}^{3}\frac{1}{9}x^3dx$
	$=[\frac{1}{9}*\frac{x^4}{4}]_{0}^{3}$
	$= [\frac{x^4}{36}]_{0}^{3}$
	$=\frac{34}{36} - \frac{0^4}{36}$
	$= \frac{81}{36} = \frac{9}{4}$

### Calculer la variance

$V(X) = \int_{0}^{3}(x-\frac{9}{4})^2 f(x)=dx$
	$= \int_{0}^{3}(x - \frac{9}{4})^2 * \frac{1}{9}x^2dx$
	$= \int_{0}^{3} (x^2-2x*\frac{9}{4}+(\frac{9}{4})^2)*\frac{1}{9}x^2dx$
	$= \int_{0}^{3}(x^2-\frac{9}{2}x + \frac{81}{16}x^2)dx$
	$= [\frac{1}{9}*\frac{x^5}{5}-\frac{1}{2}*\frac{x^4}{4}+\frac{9}{16}*\frac{x^3}{3}]_{0}^{3}$
	$=[\frac{1}{45}x^5-\frac{1}{8}x^4+\frac{3}{16}x^3]_{0}^{3}$
	$=\frac{1}{45}*3^5 - \frac{1}{8}*3^4 + \frac{3}{16}*3^3$
	$=\frac{243}{45}-\frac{81}{8}+\frac{3}{16}*27$
	$=\frac{27}{80}$
	$\sigma(X) = \sqrt{V(X)} = \sqrt\frac{27}{80}$

### Calculer `I(a)`

Soit
$$f(x)=2xe^{-x^2}$$

$I(a)= \int_{0}^{a}f(x)dx$
	$= \int_{0}^{a}2xe^{-x^2}dx$
	$=(e^{-x^2}) = -2e^{-x^2}$
	$=(-e^{-x^2}) = +2e^{-x^2}$

Donc $I(a) = [-e^{-x^2}]_{0}^{a} = -e^{-a^2}$
	$= e^{-a^2}+e^0 = 1-e^{-a^2}$

$\lim_{a\to\infty} I(a) = \lim_{a\to\infty}(1-e^{-a^2})$

$\lim_{a\to\infty} ({-a^2}) = -\infty$
$\lim_{a\to\infty} e^X =0$

Donc, par composition :
$\lim_{a\to\infty} e^{-a^2} = 0$

Alors $\lim_{a\to\infty} (1 - e^{-a^2}) = 1$



`f` est positive sur $[0;+\infty[$ car $2x >= 0$ et $e^{-x^2}> 0$ donc $2xe^{-x^2} >= 0$

`f` est continue sur $[0;+\infty[$ (admis)

$$
\int_{0}^{+\infty} f(x)dx = \lim_{a\to\infty} \int_{0}^{a}f(x)dx = \lim_{a\to\infty} I(A) = 1
$$

