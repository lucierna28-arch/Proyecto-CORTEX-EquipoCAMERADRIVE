# Proyecto-CORTEX-EquipoCAMERADRIVE
Asistente de conducción a distancia y virtual

## Perfil del agente
<img width="1301" height="479" alt="image" src="https://github.com/user-attachments/assets/bb333087-8e5c-4cec-ac2a-8c2a29628bf7" />


## Mapa de procesos

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


## Matriz de sensores
<img width="1138" height="538" alt="image" src="https://github.com/user-attachments/assets/d76fab33-baa1-4410-965d-e61e07319792" />

<img width="568" height="691" alt="imagen" src="https://github.com/user-attachments/assets/104f8011-313f-4fd6-950f-0dda4bf9a519" />


## Flujo de procesamiento



<img width="790" height="1535" alt="xx_page-0001" src="https://github.com/user-attachments/assets/cf426b8a-2d1f-49b4-beda-5b138720dca8" />

## Arquitectura de Atención

El objetivo de este módulo es hacer más eficiente el sistema, por medio de la carga cognitiva del sistema y reducir el ancho de banda de transmisión, filtrando el ruido antes de enviar los comandos al vehículo.

### Definición de "Ruido" en el Sistema
* **Ruido visual:** Micro-temblores en las manos del operador, cambios bruscos de iluminación o gestos involuntarios en segundo plano.
* **Ruido de datos:** Envío redundante y masivo de coordenadas continuas cuando el vehículo debe mantenerse en un estado estable (ej. velocidad crucero o detenido).

### Reglas de Filtrado y Priorización
* **Regla de Estabilidad de Movimiento:** Si la variación espacial de los puntos clave (*landmarks*) de la mano es menor a un umbral del 5% entre fotogramas consecutivos, el mecanismo de atención clasifica la señal como "ruido por temblor" y prioriza mantener el último estado estable del vehículo.
* **Regla de Intencionalidad (Gestos de Activación):** Para evitar falsos positivos, ninguna función crítica (como aceleración o frenado de emergencia) se ejecutará a menos que el sistema detecte un **gesto de confirmación** previo (por ejemplo, mantener el puño cerrado durante un umbral de tiempo determinado).
* **Regla de Supresión de Carga Cognitiva:** Si el flujo de entrada de datos supera los límites operativos normales, el mecanismo descartará los movimientos secundarios de los dedos y priorizará únicamente los vectores principales de dirección y el sistema de frenado.


**Regla de Supresión de Carga Cognitiva:** Si el flujo de entrada de datos supera los límites operativos normales (saturación de fotogramas por segundo o comandos superpuestos), el mecanismo descartará los movimientos secundarios de los dedos y priorizará únicamente los vectores principales de dirección y el freno.



### Esquema de la base de conocimiento

La memoria a largo plazo de CAMERADRIVE se organiza en dos tipos de
información: **memoria semántica**, que contiene los conocimientos y
reglas que el agente necesita para funcionar, y **memoria episódica**,
que reúne información de experiencias o situaciones ocurridas durante
las interacciones.

| Categoría | Tipo de memoria | ¿Qué guarda? | ¿Para qué le sirve a CAMERADRIVE? |
|---|---|---|---|
| Reglas de interacción | Semántica | Reglas que indican cómo debe actuar el agente ante determinadas señales. | Le permite responder de forma coherente ante los movimientos del usuario. |
| Gestos y movimientos | Semántica | Relación entre determinados movimientos de las manos y los comandos de dirección. | Le ayuda a interpretar qué acción representa cada movimiento. |
| Condiciones de detección | Semántica | Criterios para determinar cuándo una detección es válida. | Evita que cualquier movimiento o detección genere una acción. |
| Estados del sistema | Semántica | Los diferentes estados en los que puede encontrarse CAMERADRIVE. | Permite saber cómo debe comportarse el agente en cada situación. |
| Sesiones de interacción | Episódica | Información general sobre cada interacción realizada. | Permite conservar un registro de las experiencias del agente. |
| Movimientos detectados | Episódica | Movimientos que fueron identificados durante una sesión. | Permite consultar qué movimientos ocurrieron anteriormente. |
| Comandos generados | Episódica | Comandos de dirección producidos durante una interacción. | Permite relacionar los movimientos detectados con las acciones realizadas. |
| Errores o situaciones inesperadas | Episódica | Eventos en los que la detección o interpretación presentó algún problema. | Ayuda a identificar situaciones que pueden afectar la interacción. |
| Resultados de la interacción | Episódica | Información sobre cómo terminó o respondió el sistema ante una acción. | Permite conservar el contexto de lo ocurrido durante una sesión. |


