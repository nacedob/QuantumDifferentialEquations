# De la integración numérica a la simulación cuántica de ecuaciones diferenciales

**Seminario · Universidad Nebrija · 10 de noviembre de 2026**  
**Ponente:** Ignacio Acedo

Este repositorio reúne los materiales prácticos del seminario. A través de dos ejemplos guiados, recorreremos dos maneras de estudiar la evolución de un sistema descrito por ecuaciones diferenciales ordinarias (EDO): primero, mediante un método numérico clásico; después, explorando cómo una parte de esa dinámica puede representarse como la evolución de un estado cuántico.

El objetivo no es ofrecer una introducción completa a la computación cuántica ni demostrar una ventaja computacional. La intención es construir una conexión entre conceptos matemáticos familiares y una idea de la simulación cuántica: **codificar un vector en las amplitudes de un estado, construir un Hamiltoniano adecuado y utilizar su evolución para reproducir una dinámica**.

## Contexto y objetivos

Las EDO aparecen al describir sistemas que cambian con el tiempo, desde el movimiento de un muelle hasta procesos de enfriamiento o modelos más complejos. En muchos casos no podemos obtener una solución analítica sencilla y necesitamos aproximar la evolución mediante métodos numéricos.

El seminario parte de esa perspectiva clásica. Veremos cómo discretizar el tiempo, cómo organizar las ecuaciones resultantes en forma matricial y por qué una aproximación numérica puede alejarse del comportamiento físico si el paso temporal no es adecuado. Con esa base, pasaremos a un ejemplo cuántico sencillo para identificar una correspondencia entre la dinámica de una EDO lineal y una evolución hamiltoniana.

Al terminar, la audiencia debería poder:

- Explicar la intuición del método de Euler y reconocer el papel del paso temporal.
- Interpretar la discretización de un sistema lineal como un problema de álgebra lineal.
- Identificar errores de discretización y problemas de estabilidad en ejemplos concretos.
- Entender cómo las componentes de un vector pueden asociarse a las amplitudes de un qubit.
- Seguir la construcción de un Hamiltoniano cuya evolución reproduce la parte rotacional de un sistema diferencial.
- Reconocer las limitaciones de esta demostración y las preguntas que habría que resolver para escalarla.

## Recorrido por los notebooks

### 1. Método clásico: de la pendiente a la solución numérica

Archivo: [`1-Metodo Clásico Euler.ipynb`](<./1-Metodo Clásico Euler.ipynb>)

El primer notebook introduce el método de Euler a partir de su interpretación geométrica: en cada paso se aproxima la curva solución por la recta tangente en el punto actual. A partir de la regla

$$
y_{n+1}=y_n+h\,f(t_n,y_n),
$$

se estudia cómo construir una aproximación de la evolución a lo largo del tiempo.

El notebook desarrolla tres ideas:

1. **La regla de Euler y su formulación matricial.** Para un sistema lineal, se expresa la actualización como $\mathbf y_{n+1}=(I+hA)\mathbf y_n$. Después se agrupan los estados de todos los instantes en un único sistema lineal por bloques.
2. **El oscilador armónico simple.** Se transforma la ecuación de segundo orden en un sistema de primer orden y se compara la solución numérica de Euler con la solución analítica. El ejemplo permite observar cómo la amplitud numérica puede crecer aunque el oscilador ideal conserve una amplitud constante.
3. **Las limitaciones de la integración paso a paso.** Se discuten el error de discretización, su acumulación y la estabilidad. Un ejemplo de enfriamiento muestra cómo un paso temporal demasiado grande puede producir una evolución numérica oscilatoria y creciente, aunque la solución física se aproxime monótonamente a la temperatura ambiente.

La idea que conecta este bloque es que **aproximar una evolución continua requiere elegir y controlar cuidadosamente la discretización**.

### 2. Método cuántico: de una EDO a una evolución hamiltoniana

Archivo: [`2- Metodo Cuántico Schrodingerization.ipynb`](<./2- Metodo Cuántico Schrodingerization.ipynb>)

El segundo notebook estudia un sistema lineal de dos variables cuya trayectoria combina rotación y amortiguamiento:

