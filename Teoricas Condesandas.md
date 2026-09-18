# Compendio Condensado de las Fuentes de Teoría de Grafos y Algoritmos

Este documento reúne y condensa la información contenida en las 5 presentaciones de la materia. Se han eliminado las duplicaciones progresivas de diapositivas (frames generados paso a paso) para ofrecer un texto plano unificado, estructurado y de fácil lectura.

---

## Fuente 1: Grafos Propiedades.pdf
**Título:** Introducción a la teoría de grafos: Modelos, definiciones, isomorfismo y conexidad  
**Departamento:** Departamento de Computación, FCEyN - UBA (2do Cuatrimestre de 2026)

### 1. Definiciones Básicas y Notación
* **Grafo**: Par $G = (V,E)$ donde $V$ es el conjunto de vértices ($n = |V|$) y $E$ es el conjunto de aristas ($m = |E|$).
* **Digrafo**: $D = (V,A)$ donde $A$ contiene pares ordenados (arcos).
* **Adyacencia e Incidencia**: $u \sim v \iff uv \in E$. La arista $e=uv$ incide en $u$ y en $v$.
* **Igualdad de Grafos**: $G = H \iff V(G) = V(H) \land E(G) = E(H)$. El dibujo es solo una representación geométrica, no el objeto matemático.

### 2. Isomorfismo de Grafos
* **Definición**: Dos grafos $G$ y $H$ son isomorfos ($G \cong H$) si existe una biyección $f: V(G) 	o V(H)$ tal que:
  $$uv \in E(G) \iff f(u)f(v) \in E(H)$$
* **Invariantes por Isomorfismo**: Propiedades que deben coincidir si dos grafos son isomorfos (misma cantidad de vértices $n$, aristas $m$, secuencia de grados, conexidad).
  * *Ejemplo*: El Grafo de Petersen admite múltiples dibujos pero representa la misma estructura combinatoria. $C_6$ y $K_3 \cup K_3$ tienen la misma secuencia de grados $(2,2,2,2,2,2)$ pero no son isomorfos (uno es conexo y el otro no).

### 3. Grados y Teoremas de Estructura
* **Grado**: $\deg_G(v) = $ cantidad de aristas incidentes en $v$.
* **Lema del Apretón de Manos**:
  $$\sum_{v \in V(G)} \deg_G(v) = 2 |E(G)| = 2m$$
  * *Corolario 1*: La suma de grados de cualquier grafo es siempre par.
  * *Corolario 2*: Todo grafo tiene una cantidad par de vértices de grado impar.
* **Principio del Palomar en Grafos**:
  * Todo grafo con $n \ge 2$ vértices contiene al menos dos vértices con el mismo grado.
* **Grados en Digrafos**:
  * Grado de salida $d_{	ext{out}}(v)$, grado de entrada $d_{	ext{in}}(v)$.
  * Equilibrio: $\sum_{v \in V} d_{	ext{in}}(v) = \sum_{v \in V} d_{	ext{out}}(v) = |A(D)|$.
* **Orientación de un Grafo**: Asignar una dirección a cada arista de un grafo no dirigido (a diferencia de un digrafo general, no permite arcos en ambos sentidos entre los mismos dos vértices).

### 4. Caminos, Ciclos y Conexidad
* **Camino**: Secuencia $P = v_0, v_1, \dots, v_k$ con $\{v_i, v_{i+1}\} \in E$. Longitud $k$.
  * **Camino Simple**: No repite vértices.
  * *Lema*: Todo camino entre $u$ y $v$ contiene un camino simple entre $u$ y $v$.
* **Ciclo**: Camino cerrado $v_0, \dots, v_k$ ($v_0 = v_k$, $k \ge 3$) sin vértices repetidos en el medio.
  * *Lema*: Todo camino cerrado de longitud impar contiene un ciclo impar.
