#Agente Autónomo de Cobranzas con RAG#

##Descripción del Proyecto##

Este proyecto implementa un agente autónomo de cobranzas utilizando n8n, Inteligencia Artificial y arquitectura RAG (Retrieval-Augmented Generation).

**El objetivo es automatizar la atención de clientes con deudas pendientes permitiendo consultar información sobre:**

Estado de deuda.
Medios de pago disponibles.
Planes de financiación.
Fechas de vencimiento.
Preguntas frecuentes del proceso de cobranza.

El conocimiento utilizado por el agente se almacena en una base vectorial en Supabase, permitiendo recuperar información relevante antes de generar cada respuesta.

##Caso de Negocio##

**Actualmente los operadores de cobranzas deben responder repetidamente preguntas similares de clientes:**

¿Cuál es mi deuda?
¿Cómo puedo pagar?
¿Qué métodos de pago aceptan?
¿Puedo financiar la deuda?
¿Dónde solicito un comprobante?

Estas consultas consumen tiempo operativo y generan costos para la empresa.

La automatización mediante un agente conversacional permite responder estas consultas de forma inmediata las 24 horas del día.

**ROI Estimado**
Situación actual
100 consultas diarias.
Tiempo promedio por consulta: 3 minutos.
Total: 300 minutos diarios.

300 minutos = 5 horas por día.

Situación automatizada

El agente responde aproximadamente el 80% de las consultas frecuentes.

**Tiempo ahorrado:**

5 horas diarias x 80%
4 horas ahorradas por día
Cálculo mensual
4 horas × 22 días laborales
88 horas ahorradas por mes

**Si el costo promedio de un operador es USD 8/hora:**

88 × 8 = USD 704 mensuales
Costo de implementación
n8n: gratuito/self-hosted
Supabase: plan gratuito
Gemini/OpenAI: bajo consumo

**ROI positivo desde el primer mes de operación.**

##Arquitectura de la Solución##
Disparadores

El workflow cuenta con dos entradas:

1. Telegram Trigger

Recibe consultas de clientes directamente desde Telegram.

2. Webhook

Permite recibir consultas desde formularios web o aplicaciones externas.

##Arquitectura RAG##

Para proporcionar respuestas contextualizadas se utiliza:

Vector Store
Supabase Vector Database
Modelo de Embeddings
OpenAI Embeddings
o Gemini Embeddings (según configuración)
**Flujo**
El usuario envía una consulta.
La consulta genera embeddings.
Supabase busca documentos similares.
Se recupera el contexto relevante.
El contexto es enviado al AI Agent.
El agente genera la respuesta.
**Modelo de IA Utilizado**

Se eligió Gemini porque:

Tiene plan gratuito.
Fácil integración con n8n.
Baja latencia.
Buen desempeño en tareas conversacionales.
Menor costo para ambientes académicos.

También podría utilizarse OpenAI GPT-4o u Ollama para un despliegue local.

**¿Por qué utilizar RAG?**

Un nodo de IA simple trabaja únicamente con el conocimiento del modelo.

En cobranzas la información cambia constantemente:

Políticas de pago.
Preguntas frecuentes.
Procedimientos de gestión.
Información de clientes.

Mediante RAG el agente consulta información actualizada almacenada en Supabase antes de responder.

Esto permite:

Menor alucinación del modelo.
Información actualizada.
Fácil mantenimiento sin reentrenamiento.
##Manejo de Errores##
Error 1: Fallo en Supabase

Posible problema:

Timeout.
Conexión perdida.
Consulta sin resultados.

Solución:

Nodo IF que verifica si se recuperó contexto.

Si no existe contexto:

"Lo siento, en este momento no puedo acceder a la base de conocimiento. Intente nuevamente más tarde."

Error 2: Respuesta vacía del LLM

Posible problema:

Error del proveedor IA.
Respuesta nula.

Solución:

Nodo IF posterior al AI Agent.

Si la respuesta es nula:

"Existe un inconveniente temporal. Un operador revisará su consulta."

##System Prompt del Agente##
Plain Text
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

