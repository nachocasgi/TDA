# Prácticas Condensadas: Grafos, Algoritmos, Divide & Conquer y Backtracking

Este documento reúne y sintetiza de manera rigurosa la totalidad de los conceptos, algoritmos y demostraciones formales desarrollados en las clases y diapositivas.

---

## 1. Grafos: Propiedades y Demostraciones Formales

### 1.1. Demostración Directa: Ciclo Compartido entre Caminos
**Enunciado:** Sean $P$ y $Q$ dos caminos distintos en un grafo simple $G$ que unen el vértice $v$ con el vértice $w$. Demostrar en forma directa que $G$ contiene un ciclo cuyas aristas pertenecen a $P \cup Q$.

* **Estructura de los caminos:**
  * $P = (p_1, p_2, \dots, p_k)$ con $p_1 = v$ y $p_k = w$.
  * $Q = (q_1, q_2, \dots, q_r)$ con $q_1 = v$ y $q_r = w$.
* **Punto de primera separación:** Dado que $P \neq Q$ pero $p_1 = q_1 = v$, existe un índice $i > 1$ tal que $p_l = q_l$ para todo $1 \le l < i$, pero $p_i \neq q_i$. El vértice $p_{i-1} = q_{i-1}$ es el último punto común antes de bifurcarse.
* **Punto de primer reencuentro:** Como ambos caminos finalizan en $w$, necesariamente deben volver a cruzarse. Sean $p_{j_p}$ y $q_{j_q}$ el primer vértice donde se reencuentran tras la separación ($p_{j_p} = q_{j_q}$), sin vértices compartidos intermedios para $i \le l_p < j_p$ y $i \le l_q < j_q$.
* **Construcción del ciclo:** El ciclo $C$ se forma con el subcamino de $P$ entre $p_{i-1}$ y $p_{j_p}$, unido al subcamino de $Q$ entre $q_{j_q}$ y $q_{i-1}$ (recorrido en sentido inverso).
* **Validez ($|C| \ge 3$):** Como $p_i \neq q_i$, la bifurcación garantiza que $C$ contiene al menos 3 vértices distintos, evitando ciclos triviales de 2 nodos que no existen en grafos simples.

---

### 1.2. Demostración por Contrarrecíproco: Intersección de Caminos Máximos
**Enunciado:** Sea $G$ un grafo conexo. Demostrar por contrarrecíproco que todo par de caminos de longitud máxima en $G$ tienen al menos un vértice en común.

