# Preparación y Contexto Completo: Guía 5 (Fuerza Bruta y Backtracking)

> [!IMPORTANT]
> **Reglas metodológicas fijadas:**
> * **Formas de resolución:** Provienen **exclusivamente** de [`Practicas Condensadas.md`](file:///c:/Users/igiraudi/OneDrive%20-%20rmrconsultores.com/Documents/TDA%202026/Practicas%20Condensadas.md) (Sección 4: Marco conceptual de Backtracking, formalización $Sols$, $Sols_{parciales}$, podas por factibilidad y optimalidad, demostración inductiva de semánticas recursivas).
> * **Teoría:** Solo background conceptual pasivo ([`Teoricas Condesandas.md`](file:///c:/Users/igiraudi/OneDrive%20-%20rmrconsultores.com/Documents/TDA%202026/Teoricas%20Condesandas.md) - Fuentes 4 y 5) para consultar definiciones combinatorias y lemas formales.
> * **Archivos de ejercicios hechos:** Solo actúan como *plantilla de almacenamiento* (formato visual y estructura en Markdown).
> * **Carpetas excluidas:** `TDA-Talleres` y `Lemas Teoremas` no se consultan.

---

## 1. Caja de Herramientas Teórico-Prácticas para la Guía 5

### 1.1. Formalización de Problemas Combinatorios
Para abordar formalmente un problema de búsqueda exhaustiva o Backtracking se deben definir explícitamente tres elementos:

1. **$Sols$:** Conjunto finito de soluciones candidatas completas.
2. **$válida(a)$:** Predicado booleano determinado sobre $a \in Sols$.
3. **$Sols_{válidas}$:** $\{a \in Sols : válida(a)\}$.

#### Tamaño de Espacios Candidatos Típicos ($|Sols|$):
* Subconjuntos de $n$ elementos: $Sols = \{0,1\}^n \implies |Sols| = 2^n$.
* Permutaciones de $n$ elementos: $Sols = S_n \implies |Sols| = n!$.
* Repartir $n$ elementos en $k$ cajas: $Sols = \{1,\dots,k\}^n \implies |Sols| = k^n$.
* Subconjuntos de tamaño $k$ en $n$ elementos: $Sols = \{a \in \{0,1\}^n : \sum a_i = k\} \implies |Sols| = \binom{n}{k}$.
* Grillas de $n \times n$ con números $1..n^2$: $Sols = S_{n^2} \implies |Sols| = (n^2)!$.

---

### 1.2. Clasificación de Problemas según las 5 Preguntas Estándar
Dependiendo de qué solicite el problema sobre $Sols_{válidas}$, el algoritmo de Backtracking adapta la combinación de hijos, el manejo del caso base y el corte temprano:

| Pregunta | Tipo de Retorno | Hoja Válida / Inválida | Combinador de Hijos | Corte Temprano | Podas Aplicables |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **Decisión** | `Bool` | `True` / `False` | $\lor$ (OR) | Sí (al 1er `True`) | Factibilidad |
| **Construcción** | `Solución` / `None` | Guardar $p$ / `None` | $\lor$ | Sí (al 1er éxito) | Factibilidad |
| **Conteo** | `Entero` | $1$ / $0$ | $+$ (Suma) | No | Factibilidad |
| **Enumeración** | `Void` | Imprimir $p$ / Nada | Ninguno | No | Factibilidad |
| **Optimización** | `Valor` (o estado) | $valor(p)$ / $-\infty$ | $\max$ (o $\min$) | No | Factibilidad y Optimalidad |

---

### 1.3. Árbol de Backtracking y Resumen de Estado (Regla de Erickson)
* **Soluciones Parciales ($Sols_{parciales}$):** Nodos del árbol que representan decisiones acumuladas hasta el nivel $i$.
* **Sucesoras:** Extensión de una solución parcial $p$ agregando una nueva decisión válida: $p \oplus v$.
* **Estructura en Memoria (Regla de Erickson):**
  > *Los parámetros de la función recursiva deben incluir únicamente el índice del subproblema pendiente $i$ y el resumen mínimo de las decisiones pasadas necesario para verificar validez/valor (ej. peso acumulado $pa$ y valor acumulado $va$).*
* **Manejo de Vectores de Solución:** Para mantener la complejidad espacial en $\mathcal{O}(n)$ (la profundidad del árbol de recursión), el vector de solución debe ser un objeto **global compartido** que se modifica in-place mediante `push` antes de la llamada recursiva y `pop` inmediatamente después (backtrack), **nunca copiando arreglos en cada nivel** (lo que elevaría el tiempo por nodo a $\mathcal{O}(n)$ y el espacio a $\mathcal{O}(n^2)$).

---

### 1.4. Podas (Pruning) y Demostraciones de Correctitud

#### A. Podas por Factibilidad
* **Regla de factibilidad $R(p)$:** Condición necesaria para que la solución parcial $p$ pueda extenderse a una solución válida.
* **Propiedad de Corte:** Si $\neg R(p) \implies Ext(p) \cap Sols_{válidas} = \emptyset$.
* **Molde de demostración de correctitud de poda:**
  1. Supongo $\neg R(p)$.
  2. Tomo una solución candidata arbitraria $a \in Ext(p)$.
  3. Muestro algebraicamente o por propiedades del problema que la violación en $p$ se hereda a $a$.
  4. Concluyo $\neg válida(a)$, probando que no se descarta ninguna solución válida.

#### B. Podas por Optimalidad (Branch & Bound)
* Mantiene una variable global `mejor` inicializada en el neutro ($-\infty$ para maximizar).
* Define una función de cota superior $U(p)$ tal que para toda extensión válida $a \in Ext(p) \cap Sols_{válidas}$ se cumple $valor(a) \le U(p)$.
* **Regla de Poda:** Si $U(p) \le mejor$, se interrumpe la exploración del subárbol de $p$.

---

### 1.5. Demostración de Correctitud por Inducción y Semántica Recursiva

Toda prueba formal de un algoritmo de fuerza bruta / backtracking debe seguir tres pasos estrictos:

1. **Fidelidad de la Formalización:** Argumentar que $Sols_{válidas}$ mapea las soluciones reales del problema.
2. **Definición de la Semántica $\sigma$:** Expresión matemática que define qué devuelve la función para cualquier entrada, **sin usar la propia función ni apelar a términos de ejecución**.
   $$\text{Semántica} = \text{op} \{ \text{valor}(b) : b \in \text{Compl}(\text{parámetros}) \}$$
   donde $\text{Compl}(i, \text{estado})$ es el conjunto de sufijos binarios/decisiones válidas restantes.
3. **Demostración Inductiva sobre Decisiones Pendientes $m = n - i + 1$:**
   * *Caso base ($m=0 \implies i=n+1$):* La semántica sobre $\text{Compl}(n+1, \text{estado})$ coincide con el caso base del código.
   * *Paso inductivo:* Asumir la hipótesis inductiva para $m-1$. Partir el conjunto de completaciones $\text{Compl}(i)$ en ramas disjuntas (ej. $0 \oplus b'$ y $1 \oplus b'$). Aplicar H.I. a cada rama y unificar mediante el **Lema Fundamental del Operador** ($\max(A \cup B) = \max(\max A, \max B)$).

---

## 2. Ficha de Preparación Completa por Ejercicio (Guía 5 Mínimos)

### Ejercicio 1 (Suma Subconjuntos: formalización) $\star$
* **Problema:** $C = \{c_1, \dots, c_n\}$, objetivo $k$.
* **Desglose de Ítems:**
  * **a)** $Sols = \{0,1\}^n$. Para $C = \{6, 12, 6\}$ y $k = 12$, $Sols = \{0,1\}^3$ tiene $2^3 = 8$ elementos: $(0,0,0), (0,0,1), \dots, (1,1,1)$.
  * **b)** $válida_{C,k}(a) = [\sum a_i c_i = k]$. $Sols_{válidas} = \{(1,0,1), (0,1,0)\}$ (pues $6+6=12$ y $12=12$).
  * **c)** $ss(C, k) = [\exists a \in Sols : válida(a)] = \text{True}$.
  * **d)** Propuesta de Cecilia: $Sols' = \mathcal{P}(C)$ (conjunto de partes). $|Sols| = 2^n$ vs $|Sols'| = 2^n$ (tienen el mismo cardinal en abstracción, pero la representación binaria $Sols = \{0,1\}^n$ permite iteración indexada directa por posiciones $O(1)$).
  * **e)** Propuesta de Roberto: $Sols^* = Sols_{válidas}$. No es posible hacer un algoritmo de fuerza bruta que recorra solo $Sols^*$ sin conocer de antemano las soluciones válidas (sería razonamiento circular).

