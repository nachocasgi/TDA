# Práctica 2: Introducción a Grafos
## Ejercicios del 1 al 4: Resoluciones, Análisis y Enseñanzas Metodológicas

> **Materia:** Técnicas de Diseño de Algoritmos / Algoritmos y Estructuras de Datos III  
> **Tema:** Invariantes, Isomorfismo, Inducción en Grafos, Caracterización de Árboles y Lema del Apretón de Manos

---

## Índice
1. [Ejercicio 1: Isomorfismo de Grafos](#ejercicio-1-isomorfismo-de-grafos)
2. [Ejercicio 2: Inducción en Grafos ("Estamos todos conectados")](#ejercicio-2-inducción-en-grafos-estamos-todos-conectados)
3. [Ejercicio 3: Caracterización de Árboles](#ejercicio-3-caracterización-de-árboles)
4. [Ejercicio 4: Suma de Grados (Lema del Apretón de Manos)](#ejercicio-4-suma-de-grados-lema-del-apretón-de-manos)
5. [Caja de Herramientas Metodológica (Para Exámenes y Prácticas)](#caja-de-herramientas-metodológica)

---

## Ejercicio 1: Isomorfismo de Grafos

### Enunciado
Decidir si los siguientes dos grafos son isomorfos entre sí. Si lo son, dar un isomorfismo entre ellos:
* **Grafo $G_1$ (izquierda):** Vértices superiores $u_1, u_2, u_3$ y vértices inferiores $v_1, v_2, v_3$. Cada $u_i$ está conectado a cada $v_j$.
* **Grafo $G_2$ (derecha):** Vértices $w_1, w_2, w_3, w_4, w_5, w_6$ ordenados en un ciclo hexagonal $C_6$, con 3 cuerdas interiores que conectan vértices opuestos: $(w_1, w_4)$, $(w_2, w_5)$ y $(w_3, w_6)$.

### Resolución

1. **Chequeo de Invariantes Básicos:**
   * Cantidad de vértices: $|V(G_1)| = |V(G_2)| = 6$.
   * Cantidad de aristas: $|E(G_1)| = 3 \times 3 = 9$. En $G_2$: 6 aristas del ciclo $+ 3$ cuerdas $= 9$.
   * Secuencia de grados: Ambos son $3$-regulares (todos los vértices tienen grado 3).
   * Conexidad: Ambos son conexos.

2. **Identificación Estructural:**
   * $G_1$ es por definición el grafo bipartito completo **$K_{3,3}$** con partición independiente $U = \{u_1, u_2, u_3\}$ y $V = \{v_1, v_2, v_3\}$.
   * En $G_2$, si particionamos los vértices según la paridad de sus índices:
     * $A = \{w_1, w_3, w_5\}$ (impares)
     * $B = \{w_2, w_4, w_6\}$ (pares)
     Observamos las conexiones:
     * $w_1 \in A$ se conecta a $w_2, w_6$ (en el ciclo) y a $w_4$ (por la cuerda). Es decir, a todo $B$.
     * $w_3 \in A$ se conecta a $w_2, w_4$ (en el ciclo) y a $w_6$ (por la cuerda). Es decir, a todo $B$.
     * $w_5 \in A$ se conecta a $w_4, w_6$ (en el ciclo) y a $w_2$ (por la cuerda). Es decir, a todo $B$.
     * No hay aristas entre vértices de $A$ ni entre vértices de $B$.
   * Por lo tanto, **$G_2$ también es el grafo bipartito completo $K_{3,3}$**.

3. **Construcción Formal del Isomorfismo:**
   Definimos la biyección $f: V(G_1) \to V(G_2)$ asignando la partición $U$ a $A$ y la partición $V$ a $B$:
   $$\begin{array}{lll}
   f(u_1) = w_1, & f(u_2) = w_3, & f(u_3) = w_5 \\
   f(v_1) = w_2, & f(v_2) = w_4, & f(v_3) = w_6
   \end{array}$$

4. **Justificación:**
   * $f$ es una biyección trivialmente (inyectiva y sobreyectiva sobre 6 vértices).
   * En $G_1$, dos vértices son adyacentes si y sólo si uno pertenece a $U$ y el otro a $V$.
   * En $G_2$, dos vértices son adyacentes si y sólo si uno pertenece a $f(U) = A$ y el otro a $f(V) = B$.
   * Por ende: $(x, y) \in E(G_1) \iff (f(x), f(y)) \in E(G_2)$.
   * Conclusión: **Los grafos son isomorfos**.

### Enseñanzas del Ejercicio 1
> 1. **Los invariantes descartan, pero no demuestran:** Que coincidan vértices, aristas y grados solo permite *sospechar* isomorfismo. Para demostrarlo formalmente, siempre hay que dar la función biyectiva explícita.
> 2. **La disposición geométrica engaña:** Un mismo grafo abstracto puede dibujarse como dos filas paralelas o como un hexágono con cuerdas cruzadas. Hay que buscar invariantes profundos como la **bipartitud** (ausencia de ciclos impares) o el tamaño del ciclo más corto (**cintura**).

---

## Ejercicio 2: Inducción en Grafos ("Estamos todos conectados")

### Enunciado
Federico afirma que: *"Todo grafo de $n \ge 2$ vértices cuyos grados sean todos por lo menos 1 es conexo"*, e intenta demostrarlo por inducción.

### Resolución de los Ítems

* **a) Contraejemplo de Daiana:**
  Consideremos un grafo con $n = 4$ vértices y aristas $E = \{(1, 2), (3, 4)\}$ (dos aristas disjuntas, $K_2 \cup K_2$).
  * Grados: $\deg(1) = 1, \deg(2) = 1, \deg(3) = 1, \deg(4) = 1$. Todos tienen grado $\ge 1$.
  * Conexidad: Tiene 2 componentes conexas, no existe camino entre el vértice 1 y el 3.
  * **Conclusión: La afirmación de Federico es rotundamente FALSA.**

* **b) y c) El error al construir "hacia arriba" (agregando vértices):**
  * Fede parte de un grafo conexo de $n$ vértices y le agrega un vértice $v$ conectado a él.
  * **Error lógico:** Fede solo demuestra la propiedad para los grafos que se pueden construir con esa receta particular. No demuestra que *cualquier* grafo arbitrario de $n+1$ vértices con grado $\ge 1$ provenga de agregar un vértice a un grafo conexo con grado $\ge 1$. Su demostración no cubre todo el universo de grafos posibles.

* **d) El error al desarmar "hacia abajo" sacando un vértice arbitrario:**
  * Fede intenta arreglarlo diciendo: *"Sea $G$ un grafo de $n+1$ vértices... saquemos un vértice arbitrario $v$. El grafo $G - v$ tiene $n$ vértices y **los grados de todos sus vértices son por lo menos 1**, así que aplicamos H.I."*
  * **Frase falsa exacta:** *«y los grados de todos sus vértices son por lo menos 1»*.
  * **Por qué es falso:** Si eliminamos un vértice arbitrario $v$, cualquier vecino $w$ de $v$ que en $G$ tuviese grado 1, en $G - v$ pasa a tener grado $\deg_{G-v}(w) = 0$ (queda aislado). Al tener un vértice de grado 0, $G - v$ no cumple las premisas del predicado y **es ilegal aplicarle la Hipótesis Inductiva**.

### Enseñanzas del Ejercicio 2
> 1. **La falacia de construcción ("hacia arriba"):** En inducción sobre grafos nunca se parte de tamaño $n$ agregando vértices o aristas. Siempre se empieza con un grafo genérico de tamaño $n+1$ y se reduce a tamaño $n$.
> 2. **La falacia de reducción arbitraria:** No podés remover un vértice "cualquiera" al azar. Sacar un vértice arbitrario puede destruir la estructura (desconectar el grafo, aislar vértices, romper hipótesis).
> 3. **Para reducir correctamente:**
>    * O elegís un vértice con propiedades garantizadas (como una hoja $\deg(v)=1$ en árboles).
>    * O hacés inducción en la cantidad de aristas $|E|$ (remover una arista es más controlable).
>    * O usás inducción fuerte descomponiendo en componentes conexas.
> 4. **Siempre testear con contraejemplos mínimos:** Si una propiedad suena demasiado abarcativa, probá con $K_2 \cup K_2$ u otros grafos pequeños desconectados.

---

## Ejercicio 3: Caracterización de Árboles

### Enunciado
Un árbol es un grafo conexo sin ciclos. Dado un grafo $G$ de $n \ge 2$ nodos **sin nodos de grado 0** ($\deg(v) \ge 1$), decidir cuáles ítems garantizan que $G$ sea un árbol. Si no lo garantizan, dar un contraejemplo.

### El Contraejemplo Estrella: $C_3 \cup K_2$
Un grafo de 5 vértices formado por un triángulo disjunto de una arista:
* Vértices: $\{1, 2, 3\}$ forman $C_3$; $\{4, 5\}$ forman $K_2$.
* $n = 5$, aristas $m = 3 + 1 = 4 = n - 1$.
* Grados: tres vértices de grado 2, dos vértices de grado 1. Ninguno de grado 0.
* No es conexo (2 componentes) y tiene un ciclo ($C_3$).

### Análisis Ítem por Ítem

* **a) $G$ tiene $n - 1$ aristas:**
  * **NO garantiza.** Contraejemplo: $C_3 \cup K_2$. Tiene $n=5, m=4=n-1$, sin grado 0, pero es desconexo y contiene un ciclo.
  *(Tener $n-1$ aristas solo asegura ser árbol si ya se sabe de antemano que es conexo o que es acíclico).*