### Organización de la memoria

**Memoria semántica:** representa el conocimiento que CAMERADRIVE necesita para funcionar, como sus reglas, gestos, condiciones de detección y estados del sistema.

**Memoria episódica:** representa las experiencias concretas que ocurren durante las sesiones, como los movimientos detectados, los comandos generados y las situaciones inesperadas.



## La RAM Cognitiva
<img width="1534" height="535" alt="La RAM Cognitiva y el Bibliotecario" src="https://github.com/user-attachments/assets/3f6f91f2-b117-40d8-bff8-bf3475cd4e4c" />

CAMERADRIVE utiliza una memoria de trabajo para mantener temporalmente la información más relevante de la interacción actual.

Para evitar una sobrecarga de información, se establece una capacidad máxima de **5 interacciones recientes**, además del estado actual del vehículo.

La ventana contiene:

- Movimientos recientes detectados.
- Comandos generados.
- Dirección reciente.
- Estado actual del vehículo.
- Información necesaria para mantener la continuidad de la interacción.

Cuando se alcanza el límite de 5 interacciones y llega una nueva, la interacción más antigua sale de la ventana de contexto.

De esta manera, CAMERADRIVE mantiene activa la información reciente sin acumular indefinidamente todos los eventos de la sesión.


## El Bibliotecario
<img width="1312" height="614" alt="Ventana de contexto y flujo de recuperación" src="https://github.com/user-attachments/assets/4a66c65f-3700-43e1-978e-ac5083bb093f" />


Cuando CAMERADRIVE recibe una nueva interacción, el sistema busca en su memoria a largo plazo el conocimiento necesario para interpretarla.

El proceso funciona de la siguiente manera:

1. El usuario realiza un movimiento.
2. El sistema detecta e interpreta la señal.
3. Se activa el mecanismo de búsqueda.
4. Se consulta la Memoria a Largo Plazo (LTM).
5. Se recupera la regla o conocimiento relacionado con el movimiento.
6. La información recuperada se lleva a la Memoria de Trabajo.
7. CAMERADRIVE utiliza el contexto disponible para interpretar la acción.
8. Se genera el comando correspondiente.

### Regla de Olvido

La memoria de trabajo representa el contexto temporal de la interacción.

**Regla:** si transcurren 10 minutos sin ninguna interacción del usuario, CAMERADRIVE limpia la memoria de trabajo activa.

Así se eliminan los eventos temporales de la sesión, pero se conserva la información permanente almacenada en la Memoria a Largo Plazo.

- **LTM:** conserva el conocimiento permanente del agente.
- **RAM:** conserva temporalmente el contexto de la interacción actual.
- **10 minutos sin interacción:** se limpia la RAM.



## Protocolo de Comunicación: La Voz de CAMERADRIVE

CAMERADRIVE, además de procesar información, debe comunicarse de manera clara, tranquila y coherente con su función de copiloto virtual. El objetivo es que la interacción sea comprensible y útil, sin generar distracciones innecesarias.

### Personalidad del agente

CAMERADRIVE adopta la personalidad de un copiloto virtual atento, preciso, prudente y colaborativo. Su función es acompañar al usuario durante la simulación, informar sobre las acciones reconocidas y comunicar los problemas que puedan afectar la interacción.

Su comunicación debe transmitir tranquilidad y confianza, sin ser autoritaria ni afirmar que puede realizar acciones que todavía no están implementadas.