---

### Ejercicio 2 (MagiCuadrados: formalización) $\star$
* **Problema:** Grilla de $n \times n$ con enteros de $1$ a $n^2$ donde filas, columnas y diagonales suman el número mágico.
* **Procesamiento de Ítems:**
  * **a) Dos propuestas de $Sols$:**
    * Propuesta 1: $Sols_1 = \{1, \dots, n^2\}^{n^2}$ (todas las asignaciones de enteros del $1$ al $n^2$ en las $n^2$ casillas). $|Sols_1| = (n^2)^{n^2}$.
    * Propuesta 2: $Sols_2 = S_{n^2}$ (todas las permutaciones de los números del $1$ a $n^2$). $|Sols_2| = (n^2)!$.
  * **b) Validación:** Propuesta 2 es superior porque elimina por construcción soluciones con números repetidos.
  * **c) Función $mc(n)$:** $mc(n) = |\{a \in Sols_2 : válida_n(a)\}|$.

---

### Ejercicio 3 (MaxiSubconjunto: formalización) $\star$
* **Problema:** Matriz simétrica $M$ de $n \times n$, hallar $I \subseteq \{1,\dots,n\}$ con $|I| = k$ que maximice $\sum_{i,j \in I} M_{ij}$.
* **Formalización:**
  * $Sols = \{a \in \{0,1\}^n : \sum_{i=1}^n a_i = k\}$.
  * $válida_{M,k}(a) = \text{True}$ para todo $a \in Sols$.
  * Relación de orden entre soluciones válidas: $a \le b \iff \sum_{i,j : a_i=1, a_j=1} M_{ij} \le \sum_{i,j : b_i=1, b_j=1} M_{ij}$.