* **Conexidad**: $G$ es conexo si para todo par $u,v \in V$ existe un camino entre ellos.
* **Subgrafos e Inducción**:
  * Subgrafo $H \subseteq G$: $V(H) \subseteq V(G)$ y $E(H) \subseteq E(G)$.
  * Subgrafo inducido $G[W]$ para $W \subseteq V$: contiene exactamente las aristas de $G$ con ambos extremos en $W$.
  * Operaciones: $G - v$ (eliminar vértice y sus aristas incidentes), $G - e$ (eliminar arista conservando vértices).
* **Componentes Conexas**: Subgrafos conexos maximales de $G$.
* **Partición y Cortes**:
  * Partición $V(G) = A \dot{\cup} B$. Arista que cruza: $u \in A, v \in B$.
  * *Teorema de Conexidad por Cortes*: $G$ es conexo $\iff$ para toda partición $A \dot{\cup} B$ existe al menos una arista que cruza el corte.
* **Conceptos Estructurales**:
  * **Distancia** $	ext{dist}(u,v)$: Mínima longitud de un camino entre $u$ y $v$.
  * **Punto de Articulación**: Vértice $v$ tal que $G - v$ tiene más componentes conexas que $G$.
  * **Arista de Corte (Puente)**: Arista $e$ tal que $G - e$ tiene más componentes conexas que $G$. Proposición: $e$ es puente $\iff e$ no pertenece a ningún ciclo de $G$.
  * **Biconexidad**: $G$ es conexo y $G - v$ es conexo para todo $v \in V$.
  * **Complemento ($\overline{G}$)**: Mismos vértices, $uv \in E(\overline{G}) \iff uv 
otin E(G)$. Cumple $\deg_{\overline{G}}(v) = n - 1 - \deg_G(v)$.

### 5. Grafos Bipartitos
* **Definición**: Un grafo es bipartito si $V(G) = X \dot{\cup} Y$ de modo que toda arista une un vértice de $X$ con uno de $Y$ (no hay aristas internas en $X$ ni en $Y$).
* **Coloreo**: Equivalente a poder colorear los vértices con 2 colores de forma propia (vértices adyacentes con distinto color).
* **Teorema de Caracterización de Grafos Bipartitos**:
  $$	ext{Un grafo } G 	ext{ es bipartito} \iff G 	ext{ no contiene ciclos impares.}$$
  * *Demostración*: $\implies$ Todo ciclo alterna colores entre $X$ e $Y$, luego debe tener longitud par. $\impliedby$ Se construye la bipartición por distancia par/impar desde un vértice raíz $s$ en cada componente conexa.

---

## Fuente 2: Algoritmos sobre Grafos.pdf
**Título:** Algoritmos sobre grafos: Representaciones, ordenamiento topológico, BFS y DFS  
**Departamento:** Departamento de Computación, FCEyN - UBA (2do Cuatrimestre de 2026)

### 1. Representación de Grafos
| Estructura | Espacio | Consulta $vw \in E$ | Recorrer $N(v)$ |
| :--- | :--- | :--- | :--- |
| **Listas de Adyacencia** | $\Theta(n+m)$ | $O(d(v))$ | $O(d(v))$ |
| **Matriz de Adyacencia** | $\Theta(n^2)$ | $O(1)$ | $O(n)$ |
* *Elección*: Listas para grafos dispersos ($m \ll n^2$), Matriz para grafos densos.

### 2. Ordenamiento Topológico
* **Definición**: Orden lineal $v_1, \dots, v_n$ de un digrafo $D=(V,E)$ tal que $v_i 	o v_j \in E \implies i < j$.
* **Teorema**: $D$ admite orden topológico $\iff D$ es un DAG (Digrafo Acíclico Dirigido).
* **Algoritmo de Kahn (basado en grados de entrada)**:
  1. Computar $d^-(v)$ para todo $v$. Encolar vértices con $d^-(v) = 0$.
  2. Mientras la cola $Q 