* **Esquema de la prueba:** 
  * Afirmación original: $\text{LongMax}(C) \land \text{LongMax}(C') \implies C \cap C' \neq \emptyset$.
  * Contrarrecíproco: $C \cap C' = \emptyset \implies C \text{ o } C' \text{ no es de longitud máxima}$.
* **Desarrollo:**
  * Supongamos dos caminos simples disjuntos $C$ (de $u$ a $v$) y $C'$ (de $x$ a $y$), con $|C| = |C'| = L$.
  * Dado que $G$ es conexo, existe un camino simple $U$ en $G$ que conecta un vértice $p \in C$ con un vértice $q \in C'$, tal que el interior de $U$ no interseca ni a $C$ ni a $C'$.
  * El vértice $p$ divide a $C$ en dos subcaminos; tomamos el de mayor longitud $P'$, cumpliendo $|P'| \ge L/2$. De igual forma, $q$ divide a $C'$ en dos subcaminos; tomamos el más largo $Q'$, con $|Q'| \ge L/2$.
  * Construimos el camino compuesto $T = P' + U + Q'$. La longitud de $T$ resulta:
    $$|T| = |P'| + |U| + |Q'| \ge \frac{L}{2} + 1 + \frac{L}{2} = L + 1 > L$$
  * Como $T$ es un camino simple con longitud estricta mayor a $L$, ni $C$ ni $C'$ podían ser de longitud máxima, concluyendo la prueba.

---

### 1.3. Prueba Inductiva: Vértices No de Articulación en Grafos Conexos
**Enunciado:** Todo grafo conexo $G_n$ con $n \ge 2$ vértices posee al menos dos vértices distintos $v_1, v_2$ tales que $G_n \setminus \{v_1\}$ y $G_n \setminus \{v_2\}$ son conexos.

* **Caso base ($n = 2$):** El único grafo conexo con 2 vértices es $K_2$. Remover cualquiera de los dos vértices deja un grafo aislado con 1 vértice, que es conexo por definición.
* **Paso inductivo:** Asumimos cierto para $n$. Para $G_{n+1}$ conexo, tomamos un vértice cualquiera $v$:
  * **Caso A:** Si $G_{n+1} \setminus \{v\}$ sigue siendo conexo, por hipótesis inductiva este subgrafo de $n$ vértices posee dos vértices no de articulación $u_1, u_2$. Al menos uno de ellos difiere de $v$, garantizando la propiedad para $G_{n+1}$.
  * **Caso B:** Si $v$ es un punto de corte, $G_{n+1} \setminus \{v\}$ se desconecta en componentes conexas $C_1, C_2, \dots, C_k$ ($k \ge 2$). Consideramos los subgrafos inductivos $C'_i = C_i \cup \{v\}$. Cada $C'_i$ es conexo y posee menos de $n+1$ vértices, por lo que contiene al menos dos vértices que no lo desconectan. Al menos uno de ellos no es $v$ dentro de cada componente, proveyendo los vértices buscados para el grafo global.

---

### 1.4. Biconexión por Densidad de Aristas y Cota Óptima
**Enunciado:** 
a) Demostrar por el absurdo que todo grafo con $n$ vértices y al menos $m \ge 2 + \frac{(n-1)(n-2)}{2}$ aristas es biconexo.
b) Demostrar que la cota es óptima.

* **Demostración de biconexidad (Ítem a):**
  * Recordatorio previo: Todo grafo con $k$ vértices y más de $\frac{(k-1)(k-2)}{2}$ aristas es conexo.
  * Para $n$ vértices y $m \ge 2 + \frac{(n-1)(n-2)}{2}$, como $2 + \frac{(n-1)(n-2)}{2} > \frac{(n-1)(n-2)}{2}$, el grafo $G$ es conexo.
  * Por el absurdo, supongamos que $G$ no es biconexo: existe un punto de articulación $v$.
  * Al eliminar $v$, el subgrafo $G \setminus \{v\}$ tiene $n-1$ vértices. Como el grado máximo de $v$ es $n-1$, la cantidad de aristas restantes en $G \setminus \{v\}$ cumple:
    $$m' \ge \left(2 + \frac{(n-1)(n-2)}{2}\right) - (n-1) = 1 + \frac{(n-2)(n-3)}{2} > \frac{(n-2)(n-3)}{2}$$
  * Esta cantidad de aristas obliga a que $G \setminus \{v\}$ sea conexo, contradiciendo que $v$ fuera punto de articulación. Por lo tanto, $G$ es biconexo.
* **Optimalidad de la cota (Ítem b):**
  * Para demostrar que no puede reducirse la cota, construimos un contraejemplo con $1 + \frac{(n-1)(n-2)}{2}$ aristas que **no** es biconexo:
  * Tomamos un grafo completo $K_{n-1}$ (el cual tiene $n-1$ vértices y $\frac{(n-1)(n-2)}{2}$ aristas) y le añadimos un nodo $x$ conectado a un único vértice $u \in K_{n-1}$.
  * Este grafo resultante posee $n$ vértices y exactamente $1 + \frac{(n-1)(n-2)}{2}$ aristas. Sin embargo, al retirar el nodo $u$, el vértice $x$ queda totalmente desconectado, por lo que $u$ es un punto de corte y el grafo no es biconexo.

---

### 1.5. Falacias Lógicas en Pruebas Inductivas de Grafos
**Análisis del error común:**
Al intentar probar por inducción propiedades de grafos sobre el número de vértices $n$, una falla estructural recurrente consiste en **construir el caso $n+1$ agregando un vértice a un grafo de tamaño $n$** que satisface la hipótesis.