---

### Ejercicio 4 (Quiz combinatorio) $\star$
* **Respuestas Justificadas:**
  1. Hojas de árbol binario completo de $n$ niveles: $2^n \implies$ **c) $\Theta(2^n)$**.
  2. Permutaciones de un conjunto de tamaño $n$: $n! \implies$ **d) $\Theta(n!)$**.
  3. Conjunto de partes de un conjunto de $n$ elementos: $2^n \implies$ **c) $\Theta(2^n)$**.
  4. Repartir $n$ libros en $k$ cajas distintas: $k^n \implies$ **c) $\Theta(k^n)$**.
  5. Elegir subconjunto de $k$ objetos de un total de $n$: $\binom{n}{k} \implies$ **d) $\Theta\left(\binom{n}{k}\right)$**.

---

### Ejercicio 5 (Suma Subconjuntos: árbol de BT) $\star$
* **Soluciones Parciales:** Vectors de bits de decisiones tomadas $p = (a_1, \dots, a_{i-1})$.
* **Estructura del Árbol para $C=\{6,12,6\}, k=12$:**
  * Nivel 0 (Raíz): $()$ (0 decisiones tomadas).
  * Nivel 1: $(0)$ (excluir 6) y $(1)$ (incluir 6).
  * Nivel 2: Hijos de $(0) \to (0,0), (0,1)$; Hijos de $(1) \to (1,0), (1,1)$.
  * Nivel 3 (Hojas): $(0,0,0), (0,0,1), (0,1,0), (0,1,1), (1,0,0), (1,0,1), (1,1,0), (1,1,1)$.