eq \emptyset$: desencolar $u$, agregarlo al orden $L$, reducir $d^-(v)$ para todo $v \in N^+(u)$. Si $d^-(v)$ llega a $0$, encolar $v$.
  3. Si $|L| < n$, el digrafo contiene al menos un ciclo.
* **Complejidad**: $\Theta(n+m)$ con listas de adyacencia.

### 3. Búsqueda a lo Ancho (BFS - Breadth-First Search)
* **Objetivo**: Exploración por capas de distancia en grafos/digrafos sin pesos desde una fuente $s$.
* **Estados de Vértices (Colores)**:
  * *Blanco*: No descubierto ($d[v]=\infty, \pi[v]=	ext{NIL}$).
  * *Gris*: Descubierto, en cola $Q$ (frontera activa).
  * *Negro*: Procesado completamente.
* **Estructura de Datos**: Usa una **Cola (FIFO)**.
* **Propiedades Clave**:
  * *Lema de la cola*: Los elementos en $Q = \langle v_1, \dots, v_r 
angle$ cumplen $d[v_1] \le \dots \le d[v_r] \le d[v_1] + 1$.
  * Al finalizar, $d[v] = \delta(s, v)$ (distancia mínima).
  * El subgrafo de predecesores $G_\pi$ es un árbol BFS con raíz $s$.
* **Complejidad**: $O(n+m)$. Reconstrucción de caminos con `PRINT-PATH(G, s, v)` en $O(	ext{longitud})$.

### 4. Búsqueda en Profundidad (DFS - Depth-First Search)
* **Objetivo**: Explorar aristas del vértice recién descubierto más reciente. Genera un bosque DFS.
* **Estructura de Datos**: Pila de llamadas (Recursión).
* **Marcas Temporales**:
  * $d[u]$: Instante en que $u$ se descubre (pasa a gris).
  * $f[u]$: Instante en que termina de explorarse (pasa a negro).
  * Vale $1 \le d[u] < f[u] \le 2n$.
* **Teoremas Fundamentales**:
  * **Teorema de los Paréntesis**: Para cualesquiera $u, v$, los intervalos $[d[u], f[u]]$ y $[d[v], f[v]]$ son totalmente disjuntos o uno está estrictamente contenido en el otro (relación ancestro/descendiente).
  * **Teorema del Camino Blanco**: $v$ es descendiente de $u$ en el bosque DFS $\iff$ al instante $d[u]$ existe un camino de $u$ a $v$ compuesto enteramente por vértices blancos.
* **Clasificación de Aristas $(u,v)$**:
  * **Árbol**: Descubre $v$ por primera vez ($v$ blanco, $\pi[v]=u$).
  * **Retroceso (Back)**: $v$ es un ancestro activo ($v$ gris). Indica ciclo.
  * **Avance (Forward)**: $v$ es descendiente propio ($v$ negro, $d[u] < d[v]$).
  * **Cruce (Cross)**: Cualquier otra arista ($v$ negro, $d[v] < d[u]$).
  * *Regla para Grafos No Dirigidos*: **Toda arista es de árbol o de retroceso** (no hay de avance ni de cruce).
* **Complejidad**: $\Theta(n+m)$.

### 5. Detección de Aristas de Corte (Puentes)
* **Puente**: Arista cuya eliminación incrementa la cantidad de componentes conexas de $G$.
* **Propiedad**: Toda arista que no pertenece al árbol DFS está contenida en un ciclo; por ende, **solo las aristas del árbol DFS pueden ser puentes**.
* **Definición de $low[u]$**:
  Menor $d[v]$ alcanzable desde $u$ bajando 0 o más aristas del árbol DFS y usando a lo sumo una arista de retroceso.
  $$low[u] = \min egin{cases} d[u] \ d[v] & 	ext{si } \{u,v\} 	ext{ es arista de retroceso y } v 	ext{ es ancestro} \ low[w] & 	ext{si } \pi[w] = u \end{cases}$$
