# Proyecto-CORTEX-EquipoCAMERADRIVE
Asistente de conducción a distancia y virtual

##Perfil del agente
<img width="1301" height="479" alt="image" src="https://github.com/user-attachments/assets/bb333087-8e5c-4cec-ac2a-8c2a29628bf7" />


##Mapa de procesos

<img width="493" height="593" alt="Captura de pantalla 2026-08-20 103902" src="https://github.com/user-attachments/assets/581c4551-8bae-4b8e-9ec9-375bdf953da5" />

Atención: 10/10 

La atención es el proceso cognitivo más importante para la IA CAMERADRIVE, pues su principal fuente de información es una cámara. Debe poder identificar cuáles elementos que detecta son relevantes y cuáles deben ser ignorados. Su atención debe concentrarse principalmente en las manos del usuario y en los movimientos representen una intención de dirección.

La atención ayuda a:
Detectar las manos del usuario.
Ignorar objetos y movimientos externos.
Priorizar los movimientos relevantes.
Mantener el seguimiento de las manos.

Memoria: 8/10

CAMERADRIVE requiere memoria principalmente para conservar información temporal relacionada con el movimiento. Una sola imagen no es suficiente para determinar la intención del usuario. Se necesita poder comparar diferentes momentos para identificar patrones, tendencias, trayectorias, cambios de dirección y continuidad. Su nivel debe ser muy alto, pero no máximo, porque no se requiere una memoria tan amplia como la de una persona.

La memoria puede conservar:
Posición anterior de las manos.
Posición actual.
Trayectoria.
Dirección anterior.
Estado anterior del sistema.

Lenguaje: 3/10

El lenguaje tiene una importancia secundaria para CAMERADRIVE. La interacción principal es a través de movimientos de las manos, no de interacciones verbales.

El lenguaje podría utilizarse para:
Mostrar instrucciones.
Marcar errores.
Presentar alertas.
Confirmar acciones.
Realizar una retroalimentación al usuario.

Emoción: 2/10

CAMERADRIVE no necesita reproducir emociones humanas para cumplir su función principal. Sin embargo, puede utilizar estados internos funcionales que permitan modificar su comportamiento según las condiciones.


##Matriz de sensores
<img width="1138" height="538" alt="image" src="https://github.com/user-attachments/assets/d76fab33-baa1-4410-965d-e61e07319792" />

<img width="568" height="691" alt="imagen" src="https://github.com/user-attachments/assets/104f8011-313f-4fd6-950f-0dda4bf9a519" />


##Flujo de procesamiento



<img width="790" height="1535" alt="xx_page-0001" src="https://github.com/user-attachments/assets/cf426b8a-2d1f-49b4-beda-5b138720dca8" />

##Arquitectura de Atención

El objetivo de este módulo es optimizar la carga cognitiva del sistema y reducir el ancho de banda de transmisión, filtrando el ruido antes de enviar los comandos al vehículo.

## 2. Arquitectura de Atención (El "Gatekeeper")

El objetivo de este módulo es optimizar la carga cognitiva del sistema y reducir el ancho de banda de transmisión, filtrando el ruido antes de enviar los comandos al vehículo.

### 2.1. Definición de "Ruido" en el Sistema
* **Ruido visual:** Micro-temblores en las manos del operador, cambios bruscos de iluminación o gestos involuntarios en segundo plano.
* **Ruido de datos:** Envío redundante y masivo de coordenadas continuas cuando el vehículo debe mantenerse en un estado estable (ej. velocidad crucero o detenido).

### 2.2. Reglas de Filtrado y Priorización
* **Regla de Estabilidad de Movimiento:** Si la variación espacial de los puntos clave (*landmarks*) de la mano es menor a un umbral del 5% entre fotogramas consecutivos, el mecanismo de atención clasifica la señal como "ruido por temblor" y prioriza mantener el último estado estable del vehículo.
* **Regla de Intencionalidad (Gestos de Activación):** Para evitar falsos positivos, ninguna función crítica (como aceleración o frenado de emergencia) se ejecutará a menos que el sistema detecte un **gesto de confirmación** previo (por ejemplo, mantener el puño cerrado durante un umbral de tiempo determinado).
* **Regla de Supresión de Carga Cognitiva:** Si el flujo de entrada de datos supera los límites operativos normales, el mecanismo descartará los movimientos secundarios de los dedos y priorizará únicamente los vectores principales de dirección y el sistema de frenado.

---

### 2.3. Diagrama de Flujo del Gatekeeper

```text
[ Cámara / Visión Artificial ]
               │
               ▼
   { Captura de Fotograma }
               │
               ▼
┌──────────────────────────────┐
│  Filtro de Atención (Gate)   │ ──(¿Es Ruido / Variación < 5%?)──► [ Mantener Estado Anterior ]
└──────────────┬───────────────┘
               │ (Pasa el filtro)
               ▼
┌──────────────────────────────┐
│ Verificación de Intencionalidad│ ──(¿Falta gesto de confirmación?)──► [ Descartar Comando ]
└──────────────┬───────────────┘
               │ (Comando Válido)
               ▼
   [ Transmisión al Vehículo ]

import numpy as np

class GatekeeperFilter:
    def __init__(self, umbral_estabilidad=0.05):
        self.umbral_estabilidad = umbral_estabilidad
        self.ultimo_vector = None

    def evaluar_atencion(self, vector_actual):
        """
        Filtra el ruido evaluando la variación entre el fotograma actual y el anterior.
        """
        if self.ultimo_vector is None:
            self.ultimo_vector = vector_actual
            return "COMANDO_INICIAL"

        # Calcular la diferencia porcentual (variación espacial)
        diferencia = np.linalg.norm(np.array(vector_actual) - np.array(self.ultimo_vector))

        if diferencia < self.umbral_estabilidad:
            # Clasificado como ruido o temblor menor
            return "IGNORAR_RUIDO"
        else:
            # Movimiento válido detectado
            self.ultimo_vector = vector_actual
            return "PROCESAR_COMANDO"

# Ejemplo de uso:
# gatekeeper = GatekeeperFilter(umbral_estabilidad=0.05)
# estado = gatekeeper.evaluar_atencion([0.42, 0.51, 0.12])
# print(estado)