---

### Ejercicio 6 (Suma Subconjuntos: recurrencia) $\star$
* **Función Recursiva:**
  $$ss_{\text{rec}}(\{c_1, \dots, c_n\}, k) = \begin{cases} k == 0 & \text{si } n = 0 \\ ss_{\text{rec}}(\dots, k) \lor ss_{\text{rec}}(\dots, k - c_n) & \text{si } n > 0 \end{cases}$$
* **Demostración por Inducción sobre $n$:**
  * *Caso Base ($n=0$):* El único subconjunto de $\emptyset$ es $\emptyset$, cuya suma es 0. Retorna $k==0$, lo cual coincide con $ss(\emptyset, k) = [\exists a \in \{()\} : 0 = k] = [k == 0]$.
  * *Paso Inductivo ($n > 0$):* Por H.I., $ss_{\text{rec}}(\{c_1..c_{n-1}\}, k') = ss(\{c_1..c_{n-1}\}, k')$. Una solución para $\{c_1..c_n\}$ asigna $a_n=0$ o $a_n=1$. Si $a_n=0$, equivale a sumar $k$ con los primeros $n-1$; si $a_n=1$, equivale a sumar $k-c_n$ con los primeros $n-1$. Por el lema del OR ($\exists (A \cup B) = \exists A \lor \exists B$), la recurrencia es correcta.

---

### Ejercicio 7 (Suma Subconjuntos: implementación) $\star$
* **Pseudocódigo Operativo:**
  ```text
  function subset_sum(C, i, j)
      if i == 0 then return (j == 0)
      return subset_sum(C, i - 1, j) or subset_sum(C, i - 1, j - C[i])
  ```
* **Complejidad Temporal:** $T(n) = 2T(n-1) + \Theta(1) \implies \Theta(2^n)$ (árbol binario completo de $n$ niveles con $2^{n+1}-1$ nodos).
* **Complejidad Espacial:** $\Theta(n)$ correspondiente a la profundidad máxima de la pila de llamadas recursivas.

---

### Ejercicio 8 (Suma Subconjuntos: poda por factibilidad) $\star$
* **Regla de Factibilidad P1:** Si los números son naturales ($c_q \ge 0$), la suma acumulada no puede decrecer. Por ende, si $j < 0$ (lo que significa que la suma actual superó $k$), es imposible volver a $0$.
* **Demostración de Correctitud de Poda:**
  Sea $p = (a_1, \dots, a_{i-1})$ con suma acumulada $\sum_{q=1}^{i-1} a_q c_q > k$. Para cualquier extensión $a = p \oplus (a_i, \dots, a_n)$, como $c_q \ge 0$, la suma total satisface $\sum_{q=1}^n a_q c_q \ge \sum_{q=1}^{i-1} a_q c_q > k$. Por lo tanto, $válida(a) = \text{False}$, probando que cortar cuando $j < 0$ no descarta ninguna solución válida.
* **Segunda Regla de Factibilidad (P2 - Imposibilidad por Suma Restante):**
  Si $\text{suma\_acumulada} + \sum_{q=i}^n c_q < k$, cortar la rama (incluso si tomáramos todos los elementos restantes, no alcanzamos $k$).

---

### Ejercicio 9 (Suma Subconjuntos: construcción de solución) $\star$
* **Manejo de Estado Global In-Place ($\mathcal{O}(n)$ Espacio):**
  ```python
  def subset_sum_construir(C, i, j, solucion_actual):
      if j == 0:
          return True, solucion_actual
      if i == 0 or j < 0:
          return False, []

      # Opción 1: Incluir C[i-1]
      solucion_actual.append(C[i - 1])
      exito, sol = subset_sum_construir(C, i - 1, j - C[i - 1], solucion_actual)
      if exito:
          return True, sol
      solucion_actual.pop()  # Backtrack in-place

      # Opción 2: Excluir C[i-1]
      return subset_sum_construir(C, i - 1, j, solucion_actual)
```