* **Criterio de Puente**: Una arista del árbol $\{\pi[v], v\}$ es un puente de $G \iff low[v] > d[\pi[v]]$.
* **Algoritmo**: Extiende DFS para calcular $low$ al retroceder. Complejidad: $\Theta(n+m)$ en tiempo, $O(n)$ espacio adicional.

---

## Fuente 3: Divide_and_Conquer.pdf
**Título:** Divide and Conquer  
**Departamento:** Departamento de Computación, FCEyN - UBA (2do Cuatrimestre de 2026)

### 1. La Técnica Divide & Conquer (D&C)
* **Esquema General**:
  1. **Dividir**: Subdividir la instancia $X$ de tamaño $n$ en $a$ subproblemas de tamaño $n/b$.
  2. **Conquistar**: Resolver recursivamente cada subproblema.
  3. **Combinar**: Unir las soluciones de los subproblemas para formar la solución de $X$.
* **Ejemplo: Merge Sort**:
  * Dividir: Calcular punto medio $q = \lfloor (l+r)/2 
floor$ en $\Theta(1)$.
  * Conquistar: 2 llamadas sobre arreglos de tamaño $n/2$.
  * Combinar: Procedimiento `MERGE` en $\Theta(n)$.
  * Recurrencia: $T(n) = 2T(n/2) + \Theta(n), \quad T(1) = \Theta(1) \implies \Theta(n \log n)$.

### 2. Algoritmo de Karatsuba (Multiplicación de Enteros)
* **Producto tradicional**: Multiplicación escolar toma $O(n^2)$.
* **Idea D&C Estándar**: Dividir números de $n$ dígitos en $x = x_1 2^m + x_0$, $y = y_1 2^m + y_0$ con $m pprox n/2$.
  $$x \cdot y = x_1 y_1 2^{2m} + (x_1 y_0 + x_0 y_1) 2^m + x_0 y_0$$
  Requiere 4 productos de $n/2$ dígitos $\implies T(n) = 4T(n/2) + \Theta(n) = \Theta(n^2)$.
* **Truco de Karatsuba**:
  Calcular 3 productos recursivos:
  1. $z_0 = x_0 y_0$
  2. $z_2 = x_1 y_1$
  3. $z_s = (x_0 + x_1)(y_0 + y_1)$
  Obtener la parte media: $z_1 = z_s - z_0 - z_2 = x_1 y_0 + x_0 y_1$.
  * Recurrencia: $T(n) = 3T(n/2) + \Theta(n) \implies \Theta(n^{\log_2 3}) pprox O(n^{1.585})$.

### 3. El Teorema Maestro
Para recurrencias de la forma $T(n) = a T(n/b) + f(n)$ con $a \ge 1, b > 1$, sea $q = \log_b a$:
1. **Caso 1**: Si $f(n) = O(n^{q-\epsilon})$ para algún $\epsilon > 0 \implies T(n) = \Theta(n^q)$. (Domina el trabajo en las hojas).
2. **Caso 2**: Si $f(n) = \Theta(n^q \log^r n)$ con $r \ge 0 \implies T(n) = \Theta(n^q \log^{r+1} n)$. (Trabajo distribuido uniformemente entre niveles).
3. **Caso 3**: Si $f(n) = \Omega(n^{q+\epsilon})$ y $a f(n/b) \le c f(n)$ para $c < 1 \implies T(n) = \Theta(f(n))$. (Domina el trabajo en la raíz).

### 4. Algoritmo de Strassen (Multiplicación de Matrices)
* **Producto matricial estándar**: $n 	imes n$ toma $O(n^3)$.
* **Algoritmo de Strassen**:
  Divide las matrices en bloques de $n/2 	imes n/2$. Realiza 7 multiplicaciones de bloques ($M_1, \dots, M_7$) en lugar de 8.
  * Recurrencia: $T(n) = 7 T(n/2) + \Theta(n^2)$.
  * Por Teorema Maestro (Caso 1, $q = \log_2 7 pprox 2.807$): $T(n) = \Theta(n^{\log_2 7}) pprox O(n^{2.807})$.