* **b) $G$ tiene exactamente 2 nodos de grado 1:**
  * **NO garantiza.** Contraejemplo: $C_3 \cup K_2$ (los vértices 4 y 5 tienen grado 1). O bien un triángulo conexo con dos hojas colgando (antenas).

* **c) $G$ tiene exactamente 2 nodos de grado 1 y $n - 1$ aristas:**
  * **NO garantiza.** Contraejemplo: Nuevamente $C_3 \cup K_2$. Cumple tener $n-1=4$ aristas, sin aislados, exactamente dos nodos de grado 1, y no es un árbol.

* **d) $G$ tiene exactamente 2 nodos de grado 1 y no tiene ciclos:**
  * **SÍ garantiza.**
  * **Demostración:**
    1. Como no tiene ciclos, $G$ es un bosque (colección de árboles disjuntos).
    2. Como $\deg(v) \ge 1$, ninguna componente es un vértice aislado; cada componente tiene $\ge 2$ vértices.
    3. Todo árbol con $\ge 2$ vértices tiene al menos 2 hojas (vértices de grado 1).
    4. Si $G$ tuviera $k$ componentes conexas, la cantidad total de hojas sería al menos $2k$.
    5. Por hipótesis hay exactamente 2 hojas $\implies 2k \le 2 \implies k = 1$.
    6. Al tener $k = 1$, $G$ es conexo. Como además es acíclico, **$G$ es necesariamente un árbol** (específicamente, un camino simple $P_n$). $\blacksquare$