$$
\dot{\mathbf x}=A\mathbf x,\qquad
A=\begin{pmatrix}-\gamma&-\omega\\\omega&-\gamma\end{pmatrix}.
$$

Se interpreta la solución como un punto que gira en el plano mientras su distancia al origen disminuye. A continuación, el notebook se centra en la parte rotacional y presenta la conexión con un sistema cuántico de dos niveles:

1. **Codificación.** Las dos componentes del vector se asocian a las dos amplitudes de un estado de un qubit.
2. **Construcción del Hamiltoniano.** La matriz que genera la rotación permite construir $H=\hbar\omega\sigma_y$, de modo que la ecuación de Schrödinger reproduce esa parte de la dinámica.
3. **Simulación hamiltoniana con Qiskit.** Se representa la evolución $e^{-iHt/\hbar}$ mediante `PauliEvolutionGate` y se inspecciona el estado resultante con `Statevector` en un simulador ideal.
4. **Reconstrucción.** Como la evolución hamiltoniana conserva la norma, el amortiguamiento no se incluye en el estado cuántico de este ejemplo. Se recupera la solución clásica completa aplicando por separado el factor $e^{-\gamma t}$.

Este bloque busca transmitir una idea concreta: **para ciertas dinámicas lineales, podemos identificar un Hamiltoniano cuya evolución reproduce la parte adecuada de la ecuación diferencial**. Qiskit se utiliza directamente como herramienta de demostración; no es necesario conocer previamente su API.

> **Alcance y precisión terminológica.** Aunque el nombre del archivo incluye “Schrodingerization”, el notebook implementa una correspondencia directa para la componente rotacional y trata el amortiguamiento mediante un factor clásico. No desarrolla una transformación general de Schrödingerization para convertir dinámicas no unitarias arbitrarias en evoluciones unitarias. El ejemplo tampoco demuestra una ventaja cuántica frente a los métodos clásicos ni constituye un algoritmo general para resolver cualquier EDO.

## Cómo ejecutar los materiales

Se recomienda abrir los notebooks con JupyterLab, Jupyter Notebook o Google Colab y ejecutarlos de principio a fin, respetando el orden propuesto.

### Entorno

Los notebooks utilizan Python, NumPy y Matplotlib. El segundo requiere además Qiskit para ejecutar la parte de simulación cuántica.

Instala las dependencias con:

```bash
python -m pip install numpy matplotlib "qiskit>=2,<3" jupyter
```

La parte clásica del segundo notebook puede estudiarse sin Qiskit, pero para ejecutar el circuito y obtener el vector de estado es necesario instalarlo. El notebook utiliza las clases `QuantumCircuit`, `SparsePauliOp`, `PauliEvolutionGate` y `Statevector`.

### Orden recomendado

1. Ejecutar `1-Metodo Clásico Euler.ipynb` para establecer la intuición de integración numérica y estabilidad.
2. Ejecutar `2- Metodo Cuántico Schrodingerization.ipynb` para estudiar la codificación cuántica y la simulación hamiltoniana.

## Idea para llevarse del seminario

Los métodos clásicos aproximan la evolución mediante pasos temporales. La simulación hamiltoniana ofrece otra perspectiva para determinadas EDO lineales: representar el estado del sistema mediante amplitudes cuánticas y reproducir su evolución a través de un Hamiltoniano.

La posibilidad de representar un vector de dimensión grande con un número relativamente pequeño de qubits resulta sugerente, pero por sí sola no garantiza una ventaja práctica. Para que esa posibilidad se traduzca en un algoritmo útil también hay que considerar el coste de preparar el estado inicial, implementar el Hamiltoniano y extraer la información deseada mediante mediciones.

## Referencias y documentación

- IBM Quantum, [PauliEvolutionGate](https://qiskit.qotlabs.org/docs/api/qiskit/qiskit.circuit.library.PauliEvolutionGate).
- IBM Quantum, [Statevector](https://qiskit.qotlabs.org/docs/api/qiskit/qiskit.quantum_info.Statevector).
- IBM Quantum, [Instalación de Qiskit](https://qiskit.qotlabs.org/docs/guides/install-qiskit).

---

*Material docente preparado para el seminario de la Universidad Nebrija. Autor: Ignacio Acedo.*
