# Práctica 2: Introducción a Grafos
# Ejercicio 9: Dos Caminos Distintos Implican un Ciclo

> **Materia:** Técnicas de Diseño de Algoritmos / Algoritmos y Estructuras de Datos III  
> **Tema:** Caminos Simples, Vértice de Divergencia, Primer Vértice de Reencuentro, Construcción Formal de Ciclos Simples.

---

## 1. Enunciado

> *«Sean $P$ y $Q$ dos caminos distintos (no necesariamente disjuntos en vértices) de un grafo $G$ que unen un vértice $v$ con otro $w$. Demostrar en forma directa que $G$ tiene un ciclo donde cada arista pertenece a $P$ o a $Q$.»*

---

## 2. Definiciones Formales y Notación

* **Camino simple:**  
  Un camino en un grafo es una secuencia de vértices $P = (u_0, u_1, \dots, u_k)$ tal que $(u_i, u_{i+1}) \in E(G)$ para todo $0 \le i < k$.  
  Por definición de camino simple, **todos sus vértices son distintos entre sí** ($u_i \neq u_m$ para todo $i \neq m$).

* **Representación de $P$ y $Q$:**  
  Sean los dos caminos que unen $v$ con $w$:
  $$P = (v = p_0, p_1, p_2, \dots, p_k = w)$$
  $$Q = (v = q_0, q_1, q_2, \dots, q_m = w)$$

* **Hipótesis:**  
  $P \neq Q$ (los caminos son distintos, es decir, difieren en al menos una arista o vértice intermedio).

* **Tesis a Demostrar:**  
  Existe un ciclo simple $C$ en $G$ tal que para toda arista $e \in E(C)$, se cumple $e \in E(P) \cup E(Q)$.

---

## 3. Demostración Directa Paso a Paso

### Paso 1: Identificación del Vértice de Divergencia ($s$)
Ambos caminos inician en el mismo vértice: $p_0 = q_0 = v$.  
Como $P \neq Q$, no pueden ser idénticos en toda su extensión.  
Definimos $a \ge 0$ como el **mayor índice** tal que los caminos coinciden hasta ese punto:
$$p_0 = q_0, \quad p_1 = q_1, \quad \dots, \quad p_a = q_a$$

Llamamos **$s$** a este último vértice común:
$$s := p_a = q_a$$

Por definición de $a$, en el paso inmediatamente siguiente los caminos divergen:
$$p_{a+1} \neq q_{a+1}$$
*(Esto garantiza que la arista $(s, p_{a+1}) \in E(P)$ y la arista $(s, q_{a+1}) \in E(Q)$ son distintas).*

---

### Paso 2: Identificación del Primer Vértice de Reencuentro ($j$)
A partir de $p_{a+1}$, seguimos recorriendo el camino $P$:
$$(p_{a+1}, p_{a+2}, \dots, p_k = w)$$

Notemos que el conjunto de vértices de este tramo contiene al menos un vértice que también pertenece a $Q$, pues al menos el extremo final $w$ pertenece a ambos caminos ($p_k = q_m = w$).

Definimos $b$ como el **menor índice** mayor que $a$ tal que $p_b$ pertenece a $Q$:
$$b := \min \{ i \in \{a+1, \dots, k\} \mid p_i \in V(Q) \}$$

Llamamos **$j$** a este primer vértice de reencuentro:
$$j := p_b$$

Como $j \in V(Q)$, existe un índice $c$ en el camino $Q$ tal que:
$$j = q_c$$

#### Observación fundamental sobre el índice $c$:
Demostramos que **$c > a$**:
* Si fuera $c \le a$, tendríamos que $q_c = p_c$ (porque hasta el índice $a$ ambos caminos eran idénticos).
* Pero como $j = q_c$, tendríamos $p_b = p_c$ con $c \le a < b$.
* Esto significaría que el camino $P$ repite un vértice ($p_b = p_c$), contradiciendo que $P$ es un camino simple.
* Por lo tanto, necesariamente **$c > a$**. $\checkmark$

---

### Paso 3: Definición de los Subcaminos Disjuntos
Consideramos los subcaminos de $P$ y $Q$ comprendidos entre $s$ y $j$:

1. **Subcamino en $P$:**
   $$P' = P[s, j] = (s = p_a, p_{a+1}, p_{a+2}, \dots, p_b = j)$$
2. **Subcamino en $Q$:**
   $$Q' = Q[s, j] = (s = q_a, q_{a+1}, q_{a+2}, \dots, q_c = j)$$

---

### Paso 4: Demostración de Disyunción Interna
Demostraremos que los caminos $P'$ y $Q'$ **sólo se tocan en sus extremos $s$ y $j$**:
$$V(P') \cap V(Q') = \{s, j\}$$

* **Demostración:**
  * Los vértices internos de $P'$ son $\{p_{a+1}, p_{a+2}, \dots, p_{b-1}\}$.
  * Por la definición de $b$ como el *mínimo* índice tal que $p_b \in V(Q)$, sabemos que **ningún** vértice del conjunto $\{p_{a+1}, \dots, p_{b-1}\}$ pertenece a $V(Q)$.
  * Por lo tanto, ningún vértice interno de $P'$ puede pertenecer a $Q'$.
  * Como tanto $P$ como $Q$ son caminos simples, $P'$ y $Q'$ no tienen repeticiones internas de vértices.
  * Luego, los únicos vértices comunes entre $P'$ y $Q'$ son exactamente sus extremos $s$ y $j$. $\checkmark$

---

### Paso 5: Construcción Formal del Ciclo Simple $C$
Construimos el recorrido cerrado $C$ que parte de $s$, viaja hacia $j$ a lo largo de $P'$, y regresa de $j$ hacia $s$ recorriendo $Q'$ en sentido inverso (denotado $(Q')^{-1}$):

$$C = (s = p_a, p_{a+1}, \dots, p_b = j = q_c, q_{c-1}, \dots, q_{a+1}, s = q_a)$$

Verificamos que $C$ cumple la definición de **ciclo simple**:
1. **Es cerrado:** Inicia y termina en el mismo vértice $s$.
2. **No repite vértices intermedios:**  
   * Los vértices del tramo de $P'$ son todos distintos entre sí.
   * Los vértices del tramo de $Q'$ son todos distintos entre sí.
   * Por el Paso 4, el único contacto entre ambos tramos son los extremos $s$ y $j$.
3. **Tiene longitud válida:**  
   Como $p_{a+1} \neq q_{a+1}$, las aristas que salen de $s$ por $P'$ y por $Q'$ son distintas, lo que garantiza que el ciclo no recorre la misma arista de ida y de vuelta (longitud $\ge 3$).

---

### Paso 6: Pertenencia de las Aristas
Por construcción:
* Las aristas del tramo de ida pertenecen a $P'$:
  $$E(P') \subseteq E(P)$$
* Las aristas del tramo de vuelta pertenecen a $Q'$:
  $$E(Q') \subseteq E(Q)$$

Por lo tanto:
$$E(C) = E(P') \cup E(Q') \subseteq E(P) \cup E(Q)$$

Cada arista de $C$ pertenece a $P$ o a $Q$.

---

### Conclusión
Hemos demostrado directamente la existencia de un ciclo simple $C$ en $G$ formado exclusivamente por aristas de $P$ y $Q$. $\blacksquare$

---

## 4. Enseñanzas Metodológicas y Trampas Comunes

1. **La trampa de "recorrer todo $P$ y volver por todo $Q$":**  
   Si los dos caminos comparten tramos al principio o al final (por ejemplo, aristas comunes), concatenar $P$ con $Q^{-1}$ no da un ciclo simple porque estarías yendo y viniendo por las mismas aristas.
2. **El concepto de "Disyunción Interna":**  
   Para formar un ciclo a partir de dos caminos, estos deben compartir **únicamente** el vértice inicial y el vértice final.
3. **El poder de elegir el primer reencuentro:**  
   Tomar el *mínimo* índice $b$ donde $P$ vuelve a tocar a $Q$ es la herramienta matemática que te garantiza de forma automática e indiscutible que en el medio no hay ningún cruce raro ni interferencias.
