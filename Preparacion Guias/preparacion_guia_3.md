# Preparación y Contexto: Guía 3 (Algoritmos sobre Grafos)

> [!IMPORTANT]
> **Reglas metodológicas fijadas:**
> * **Formas de resolución:** Provienen **exclusivamente** de [`Practicas Condensadas.md`](file:///c:/Users/igiraudi/OneDrive%20-%20rmrconsultores.com/Documents/TDA%202026/Practicas%20Condensadas.md) (técnicas de DFS, BFS, árbol geodésico, `cubren(v)` para puentes, coloreo DFS alternado para bipartitud, etc.).
> * **Teoría:** Solo background conceptual pasivo (`Teoricas Condesandas.md`) para consultar nociones y definiciones si hiciera falta.
> * **Archivos de ejercicios hechos:** Solo actúan como *plantilla de almacenamiento* (formato visual y secciones de Markdown).
> * **Carpetas excluidas:** `TDA-Talleres` y `Lemas Teoremas` no se consultan.

---

## 1. Caja de Herramientas Teórico-Prácticas para la Guía 3

### 1.1. Representaciones de Grafos y Costos Operacionales (Base para Ej. 1)
Dado un grafo $G = (V, E)$ con $n = |V|$ y $m = |E|$:

| Operación | Matriz de Adyacencia | Lista de Adyacencia Estándar | Lista con Punteros Cruzados | Lista con Tabla de Hash |
| :--- | :---: | :---: | :---: | :---: |
| **Espacio de memoria** | $\Theta(n^2)$ | $\Theta(n + m)$ | $\Theta(n + m)$ | $\Theta(n + m)$ promedio |
| **Inicializar desde lista de $V$ y $E$** | $\Theta(n^2 + m)$ | $\Theta(n + m)$ | $\Theta(n + m)$ | $\Theta(n + m)$ |
| **¿Son adyacentes $v$ y $w$?** | $\Theta(1)$ | $\mathcal{O}(\min(d(v), d(w)))$ | $\mathcal{O}(\min(d(v), d(w)))$ | $\mathcal{O}(1)$ esperado |
| **Recorrer $N(v)$** | $\Theta(n)$ | $\Theta(d(v))$ | $\Theta(d(v))$ | $\Theta(d(v))$ |
| **Insertar arista $(v, w)$** | $\Theta(1)$ | $\Theta(1)$ al inicio / $\mathcal{O}(d(v))$ si chequea duplicados | $\Theta(1)$ al inicio con enlace mutuo | $\mathcal{O}(1)$ esperado |
| **Borrar arista $(v, w)$** | $\Theta(1)$ | $\mathcal{O}(d(v) + d(w))$ por búsqueda lineal | $\Theta(1)$ si se tiene puntero directo | $\mathcal{O}(1)$ esperado |
| **Borrar vértice $v$ y sus aristas** | $\Theta(n)$ | $\mathcal{O}(n + m)$ (debe buscar a $v$ en cada vecino) | $\Theta(d(v))$ (usa punteros cruzados) | $\Theta(d(v))$ |
| **Mantener orden arbitrario en $N(v)$** | Costoso (índices fijos) | $\mathcal{O}(d(v))$ por inserción ordenada | $\mathcal{O}(d(v))$ por inserción ordenada | Incompatible (hash no garantiza orden) |

> [!TIP]
> **Punteros cruzados:** Si la arista $\{v, w\}$ se guarda en $N(v)$ como un nodo que almacena un puntero al nodo de $v$ dentro de $N(w)$, la eliminación de una arista ya localizada se reduce de $\mathcal{O}(d(v) + d(w))$ a $\Theta(1)$.

---

### 1.2. Cotas Globales y Suma de Grados (Base para Ej. 2)
* **Lema del Apretón de Manos:** $\sum_{v \in V} d(v) = 2m$.
* **Acotación de trabajo sobre aristas:**  
  Si un algoritmo realiza una pasada sobre todos los vecinos de un nodo para cada arista incidente, el costo total suma:
  $$\sum_{v \in V} \sum_{w \in N(v)} 1 = \sum_{v \in V} d(v) = 2m = \mathcal{O}(m)$$
  Si itera sobre todas las aristas $m$ y hace trabajo proporcional a $n$ por vértice: $\Theta(n \cdot m)$. Dado que en cualquier grafo simple $m \le \binom{n}{2} < \frac{n^2}{2}$, vale que $\mathcal{O}(n \cdot m) = \mathcal{O}(m \sqrt{m})$ o $\mathcal{O}(m^2)$.

---

### 1.3. Digrafos, DAGs y Orden Topológico (Base para Ej. 3)
* **Propiedad de existencia de fuente en DAG:**
  * Si un digrafo finito no contiene ciclos dirigidos, **necesariamente existe al menos un vértice con grado de entrada $d^-(v) = 0$**.
  * *Demostración constructiva / Palomar:* Se parte de un vértice arbitrario $v_0$. Si $d^-(v_0) > 0$, existe un predecesor $v_1 \to v_0$. Repitiendo hacia atrás, se genera una secuencia $v_k \to \dots \to v_1 \to v_0$. Al haber solo $n$ vértices, si la secuencia supera longitud $n$, por Principio del Palomar se repite un nodo, formando un ciclo dirigido. Contradicción. Por ende, la cadena debe frenar en un nodo sin predecesores ($d^- = 0$).
* **Algoritmo de Kahn:**
  * Mantiene una cola con los nodos con $d^-(v) = 0$.
  * Al desencolar $u$, se coloca en el orden topológico y se decrementa $d^-(w)$ para todo $w \in N^+(u)$. Si llega a 0, entra a la cola.
  * Complejidad: $\Theta(n + m)$. Si al terminar se procesaron menos de $n$ vértices $\implies$ el digrafo contiene un ciclo.

---

### 1.4. Digrafos con $d_{out}(v) = 1$ (Digrafos $\rho$, Base para Ej. 4)
* En cualquier digrafo donde todo nodo tiene $d_{out}(v) = 1$:
  * Al comenzar una caminata dirigida desde cualquier vértice $v_0$, como siempre existe un único arco saliente, el camino continúa indefinidamente.
  * Al ser el grafo finito ($n$ vértices), tras a lo sumo $n$ pasos se debe repetir un vértice ya visitado $\implies$ se ingresa inevitablemente a un ciclo dirigido.
  * Como de cada nodo del ciclo solo sale una arista (la que va al siguiente nodo del ciclo), **ningún camino puede escapar del ciclo**.
  * En una componente conexa (débilmente conexa), **no pueden coexistir dos ciclos dirigidos distintos**: si los hubiera, o bien se conectan (lo que exigiría que algún nodo del primer ciclo bifurque, imposible pues $d_{out}=1$), o bien sus árboles de entrada colisionan en un nodo que debería tener dos salidas.

---

### 1.5. Propiedades Fundamentales del Recorrido DFS (Base para Ej. 10, 12, 13)
* **Clasificación en Grafos No Dirigidos:**
  * Toda arista explorada por DFS es una **Tree-edge** (arista de árbol) o una **Back-edge** (arista de retroceso hacia un ancestro).
  * **No existen aristas de cruce (cross-edges) ni de avance (forward-edges)** en grafos no dirigidos.
* **Bipartitud y Paridad de Capas:**
  * Las capas de un árbol DFS o BFS asignan a cada nodo una distancia/profundidad $h(v)$.
  * Si existe una arista no-árbol $(u, v)$ entre nodos a distancias de la misma paridad ($h(u) \equiv h(v) \pmod 2$), la arista cierra un camino en el árbol de longitud par más la arista misma ($+1$) $\implies$ **ciclo de longitud impar**.
* **Puentes y Función $low$ / `cubren(v)`:**
  * Una arista es puente $\iff$ no pertenece a ningún ciclo.
  * En DFS, las *back-edges* nunca son puentes (pertenecen al ciclo formado con el camino de árbol hasta el ancestro).
  * Una *tree-edge* $(p(u), u)$ es puente $\iff$ ningún descendiente de $u$ (ni el propio $u$) tiene una *back-edge* que alcance a un ancestro estricto de $u$.
  * Condición analítica: $low[u] > d[p(u)]$, o equivalentemente en términos de flujo de backedges: $\text{cubren}(u) = 0$.
* **Orientaciones Fuertemente Conexas (Teorema de Robbins):**
  * $G$ admite orientación fuertemente conexa $\iff G$ es conexo y **no tiene puentes**.
  * Construcción canónica mediante DFS: orientar las *tree-edges* hacia abajo (padre $\to$ hijo) y las *back-edges* hacia arriba (descendiente $\to$ ancestro).

---

### 1.6. Propiedades del Recorrido BFS y Árboles Geodésicos (Base para Ej. 15, 16)
* **BFS:**
  * Explora por niveles de distancia mínima en aristas $d(s, v)$.
  * Cada arista del grafo solo puede conectar nodos del mismo nivel ($|d(u) - d(v)| = 0$) o de niveles consecutivos ($|d(u) - d(v)| = 1$).
  * Todo árbol BFS enraizado en $v$ es **$v$-geodésico** (las distancias en el árbol desde $v$ son iguales a las distancias en $G$).
* **La vuelta no vale (Contraejemplo canónico):**
  * Existen árboles generadores $v$-geodésicos que **no** pueden ser generados por BFS.
  * Razón: BFS impone restricciones adicionales sobre el orden en que se descubren y eligen los padres entre niveles iguales.

---

## 2. Ficha de Preparación por Ejercicio (Guía 3 Mínimos)

A continuación se mapea cada ejercicio con la técnica exacta y la estrategia de resolución para cuando nos sentemos a resolverlos:

```
[Guía 3 Mínimos]
 │
 ├── Ejercicio 1  ──> Tema: Representación de grafos (8 ops x 4 estructuras)
 │                    Estrategia: Matriz analítica de complejidades, trade-offs memoria vs consulta.
 │
 ├── Ejercicio 2  ──> Tema: Detección de triángulos
 │                    Estrategia: Algoritmo cúbico A³ / combinaciones vs Algoritmo cuadrático con marcas de vecinos.
 │                    Clave: Suma de grados ∑ d(v) = 2m para probar O(nm).
 │
 ├── Ejercicio 3  ──> Tema: Orden Topológico (DAGs)
 │                    Estrategia: Demostración por contradicción/palomar de d⁻=0. Algoritmo de Kahn con cola.
 │                    Clave: Ida por contrarrecíproco (si tiene ciclo, no hay orden topológico).
 │
 ├── Ejercicio 4  ──> Tema: Digrafos con forma de ρ (d_out = 1)
 │                    Estrategia: Caminata dirigida infinita en espacio finito. Detección de ciclos en O(n).
 │
 ├── Ejercicio 10 ──> Tema: Grafos Bipartitos y DFS
 │                    Estrategia: Partición de vértices por profundidad par/impar. 
 │                    Clave: Si una arista conecta misma paridad, reconstruir el ciclo impar en O(n+m).
 │
 ├── Ejercicio 12 ──> Tema: Detección de Puentes
 │                    Estrategia: DFS con marcas de nivel y cálculo de low[v] / cubren(v).
 │                    Clave: Separar cálculo en dos fases o calcular low en el post-order del DFS.
 │
 ├── Ejercicio 13 ──> Tema: Orientaciones Fuertes (Robbins)
 │                    Estrategia: Demostración de equivalencias I <=> II <=> III <=> IV.
 │                    Clave: Inducción en el nivel de los nodos para probar que la raíz es alcanzable.
 │
 ├── Ejercicio 15 ──> Tema: Conteo y Tamaño de Componentes Conexas
 │                    Estrategia: BFS/DFS global con arreglo de visitados, etiquetando identificador de componente.
 │
 └── Ejercicio 16 ──> Tema: Árboles Geodésicos
                      Estrategia: Prueba de que BFS preserva distancias geodésicas.
                      Clave: Construir un contraejemplo mínimo para la vuelta (grafo con ciclos donde un árbol geodésico rompe el orden FIFO de BFS).
```

---

## 3. Estado de Alistamiento para Mañana

1. **Contexto asimilado:** Los principios de inducción sobre grafos, clasificación de aristas en DFS y capas de BFS están alineados con el estándar de `Practicas Condensadas.md`.
2. **Archivos de salida:** Los ejercicios se redactarán directamente en archivos individuales (e.g. `Ejercicio 1 practica 3.md`) con el mismo formato riguroso de demostración, pasos y enseñanzas metodológicas visto en la Práctica 2.
3. **Punto de inicio listo:** Mañana comenzaremos directamente atacando el **Ejercicio 1** (discusión de las 8 operaciones en las 4 estructuras de representación).