* **e) $G$ tiene exactamente 2 nodos de grado 1 y es conexo:**
  * **NO garantiza.** Contraejemplo: Un ciclo triángulo con dos aristas pendientes ("antenas") unidas a dos de sus vértices. Es conexo, sin grado 0, tiene exactamente 2 hojas, pero tiene un ciclo.

* **f) $G$ tiene exactamente 2 nodos de grado 1 y todos los demás nodos tienen grado 2:**
  * **NO garantiza (en el caso general donde no se asume conexidad).**
  * **Contraejemplo:** Nuevamente $C_3 \cup K_2$. Los extremos del $K_2$ tienen grado 1 y los 3 vértices del $C_3$ tienen grado 2.
  *(Nota: si el enunciado hubiera exigido explícitamente conexidad, entonces sí forzaría a ser un camino $P_n$).*

### Resumen del Ejercicio 3

| Ítem | ¿Garantiza ser árbol? | Contraejemplo o Razón |
| :---: | :---: | :--- |
| **a** | **NO** | $C_3 \cup K_2$ |
| **b** | **NO** | $C_3 \cup K_2$ |
| **c** | **NO** | $C_3 \cup K_2$ |
| **d** | **SÍ** | Acíclico $+$ cada componente aporta $\ge 2$ hojas $\implies 1$ sola componente (conexo). |
| **e** | **NO** | Triángulo con 2 antenas (es conexo pero tiene ciclo). |
| **f** | **NO** | $C_3 \cup K_2$ (camino $K_2$ más ciclo disjunto $C_3$). |