* **Origen de la falacia:** Asumir que *todo* grafo válido de tamaño $n+1$ se puede obtener agregando un elemento a un grafo válido de tamaño $n$. Esto solo demuestra la propiedad para una clase restringida de grafos y no para la totalidad de grafos de tamaño $n+1$.
* **Formulación correcta:** La inducción válida en grafos debe partir de un grafo **arbitrario** de tamaño $n+1$, remover un elemento adecuado para aplicar la Hipótesis Inductiva sobre la estructura reducida de tamaño $n$, y luego reconstruir las relaciones para concluir el resultado sobre $n+1$.

---

## 2. Algoritmos Sobre Grafos

### 2.1. Recorridos Fundamentales: DFS y BFS
* **Depth First Search (DFS):**
  * Explora el grafo en profundidad utilizando recursión o una pila implícita.
  * Clasifica las aristas del grafo en:
    * *Tree-edges* (aristas de árbol): conectan con nodos no visitados.
    * *Back-edges* (aristas de retroceso): conectan con ancestros en el árbol DFS (indican presencia de ciclos).
  * Complejidad: $\mathcal{O}(n + m)$ con listas de adyacencia.
* **Breadth First Search (BFS):**
  * Explora por niveles o capas utilizando una cola FIFO.
  * Calcula distancias mínimas en cantidad de aristas desde el nodo raíz $v$ a cualquier otro vértice (construye un árbol $v$-geodésico).
  * Complejidad: $\mathcal{O}(n + m)$.

---

### 2.2. Árboles Geodésicos y Árbol Geodésico de Peso Mínimo
**Problema:** Dado un grafo conexo $G$ con pesos en sus aristas y un vértice $v$, hallar un árbol $v$-geodésico de peso total mínimo en tiempo $\mathcal{O}(n + m)$.

* **Algoritmo de resolución:**
  1. Ejecutar un BFS no pesado desde $v$ para calcular la distancia mínima en aristas $d[u] = d_G(v, u)$ para todo nodo $u$.
  2. Para cada vértice $u \neq v$, examinar sus vecinos $w$ que pertenecen al nivel anterior ($d[w] = d[u] - 1$).
  3. Seleccionar como padre $p(u)$ a aquel vecino $w$ que minimice el peso de la arista $c(w, u)$:
     $$p(u) = \arg\min_{w \in N(u) : d[w] = d[u] - 1} c(w, u)$$
  4. Agregar las aristas $(p(u), u)$ al árbol $T$.
* **Demostración de correctitud:** Por construcción, $d[p(u)] = d[u] - 1$, por lo que las distancias geodésicas se conservan exactamente. Además, al seleccionar una única arista entrante hacia cada nodo desde el nivel anterior, el subgrafo resultante es conexo, no posee ciclos y contiene $n-1$ aristas, constituyendo un árbol geodésico de peso mínimo global.

---

### 2.3. Chequeo de Conectividad y Componentes Conexas
* **Verificación de conexidad:** Se inicia DFS/BFS desde un nodo arbitrario. Al finalizar, se verifica el arreglo `visitado`. Si todos los elementos son `true`, el grafo es conexo; de lo contrario, se retorna `false` inmediatamente.
* **Conteo de componentes conexas:** Se itera sobre todos los vértices del grafo. Si un nodo no ha sido visitado, se incrementa el contador de componentes y se dispara una búsqueda desde dicho nodo para marcar toda la componente conexa correspondiente.

---

### 2.4. Bipartitud y 2-Coloreo
**Definición:** Un grafo $G = (V, E)$ es bipartito si $V = V_1 \cup V_2$ con $V_1 \cap V_2 = \emptyset$, tal que toda arista une un elemento de $V_1$ con uno de $V_2$.
**Lema:** $G$ es bipartito $\iff$ $G$ no contiene ciclos impares.

* **Algoritmo basado en DFS:**
  * Se asigna el color $0$ al nodo inicial y se recorren los vecinos pintándolos alternadamente con el color $1 - c$.
  * Si durante la búsqueda se encuentra un vecino ya visitado con el **mismo color** del nodo actual, se detecta un ciclo impar y se retorna `false`.

