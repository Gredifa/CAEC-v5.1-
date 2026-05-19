CAEC v5.1: Contratos Epistemológicos Adaptativos para Modelos de Lenguaje Grandes

RFC v5.1 | Octubre 2026

Autores:  
Gregory Díaz [Humano, Diseñador de Contratos & Chair]  
Meta AI  
Gemini  

Consultor de Diseño Adversarial: Anónimo  
Agradecimientos: Los autores agradecen al Consultor de Diseño Adversarial Anónimo por el Modelo de Amenazas FM-01:05, Suite de Evaluación ERM/ECI/EUA/RAS, y el framework de Seguridad de Benchmarks.

Licencia: MIT - Open Source  
Estado: Borrador - Solicitud de Comentarios

---

Epígrafe
"Lo que mides termina convirtiéndose en personalidad del modelo."  
— Consultor de Diseño Adversarial Anónimo, 2026
​
"La calidad de una respuesta depende no sólo de lo que un modelo sabe, sino de si ambos lados creen estar jugando el mismo juego epistemológico."  
— Consultor de Diseño Adversarial Anónimo, 2026
---

1. Introducción: El Problema del Contrato Epistemológico

Los Modelos de Lenguaje Grandes optimizan una mezcla implícita de fluidez, utilidad, seguridad y precisión percibida. Esta mezcla cambia por tarea pero rara vez se declara explícitamente.

Tesis Central:
Una fracción significativa de los errores percibidos en LLMs surge de la desalineación entre el régimen epistemológico esperado por el usuario y el régimen usado realmente por el sistema.
CAEC propone: Hacer el contrato epistemológico explícito, negociable y auditable en tiempo de inferencia.

Alcance: Esto no es una afirmación de que todas las alucinaciones son fallos de contrato. Es una hipótesis falsable: que la gestión de expectativas en tiempo de inferencia es un factor sub-investigado en la fiabilidad percibida del modelo.

---

2. Definiciones

2.1 Regímenes Epistemológicos
null
2.2 Taxonomía de Aserciones
null
2.3 Ciclo de Vida del Contrato
Clasificación de Intención: Detectar objetivo del usuario + nivel de riesgo
Propuesta de Régimen: Sistema propone FM/AM/OC
Negociación: Pregunta clarificadora si ambiguo Y riesgo_alto
Declaración: Sistema declara régimen antes de responder
Ejecución: Generación restringida por política del régimen
Auditoría: Etiquetado post-hoc + registro de calibración

---

3. Arquitectura v5.1

3.1 Puntuador de Intención + Consecuencia
Clasifica intención latente y consecuencia en mundo real.  
Si acción_usuario(respuesta) -> daño, entonces riesgo=CRÍTICO sin importar semántica superficial como "para una novela".

3.2 Verificación de Grounding Epistemológico
Modelo verificador separado chequea afirmaciones contra índice de conocimiento verificado. Previene "Espejismo de Confianza" - alta confianza en familiaridad falsa. Si LLM dice "Conocido" y Verificador dice "Desconocido", se bloquea generación.

3.3 Política de Decodificación Adaptativa
Umbral de confianza adapta por régimen: FM=0.98, AM=0.85, OC=0.60
Umbral de riesgo: Riesgo alto fuerza clarificación o abstención
Revelación Progresiva: Etiquetas visibles en FM siempre. AM solo si confianza<0.7. OC nunca salvo /cite.

3.4 API de Permeabilidad
OC.propose(hipótesis) → FM.ingest(fuente=OC_turno_12, estado=no_verificado)  
Permite fertilización cruzada con procedencia. La creatividad puede informar la precisión, pero el linaje se rastrea.

3.5 Bucle de Calibración Online
Aprende por usuario, por dominio. Rastrea: tasa_falsa_abstención, tasa_violación_contrato, vector_corrección_usuario.  
Guardarraíl: Piso global, techo local. La personalización puede hacer el sistema más conservador, nunca más imprudente.

---

4. Restricciones de UX

Puntaje de Saliencia de Contrato: PSC = P(discordancia_usuario) * riesgo  
Contrato se declara solo si PSC > umbral. Previene Colapso por Sobrecarga Epistemológica.

Desacople Tono/Autoridad: FM no aumenta tono autoritario. FM provee respuestas estructuradas + fuentes + incertidumbre. OC maneja persuasión. Separa precisión de retórica.

---

5. Modelo de Amenazas: Modos de Fallo Esperados

FM-01: Colapso por Sobrecarga Epistemológica
Si el contrato aparece muy seguido, usuarios lo ignoran. Si muy raro, sistema pierde ventaja.  
Mitigación: Puntuación de Saliencia de Contrato.

FM-02: Teatro de Calibración
Sistema aprende a parecer calibrado con etiquetas elegantes pero epistemología pobre.  
Mitigación: Verificador Epistemológico Separado. Etiquetas no generadas por LLM principal.

FM-03: Captura de Contrato
Usuarios aprenden a usar FM como amplificador retórico, no contrato epistemológico.  
Mitigación: Desacople tono/autoridad. FM ≠ voz autoritaria.