### Enseñanzas del Ejercicio 3
> 1. **No asumir conexidad a menos que el enunciado lo diga:** Si un enunciado no incluye la palabra "conexo", asumí que el grafo puede estar roto en varias componentes.
> 2. **Para probar que es árbol, hay que garantizar dos frentes:** Que sea acíclico Y que sea conexo. Cumplir $m=n-1$ no basta por sí solo.
> 3. **Razonar por suma de componentes conexas:** Si sabés cuántas hojas o aristas hay en total, analizá la cota mínima que aporta cada componente por separado para acotar la cantidad de componentes $k$.

---

## Ejercicio 4: Suma de Grados (Lema del Apretón de Manos)

### Enunciado
Demostrar por inducción en la cantidad de aristas $|E(G)|$ que para todo grafo $G$ se cumple:
$$\sum_{v \in V(G)} \deg(v) = 2 \cdot |E(G)|$$
Recordar que para un vértice $v$ de un grafo $G$: $\deg(v) := |\{e \in E(G) \mid e \text{ incide en } v\}|$.

### Resolución

Definimos el predicado sobre $m \in \mathbb{N}_0$ (la cantidad de aristas):
$$P(m) := \text{“Para todo grafo } G \text{ con } |E(G)| = m \text{ aristas, se cumple que } \sum_{v \in V(G)} \deg(v) = 2m\text{”.}$$

1. **Caso Base ($m = 0$):**
   * Sea $G = (V, E)$ un grafo arbitrario con $|E| = 0$ aristas (grafo sin aristas).
   * Ningún vértice tiene aristas incidentes, por lo que para todo $v \in V$: $\deg(v) = 0$.
   * La suma de grados es $\sum_{v \in V} \deg(v) = \sum_{v \in V} 0 = 0$.
   * Evaluando el lado derecho: $2 \cdot |E| = 2 \cdot 0 = 0$.
   * Como $0 = 0$, $P(0)$ es verdadero.