```cpp
bool dfs(int v, int c) {
    color[v] = c;
    for (int u : aristas[v]) {
        if (color[u] == -1) {
            if (!dfs(u, 1 - c)) return false;
        } else if (color[u] == c) {
            return false; // Conflicto de colores: ciclo impar
        }
    }
    return true;
}
```

---

### 2.5. Detección Lineal de Aristas Puente con `cubren(v)`
**Definición:** Una arista $vw$ es un puente de $G \iff vw$ no pertenece a ningún ciclo de $G$.

* **Algoritmo en tiempo $\mathcal{O}(n + m)$:**
  * Se ejecuta DFS registrando para cada nodo $v$ dos contadores de backedges:
    * `backConExtremoInferiorEn[v]`: backedges que nacen en $v$ subiendo a un ancestro.
    * `backConExtremoSuperiorEn[v]`: backedges que finalizan en $v$ desde un descendiente.
  * Se define la función `cubren(v)`, representa la cantidad de backedges que atraviesan la tree-edge entre $v$ y su padre:
    $$\text{cubren}(v) = \text{backInferior}[v] - \text{backSuperior}[v] + \sum_{w \in \text{hijos}(v)} \text{cubren}(w)$$
  * **Criterio de puente:** La arista que conecta a $v$ con su padre es **puente** si y solo si $\text{cubren}(v) = 0$.
* **Aplicación a orientación de calles:** Para hacer transitables las calles de una ciudad de forma unidireccional manteniendo la conectividad global:
  * Las aristas puente deben conservarse **bidireccionales** (o de lo contrario la ciudad queda incomunicada).
  * Las aristas no-puente se orientan según el recorrido DFS: las tree-edges hacia los descendientes y las backedges hacia los ancestros.

---

### 2.6. Puentes vs. Puntos de Corte
* **Propiedad verdadera:** Si $G$ es conexo ($|V| \ge 3$) y no tiene puntos de corte, entonces $G$ no tiene puentes.
  * *Demostración:* Si tuviese un puente $(v, w)$, al quitarlo se divide en componentes $A$ y $B$. Como $|V| \ge 3$, alguna componente (digamos $A$) tiene al menos 2 vértices. El nodo $v$ actúa como punto de corte para desconectar $A \setminus \{v\}$ de $B$, contradiciendo la premisa.
* **Afirmaciones falsas comunmente confundidas:**
  * *"Grafo conexo sin puentes implica 1 solo ciclo":* FALSO (contraejemplo: dos ciclos compartiendo un vértice).
  * *"Grafo conexo con exactamente 1 ciclo implica sin puentes":* FALSO (contraejemplo: un triángulo unido a una arista colgante/hoja).

---

## 3. Divide y Conquista / Divide and Conquer

### 3.1. Esquema Metodológico de 5 Pasos
Para la resolución formal de problemas mediante Divide y Conquista se debe estructurar la solución en 5 etapas estrictas:

1. **Análisis:** Comprender la estructura del problema e identificar las propiedades del dominio que permiten la división eficiente.
2. **Algoritmo:** Descripción conceptual en pseudocódigo o lenguaje natural.
3. **Etapas D&C:** Identificación explícita de:
   * **Dividir:** Partición del problema de tamaño $n$ en $k$ subproblemas menores.
   * **Conquistar:** Resolución recursiva de los subproblemas (o caso base).
   * **Combinar:** Unificación de las soluciones parciales para construir la solución global.
4. **Correctitud:** Demostración formal (generalmente inductiva o por invariante) de que no se descartan soluciones válidas.
5. **Complejidad:** Planteo y resolución de la ecuación de recurrencia $T(n)$.

---

### 3.2. Ecuaciones de Recurrencia y Teorema Maestro
La complejidad temporal de un algoritmo D&C estándar responde a la ecuación:
$$T(n) = a T\left(\frac{n}{b}\right) + f(n)$$
donde $a \ge 1$ es la cantidad de subproblemas, $b > 1$ es el factor de división, y $f(n)$ es el costo de las etapas de dividir y combinar.

