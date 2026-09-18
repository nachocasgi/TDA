# Práctica 2: Introducción a Grafos
# Ejercicio 8: Unicidad y Existencia en Digrafos Orientados

> **Materia:** Técnicas de Diseño de Algoritmos / Algoritmos y Estructuras de Datos III  
> **Tema:** Grafos Orientados, Grados de Salida ($d_{out}$), Inducción Matemática Estructural (Existencia y Unicidad simultáneas).

---

## 1. Enunciado

> *«Un grafo orientado es un digrafo $D$ tal que al menos uno de $v \to w$ y $w \to v$ no es una arista de $D$, para todo $v, w \in V(D)$. En otras palabras, un grafo orientado se obtiene a partir de un grafo no dirigido dando una dirección a cada arista. Demostrar que para cada $n$ existe un único grafo orientado cuyos vértices tienen todos grados de salida distintos.»*

---

## 2. Datos y Condiciones de Partida

* **Objeto:** Digrafo orientado $D = (V, E)$ (sin bucles y sin aristas bidireccionales: si $(u, v) \in E \implies (v, u) \notin E$).
* **Vértices:** $|V(D)| = n$, con $n \ge 1$.
* **Grados de salida:** Para cada $v \in V$, $0 \le d_{out}(v) \le n - 1$.
* **Condición clave:** Todos los grados de salida son distintos entre sí.
  Como hay $n$ vértices y los valores posibles de $d_{out}$ van de $0$ a $n - 1$ (exactamente $n$ valores disponibles), los grados de salida están **obligados** a ser:
  $$\{d_{out}(v) \mid v \in V\} = \{0, 1, 2, \dots, n - 1\}$$
* **Meta a demostrar ($\exists!$):** Que para cada $n$ existe **un único** grafo orientado con esta propiedad.

---

## 3. Demostración Paso a Paso por Inducción Matemática

### Paso 1: Definición Formal del Predicado $P(n)$
Para cada $n \in \mathbb{N}_{\ge 1}$, definimos:
$$P(n) := \text{“Existe un único grafo orientado } D_n \text{ de } n \text{ vértices con grados de salida } \{0, 1, \dots, n - 1\}\text{”.}$$

---

### Paso 2: Caso Base ($n = 1$)
Consideramos un grafo con $n = 1$ vértice: $V = \{v_0\}$.
* Como no hay otros vértices en el grafo, no puede salir ninguna arista: $d_{out}(v_0) = 0$.
* El conjunto de grados de salida es $\{0\}$, cuyos elementos son todos distintos entre sí (es un único vértice).
* **Existencia:** El grafo con 1 vértice y 0 aristas cumple la propiedad.
* **Unicidad:** No hay ninguna otra configuración posible para 1 solo vértice sin bucles.

Por lo tanto, **$P(1)$ es verdadero**. $\checkmark$

---

### Paso 3: Paso Inductivo ($n \ge 2$)

* **Hipótesis Inductiva (H.I.):**  
  Asumimos que $P(n - 1)$ es verdadero:  
  > *«Existe un único grafo orientado $D_{n-1}$ de $n - 1$ vértices cuyos grados de salida son $\{0, 1, \dots, n - 2\}$».*

* **Tesis Inductiva (T.I.):**  
  Queremos demostrar que $P(n)$ es verdadero:  
  > *«Existe un único grafo orientado $D_n$ de $n$ vértices cuyos grados de salida son $\{0, 1, \dots, n - 1\}$».*

---

### Paso 4: Identificación del Vértice de Grado Máximo
Sea $D$ cualquier grafo orientado de $n$ vértices que cumpla la propiedad.  
Dado que los grados de salida son $\{0, 1, \dots, n - 1\}$, existe un único vértice con grado de salida máximo:
$$d_{out}(v_{n-1}) = n - 1$$

Analizamos las conexiones de $v_{n-1}$:
1. Como $v_{n-1}$ tiene $n - 1$ aristas salientes y sólo hay otros $n - 1$ vértices en el grafo, **$v_{n-1}$ debe tener una arista hacia absolutamente todos los demás vértices**:
   $$(v_{n-1}, u) \in E \quad \forall u \in V \setminus \{v_{n-1}\}$$