2. **Paso Inductivo:**
   * Sea $m \ge 0$.
   * **Hipótesis Inductiva (H.I.):** Asumimos que vale $P(m)$, es decir, que para cualquier grafo $G'$ con $|E(G')| = m$ aristas se cumple:
     $$\sum_{v \in V(G')} \deg_{G'}(v) = 2m$$
   * **Tesis Inductiva:** Queremos probar $P(m + 1)$, es decir, que para cualquier grafo $G$ con $|E(G)| = m + 1$ aristas se cumple:
     $$\sum_{v \in V(G)} \deg_G(v) = 2(m + 1)$$

   * **Desarrollo (reduciendo desde $m + 1$ hacia $m$):**
     1. Sea $G = (V, E)$ un grafo genérico arbitrario con $|E| = m + 1$ aristas.
     2. Como $m + 1 \ge 1$, el conjunto de aristas no es vacío. Tomamos una arista cualquiera $e \in E$, con extremos $x, y \in V$, es decir, $e = (x, y)$.
     3. Consideramos el subgrafo $G' = G - e = (V, E \setminus \{e\})$.
     4. **Chequeo de condiciones:** $G'$ conserva el mismo conjunto de vértices $V$ y tiene exactamente $|E(G')| = (m + 1) - 1 = m$ aristas.
     5. Por Hipótesis Inductiva aplicada sobre $G'$:
        $$\sum_{v \in V} \deg_{G'}(v) = 2m$$
     6. Relación de grados entre $G$ y $G'$:
        * Para los extremos de la arista removida: $\deg_G(x) = \deg_{G'}(x) + 1$ y $\deg_G(y) = \deg_{G'}(y) + 1$.
        * Para cualquier otro vértice $v \in V \setminus \{x, y\}$: $\deg_G(v) = \deg_{G'}(v)$.
     7. Calculamos la suma de grados en $G$:
        $$\sum_{v \in V} \deg_G(v) = \deg_G(x) + \deg_G(y) + \sum_{v \in V \setminus \{x, y\}} \deg_G(v)$$
        $$= (\deg_{G'}(x) + 1) + (\deg_{G'}(y) + 1) + \sum_{v \in V \setminus \{x, y\}} \deg_{G'}(v)$$
        $$= \underbrace{\left( \deg_{G'}(x) + \deg_{G'}(y) + \sum_{v \in V \setminus \{x, y\}} \deg_{G'}(v) \right)}_{\sum_{v \in V} \deg_{G'}(v)} + 2$$
     8. Reemplazamos por la Hipótesis Inductiva:
        $$\sum_{v \in V} \deg_G(v) = 2m + 2 = 2(m + 1) = 2 \cdot |E(G)|$$
   * Queda demostrado que $P(m + 1)$ es verdadero.

3. **Conclusión:**
   Por principio de inducción matemática, para todo grafo $G$ vale que:
   $$\sum_{v \in V(G)} \deg(v) = 2 \cdot |E(G)| \quad \blacksquare$$

### Enseñanzas del Ejercicio 4
> 1. **La inducción en aristas es "quirúrgica":** Quitar una arista $e = (x, y)$ solo altera el grado de **exactamente dos vértices** (restando 1) y deja a todo el resto del grafo inalterado.
> 2. **El "chequeo de admisión" para aplicar H.I.:** En un examen, nunca apliques la H.I. "de una" sin explicitar que el grafo resultante cumple las condiciones. Siempre poné la frase de admisión: *"Como $G' = G - e$ tiene $|E(G')| = m$ aristas..."*. Si el teorema tuviese hipótesis adicionales (ej: ser árbol o conexo), debés justificar en media línea por qué $G'$ las conserva.
> 3. **Corolario del Apretón de Manos (Handshaking Lemma):** Como la suma es siempre $2|E|$ (un número par), **la cantidad de vértices de grado impar en cualquier grafo es siempre PAR**.

---

## Caja de Herramientas Metodológica

1. **Para Isomorfismos:**
   * ¿Descartar? $\to$ Grados, aristas, ciclos impares, bipartitud, diámetro.
   * ¿Demostrar? $\to$ Tabla o función biyectiva explícita $f(v)$ y justificación de aristas.

2. **Para Demostraciones por Inducción:**
   * Empezar siempre con: *"Sea $G$ un grafo de tamaño $n+1$ (o $m+1$) que satisface..."*
   * Al reducir, justificar siempre el **chequeo de admisión**: tamaño $n$ (o $m$) y preservación de hipótesis.
   * Considerar inducción en aristas $|E|$ cuando la inducción en vértices rompa grados o desconecte el grafo.

3. **Contraejemplos Clásicos para Recordar:**
   * $K_2 \cup K_2$: Dos aristas disjuntas (desconexo, todos grado 1).
   * $C_3 \cup K_2$: Triángulo y arista disjunta (desconexo, tiene ciclo, $m=n-1$, dos hojas).
   * Triángulo con antenas: Conexo, con ciclos y hojas controladas.
