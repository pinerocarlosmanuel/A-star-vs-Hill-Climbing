# Análisis Comparativo de Algoritmos de Búsqueda: A* frente a Hill Climbing en un Problema de Navegación

**Autor:** Carlos Manuel Piñero Ramírez
**Institución:** Facultad de Tecnologías Libres — Universidad de las Ciencias Informáticas

Trabajo de la asignatura **Inteligencia Artificial** que contrasta empíricamente una
búsqueda heurística **global y óptima** (A*) frente a una búsqueda **local y voraz**
(Steepest-Ascent Hill Climbing con reinicios aleatorios) sobre un problema de navegación
en un plano 2D discreto.

---

## Descripción del problema

Se dispone de un plano 2D discreto definido por el archivo `dataset_coordenadas.csv`,
con `225` registros `(x, y)` etiquetados como `libre`, `inicio`, `objetivo` u
`obstaculo`. El espacio resultante es una cuadrícula de **15 × 15** celdas con
**206 celdas libres**, **17 obstáculos**, el estado inicial en `(1, 1)` y el objetivo
en `(13, 13)`.

### Formalización

- **Estado:** tupla `(x, y)` que representa una celda transitable.
- **Estado inicial:** `(1, 1)`.
- **Estado objetivo:** `(13, 13)`.
- **Acciones:** `A = {ARRIBA, ABAJO, IZQUIERDA, DERECHA}` (4-conectividad, sin diagonales).
- **Modelo de transición:** `resultado(s, a) = s + Δ(a)`, válido si el destino está dentro
  del tablero y no es un obstáculo.
- **Costo de arista:** distancia euclídea entre celdas consecutivas (`1.0` para
  movimientos ortogonales).
- **Heurística:** distancia euclídea al objetivo `h(n) = √((xₙ−x_g)² + (yₙ−y_g)²)`,
  **admisible** y **consistente**.

## Algoritmos implementados

- **A\*** — búsqueda informada global con cola de prioridad (`heapq`) ordenada por
  `f(n) = g(n) + h(n)`. Al ser `h(n)` admisible y consistente, A* es **completo** y
  **óptimo**.
- **Hill Climbing (steepest ascent)** — búsqueda local que evalúa `f(n) = −h(n)` y avanza
  al mejor vecino inmediato. Se le añaden **reinicios aleatorios** para mitigar óptimos
  locales. No ofrece garantías de completitud ni optimalidad.

Ambos algoritmos se implementaron **desde cero**, usando exclusivamente `numpy`, `pandas`,
`matplotlib` y `heapq`.

## Resultados clave

| Métrica | A* | Hill Climbing |
|---|---|---|
| ¿Encontró el objetivo? | Sí | Sí (100 % de 30 ejecuciones) |
| Longitud del camino (nodos) | 25 | 25.0 ± 0.0 |
| Costo total (distancia euclídea) | 24.00 | 24.00 ± 0.00 |
| Nodos expandidos | 152 | 59.0 ± 0.0 |
| Reinicios aleatorios | — (determinista) | 0.0 |

- En **este** tablero Hill Climbing alcanzó el óptimo global expandiendo menos nodos que
  A*, porque el muro de obstáculos es convexo respecto al objetivo y la distancia euclídea
  decrece monótonamente. Un barrido voraz desde las **206 celdas libres** confirmó la
  ausencia de óptimos locales (100 % de éxito).
- Para evidenciar la debilidad estructural de la búsqueda local se diseñó un **escenario
  adversario en «U»**: Hill Climbing queda atrapado en un máximo local `(5, 8)` a distancia
  `7.00` de la meta, mientras que A* bordea el obstáculo y resuelve con costo `14.00`.

## Estructura del repositorio

```
.
├── T2Carlos_Piñero.ipynb      # Notebook principal (análisis completo y ejecutable)
├── dataset_coordenadas.csv    # Dataset de entrada (REQUERIDO)
├── informe.tex                # Fuente LaTeX del informe
├── informe.pdf                # Informe compilado
├── requirements.txt           # Dependencias de Python
├── LICENSE                    # GNU General Public License v3.0
├── .gitignore
└── README.md
```

## Requisitos

- **Python ≥ 3.12**
- Dependencias listadas en `requirements.txt`:
  - `numpy >= 1.26`
  - `pandas >= 2.1`
  - `matplotlib >= 3.8`
  - `jupyterlab >= 4.0`

## Instalación y uso

```bash
# 1. Crear y activar un entorno virtual
python3 -m venv venv
source venv/bin/activate        # En Windows: venv\Scripts\activate

# 2. Instalar dependencias
pip install -r requirements.txt

# 3. Abrir el notebook
jupyter lab T2Carlos_Piñero.ipynb
```

Ejecuta todas las celdas en orden. El notebook cargará `dataset_coordenadas.csv` desde el
directorio de trabajo y generará las figuras y la tabla comparativa.

> **Importante:** el archivo `dataset_coordenadas.csv` debe estar en la raíz del
> repositorio para que el notebook se ejecute correctamente.

## Salidas generadas

Al ejecutar el notebook se generan los siguientes archivos (excluidos vía `.gitignore`):

- `mapa_espacio_estados.png` — visualización del espacio de estados.
- `ruta_a_estrella.png` — ruta óptima de A* y su orden de expansión.
- `ruta_hill_climbing.png` — mejor trayectoria de Hill Climbing.
- `comparacion_rutas.png` — superposición de ambas rutas.
- `analisis_optimos_locales.png` — nodos expandidos, costo y reinicios.
- `escenario_trampa.png` — escenario adversario en «U».
- `metricas_comparativas.csv` — tabla de métricas comparativas.

## Bibliografía

- Stuart J. Russell and Peter Norvig. *Artificial Intelligence: A Modern Approach*.
  4th Edition. Pearson, 2020.
- P. E. Hart, N. J. Nilsson, and B. Raphael. *A Formal Basis for the Heuristic
  Determination of Minimum Cost Paths*. IEEE Transactions on Systems Science and
  Cybernetics, 4(2):100–107, 1968.

## Licencia

Este proyecto se distribuye bajo la **GNU General Public License v3.0**
(ver el archivo [`LICENSE`](LICENSE)).

Copyright (C) 2026 Carlos Manuel Piñero Ramírez.