### 5. Problema de la Timba Financiera / Máximo Subarreglo
* **Problema**: Dadas predicciones de precios $p_1, \dots, p_n$, maximizar $p_j - p_i$ con $i < j$.
* **Reducción**: Definir diferencias diarias $d_k = p_{k+1} - p_k$. $p_j - p_i = \sum_{k=i}^{j-1} d_k$. Se reduce al **Problema del Máximo Subarreglo**.
* **Enfoque D&C Inicial**:
  Dividir en mitades $L$ y $R$. El máximo subarreglo está en $L$, en $R$, o cruza el centro.
  Calculo del cruce toma $\Theta(n) \implies T(n) = 2T(n/2) + \Theta(n) = \Theta(n \log n)$.
* **Enfoque D&C Óptimo ($\Theta(n)$)**:
  Cada llamada devuelve 4 valores en $O(1)$:
  * `tot(X)`: Suma total del segmento.
  * `pre(X)`: Máximo prefijo.
  * `suf(X)`: Máximo sufijo.
  * `best(X)`: Máximo subarreglo global.
  Combinar: `best(X) = max{best(L), best(R), suf(L) + pre(R)}`.
  Recurrencia: $T(n) = 2T(n/2) + \Theta(1) \implies \Theta(n)$.

### 6. Búsqueda Binaria Generalizada
* Recurrencia: $T(n) \le T(n/2) + \Theta(1) \implies O(\log n)$.
* **Generalización**: Dominio ordenado $T$ y predicado monótono $P(t)$ que pasa de `falso` a `verdadero` en un umbral $t^*$.
* **Ejemplo del Encuentro**: $n$ amigos en posiciones $p_i$ con velocidades $v_i$. Menor tiempo $t$ tal que la intersección de intervalos de alcance $I_{i,t} = [p_i - v_i t, p_i + v_i t]$ sea no vacía. Búsqueda binaria sobre $t \in [0, T]$ en $O(n \log T)$.

---

## Fuente 4: Fuerza Bruta y BackTraking.pdf
**Título:** Fuerza bruta y backtracking: Conjuntos de soluciones, árbol de backtracking y podas  
**Departamento:** Departamento de Computación, FCEyN - UBA (2do Cuatrimestre de 2026)

### 1. Motivación y Combinatoria
* **Problema de las $n$ Reinas** (Max Bezzel, 1848; Gauss, 1850): Ubicar $n$ reinas en un tablero de $n 	imes n$ sin que se ataquen entre sí (misma fila, columna o diagonal).
* **Definición del universo de candidatas ($Sols$)**:
  1. *Ocho casillas cualesquiera*: $inom{64}{8} pprox 4,4 	imes 10^9$ posibilidades.
  2. *Una reina por fila*: $8^8 pprox 1,7 	imes 10^7$ posibilidades.
  3. *Una reina por fila y columna (permutaciones)*: $8! = 40.320$ posibilidades.
  *Lección*: Elegir correctamente el espacio de candidatas antes de programar reduce drásticamente el espacio de búsqueda.
* **Quiz Combinatorio Básicos**:
  * Hojas de árbol binario de $n$ niveles: $2^n$.
  * Subconjuntos de un conjunto de $n$ elementos: $2^n$.
  * Permutaciones de $n$ elementos: $n!$.
  * Repartir $n$ elementos en $k$ cajas: $k^n$.
  * Subconjuntos de tamaño $k$ en $n$ elementos: $inom{n}{k}$.

