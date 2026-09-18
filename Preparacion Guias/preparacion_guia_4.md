# Preparación y Contexto Completo: Guía 4 (Dividir y Conquistar)

> [!IMPORTANT]
> **Reglas metodológicas fijadas:**
> * **Formas de resolución:** Provienen **exclusivamente** de [`Practicas Condensadas.md`](file:///c:/Users/igiraudi/OneDrive%20-%20rmrconsultores.com/Documents/TDA%202026/Practicas%20Condensadas.md) (Sección 3: Esquema de 5 pasos, Teorema Maestro, balance de subproblemas, optimización por tuplas acumuladas).
> * **Teoría:** Solo background conceptual pasivo ([`Teoricas Condesandas.md`](file:///c:/Users/igiraudi/OneDrive%20-%20rmrconsultores.com/Documents/TDA%202026/Teoricas%20Condesandas.md) - Fuente 3) para consultar definiciones y ecuaciones formales.
> * **Archivos de ejercicios hechos:** Solo actúan como *plantilla de almacenamiento* (formato visual y estructura en Markdown).
> * **Carpetas excluidas:** `TDA-Talleres` y `Lemas Teoremas` no se consultan.

---

## 1. Caja de Herramientas Teórico-Prácticas para la Guía 4

### 1.1. Esquema Metodológico Estricto de 5 Pasos para D&C
Toda resolución formal de un problema de Divide y Conquista según el estándar práctico debe desarrollar explícitamente estas 5 etapas:

1. **Análisis:** Comprender la estructura del problema, dominio y la propiedad matemática que permite descomponer la instancia en subproblemas independientes sin perder información.
2. **Algoritmo:** Descripción conceptual en pseudocódigo o lenguaje formal sin ambigüedades.
3. **Etapas D&C:** Identificación explícita de:
   * **Dividir:** Partición de la instancia de tamaño $n$ en $a$ subproblemas de tamaño $n/b$.
   * **Conquistar:** Resolución recursiva de los subproblemas (o solución directa en el caso base).
   * **Combinar:** Algoritmo y fórmula de unificación de las soluciones parciales para construir la solución global.
4. **Correctitud:** Demostración formal (inductiva o por invariante) de que el paso de combinación preserva la validez y no descarta soluciones óptimas/correctas.
5. **Complejidad:** Planteo formal de la ecuación de recurrencia $T(n)$ y resolución matemática (Teorema Maestro o árbol de expansión).

---

### 1.2. El Teorema Maestro y Análisis de Recurrencias

#### Formulación Estándar
Para ecuaciones de recurrencia de la forma:
$$T(n) = a T\left(\frac{n}{b}\right) + f(n) \quad \text{con } a \ge 1, b > 1$$
donde $a$ es la cantidad de subproblemas recursivos, $b$ el factor de división del tamaño de entrada, y $f(n)$ el costo acumulado de las etapas de **Dividir** y **Combinar**.

Definiendo la cota crítica $n^q$ con $q = \log_b a$:

| Caso | Condición Matemática sobre $f(n)$ | Solución Asintótica $T(n)$ | Interpretación del Árbol |
| :---: | :--- | :---: | :--- |
| **Caso 1** | $f(n) = \mathcal{O}(n^{q - \varepsilon})$ para algún $\varepsilon > 0$ | $\Theta(n^{\log_b a})$ | Domina el trabajo en las hojas ($a^k$ hojas de costo $\Theta(1)$). |
| **Caso 2** | $f(n) = \Theta(n^q \log^r n)$ con $r \ge 0$ | $\Theta(n^{\log_b a} \log^{r+1} n)$ | Trabajo distribuido equitativamente entre los $\log_b n$ niveles. |
| **Caso 3** | $f(n) = \Omega(n^{q + \varepsilon})$ para $\varepsilon > 0$ y cumple regularidad $a f(n/b) \le c f(n)$ ($c < 1$) | $\Theta(f(n))$ | Domina el trabajo en la raíz (etapa de combinación). |

> [!WARNING]
> **Condición de brecha polinomial en Caso 1 y 3:**  
> $f(n)$ debe ser polinomialmente menor u mayor que $n^q$. Por ejemplo, si $f(n) = n^q / \log n$, **no aplica el Caso 1** porque la diferencia es logarítmica y no $n^\varepsilon$. En tales casos se requiere expansión explícita del árbol de recurrencia.

#### Recurrencias No Divisivas (Restativas y Exponenciales)
Cuando la recurrencia reduce el tamaño sustrayendo una constante en lugar de dividiendo ($T(n) = a T(n - c) + f(n)$), **el Teorema Maestro NO es aplicable**. Se resuelven mediante expansión en serie:

1. **Sumatorias Lineales:** $T(n) = T(n - 1) + n^k \implies T(n) = \sum_{i=1}^n i^k = \Theta(n^{k+1})$.
2. **Constantes Restativas:** $T(n) = T(n - c) + k \implies T(n) = \frac{n}{c} \cdot k = \Theta(n)$.
3. **Multiplicativas Exponenciales:** $T(n) = 2T(n - c) + \Theta(1) \implies T(n) = \sum_{i=0}^{n/c} 2^i = \Theta(2^{n/c})$.

---

### 1.3. Patrones de Optimización en D&C

#### A. Evitar Recorridos Redundantes mediante Tuplas Acumuladas
* **Problema:** Si en cada nivel de recursión se realiza un algoritmo auxiliar lineal $\mathcal{O}(n)$ (como calcular la altura de un árbol en cada nodo), la recurrencia $T(n) = 2T(n/2) + \mathcal{O}(n)$ se degrada a $\mathcal{O}(n \log n)$ o $\mathcal{O}(n^2)$.
* **Solución Metodológica:** Modificar la firma del método recursivo para devolver una **tupla acumulada** con todos los valores necesarios en $\Theta(1)$ (ej. `(altura, diametro)` o `(suma_total, max_prefijo, max_sufijo, max_subarreglo)`).
* **Resultado:** La combinación se realiza en $\Theta(1)$, reduciendo la recurrencia a $T(n) = 2T(n/2) + \Theta(1) \implies \Theta(n)$.

#### B. Búsqueda Binaria Generalizada sobre Predicados Monótonos
* **Dominio:** Un conjunto ordenado $S$ y un predicado booleano $P: S \to \{\text{False}, \text{True}\}$.
* **Condición de Monotonía:** Existe un punto de corte $t^*$ tal que $P(t) = \text{False}$ para todo $t < t^*$ y $P(t) = \text{True}$ para todo $t \ge t^*$.
* **Algoritmo D&C:** Evaluar el elemento medio $m = \lfloor (izq + der)/2 \rfloor$. Si $P(m) == \text{True}$, buscar la primera ocurrencia en la mitad izquierda $[izq, m]$; si es $\text{False}$, buscar en $[m+1, der]$.
* **Complejidad:** $T(n) = T(n/2) + \Theta(1) \implies \Theta(\log n)$.

---

## 2. Ficha de Preparación Completa por Ejercicio (Guía 4 Mínimos)

### Ejercicio 1 (MergeSort) $\star$
* **Objetivo:** Desglosar formalmente el algoritmo MergeSort en Python.
* **Análisis de Líneas:**
  * *Dividir:* `medio = len(arr) // 2`, slicing `arr[:medio]` y `arr[medio:]` ($\Theta(1)$ en concepto, $\Theta(n)$ si hay copia).
  * *Conquer:* Llamadas recursivas `merge_sort(mitad_izq)` y `merge_sort(mitad_der)`.
  * *Combine:* Función `merge(mitad_izq, mitad_der)` que recorre ambas listas con dos punteros $i, j$ insertando el menor elemento.
* **Preguntas Teóricas:**
  * Subproblemas: $a = 2$.
  * Tamaño: $n/b = n/2$.
  * Costo de combinar: $\Theta(n)$ en tiempo por el bucle `while`.
  * Ecuación: $T(n) = 2T(n/2) + \Theta(n)$, con $T(1) = \Theta(1)$.
  * Teorema Maestro: $a=2, b=2 \implies q = \log_2 2 = 1$. Como $f(n) = \Theta(n^1)$, aplica **Caso 2** con $r=0 \implies T(n) = \Theta(n \log n)$.

---

### Ejercicio 2 (Búsqueda binaria) $\star$
* **Objetivo:** Analizar la búsqueda binaria recursiva sobre arreglos ordenados.
* **Análisis de Líneas:**
  * *Dividir:* `medio = (izq + der) // 2` ($\Theta(1)$).
  * *Conquer:* Una sola llamada recursiva sobre el subarreglo izquierdo o derecho según `arr[medio] > objetivo`.
  * *Combine:* Nulo ($\Theta(1)$), simplemente se retorna el índice devuelto por la llamada.
* **Preguntas Teóricas:**
  * Subproblemas: $a = 1$.
  * Tamaño: $n/b = n/2$.
  * Costo de combinar: $\Theta(1)$.
  * Ecuación: $T(n) = T(n/2) + \Theta(1)$, con $T(1) = \Theta(1)$.
  * Teorema Maestro: $a=1, b=2 \implies q = \log_2 1 = 0$. Como $f(n) = \Theta(1) = \Theta(n^0)$, aplica **Caso 2** con $r=0 \implies T(n) = \Theta(\log n)$.

---

### Ejercicio 3 (Complexity quest) $\star$
* **Objetivo:** Calibración de cálculo de complejidades para 12 ecuaciones de recurrencia.

#### Desglose Resuelto de las 12 Ecuaciones:
1. $T(n) = T(n - 2) + 5 \implies$ Recurrencia restativa. $T(n) = \sum_{i=1}^{n/2} 5 = \Theta(n)$.
2. $T(n) = T(n - 1) + n \implies$ Sumatoria $\sum_{i=1}^n i = \frac{n(n+1)}{2} = \Theta(n^2)$.
3. $T(n) = T(n - 1) + \sqrt{n} \implies$ Sumatoria $\sum_{i=1}^n \sqrt{i} = \int_1^n x^{1/2} dx = \Theta(n^{3/2})$.
4. $T(n) = T(n - 1) + n^2 \implies$ Sumatoria $\sum_{i=1}^n i^2 = \frac{n(n+1)(2n+1)}{6} = \Theta(n^3)$.
5. $T(n) = 2T(n - 1) \implies$ Expansión: $2^k T(n-k) \implies \Theta(2^n)$.
6. $T(n) = T(n/2) + n \implies$ TM: $a=1, b=2, q=0$. $f(n) = n = \Omega(n^{0 + 1})$. Caso 3 $\implies T(n) = \Theta(n)$.
7. $T(n) = T(n/2) + \sqrt{n} \implies$ TM: $a=1, b=2, q=0$. $f(n) = n^{1/2} = \Omega(n^{0 + 1/2})$. Caso 3 $\implies T(n) = \Theta(\sqrt{n})$.
8. $T(n) = T(n/2) + n^2 \implies$ TM: $a=1, b=2, q=0$. $f(n) = n^2 = \Omega(n^{0+2})$. Caso 3 $\implies T(n) = \Theta(n^2)$.
9. $T(n) = 2T(n - 4) \implies$ Expansión restativa: $2^{n/4} \implies \Theta(2^{n/4}) = \Theta((\sqrt[4]{2})^n)$.
10. $T(n) = 2T(n/2) + \log n \implies$ TM: $a=2, b=2, q=1$. $f(n) = \log n = \mathcal{O}(n^{1 - \varepsilon})$ para $\varepsilon = 0.5$. Caso 1 $\implies T(n) = \Theta(n)$.
11. $T(n) = 3T(n/4) \implies$ TM: $a=3, b=4, q=\log_4 3 \approx 0.793$. $f(n) = 0 = \mathcal{O}(n^q)$. Caso 1 $\implies T(n) = \Theta(n^{\log_4 3})$.
12. $T(n) = 3T(n/4) + n \implies$ TM: $a=3, b=4, q=\log_4 3 \approx 0.793$. $f(n) = n = \Omega(n^{q + \varepsilon})$. Caso 3 $\implies T(n) = \Theta(n)$.

---

### Ejercicio 4 (Izquierda dominante) $\star$
* **Objetivo:** Arreglo de tamaño potencia de 2 donde cada mitad izquierda suma estrictamente más que su mitad derecha, y recursivamente cada mitad es izquierda dominante.
* **Estrategia D&C Óptima ($\Theta(n)$):**
  * Si el arreglo tiene tamaño 1, retorna `(arr[0], True)`.
  * Llamar recursivamente sobre la mitad izquierda: `(suma_izq, es_dom_izq)`.
  * Llamar recursivamente sobre la mitad derecha: `(suma_der, es_dom_der)`.
  * **Combinar en $\Theta(1)$:**
    `es_dom = es_dom_izq and es_dom_der and (suma_izq > suma_der)`
    `suma_total = suma_izq + suma_der`
    Retornar `(suma_total, es_dom)`.
* **Complejidad:** $T(n) = 2T(n/2) + \Theta(1) \implies \Theta(n)$ por Teorema Maestro (Caso 1, $a=2, b=2, q=1, f(n)=\Theta(1)$), lo cual es estrictamente menor a $\mathcal{O}(n^2)$.

---

### Ejercicio 5 (Índice espejo) $\star$
* **Enunciado:** Arreglo $a = [a_1, \dots, a_n]$ estrictamente creciente de enteros distintos. Determinar si existe $i$ tal que $a[i] == i$.
* **Análisis y Monotonía:**
  * Sea la función $f(i) = a[i] - i$.
  * Como $a$ es estrictamente creciente, $a[i+1] \ge a[i] + 1 \implies a[i+1] - (i+1) \ge a[i] - i \implies f(i+1) \ge f(i)$.
  * Por lo tanto, $f(i)$ es **monótona no decreciente**. Buscamos un raíz $f(i) = 0$.
* **Algoritmo D&C ($\Theta(\log n)$):**
  * Tomar $m = \lfloor (izq + der)/2 \rfloor$.
  * Si $a[m] == m$, retornar $m$.
  * Si $a[m] > m$, entonces para todo $j > m$ vale $a[j] - j \ge a[m] - m > 0$, luego no puede haber solución a la derecha. Buscar en $[izq, m-1]$.
  * Si $a[m] < m$, por analogía buscar en $[m+1, der]$.
* **Complejidad:** $T(n) = T(n/2) + \Theta(1) \implies \Theta(\log n)$.

---

### Ejercicio 6 (Potencia logarítmica) $\star$
* **Objetivo:** Calcular $a^b$ en tiempo $\Theta(\log b)$.
* **Algoritmo (Exponenciación Binaria):**
  $$a^b = \begin{cases} 
  1 & \text{si } b = 0 \\
  (a^{b/2})^2 & \text{si } b \text{ es par} \\
  a \cdot (a^{(b-1)/2})^2 & \text{si } b \text{ es impar}
  \end{cases}$$
* **Clave de Eficiencia:** Guardar el resultado de la llamada única `half = potencia(a, b // 2)` y computar `half * half`, evitando realizar dos llamadas recursivas identicas (lo cual degradaría la complejidad a $\Theta(b)$).
* **Complejidad:** $T(b) = T(b/2) + \Theta(1) \implies \Theta(\log b)$ por Teorema Maestro.

---

### Ejercicio 7 (Distancia máxima / Diámetro en Árbol Binario) $\star$
* **Objetivo:** Hallar el camino más largo en un árbol binario en $\Theta(n)$ sin recorridos redundantes.
* **Análisis Metodológico:**
  * El diámetro $D(T)$ en la raíz $nodo$ es el máximo entre:
    1. El diámetro del subárbol izquierdo $D(T_{izq})$.
    2. El diámetro del subárbol derecho $D(T_{der})$.
    3. El camino que cruza por la raíz: $h(T_{izq}) + h(T_{der}) + 2$ (donde $h$ es la altura con $h(\emptyset) = -1$).
* **Algoritmo $\Theta(n)$ por Tuplas Acumuladas:**
  * Cada llamada recursiva devuelve `(altura, diametro)`.
  * En el caso base `nodo is None`: retorna `(-1, 0)`.
  * Combinar en $\Theta(1)$:
    `altura = 1 + max(h_izq, h_der)`
    `diametro = max(d_izq, d_der, h_izq + h_der + 2)`
* **Complejidad:** $T(n) = T(k) + T(n - k - 1) + \Theta(1) \implies \Theta(n)$ (visita cada nodo una sola vez).

---

### Ejercicio 8 (Cazador de falsos) $\star$
* **Estructura:** Matriz booleana $A$ de $n \times n$ con consulta en bloque `conjunciónSubmatriz(i0, i1, j0, j1)` en $\mathcal{O}(1)$.

#### Parte 1: Encontrar la posición de AL MENOS UN `false`
* **Algoritmo D&C:**
  1. Dividir la matriz $n \times n$ en 4 cuadrantes de $n/2 \times n/2$.
  2. Consultar `conjunciónSubmatriz` para cada cuadrante en $\mathcal{O}(1)$.
  3. Elegir el primer cuadrante cuyo resultado sea `False` (al menos uno lo será por premisa).
  4. Realizar **una sola llamada recursiva** sobre dicho cuadrante.
* **Complejidad:** $T(n) = T(n/2) + \Theta(1) \implies \Theta(\log n)$, lo cual es estrictamente menor a $\mathcal{O}(n^2)$.

#### Parte 2: Contar cuántos `false` hay en la matriz (sabiendo que hay a lo sumo 5)
* **Algoritmo con Poda por Factibilidad:**
  1. Para la submatriz actual, consultar `conjunciónSubmatriz`.
  2. **PODA:** Si la conjunción devuelve `True`, la submatriz completa contiene solo `True` $\implies$ **retornar 0 inmediatamente sin descender**.
  3. Si devuelve `False` y la submatriz es de $1 \times 1$, retornar 1.
  4. Si devuelve `False` y el tamaño es mayor a 1, llamar recursivamente sobre los cuadrantes cuyo `conjunciónSubmatriz` sea `False` y sumar los resultados.
* **Análisis de Complejidad:**
  * Como hay a lo sumo 5 celdas `False` en toda la matriz, a lo sumo 5 ramas del árbol de división pueden permanecer activas simultáneamente en cualquier nivel.
  * La cantidad total de nodos visitados en el árbol de recursión está acotada por $5 \times \log_2 n$.
  * Por lo tanto, la complejidad temporal es $T(n) \le 5 \cdot \log_2 n + \Theta(1) = \Theta(\log n)$, estrictamente menor a $\mathcal{O}(n^2)$.
