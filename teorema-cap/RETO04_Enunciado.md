RETO 04 - Simulación de Sistema distribuido. - RA2
Requisitos de finalización
Apertura: miércoles, 29 de enero de 2025, 13:41
Cierre: martes, 3 de febrero de 2026, 23:41

Enunciado.

RETO 04 — CAP, Consistencia y Quórum (en Colab) - Simulación Sistema Distribuido 

Objetivo: Resolver los ejercicios del apartado 9) Ejercicios al final del Colab sobre CAP, consistencia y quórum, realizando simulaciones y explicando resultados. Todo debe resolverse dentro del mismo Colab (código + respuestas razonadas).

Entorno obligatorio:

El mismo Colab proporcionado por el profesor (no crear otro archivo).
Las respuestas deben quedar en celdas de texto (Markdown) y las simulaciones en celdas de código.
9) Ejercicios (a resolver en el Colab)
Ejercicio 1 — Mayoría con N=5

Cambia N=5 y particiona el sistema en dos grupos: {0,1} / {2,3,4}.
Responde: ¿Dónde está la mayoría?
Justifica: explica qué significa “mayoría” y cuántos nodos necesita.
Ejercicio 2 — Quórum con W=3, R=3 (N=5) durante partición

Con N=5, prueba W=3 y R=3.
Simula una partición {0,1} / {2,3,4}.
Responde: ¿qué pasa durante la partición?
Indica claramente si las lecturas y/o escrituras pueden completarse en cada lado y por qué (por quórum).
Ejercicio 3 — Dos escrituras concurrentes (LWW)

Simula dos escrituras concurrentes en lados distintos de la partición (por ejemplo, cada lado escribe un valor diferente para la misma clave).
Responde: ¿cómo “gana” una (LWW — Last-Write-Wins)?
Justifica: explica con qué criterio se decide el valor final (timestamp/versión/reloj lógico según implementación del Colab).
Ejercicio 4 — Discusión CP vs AP (casos Big Data)

Discute (en 8–15 líneas): ¿dónde prefieres CP (config) y dónde AP (métricas/logs)?
Incluye al menos 2 ejemplos reales (p.ej. configuración de clúster vs pipeline de logs/observabilidad).
Explica el impacto: consistencia, disponibilidad y consecuencias de equivocarse.
Requisito clave: Las respuestas deben ser reproducibles: incluye la salida relevante (prints/tablas/gráficas) y un texto breve interpretando el resultado.

Criterios de puntuación. Total 10 puntos.

1) Ejercicio 1 — Mayoría con N=5 (2.0 pts)

Configura correctamente N=5 y partición {0,1}/{2,3,4} (0.5)
Identifica correctamente la mayoría (0.5)
Justifica con definición clara (mayoría = ⌊N/2⌋+1) (1.0)
2) Ejercicio 2 — Quórum W=3, R=3 en partición (3.0 pts)

Configura N=5, W=3, R=3 (0.5)
Simula partición y prueba operaciones (1.5)
Conclusión correcta sobre disponibilidad de lectura/escritura en cada lado (1.0)
3) Ejercicio 3 — Escrituras concurrentes + LWW (3.0 pts)

Simula dos escrituras concurrentes en lados distintos (1.0)
Explica correctamente el mecanismo de resolución (LWW) (1.5)
Aporta evidencia del valor final tras reconciliación/curación de partición (0.5)
4) Ejercicio 4 — Discusión CP vs AP (2.0 pts)

Argumenta dónde aplicar CP vs AP con claridad (1.0)
Incluye ejemplos Big Data y consecuencias prácticas (1.0)
Nota: Se valora especialmente que conectes los resultados del Colab con decisiones reales (configuración, logs, métricas, data pipelines).

Recursos necesarios para realizar la Tarea.

Colab proporcionado por el profesor (mismo archivo).
Conexión a Internet (para ejecutar Colab).
Conocimientos básicos: CAP, quórum (N/R/W), particiones.
Consejos y recomendaciones.

Deja visible el valor final de N, R y W en cada ejercicio (por ejemplo, con un print()).
Cuando simules partición, anota explícitamente qué nodos quedan a cada lado.
En el ejercicio de LWW, muestra el “antes/después” (valor por lado y valor final tras reconciliación).
En la discusión CP/AP, piensa en: ¿qué pasa si leo un valor “viejo”? ¿qué pasa si el sistema “no responde”?
Evita respuestas genéricas: conecta CP/AP con casos Big Data reales (configuración de clúster, feature flags, logs, métricas, monitoring).
Indicaciones de entrega.

Entrega lo indicado por el profesor. Recomendación estándar:

Enlace al Colab (con permisos correctos) en COMENTARIOS
El Colab debe incluir:
Código ejecutable para cada ejercicio (celdas de código).
Respuestas razonadas en Markdown justo debajo (celdas de texto).
Evidencias (salida de celdas, prints, tablas o logs del simulador).
Estructura recomendada dentro del Colab:

Sección 9.1: N=5 y mayoría.
Sección 9.2: Quórum W=3, R=3 durante partición.
Sección 9.3: Dos escrituras concurrentes + LWW.
Sección 9.4: Discusión CP vs AP.