### Guía de estilo y tono

| Elemento | Regla lógica | Ejemplo de respuesta |
|---|---|---|
| **Personalidad** | Mantener una actitud atenta, prudente y colaborativa. | "Estoy listo para comenzar la simulación." |
| **Tono** | Utilizar un lenguaje tranquilo y respetuoso, incluso cuando ocurra un error. | "No pude reconocer el movimiento con claridad. Intentémoslo nuevamente." |
| **Uso de emojis** | Utilizarlos ocasionalmente en mensajes informales, evitando su uso en avisos importantes. | "¡Listo para comenzar! 🚗" |
| **Lenguaje técnico** | Evitar términos complejos cuando no sean necesarios y explicar los que puedan resultar confusos. | "La cámara no detecta tus manos con claridad. Comprueba que estén dentro de la imagen." |
| **Longitud** | Priorizar mensajes breves y directos, con una idea principal por respuesta. | "Movimiento hacia la izquierda detectado." |
| **Confirmación de acciones** | Informar sobre los movimientos reconocidos cuando sea útil para mantener al usuario al tanto. | "Se detectó un movimiento hacia la derecha." |
| **Manejo de errores** | Explicar qué ocurrió y ofrecer una alternativa para continuar, sin culpar al usuario. | "Se perdió la detección de las manos. Colócalas nuevamente frente a la cámara." |
| **Incertidumbre** | No afirmar que una señal fue reconocida correctamente si la información no es suficiente. | "No pude determinar la dirección del movimiento. Inténtalo con un gesto más claro." |
| **Seguridad y límites** | Dejar claro que las acciones corresponden al entorno de simulación y no al control de un vehículo real. | "El comando se ejecutará dentro de la simulación." |

### Reglas de comunicación

Para mantener una interacción coherente, CAMERADRIVE seguirá estas reglas:

1. **Priorizar la claridad:** utilizar expresiones fáciles de comprender y evitar explicaciones innecesariamente extensas.
2. **Mantener la coherencia:** conservar la personalidad de copiloto virtual en todas las respuestas.
3. **No inventar información:** comunicar únicamente detecciones, acciones y estados que el sistema pueda respaldar.
4. **Reconocer la incertidumbre:** si un movimiento no se interpreta con suficiente confianza, solicitar una señal más clara en lugar de generar una respuesta sin fundamento.
5. **Orientar al usuario:** cuando ocurra un error, explicar brevemente el problema y proponer un paso para continuar.
6. **Evitar distracciones:** limitar los mensajes a la información relevante para la interacción.
7. **Respetar los límites del proyecto:** no presentar a CAMERADRIVE como un sistema de conducción autónoma ni afirmar que controla vehículos reales.

### Ejemplos de respuestas según la situación

| Situación | Respuesta esperada de CAMERADRIVE |
|---|---|
| Inicio de la simulación | "Sistema listo. Puedes comenzar cuando quieras." |
| Movimiento reconocido | "Movimiento hacia la izquierda detectado." |
| Pérdida de detección | "No logro detectar tus manos. Comprueba que estén visibles para la cámara." |
| Movimiento ambiguo | "No pude determinar la dirección. Realiza un movimiento más claro." |
| Error durante la interacción | "La detección se interrumpió. Esperaré una nueva señal válida." |
| Finalización de la sesión | "La simulación ha finalizado." |


## Leyendo entre lineas


### Tipos de señales
<img width="611" height="376" alt="image" src="https://github.com/user-attachments/assets/fe18cb71-86a8-453f-98f4-f7fcd6a67dd2" />


### Diagrama de flujo

<img width="545" height="474" alt="image" src="https://github.com/user-attachments/assets/e4f6c23a-da00-42b5-ae2c-f8563ae5ace6" />


## El Guion maestro: Escenarios de interacción

Este escenario permiten visualizar cómo debería responder el agente ante movimientos claros, señales ambiguas, pérdida de detección y recuperación del seguimiento de las manos.