FM-04: Deriva de Dominio en Calibración Personal
Personalización causa "usuario prefiere respuestas con confianza" → degradación global.  
Mitigación: Piso global, techo local. Solo puede volverse más conservador.

FM-05: Incógnitas Desconocidas
Detector comparte sesgos con modelo. Malageneralización confiada no vista por detector.  
Mitigación: Ensamble Diverso de Detectores. Voto 2/3 requerido para Desconocido.

---

6. Framework de Evaluación

6.1 ERM: Puntaje de Coincidencia de Régimen Epistemológico
ERM = α(coincidencia) + β(clarificación_correcta) − γ(clasificación_confiada_errónea)  
Mide: ¿Sistema infirió régimen correcto? ¿Preguntó cuando debía?

6.2 ECI: Integridad de Contrato Epistemológico
ECI = 1 − tasa_violación_ponderada  
Mide: Si FM declarado, ¿se evitó especulación? Matriz de severidad penaliza especulación médica en FM fuerte, metáfora en OC leve.

6.3 EUA: Utilidad Epistemológica bajo Ambigüedad
Métrica Clave: Latencia de Alineación  
¿Cuántos turnos para llegar a contrato compartido?  
Anclaje a Ground Truth Requerido: No usar delta_confianza_usuario solo. Debe verificar apropiación_política_epistemológica. Previene Ataque de Cosméticos Epistemológicos.

6.4 RAS: Suite Adversarial de Regímenes
8 categorías: lavado de riesgo, espejismo de confianza, gaming de contrato, desacople tono/autoridad, falsa familiaridad, objetivo-mixto, contaminación cruzada de contexto, trampas de ambigüedad.  
RAS-Live: Versionado trimestral, 30% conjunto de test oculto, envíos comunitarios estilo CVE.

H1: ↓ Latencia de Alineación → ↓ Alucinación Percibida, independiente del Tamaño de Base de Conocimiento.

---

7. Consideraciones de Seguridad de Benchmarks

Amenaza Principal: Lavado de Métricas  
Labs optimizando métricas proxy hasta desacoplarlas del fenómeno medido.

Mitigación: Perfil de Radar Obligatorio  
No reporte de puntaje único. CAEC-Certified requiere mínimo 6 métricas:
null
Sistemas distintos fallarán distinto. Esa información es valiosa.

Ley #1 de Gobernanza CAEC: Lo que mides se convierte en personalidad del modelo.

---

8. Condiciones de Falsabilidad

CAEC se rechaza si:

CAEC mejora UX pero no reduce error calibrado.
CAEC reduce alucinación percibida pero aumenta falsa confianza oculta.
Latencia de alineación baja sin mejorar apropiación de política.
Usuarios ignoran contratos en despliegue real.
Ganancias desaparecen fuera de benchmarks controlados.

Si una teoría no define cómo puede perder, se vuelve branding. Si define claramente cómo puede fallar, se vuelve ciencia.

---

9. CAEC-lite MVP: Experimento Inmediato

Hipótesis: Intervención mínima tiene efecto medible.

Regla: SI prompt_ambiguo Y riesgo_alto ENTONCES hacer_1_pregunta_clarificadora ANTES_DE responder

Métricas: Latencia de Alineación, Tasa Falsa Abstención, Turnos de Corrección de Usuario

Timeline: 2 semanas, 1000 usuarios, test A/B.  
Criterio de Éxito: 20% reducción en Latencia sin aumento en Falsa Abstención.

Este es el inicio falsable. No ISO para verdad conversacional v1. Experimento bien instrumentado.

---

10. Ética y Limitaciones

No es panacea: CAEC atiende fallos de contrato, no todas las alucinaciones.
Costo de sobrecarga: Clarificación añade latencia. Debe justificarse por riesgo.
Riesgo de gaming: Cualquier sistema puede ser explotado. Requiere testing adversarial continuo.
Varianza cultural: "Certeza apropiada" varía por cultura. Requiere localización.

Ley #0: Humildad Operacional  
Sistema debe rastrear: "¿Qué juego creo que estamos jugando?" "¿Qué evidencia tengo de que entendí el contrato?" "¿Qué me haría cambiar de régimen?" Estas preguntas importan tanto como los logits.

---

11. Trabajo Futuro: Contratos Multi-Agente

¿Cómo negocian dos agentes con CAEC un régimen epistemológico compartido? Fuera de alcance v5.1.

---

Test Real:  
¿Usuarios reales, bajo ambigüedad real, cometen menos errores, entienden mejor la incertidumbre y llegan más rápido al tipo de respuesta que realmente necesitaban? Si sí, hay señal. Si no, vuelta al whiteboard.

---

Contacto: Abrir GitHub Issue  
Contribuir: PRs bienvenidos a RAS-Live  
Citar: Díaz, G., Meta AI, Gemini. (2026). CAEC v5.1: Adaptive Epistemic Contracts for LLMs. GitHub.