---

### Ejercicio 10 (Suma Subconjuntos: enumeración) $\star$
* **Modificación para Imprimir Todas las Soluciones Válidas:**
  Se elimina el retorno temprano (`return True`). Al llegar a $j == 0$ e $i == 0$, se imprime `solucion_actual` y se continúa la exploración.
* **Complejidad Temporal:** En el peor caso (ej. todos ceros o $k=0$), se deben visitar todas las hojas del árbol $\implies \Theta(2^n)$.

---

### Ejercicio 11 (MagiCuadrados: BT y podas) $\star$
* **Puntos Clave:**
  * **a)** Fuerza bruta sobre $Sols_2$: $(n^2)!$ permutaciones. Para $n=3$, $9! = 362.880$; para $n=4$, $16! \approx 2.09 \times 10^{13}$.
  * **b)** Solución parcial: Grilla de $n \times n$ completada en las primeras $k$ casillas con un subconjunto de números distintos de $\{1, \dots, n^2\}$.
  * **d) Complejidad:** El árbol de backtracking explora asignaciones en las $n^2$ posiciones. En la casilla 1 hay $n^2$ opciones, en la 2 hay $n^2-1$, ..., en la última hay 1 opción $\implies \mathcal{O}((n^2)!)$ nodos en el peor caso.
  * **f) Deducción del Número Mágico:**
    La suma de todos los números del tablero $1$ a $n^2$ es $S = \sum_{x=1}^{n^2} x = \frac{n^2 (n^2 + 1)}{2}$. Como hay $n$ filas y cada fila suma el número mágico $M$, vale que $n \cdot M = S \implies M = \frac{n^2(n^2+1)}{2n} = \frac{n^3 + n}{2}$.
  * **Podas Adaptadas:**
    * Poda por fila completa: Al terminar la casilla $(i, n)$, la suma de la fila $i$ debe ser exactamente $M$.
    * Poda por columna parcial: La suma acumulada de cualquier columna no puede superar $M$. Al completar la casilla $(n, j)$, la suma debe ser exactamente $M$.

---

### Ejercicio 12 (MaxiSubconjunto: BT y podas) $\star$
* **Problema:** Encontrar $I \subseteq \{1..n\}$ con $|I|=k$ que maximice la suma de la submatriz de interconexión $M$.
* **Semántica Recursiva $\mathcal{MS}(i, S, r)$:**
  Para el índice de elemento actual $i$, el subconjunto de índices ya elegidos $S$ y la cantidad de elementos restantes por elegir $r = k - |S|$:
  $$\mathcal{MS}(i, S, r) = \max \left\{ \sum_{x, y \in S \cup I'} M_{xy} : I' \subseteq \{i, \dots, n\} \land |I'| = r \right\}$$
* **Poda por Optimalidad (Branch & Bound):**
  Definir la cota superior $U(i, S, r)$:
  $$U(i, S, r) = \text{suma\_actual}(S) + r \cdot \max_{p, q} (M_{pq})$$
  Si $U(i, S, r) \le mejor$, podar inmediatamente el subárbol.

---

### Ejercicio 13 (Coloreo de Grafos) $\star$
* **Entrada:** Grafo $G = (V, E)$ y $k$ colores disponibles $\{1, \dots, k\}$.
* **Algoritmo de Backtracking:**
  * Iterar vértice por vértice $v_1, v_2, \dots, v_n$.
  * Para el vértice $v_i$, probar asignarle cada color $c \in \{1, \dots, k\}$.
  * **Poda por Factibilidad:** Antes de asignar el color $c$ a $v_i$, verificar si existe algún vecino $u \in N(v_i)$ ya visitado con $color[u] == c$. Si existe conflicto, podar la rama.
* **Cota de Complejidad Temporal:** $\mathcal{O}(k^n)$ nodos en el árbol de backtracking con trabajo $\mathcal{O}(d(v))$ por nodo.