### 2. Formalización de Problemas
Para abordar formalmente un problema se deben definir tres elementos:
1. **$Sols$**: Conjunto finito de soluciones candidatas.
2. **$válida(a)$**: Predicado booleano que determina si $a \in Sols$ es solución.
3. **$Sols_{válidas}$**: $\{a \in Sols : válida(a)\}$.

**Las 5 preguntas estándar sobre $Sols_{válidas}$**:
* **Decisión**: ¿Existe alguna? ($\exists a \in Sols : válida(a)$).
* **Construcción**: Dar una solución $a \in Sols_{válidas}$.
* **Conteo**: ¿Cuántas soluciones hay? ($|Sols_{válidas}|$).
* **Enumeración**: Listar todas las soluciones ($Sols_{válidas}$).
* **Optimización**: Encontrar $a^* \in Sols_{válidas}$ que maximice/minimice $valor(a)$.

**Ejemplos de Formalización**:
* **Mochila 0/1**: $n$ objetos con pesos $w_i \in \mathbb{N}$, valores $v_i \in \mathbb{N}$, capacidad $W$.
  * $Sols = \{0,1\}^n$. $peso(a) = \sum a_i w_i$, $valor(a) = \sum a_i v_i$.
  * $válida(a) \iff peso(a) \le W$.
  * Objetivo: Maximizar $valor(a)$ sobre $Sols_{válidas}$.
* **Asignación de Tareas**: $n$ personas y $n$ tareas, costo $c_{ij}$.
  * $Sols = 	ext{permutaciones } \pi 	ext{ de } \{1, \dots, n\}$. $válida(\pi) = 	ext{True}$.
  * $costo(\pi) = \sum c_{i, \pi(i)}$. Objetivo: Minimizar $costo(\pi)$.
* **Cuadrados Latinos**: Grillas de $n 	imes n$ con números del $1$ al $n$ sin repetir por fila ni columna.
  * Opción 1: $Sols_1 = \{1,\dots,n\}^{n^2}$, $|Sols_1| = n^{n^2}$.
  * Opción 2: $Sols_2 = (S_n)^n$ (vectores de permutaciones), $|Sols_2| = (n!)^n$.

### 3. Fuerza Bruta y Árbol de Backtracking
* **Fuerza Bruta**: Recorre exhaustivamente $Sols$ evaluando $válida(a)$.
* **Generación Recursiva**:
  * Para $Sols = \{0,1\}^n$, la función `Generar(i, p)` toma la decisión $i$-ésima manteniendo $p = (a_1, \dots, a_{i-1})$.
* **Ahorro de Memoria (Resumen de decisiones pasadas)**:
  * En lugar de pasar todo el vector $p$, se pasan parámetros acumulados (e.g. peso acumulado $pa$ y valor acumulado $va$).
  * *Regla de Erickson*: Los parámetros deben incluir el índice del problema pendiente y el resumen mínimo de las decisiones pasadas necesario para verificar validez/valor.
* **Estructura del Árbol**:
  * **Nodos**: Soluciones parciales (prefijos o sufijos).
  * **Hijos**: Sucesoras (agregar una decisión).
  * **Hojas**: Soluciones candidatas.
  * Recorrido en profundidad (DFS) explorando y deshaciendo alternativas (backtracking).

### 4. Podas (Pruning)
* **Podas por Factibilidad**:
  * Regla de factibilidad $R(p)$: Condición necesaria para que $p$ tenga alguna extensión válida.
  * Si $
eg R(p) \implies Ext(p) \cap Sols_{válidas} = \emptyset$. Se corta la búsqueda en $p$.
  * *Ejemplo Mochila*: Si $pa > W$, como $w_j \ge 0$, toda extensión $a \in Ext(p)$ cumple $peso(a) \ge pa > W$.
  * *Molde de demostración*: Supongo $
