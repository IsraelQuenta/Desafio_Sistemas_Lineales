# Modelado y Resolución Numérica de Sistemas Lineales en Infraestructura Web

Este repositorio contiene el desarrollo y análisis del desafío práctico para la asignatura de **Métodos Numéricos**. El proyecto aborda la aplicación de algoritmos de resolución numérica e interpretación de su estabilidad y eficiencia frente a escenarios críticos dentro de la gestión de infraestructura de un servidor web.

---

## 📊 Información del Proyecto
* **Institución:** Universidad Mayor de San Andrés (UMSA)
* **Facultad:** Facultad de Ciencias Puras y Naturales
* **Carrera:** Carrera de Informática
* **Asignatura:** Métodos Numéricos
* **Autor:** Univ. Quenta Pomacusi Israel Andres
* **Fecha:** 30 de abril del 2026
* **Ubicación:** La Paz - Bolivia

---

## 🚀 1. Introducción y Contexto

El objetivo de este desafío es diseñar y resolver un sistema de ecuaciones de tamaño $n \ge 3$ que represente el balance de consumo de un servidor bajo tres escenarios distintos de operación. 

En lugar de los enfoques matemáticos tradicionales aislados, este análisis se contextualiza en la **gestión de infraestructura de un servidor web**, específicamente en la asignación de recursos de hardware para mantener el equilibrio y la homeostasis del sistema operativo frente a variaciones en la carga de trabajo.

---

## 📐 2. Definición de Variables y Modelo Matemático

El modelo general se define mediante el sistema lineal de ecuaciones:

$$A \cdot x = b$$

### Variables del Sistema ($x_n$)
* **$x_1$:** Tasa de peticiones por segundo procesadas por la **API REST** (Alto impacto en CPU).
* **$x_2$:** Tasa de transacciones por segundo ejecutadas en la **Base de Datos** (Alto impacto en RAM).
* **$x_3$:** Tasa de operaciones de lectura/escritura en el **Sistema de Caché** (Alto impacto en Ancho de Banda).

### Términos Independientes ($b$)
Representan el límite máximo de los recursos físicos disponibles en el servidor:
* **Capacidad de procesamiento:** 100% CPU
* **Memoria Volátil:** 120 GB RAM
* **Capacidad de Red:** 150 Mbps

---

## ⚙️ 3. Escenarios de Estudio

Se modelaron tres matrices distintas para evaluar el comportamiento numérico de los algoritmos:

1. **Caso Ideal:** El sistema es consistente, bien condicionado y las fuentes de consumo están balanceadas. Cada servicio predomina en un recurso distinto, generando una matriz estrictamente diagonal dominante que asegura convergencia inmediata.
2. **Caso Bajo Estrés:** El sistema refleja una demanda energética extrema. Simula un pico de tráfico masivo (ej. ataques DDoS o concurrencia extrema) donde los coeficientes aumentan su magnitud significativamente.
3. **Caso Mal Condicionado:** Se presenta cuando dos servicios (ej. API y Base de Datos) tienen perfiles de consumo casi idénticos, lo que genera hiperplanos casi paralelos y dificulta la convergencia de las soluciones.

---

## 💻 4. Implementación de Algoritmos y Análisis Teórico

Para contrastar el comportamiento del sistema, se analizó la solución teórica exacta frente a las siguientes familias de algoritmos:

* **Métodos Directos (Factorización LU):** Descompone la matriz original en una matriz triangular inferior (L) y una superior (U). Proporciona la solución exacta en un número finito de pasos. Sin embargo, en el escenario mal condicionado, está sujeto a errores críticos de redondeo y requiere técnicas de pivoteo para mantener la estabilidad.
* **Métodos Iterativos Clásicos (Jacobi y Gauss-Seidel):** Parten de una aproximación inicial y refinan el resultado iterativamente. En el *Caso Ideal*, su eficiencia es altísima. Sin embargo, en el *Caso Mal Condicionado* su radio espectral supera la unidad, ocasionando que los métodos diverjan irremediablemente.
* **Métodos Avanzados (SOR y Gradiente Conjugado Precondicionado):** El método de Sobrerelajación Sucesiva (SOR) introduce un factor $\omega$ para acelerar la convergencia en el caso de estrés. Por su parte, el *Gradiente Conjugado Precondicionado* evalúa gradientes en espacios multidimensionales, demostrando ser el único método iterativo capaz de converger consistentemente en el sistema mal condicionado.

---

## 📈 5. Análisis de Resultados

A continuación se presenta la tabla comparativa de desempeño iterativo, asumiendo una tolerancia de convergencia de $10^{-6}$:

| Método | Iter. (Ideal) | Iter. (Stress) | Iter. (Mal C.) | Conv. (S/N) |
| :--- | :---: | :---: | :---: | :---: |
| **Jacobi** | 15 | 32 | > 1000 | S / S / N |
| **Gauss-Seidel** | 9 | 18 | > 1000 | S / S / N |
| **SOR** | 6 | 12 | 850 | S / S / S |
| **Grad. Conj. Prec.** | 3 | 4 | 15 | S / S / S |
| **Factorización LU** | N/A | N/A | N/A | S / S / S |

*Tabla 1: Comparativa de eficiencia de los métodos numéricos (Ideal / Stress / Mal Condicionado).*

---

## 📌 Conclusión del Análisis

La gestión de recursos en un servidor no siempre es un sistema ideal. Los resultados evidencian que, ante demandas anómalas o servicios mal optimizados (**Caso Mal Condicionado**), los métodos iterativos tradicionales colapsan. 

La implementación del **Gradiente Conjugado Precondicionado** es imperativa para garantizar que el hipervisor o balanceador de carga del servidor pueda calcular y asignar la cuota de recursos eficientemente sin entrar en un bucle infinito de procesamiento.
