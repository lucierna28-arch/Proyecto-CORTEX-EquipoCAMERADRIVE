# Proyecto-CORTEX-EquipoCAMERADRIVE
Asistente de conducción a distancia y virtual

##Perfil del agente
<img width="1470" height="487" alt="image" src="https://github.com/user-attachments/assets/bd21d679-0775-48be-b0e8-45da744f06d6" />


##Mapa de procesos

<img width="493" height="593" alt="Captura de pantalla 2026-08-20 103902" src="https://github.com/user-attachments/assets/581c4551-8bae-4b8e-9ec9-375bdf953da5" />

Atención: 10/10 
La atención es el proceso cognitivo de mayor importancia para la IA CAMERADRIVE, pues su principal fuente de información es una cámara. Debe poder identificar cuáles elementos que detecta son relevantes y cuáles deben ser ignorados. Su atención debe concentrarse principalmente en las manos del usuario y en los movimientos representen una intención de dirección.

La atención ayuda a:
Detectar las manos del usuario.
Filtrar información visual irrelevante.
Ignorar objetos y movimientos del entorno.
Priorizar los movimientos relevantes.
Reducir el efecto del ruido visual.
Mantener el seguimiento de las manos.

Memoria: 8/10

CAMERADRIVE requiere memoria principalmente para conservar información temporal relacionada con el movimiento. Una sola imagen no es suficiente para determinar la intención del usuario. Se necesita poder comparar diferentes momentos para identificar patrones, tendencias, trayectorias, cambios de dirección y continuidad. Su nivel debe ser muy alto, pero no máximo, porque no se requiere una memoria tan amplia como la de una persona.

La memoria puede conservar:
Posición anterior de las manos.
Posición actual.
Trayectoria.
Dirección anterior.
Estado anterior del sistema.
Última detección válida.
Nivel de confianza de detecciones anteriores.

Lenguaje: 3/10

El lenguaje tiene una importancia secundaria para CAMERADRIVE. La interacción principal es a través de movimientos de las manos, no de interacciones verbales.

El lenguaje podría utilizarse para:
Mostrar instrucciones.
Comunicar errores.
Presentar alertas.
Confirmar determinadas acciones.
Proporcionar retroalimentación al usuario.

Emoción: 2/10

CAMERADRIVE no necesita reproducir emociones humanas para cumplir su función principal. Sin embargo, puede utilizar estados internos funcionales que permitan modificar su comportamiento según las condiciones del sistema.