eg R(p) \implies$ tomo $a \in Ext(p)$ arbitraria $\implies$ muestro que la violación de restricción se hereda $\implies 
eg válida(a)$.
* **Podas por Optimalidad**:
  * Mantiene una variable global `mejor` con la mejor solución válida encontrada hasta el momento.
  * Define una cota superior $U(p)$ tal que $orall a \in Ext(p) \cap Sols_{válidas}, valor(a) \le U(p)$.
  * Si $U(p) \le mejor \implies$ no se explora el subárbol de $p$.
  * *Ejemplo Mochila*: $U(p) = va + \sum_{j=i}^n v_j$. Precalculando $S_i = \sum_{j=i}^n v_j$ en $O(n)$, la cota se evalúa en $O(1)$.
* **Branch and Bound**: Fuerza bruta + cotas + buen orden de exploración (e.g., explorar primero objetos con mayor relación valor/peso $v_j/w_j$).

### 5. Resumen de Variantes del Algoritmo
| Pregunta | Hoja Válida / Inválida | Combinar Hijos | Corte Temprano | Podas Aplicables |
| :--- | :--- | :--- | :--- | :--- |
| **Decisión** | `True` / `False` | $\lor$ | Sí (al 1er `True`) | Factibilidad |
| **Construcción** | Guardar $p$ / Nada | $\lor$ | Sí | Factibilidad |
| **Conteo** | $1$ / $0$ | $+$ | No | Factibilidad |
| **Enumeración** | Imprimir $p$ / Nada | (Ninguno) | No | Factibilidad |
| **Optimización** | $valor(p)$ / $-\infty$ | $\max$ | No | Factibilidad y Optimalidad |

### 6. Complejidad
* **Tiempo**: $T \le (	ext{nodos visitados}) 	imes (	ext{trabajo por nodo})$.
  * Árbol completo de grado $k$ y altura $n$: $O(k^n)$ nodos.
  * Mochila: $O(2^n)$ nodos, $O(1)$ trabajo por nodo (usando vector compartido/acumuladores) $\implies O(2^n)$.
  * $n$-Reinas con permutaciones: $O(n!)$ nodos, trabajo $O(n)$ por nodo $\implies O(n \cdot n!)$.
* **Espacio**: Profundidad del árbol $	imes$ estado por llamada. Típicamente $O(n)$ si el vector de solución se comparte globalmente y se modifica in-place (`push/pop`).

---

## Fuente 5: Algoritmos Fuerza Bruta.pdf
**Título:** Demostraciones de algoritmos de fuerza bruta: Semántica, inducción, correctitud de podas y complejidad  
**Departamento:** Departamento de Computación, FCEyN - UBA (2do Cuatrimestre de 2026)

### 1. Correctitud en Backtracking
Para demostrar formalmente un algoritmo de backtracking se requieren tres pasos:
1. **Fidelidad de la formalización**: Argumentar que $Sols_{válidas}$ mapea las soluciones reales del problema.
2. **Correctitud de la función recursiva**: Probar por inducción que la función recursiva cumple su **semántica**.
3. **Correctitud de las podas**: Demostrar matemáticamente que no se eliminan soluciones válidas ni óptimas.

### 2. La Semántica de una Función Recursiva
* **Definición**: Una fórmula matemática $\sigma(x_1, \dots, x_k)$ expresada **sin usar la propia función** $f$ ni apelar a términos de ejecución (como "esta llamada" o "el algoritmo").
* **Patrón Operador**:
  $$	ext{Semántica} = 	ext{op} \{ 	ext{valor}(b) : b \in 	ext{Compl}(	ext{parámetros}) \}$$
  donde $	ext{op} \in \{\max, \min, \exists, \sum\}$ y se define $	ext{op}(\emptyset) = 	ext{neutro}$ ($-\infty$ para $\max$, $\infty$ para $\min$, $	ext{False}$ para $\exists$, $0$ para $\sum$).