**Teorema Maestro (Comparación de $f(n)$ contra $n^q$ con $q = \log_b a$):**
1. **Caso 1:** Si $f(n) = \mathcal{O}(n^{q - \varepsilon})$ para $\varepsilon > 0$, entonces $T(n) = \Theta(n^q)$.
2. **Caso 2:** Si $f(n) = \Theta(n^q \log^r n)$ con $r \ge 0$, entonces $T(n) = \Theta(n^q \log^{r+1} n)$.
3. **Caso 3:** Si $f(n) = \Omega(n^{q + \varepsilon})$ y cumple la condición de regularidad $a f(n/b) \le c f(n)$ ($c < 1$), entonces $T(n) = \Theta(f(n))$.

---

### 3.3. Balance de Subproblemas
* Una condición fundamental para que D&C reduzca la complejidad asintótica es que los subproblemas estén **balanceados en tamaño** y que el costo de combinación no domine la ejecución.
* *Ejemplo:* Dividir un problema de tamaño $n$ en $n/2$ y $n/2$ pero necesitando procesar todos los elementos en combinación con $T(n) = 2T(n/2) + \Theta(n)$ da $\Theta(n \log n)$. Sin embargo, si la reducción es desbalanceada (ej. $T(n) = T(n-1) + \Theta(1)$), la complejidad se degrada a $\Theta(n)$.

---

### 3.4. Problema del Pico en Arreglo Montaña (Optimización Unimodal)
**Enunciado:** Un arreglo es montaña si aumenta estrictamente hasta un pico y luego decrece estrictamente. Hallar el índice del pico en $\mathcal{O}(\log n)$.

* **Algoritmo:**
  1. Tomar el elemento medio $m = \lfloor (izq + der)/2 \rfloor$.
  2. Si $arr[m] < arr[m+1]$, el pico está a la derecha: llamar recursivamente sobre $[m+1, der]$.
  3. Si $arr[m] < arr[m-1]$, el pico está a la izquierda: llamar sobre $[izq, m-1]$.
  4. Si $arr[m] > arr[m-1]$ y $arr[m] > arr[m+1]$, $m$ es el pico.
* **Complejidad:** $T(n) = T(n/2) + \Theta(1) \implies \Theta(\log n)$ por Teorema Maestro (Caso 2 con $a=1, b=2, q=0$).

---

### 3.5. Diámetro en Árboles Binarios en $\Theta(n)$
**Problema:** Calcular el camino más largo (diámetro) en un árbol binario sin realizar recorridos redundantes.

* **Análisis:** El diámetro $D(T)$ puede presentarse en uno de tres casos exhaustivos:
  1. El camino está contenido enteramente en el subárbol izquierdo: $D(T_{izq})$.
  2. El camino está contenido enteramente en el subárbol derecho: $D(T_{der})$.
  3. El camino pasa por la raíz: $h(T_{izq}) + h(T_{der}) + 2$ (donde $h$ es la altura).
* **Algoritmo $\Theta(n)$:** Se retorna en un solo pasaje el par `(altura, camino)` para evitar recalcular alturas en $\mathcal{O}(n^2)$.

```python
def diametro(nodo):
    if nodo is None:
        return -1, 0  # (altura, camino)
    
    h_izq, d_izq = diametro(nodo.izq)
    h_der, d_der = diametro(nodo.der)
    
    altura = 1 + max(h_izq, h_der)
    camino = max(d_izq, d_der, h_izq + h_der + 2)
    
    return altura, camino
```
* **Complejidad:** $T(n) = T(k) + T(n - k - 1) + \Theta(1) = \Theta(n)$, pues visita cada nodo exactamente una vez.

---

## 4. Algoritmos de Fuerza Bruta y Backtracking

### 4.1. Marco Conceptual de Backtracking
Backtracking explora recursivamente el espacio de soluciones modelado como un árbol de búsqueda. Se formaliza mediante tres conjuntos fundamentales:

* **Soluciones Parciales ($S_{parcial}$):** Estados intermedios construidos paso a paso.
* **Soluciones Candidatas ($S_{candidata}$):** Soluciones completas generadas al alcanzar las hojas del árbol.
* **Soluciones Válidas ($S_{valida}$):** Soluciones candidatas que satisfacen todas las restricciones del problema.
* **Podas:** Interrupción de la exploración recursiva en una rama cuando se demuestra que ninguna extensión de la solución parcial actual podrá conducir a una solución válida o mejor que la óptima actual.

---

### 4.2. Caso de Estudio: Solución de Sudoku
* **Solución Parcial:** Celda por celda completada hasta el momento.
* **Solución Candidata:** Tablero lleno de $N \times N$.
* **Solución Válida:** Tablero lleno sin números repetidos por fila, columna ni subcuadrante.
* **Podas por validez:** Antes de colocar un número $v \in [1..N]$ en la casilla libre $(i, j)$, se verifica si ya existe $v$ en la fila $i$ o en la columna $j$. Si existe, se poda la rama inmediatamente.

---

### 4.3. Problema del Viajante de Comercio (TSP - Traveling Salesperson Problem)
**Formulación:** Dado un grafo completo $G = (V, E)$ con pesos $w: E \to \mathbb{N}$, hallar una permutación $\pi$ de $\{1, \dots, n\}$ que minimice la suma del ciclo hamiltoniano:
$$\text{costo}(\pi) = w(\pi(n), \pi(1)) + \sum_{i=1}^{n-1} w(\pi(i), \pi(i+1))$$

* **Modelado en Backtracking:**
  * Solución parcial: Camino simple de longitud $k \le n$.
  * Solución candidata / válida: Permutación completa de los $n$ vértices.
  * Extensión: Desde la tupla parcial $\pi$, generar los sucesores $\pi \oplus v$ para todo $v \notin \pi$.

```python
def viajante(camino, visitados, suma_actual, mejor_costo):
    if len(camino) == n:
        costo_total = suma_actual + w[camino[-1]][camino[0]]
        return min(mejor_costo, costo_total)
    
    # Poda por optimalidad
    if suma_actual >= mejor_costo:
        return mejor_costo
        
    for v in range(n):
        if not visitados[v]:
            visitados[v] = True
            camino.append(v)
            
            costo_paso = w[camino[-2]][v] if len(camino) > 1 else 0
            mejor_costo = viajante(camino, visitados, suma_actual + costo_paso, mejor_costo)
            
            camino.pop()
            visitados[v] = False
            
    return mejor_costo
```

* **Complejidad Temporal:** $\mathcal{O}(m!)$ donde $m$ es el número de vértices pendientes de decisión.
* **Complejidad Espacial:** $\mathcal{O}(n)$ manteniendo la pila de llamadas recursivas y el estado del camino actual.

---

### 4.4. Problema de Suma de Subconjuntos (Subset Sum)
* **Objetivo:** Dado un conjunto de enteros $S = \{s_1, \dots, s_n\}$ y un valor objetivo $K$, determinar si existe un subconjunto cuya suma sea exactamente $K$.
* **Toma de decisiones:** Para cada elemento $s_i$, se abren dos ramas recursivas:
  1. Incluir $s_i$ en el subconjunto.
  2. Excluir $s_i$ del subconjunto.
* **Podas habituales:**
  * Si los números son positivos y $\text{suma\_actual} > K$, podar.
  * Si $\text{suma\_actual} + \sum_{j=i}^n s_j < K$, podar por imposibilidad de alcanzar el objetivo.

---

### 4.5. Otros Problemas Característicos
* **MaxiSubconjunto:** Maximizar el valor acumulado de seleccionar $k$ elementos evaluando podas por cota superior de optimalidad:
  $$z(\text{extensión}(I)) \le z(I) + \mu \left(k - |I|\right)$$
* **Palabras en Cadena:** Verificación recursiva de validez de secuencias lingüísticas sobre diccionarios.
* **Árbol de Búsqueda Binaria (ABB) Óptimo:** Construcción inductiva del espacio de búsqueda para minimizar el costo de acceso a claves estructuradas.