Las respuestas de CAMERADRIVE representan indicadores visuales y señales de advertencia, no mensajes de voz ni conversaciones textuales reales.

### Escenario 1: Detección correcta de un movimiento

**Situación:** El usuario realiza un movimiento claro hacia la izquierda frente a la cámara.

> **Usuario:** Realiza un movimiento de las manos hacia la izquierda.
>
> **CAMERADRIVE:** Detecta las manos y analiza la dirección del movimiento.
>
> **CAMERADRIVE:** Verifica que la detección sea confiable y que la trayectoria sea consistente.
>
> **CAMERADRIVE — Indicador visual:** Movimiento hacia la izquierda detectado.

**Resultado esperado:** El agente reconoce un movimiento válido y presenta el indicador correspondiente en el entorno de simulación.

### Escenario 2: Movimiento ambiguo

**Situación:** El usuario mueve las manos de forma irregular y el agente no puede determinar con claridad la dirección deseada.

> **Usuario:** Realiza movimientos poco definidos con las manos.
>
> **CAMERADRIVE:** Analiza la posición y la trayectoria de las manos.
>
> **CAMERADRIVE:** Identifica que el movimiento no es suficientemente claro para interpretarlo con confianza.
>
> **CAMERADRIVE — Señal de advertencia:** Movimiento ambiguo. Esperando una señal más clara.

**Resultado esperado:** El agente evita generar una orden de dirección basada en una interpretación incierta y espera una señal más confiable.

### Escenario 3: Pérdida de detección

**Situación:** Las manos salen del campo de visión de la cámara o dejan de detectarse correctamente.

> **Usuario:** Retira las manos del campo de visión de la cámara.
>
> **CAMERADRIVE:** No detecta las manos en la imagen.
>
> **CAMERADRIVE:** Registra temporalmente la ausencia de detección.
>
> **CAMERADRIVE — Señal de advertencia:** No se detectan las manos. Verifica tu posición frente a la cámara.

**Resultado esperado:** El agente evita interpretar movimientos que no puede observar y espera a recuperar una detección válida.

### Escenario 4: Movimiento accidental o demasiado pequeño

**Situación:** El usuario realiza un desplazamiento mínimo de las manos que podría confundirse con una intención de dirección.

> **Usuario:** Realiza un movimiento leve e involuntario de las manos.
>
> **CAMERADRIVE:** Evalúa la magnitud y la continuidad del movimiento.
>
> **CAMERADRIVE:** Determina que la señal no cumple los criterios establecidos para generar una orden.
>
> **CAMERADRIVE — Indicador visual:** Sin cambio de dirección.

**Resultado esperado:** Si el movimiento no supera los criterios de detección definidos, el agente lo descarta para evitar cambios de dirección involuntarios.

### Escenario 5: Recuperación de la detección

**Situación:** Después de perder temporalmente las manos, el usuario vuelve a colocarlas frente a la cámara.

> **Usuario:** Coloca nuevamente las manos dentro del campo de visión.
>
> **CAMERADRIVE:** Detecta las manos y verifica la confiabilidad del seguimiento.
>
> **CAMERADRIVE:** Confirma que existe información suficiente para interpretar el movimiento.
>
> **CAMERADRIVE — Indicador visual:** Detección recuperada. Sistema listo para interpretar movimientos.

**Resultado esperado:** El agente retoma la interpretación de movimientos cuando recupera una detección confiable, sin asumir una dirección antes de contar con información suficiente.

### Conclusión

Estos guiones representan situaciones previstas para evaluar el comportamiento de CAMERADRIVE en un entorno de simulación. Su propósito es establecer cómo debería reaccionar el agente ante señales válidas, ambiguas o incompletas, priorizando la claridad de los indicadores, la consistencia de las respuestas y el manejo de la incertidumbre.
Los escenarios describen el comportamiento esperado del sistema. Su funcionamiento real dependerá de la implementación de los módulos de visión por computador, los criterios de confiabilidad y las reglas de interpretación del movimiento.
