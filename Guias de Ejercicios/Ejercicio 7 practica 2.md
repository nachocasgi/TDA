# Práctica 2: Introducción a Grafos
# Ejercicio 7: Muchas Aristas Implica Conexo

> **Materia:** Técnicas de Diseño de Algoritmos / Algoritmos y Estructuras de Datos III  
> **Tema:** Demostración por Inducción Matemática en la cantidad de vértices $n$, Lema Auxiliar por Reducción al Absurdo, Álgebra de Desigualdades y Demostración Formal de Conexidad por Caminos.

---

## 1. Enunciado

> *«Demostrar, usando inducción en la cantidad de vértices, que todo grafo de $n$ vértices que tiene más de $\frac{(n - 1)(n - 2)}{2}$ aristas es conexo.»*

---

## 2. Datos y Condiciones de Partida

* **Objeto:** Grafo simple no dirigido $G = (V, E)$.
* **Vértices:** $|V(G)| = n$, con $n \ge 2$ (grafos no triviales).
* **Premisa / Hipótesis del Teorema:** $|E(G)| > \frac{(n - 1)(n - 2)}{2}$.
* **Método requerido:** Inducción matemática sobre $n$.
* **Meta (Tesis):** Demostrar que $G$ es **conexo**.

---

## 3. Demostración Paso a Paso

### Paso 1: Definición Formal del Predicado $P(n)$
Para cada $n \in \mathbb{N}_{\ge 2}$, definimos el predicado:
$$P(n) := \forall G = (V, E) \text{ con } |V| = n : \left( |E| > \frac{(n-1)(n-2)}{2} \implies G \text{ es conexo} \right)$$

---

### Paso 2: Caso Base ($n = 2$)
Queremos verificar que $P(2)$ es verdadero.

1. Evaluamos la fórmula del enunciado para $n = 2$:
   $$|E| > \frac{(2 - 1)(2 - 2)}{2} = \frac{1 \cdot 0}{2} = 0 \implies |E| \ge 1$$
2. Como un grafo simple con 2 vértices puede tener a lo sumo $\binom{2}{2} = 1$ arista, necesariamente $|E| = 1$.
3. El único grafo de 2 vértices y 1 arista es $K_2$ (dos vértices unidos por una arista), el cual es conexo por definición.
4. Por lo tanto, **$P(2)$ es verdadero**. $\checkmark$

---

### Paso 3: Paso Inductivo ($n \ge 3$)

* **Hipótesis Inductiva (H.I.):**  
  Asumimos que $P(n - 1)$ es verdadero. Es decir, para todo grafo $G' = (V', E')$ con $|V'| = n - 1$:
  $$|E'| > \frac{(n-2)(n-3)}{2} \implies G' \text{ es conexo}$$

* **Tesis Inductiva (T.I.):**  
  Queremos demostrar que $P(n)$ es verdadero. Es decir, para todo grafo $G = (V, E)$ con $|V| = n$:
  $$|E| > \frac{(n-1)(n-2)}{2} \implies G \text{ es conexo}$$

---

### Paso 4: Lema Auxiliar (Imposibilidad de Vértices Aislados)

> **Afirmación:** En $G$, ningún vértice tiene grado 0. Es decir, $\deg(u) \ge 1$ para todo $u \in V$.

* **Demostración por el absurdo:**
  1. Supongamos que existe un vértice $u \in V$ tal que $\deg(u) = 0$.
  2. Al tener grado 0, ninguna arista de $G$ incide en $u$.
  3. Por lo tanto, todas las aristas de $E$ deben estar formadas únicamente por los restantes $n - 1$ vértices ($V \setminus \{u\}$).
  4. La cantidad máxima posible de aristas entre $n - 1$ vértices ocurre en el grafo completo $K_{n-1}$, cuya cantidad es:
     $$\binom{n - 1}{2} = \frac{(n - 1)(n - 2)}{2}$$
  5. Esto obligaría a que:
     $$|E| \le \frac{(n - 1)(n - 2)}{2}$$
  6. ¡Contradicción frontal con la hipótesis $|E| > \frac{(n - 1)(n - 2)}{2}$!
  7. El absurdo provino de suponer que existía un vértice de grado 0.  
  **Conclusión del lema:** $\deg(u) \ge 1$ para todo $u \in V$. $\checkmark$

---

### Paso 5: Separación en Casos Exhaustivos

Analizamos los grados de los vértices de $G$:

* **Caso A (Todos los vértices tienen grado máximo):**  
  Si para todo $u \in V$ se cumple $\deg(u) = n - 1$, entonces cada vértice está conectado con todos los demás. El grafo es el grafo completo **$K_n$**, que es **conexo** directamente.

* **Caso B (Existe al menos un vértice con grado menor al máximo):**  
  Existe un vértice $v \in V$ tal que $\deg(v) \le n - 2$.  
  Como por el Paso 4 sabemos que $\deg(v) \ge 1$, este vértice satisface:
  $$1 \le \deg(v) \le n - 2$$
  *(Este es el vértice que vamos a remover para aplicar inducción).*