2. Por definición de grafo orientado, no puede haber aristas bidireccionales. Dado que $(v_{n-1}, u) \in E$, se deduce que:
   $$(u, v_{n-1}) \notin E \quad \forall u \in V \setminus \{v_{n-1}\}$$
   Es decir: **ningún vértice del grafo apunta hacia $v_{n-1}$**.

---

### Paso 5: Deconstrucción a $D' = D - v_{n-1}$
Removemos el vértice $v_{n-1}$ del grafo. Obtenemos el subgrafo:
$$D' = D - v_{n-1}$$

* **Cantidad de vértices:** $|V(D')| = n - 1$.
* **Grados de salida en $D'$:**  
  Para cualquier vértice $u \in V(D')$, sus aristas salientes sólo podían ir hacia otros vértices de $D'$ o hacia $v_{n-1}$.  
  Pero en el Paso 4 demostramos que $(u, v_{n-1}) \notin E$ (ningún vértice apuntaba hacia $v_{n-1}$).  
  Por lo tanto, **al eliminar $v_{n-1}$, ningún vértice de $D'$ pierde aristas de salida**:
  $$d_{out}^{D'}(u) = d_{out}^D(u) \quad \forall u \in V(D')$$
* En consecuencia, los grados de salida de $D'$ son exactamente los mismos que en $D$ (restando el $n - 1$ de $v_{n-1}$):
  $$\{d_{out}^{D'}(u) \mid u \in V(D')\} = \{0, 1, \dots, n - 2\}$$

---

### Paso 6: Aplicación de la Hipótesis Inductiva
El grafo orientado $D'$ tiene $n - 1$ vértices y sus grados de salida son todos distintos: $\{0, 1, \dots, n - 2\}$.  
Por la **Hipótesis Inductiva $P(n - 1)$**:
> **El grafo orientado $D'$ existe y es ÚNICO.**

---

### Paso 7: Remate de Unicidad y Existencia

1. **Unicidad (!):**  
   Cualquier grafo orientado $D$ de $n$ vértices con grados $\{0, 1, \dots, n - 1\}$ está obligado a contener al grafo único $D'$ y al vértice $v_{n-1}$.  
   Como las conexiones de $v_{n-1}$ están unívocamente forzadas (debe apuntar a todos los nodos de $D'$ y nadie a él), **no existe ninguna otra forma posible de construir $D$**. Por lo tanto, **$D$ es único**.

2. **Existencia ($\exists$):**  
   Por H.I. sabemos que existe el grafo $D'$.  
   Si tomamos $D'$ y agregamos un nuevo vértice $v_{n-1}$ con aristas salientes hacia todos los vértices de $D'$, obtenemos un grafo orientado $D$ de $n$ vértices donde:
   * $d_{out}(v_{n-1}) = n - 1$.
   * Los demás vértices conservan intactos sus grados de salida $\{0, 1, \dots, n - 2\}$.
   * Todos los grados son distintos y no se generan aristas bidireccionales.  
   Por lo tanto, **$D$ existe**.

---

### Paso 8: Conclusión
Queda demostrado que $P(n)$ es verdadero.  
Por el **Principio de Inducción Matemática**, para todo $n \in \mathbb{N}_{\ge 1}$ existe un único grafo orientado con grados de salida todos distintos. $\blacksquare$

---

## 4. Enseñanza Metodológica: Demostración Estructural vs. Demostración Algebraica

* **No todo en inducción son cuentas y desigualdades:**  
  En el Ejercicio 7 necesitábamos álgebra para que cuadraran las cotas de aristas. En el Ejercicio 8 no hay cuentas algebraicas: es una **inducción puramente estructural**.
* **El truco de "nadie le apunta":**  
  Elegir sacar el vértice de grado de salida máximo ($n - 1$) fue la clave porque garantizaba que $d_{in}(v_{n-1}) = 0$. Al no tener flechas entrantes, su eliminación deja **completamente congelados e inalterados** los grados de salida de todos los demás vértices, permitiendo aplicar la H.I. de forma limpia e inmediata.