* **Completaciones válidas ($	ext{Compl}$)**:
  Para Mochila: $	ext{Compl}(i, pa) = \{ b \in \{0,1\}^{n-i+1} : pa + \sum_{j=i}^n b_j w_j \le W \}$.
  Semántica: $\mathcal{M}(i, pa, va) = \max \{ va + \sum_{j=i}^n b_j v_j : b \in 	ext{Compl}(i, pa) \}$.

### 3. Estructura de la Demostración por Inducción
* **Variable de inducción**: Se induce sobre la cantidad de decisiones pendientes $m = n - i + 1$ (que decrece hacia $0$).
* **Propiedad de inducción**:
  $$P(m) : orall pa, va \in \mathbb{N}, \quad 	ext{mochila}(n-m+1, pa, va) = \mathcal{M}(n-m+1, pa, va)$$
* **Lema Fundamental (Máximo de una Unión)**:
  $$\max(A \cup B) = \max(\max A, \max B) \quad 	ext{con } \max \emptyset = -\infty$$
* **Pasos de la Demostración**:
  1. *Caso base*: $m = 0 \implies i = n+1$. $	ext{Compl}(n+1, pa) = \{()\}$ si $pa \le W$, else $\emptyset$. Coincide con el caso base del código.
  2. *Paso inductivo*: Asumir $P(m-1)$. Partir $	ext{Compl}(i, pa) = \{0 \oplus b'\} \cup \{1 \oplus b'\}$. Aplicar la HI a ambas partes y combinar con el lema del máximo de la unión.

### 4. Demostración de Podas y Variables Globales
* **Poda por Factibilidad**: Se demuestra como un caso base adicional: Si $pa > W$, $	ext{Compl}(i, pa) = \emptyset \implies \mathcal{M}(i, pa, va) = -\infty$.
* **Poda por Optimalidad (con variable global `mejor`)**:
  * Se demuestra mediante un invariante de la ejecución:
    * $H1$: `mejor` nunca decrece.
    * $H2$: `mejor` es siempre $-\infty$ o el valor de alguna solución válida.
    * $H3$ (Lema Principal): Tras la llamada, $	ext{mejor}_1 \ge \mathcal{M}(i, pa, va)$.
  * Para $H3$, en el caso de la poda $va + S_i \le 	ext{mejor}_0$, se usa la cota $\mathcal{M}(i, pa, va) \le va + S_i \le 	ext{mejor}_0 = 	ext{mejor}_1$.

### 5. Problemas Completos Demostrados
* **Conjunto Independiente de Tamaño $k$**:
  * Grafo $G=(V,E)$, decidir si existe $I \subseteq V$ con $|I|=k$ independiente.
  * Función $	ext{ind}(i, S, r)$: $i$ vértice actual, $S$ conjunto independiente acumulado, $r = k - |S|$ restantes.
  * Semántica: $	ext{ind}(i, S, r) = [\exists I \subseteq \{i, \dots, n\} : |I| = r \land S \cup I 	ext{ es independiente}]$.
  * Complejidad: $O(n \cdot 2^n)$.
* **Cambio Mínimo**:
  * Tipos de monedas $c_1, \dots, c_r \ge 1$, monto $m$.
  * Semántica: $	ext{cambio}(m') = \min \{ \ell \in \mathbb{N} : \exists s \in \{1,\dots,r\}^\ell, \sum c_{s_t} = m' \}$.
  * Inducción fuerte sobre el monto $m'$. Poda por cota inferior $L(m', \ell) = \ell + \lceil m' / c_{\max} 
ceil$.

### 6. Errores Comunes
1. *Falta de semántica o semántica circular* ("devuelve lo mejor a partir de la llamada").
2. *Inducir sobre variables que crecen* (inducir en $i$ directamente en lugar de $n-i+1$).
3. *Olvidar cuantificar parámetros acumulados* en la hipótesis inductiva ($orall pa, va$).
4. *Particiones no disjuntas* al contar soluciones.
5. *Trabajo no considerado por nodo* (e.g., copiar arreglos $O(n)$ versus referencias globales $O(1)$).