---

### Paso 6: Construcción de $G'$ y Desigualdad de Aristas

Definimos el subgrafo inducido por remover $v$:
$$G' = G - v = (V', E')$$
donde $V' = V \setminus \{v\}$ y $E' = E \setminus \{e \in E \mid v \in e\}$.

1. **Vértices de $G'$:**  
   $$|V'| = |V| - 1 = n - 1$$
2. **Aristas de $G'$:**  
   $$|E'| = |E| - \deg(v)$$
3. **Encadenamiento de desigualdades:**
   * Sabemos que $|E| > \frac{(n-1)(n-2)}{2}$.
   * Sabemos que $\deg(v) \le n - 2 \implies -\deg(v) \ge -(n - 2)$.
   * Reemplazando:
     $$|E'| = |E| - \deg(v) > \frac{(n-1)(n-2)}{2} - (n - 2)$$
4. **Cálculo algebraico:**
   $$\frac{(n-1)(n-2)}{2} - (n - 2) = (n - 2) \cdot \left( \frac{n - 1}{2} - 1 \right) = (n - 2) \cdot \left( \frac{n - 3}{2} \right) = \mathbf{\frac{(n-2)(n-3)}{2}}$$
5. **Resultado:**
   $$|E'| > \frac{(n-2)(n-3)}{2}$$

---

### Paso 7: Aplicación de la Hipótesis Inductiva
El subgrafo $G'$ tiene $n - 1$ vértices y su cantidad de aristas supera estrictamente $\frac{(n-2)(n-3)}{2}$.  
Por lo tanto, **$G'$ cumple todas las condiciones de la Hipótesis Inductiva $P(n - 1)$**.

> **Por H.I., $G'$ es CONEXO.**

---

### Paso 8: Demostración Formal de la Conexidad de $G$

Para demostrar formalmente que $G$ es conexo, debemos probar que:
> *Para todo par de vértices distintos $x, y \in V$, existe al menos un camino en $G$ que une $x$ con $y$.*

Dado que $V = V' \cup \{v\}$, analizamos todos los casos posibles para el par $\{x, y\}$:

* **Situación 1 ($x, y \in V'$):**  
  Ambos vértices pertenecen a $G'$. Como $G'$ es conexo, existe un camino $P$ que los une usando aristas de $G'$. Dado que $E(G') \subseteq E(G)$, ese camino $P$ es también un camino en $G$. $\checkmark$

* **Situación 2 (Uno de los vértices es $v$, digamos $x = v$ y el otro es $y \in V'$):**  
  Como demostramos que $\deg(v) \ge 1$, el vértice $v$ tiene **al menos un vecino adentro de $V'$**. Llamemos a ese vecino **$w \in V'$** (la arista $(v, w) \in E(G)$ existe).
  * Si $y = w$, la propia arista $(v, w)$ es el camino que une $v$ con $y$.
  * Si $y \neq w$, tanto $w$ como $y$ pertenecen a $V'$. Como $G'$ es conexo, existe un camino $P' = (w = u_0, u_1, \dots, u_m = y)$ en $G'$.
  * Concatenamos a $v$ con el camino $P'$ cruzando la arista $(v, w)$:
    $$P_{\text{total}} = (v, w = u_0, u_1, \dots, u_m = y)$$
  * Como la arista $(v, w) \in E(G)$ y todas las aristas de $P'$ pertenecen a $E(G)$, $P_{\text{total}}$ es un camino válido en $G$ que une $v$ con $y$. $\checkmark$

En todos los casos posibles existe un camino entre $x$ e $y$.  
Por lo tanto, **$G$ es conexo**.

Esto demuestra que $P(n)$ es verdadero.

---

### Paso 9: Conclusión Final
Por el **Principio de Inducción Matemática**, el predicado $P(n)$ es verdadero para todo $n \in \mathbb{N}_{\ge 2}$. $\blacksquare$

---

## 4. Enseñanzas y Claves Metodológicas para Exámenes

1. **Evitar el razonamiento circular (Petición de principio):**  
   Nunca digas *"el grafo no tiene grado 0 porque si no, no sería conexo"*. Si asumís que es conexo antes de probarlo, la demostración es inválida. La ausencia de grado 0 se demuestra únicamente por el tope de aristas $\binom{n-1}{2}$.
2. **La diferencia de umbrales revela el grado a sacar:**  
   La cuenta $\frac{(n-1)(n-2)}{2} - \frac{(n-2)(n-3)}{2} = n - 2$ te indica exactamente qué condición debías pedirle al vértice a remover ($\deg(v) \le n - 2$).
3. **El formalismo de reconexión por caminos:**  
   Para probar conexidad de $G$ a partir de $G'$, no basta decir *"lo vuelvo a conectar y queda conexo"*. Hay que plantear los dos casos ($x, y \in V'$ vs. $x = v, y \in V'$) y mostrar la concatenación formal usando al vecino $w$ como puente.
