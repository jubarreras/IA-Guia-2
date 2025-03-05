# IA-Guia-2
Solución a la guía #2 de la materia de Inteligencia Artificial en la Universidad Nacional de Colombia, hecha por el estudiante: Juan David Barrera Salamanca, Yeray Dario Moreno Rangel y Juan Diego Lozano Colmenares.
### 1. Observe sus comportamientos en la casa, en la universidad y en el medio de transporte que utiliza, encuentre, para cada uno de estos escenarios sus reglas básicas.
1. En la Casa 🏠 <br>
Vecindad: Familiares o compañeros de vivienda. <br>
Reglas básicas:

- Respetar horarios y espacios compartidos.
- Seguir normas de limpieza y orden.
- Participar en actividades comunes (comer juntos, ver televisión, etc.).
- Regular el volumen de la música o conversaciones para no afectar a los demás.
- Ajustar comportamientos según la rutina de los convivientes. <br>
📌 Ejemplo de comportamiento emergente: Si todos siguen reglas básicas de convivencia, el ambiente en casa es armonioso, pero si uno rompe la norma (ej., hace ruido a altas horas), se genera una reacción en cadena que afecta a los demás.

2. En la Universidad 🎓 <br>
Vecindad: Compañeros de clase, profesores, personal administrativo.<br>
Reglas básicas:

- Llegar a tiempo a las clases y respetar la puntualidad.
- Mantener la atención y participación en las actividades académicas.
- Seguir normas de respeto en el aula (ej., no interrumpir al profesor).
- Organizar trabajos en equipo siguiendo acuerdos internos. <br>
📌 Ejemplo de comportamiento emergente: Si los estudiantes respetan las reglas de participación en clase, se genera un aprendizaje colaborativo. Si muchos comienzan a faltar o a no participar, la dinámica de enseñanza se ve afectada.

3. En el Medio de Transporte 🚍🚆 <br>
Vecindad: Pasajeros, conductor, personal de servicio.<br>
Reglas básicas:

- Respetar el turno de ingreso y salida.
- Ceder el asiento a personas con prioridad (adultos mayores, mujeres embarazadas).
- Evitar hablar en voz alta o generar disturbios.
- Pagar el pasaje y respetar normas del servicio público.
- Mantener la higiene y orden dentro del transporte. <br>
📌 Ejemplo de comportamiento emergente: Si los pasajeros siguen las normas de cortesía y orden, el transporte fluye sin problemas. Si varios intentan colarse o no respetan los turnos, se genera caos y retrasos.
### 2. Suponga una enfermedad, o un incendio forestal, o una moda, desarrolle un modelo de difusión usando ACs probabilísticos.

El siguiente será un modelo utilizando autómatas celulares probabilísticos para representar la difusión de una enfermedad en una población. Este modelo puede extenderse a otros fenómenos como incendios forestales o modas, con algunos ajustes.

1. Definición del Modelo:
Espacio Discreto: La población está representada como una rejilla bidimensional de celdas. Cada celda puede tener diferentes estados, como:

- Sano (S): La persona está sana.
- Infectado (I): La persona está infectada (en el caso de una enfermedad).
- Recuperado (R): La persona se ha recuperado.
- Vacío (V): Área sin persona o no afectada.
- Vecindad: Cada celda está influenciada por las celdas vecinas. Usaremos una vecindad de Moore (8 vecinos), donde la celda central tiene 8 celdas circundantes.

Reglas de Transición:

- Infección: Si una persona sana (S) está rodeada por celdas infectadas (I) con una probabilidad  𝑝 infeccion, la persona sana se infecta. 
- Recuperación: Si una persona está infectada (I), después de un tiempo determinado o con probabilidad p recuperación, pasa al estado recuperado (R).
- Espacios Vacíos: Los espacios vacíos no afectan a la propagación de la enfermedad.

Evolución del tiempo:
La simulación avanza por pasos discretos en el tiempo. Después de cada ciclo, las celdas son actualizadas simultáneamente. <br>
**El codigo en Python que se desarrollo para mostrar el comportamiento simulado se muestra a continuación:**
```python
import random
import numpy as np

# Parámetros del modelo
TAM_REJILLA = 50  # Tamaño de la rejilla
p_infeccion = 0.3  # Probabilidad de infección
p_recuperacion = 0.1  # Probabilidad de recuperación
CICLOS = 100  # Número de ciclos de la simulación

# Estado de la celda: S=0, I=1, R=2, V=3
rejilla = np.zeros((TAM_REJILLA, TAM_REJILLA))

# Inicialización: porcentaje de población infectada
porcentaje_infectados = 0.1
for i in range(TAM_REJILLA):
    for j in range(TAM_REJILLA):
        if random.random() < porcentaje_infectados:
            rejilla[i, j] = 1  # Persona infectada

# Función para obtener los vecinos de una celda
def vecinos(i, j):
    vecinos_list = []
    for di in [-1, 0, 1]:
        for dj in [-1, 0, 1]:
            ni, nj = (i + di) % TAM_REJILLA, (j + dj) % TAM_REJILLA
            if (di != 0 or dj != 0):
                vecinos_list.append(rejilla[ni, nj])
    return vecinos_list

# Función para actualizar el estado de la rejilla
def actualizar_rejilla():
    global rejilla
    nueva_rejilla = np.copy(rejilla)
    
    for i in range(TAM_REJILLA):
        for j in range(TAM_REJILLA):
            if rejilla[i, j] == 0:  # Estado Sano (S)
                # Comprobamos la infección
                vecinos_infectados = sum(1 for v in vecinos(i, j) if v == 1)
                if vecinos_infectados > 0 and random.random() < p_infeccion:
                    nueva_rejilla[i, j] = 1  # Se infecta
            elif rejilla[i, j] == 1:  # Estado Infectado (I)
                # Comprobamos la recuperación
                if random.random() < p_recuperacion:
                    nueva_rejilla[i, j] = 2  # Se recupera
    rejilla = nueva_rejilla

# Simulación
for ciclo in range(CICLOS):
    actualizar_rejilla()

# Visualización de resultados
print("Simulación finalizada.")
print(rejilla)
```
Resultado del código:
```python
Simulación finalizada.
[[1. 1. 1. 1. 2. 1. 2. 2. 2. 2.]
 [1. 2. 2. 2. 2. 1. 2. 2. 1. 2.]
 [1. 2. 1. 1. 2. 2. 2. 1. 2. 2.]
 [2. 1. 2. 1. 2. 2. 1. 2. 2. 2.]
 [2. 1. 2. 2. 1. 1. 1. 2. 1. 1.]
 [0. 2. 1. 1. 1. 2. 1. 2. 2. 1.]
 [2. 1. 1. 2. 1. 2. 2. 2. 1. 1.]
 [1. 1. 2. 1. 1. 1. 1. 1. 2. 2.]
 [1. 1. 2. 2. 2. 1. 1. 1. 1. 2.]
 [1. 2. 1. 1. 2. 2. 1. 1. 1. 2.]]
```
### 3. Tome el plano de una ciudad pequeña y localice, por ejemplo, las droguerías, o colegios ¿es posible que falte alguno en la ciudad? Utilice diagramas de Voronoi.
