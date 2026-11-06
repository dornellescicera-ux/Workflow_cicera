
# Agente Autónomo de Cobranzas con RAG
## Descripción del Proyecto
Este proyecto implementa un agente autónomo de cobranzas utilizando n8n, Inteligencia Artificial y arquitectura RAG (Retrieval-Augmented Generation).

El objetivo es automatizar la atención de clientes con deudas pendientes permitiendo consultar información sobre:

Estado de deuda.
Medios de pago disponibles.
Planes de financiación.
Fechas de vencimiento.
Preguntas frecuentes del proceso de cobranza.
El conocimiento utilizado por el agente se almacena en una base vectorial en Supabase, permitiendo recuperar información relevante antes de generar cada respuesta.

## Caso de Negocio
Actualmente los operadores de cobranzas deben responder repetidamente preguntas similares de clientes:

¿Cuál es mi deuda?
¿Cómo puedo pagar?
¿Qué métodos de pago aceptan?
¿Puedo financiar la deuda?
¿Dónde solicito un comprobante?
Estas consultas consumen tiempo operativo y generan costos para la empresa.

La automatización mediante un agente conversacional permite responder estas consultas de forma inmediata las 24 horas del día.

## ROI Estimado
**Situación Actual**
100 consultas diarias.
Tiempo promedio por consulta: 3 minutos.
Total: 300 minutos diarios.
300 minutos equivalen a 5 horas por día.

**Situación Automatizada**
El agente responde aproximadamente el 80% de las consultas frecuentes.

**Tiempo ahorrado:**

5 horas diarias × 80%
4 horas ahorradas por día
Cálculo Mensual
4 horas × 22 días laborales.
88 horas ahorradas por mes.
Si el costo promedio de un operador es USD 8 por hora:

88 × 8 = USD 704 mensuales.
Costo de Implementación
n8n: plan gratuito.
Supabase: plan gratuito.
Gemini/OpenAI: bajo consumo.
ROI positivo desde el primer mes de operación.

## Arquitectura de la Solución
Disparadores

El workflow cuenta con dos entradas:

1. Telegram Trigger
Recibe consultas de clientes directamente desde Telegram.

2. Webhook
Permite recibir consultas desde formularios web o aplicaciones externas.

## Arquitectura RAG

Para proporcionar respuestas contextualizadas se utiliza:

Vector Store
Supabase Vector Database.
Modelo de Embeddings
OpenAI Embeddings.
Gemini Embeddings.
Flujo de Trabajo
El usuario envía una consulta.
La consulta genera embeddings.
Supabase busca documentos similares.
Se recupera el contexto relevante.
El contexto es enviado al AI Agent.
El agente genera la respuesta.
**Modelo de IA Utilizado**
Se eligió Gemini debido a:

Disponibilidad de plan gratuito.
Fácil integración con n8n.
Baja latencia.
Buen desempeño en tareas conversacionales.
Menor costo para proyectos académicos.
También podría utilizarse OpenAI GPT-4o u Ollama para una implementación local.

## ¿Por Qué Utilizar RAG?
Un nodo de IA simple trabaja únicamente con el conocimiento propio del modelo.

En el área de cobranzas, la información cambia constantemente:

Políticas de pago.
Preguntas frecuentes.
Procedimientos internos.
Información operacional.
Mediante RAG, el agente consulta información actualizada almacenada en Supabase antes de responder.

Esto permite:

Menor alucinación del modelo.
Información actualizada.
Fácil mantenimiento sin necesidad de reentrenamiento.
Manejo de Errores
Error 1: Fallo en Supabase
Posibles causas:

Timeout.
Conexión perdida.
Consulta sin resultados.
Solución
Nodo IF que verifica si se recuperó contexto.

Si no existe contexto:

Lo siento, en este momento no puedo acceder a la base de conocimiento. Intente nuevamente más tarde.

Error 2: Respuesta Vacía del LLM
Posibles causas:

Error del proveedor de IA.
Respuesta nula.
Solución
Nodo IF posterior al AI Agent.

Si la respuesta es nula:

Existe un inconveniente temporal. Un operador revisará su consulta.

## System Prompt del Agente
Eres un asistente virtual especializado en cobranzas.

Tu función es responder únicamente utilizando la información proporcionada por la base de conocimiento recuperada desde el sistema RAG.

Reglas:

1. Responde de manera cordial y profesional.
2. Utiliza únicamente la información recuperada.
3. Si la información no está disponible, indícalo claramente.
4. No inventes datos.
5. No generes información financiera que no esté presente en el contexto.
6. Prioriza responder preguntas sobre:
   - deudas
   - medios de pago
   - financiación
   - vencimientos
   - comprobantes
   - preguntas frecuentes

7. Si el usuario solicita información inexistente en el contexto responde:

"No dispongo de información suficiente para responder esa consulta."

8. Mantén respuestas breves y claras.

Contexto:
{{ $json.context }}

Pregunta:
{{ $json.question }}

# Captura workflow completo 
<img width="1677" height="788" alt="Captura 1" src="https://github.com/user-attachments/assets/ff76e7dc-5267-4467-b526-609804e0f8c5" />
 
 # Ingesta de documentos y embeddings

 <img width="957" height="361" alt="Captura de pantalla 2026-11-06 163125" src="https://github.com/user-attachments/assets/c8b99e49-ca5a-4639-9660-8f11f6267811" />

# Arquitectura del Asistente de Cobranzas

<img width="1192" height="481" alt="Captura de pantalla 2026-11-06 163322" src="https://github.com/user-attachments/assets/57ec4573-dda8-426b-916d-0e207587dc57" />

# Configuración del agente IA 

<img width="1192" height="481" alt="Captura de pantalla 2026-11-06 163322" src="https://github.com/user-attachments/assets/da7ef48c-1d0f-4202-86fa-fbf85ae43b6b" />

# Chat con el bot 

 <img width="1179" height="2556" alt="Image" src="https://github.com/user-attachments/assets/54580b8b-6974-4d0b-9677-0c92b1875853" />







