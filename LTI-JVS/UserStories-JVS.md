# User Stories & Backlog - LTI (Lead Talent Intelligence)

Este documento contiene las User Stories, el Backlog priorizado y el desglose técnico de la funcionalidad principal para el sistema LTI, generado utilizando **Gemini CLI**.

---

## 1. User Stories

A continuación se presentan 3 User Stories clave para el MVP de LTI, centradas en las funcionalidades de "Smart Screening", "Auto-Scheduler" y "AI Job Crafter".

### US-01: Filtrado Inteligente de Candidatos (Smart Screening)
**Título:** Clasificación y Resumen Automático de Candidatos con IA  
**Prioridad:** Alta (Core)  
**Estimación:** 13 Puntos de Historia

**Como** Recruiter,  
**Quiero** que el sistema analice automáticamente los CVs recibidos y les asigne una puntuación de compatibilidad con la oferta,  
**Para** poder identificar y contactar a los mejores candidatos inmediatamente sin tener que leer cientos de CVs irrelevantes.

**Criterios de Aceptación:**
1.  El sistema debe procesar archivos en formato PDF y DOCX subidos por los candidatos.
2.  Debe comparar el contenido del CV con la Descripción del Trabajo (Job Description) utilizando análisis semántico (no solo palabras clave).
3.  Debe generar un "Score de Ajuste" del 0 al 100 visible en el listado de candidatos.
4.  Si el Score es > 80, el candidato se marca visualmente como "Top Talent".
5.  Debe generar un resumen de 3 puntos (bullets) explicando "Por qué encaja" y "Posibles carencias".
6.  El tiempo de procesamiento no debe exceder los 30 segundos por CV.

---

### US-02: Agendamiento Automatizado (Auto-Scheduler)
**Título:** Coordinación Automática de Entrevistas  
**Prioridad:** Media  
**Estimación:** 8 Puntos de Historia

**Como** Hiring Manager,  
**Quiero** que el sistema coordine automáticamente los huecos de mi agenda con la del candidato,  
**Para** evitar la cadena de emails manuales para concertar una entrevista.

**Criterios de Aceptación:**
1.  El sistema debe tener acceso a mi calendario (Google Calendar/Outlook).
2.  Debe enviar un link al candidato seleccionado con mis huecos disponibles.
3.  Una vez el candidato elige hora, se debe bloquear en ambos calendarios y enviar la invitación con el link de videoconferencia.
4.  Debe permitir configurar "tiempos de buffer" entre entrevistas.

---

### US-03: Generador de Ofertas (AI Job Crafter)
**Título:** Asistente de Redacción de Ofertas Inclusivas  
**Prioridad:** Baja (Enhancement)  
**Estimación:** 5 Puntos de Historia

**Como** Hiring Manager,  
**Quiero** generar una descripción de oferta detallada a partir de unos pocos bullet points,  
**Para** asegurar que el lenguaje sea inclusivo, optimizado para SEO y atractivo sin gastar horas redactando.

**Criterios de Aceptación:**
1.  Input: Título del puesto, Seniority, Skills principales (3-5), Beneficios clave.
2.  Output: Descripción completa estructurada (Intro, Responsabilidades, Requisitos, Oferta).
3.  El sistema debe sugerir mejoras si detecta lenguaje sesgado (ej. "ninja", "rockstar").

---

## 2. Product Backlog Priorizado

Para priorizar el backlog, he utilizado la metodología **MoSCoW** (Must have, Should have, Could have, Won't have) combinada con **Value vs. Effort**.

| Orden | ID | Título | Metodología (MoSCoW) | Razón de Prioridad |
| :--- | :--- | :--- | :--- | :--- |
| 1 | **US-01** | **Smart Screening** | **Must Have** | Es la ventaja competitiva ("Unfair Advantage") definida en el Lean Canvas. Sin esto, LTI es solo otro repositorio de CVs. Alto Valor. |
| 2 | **US-02** | **Auto-Scheduler** | **Should Have** | Reduce drásticamente el tiempo administrativo (Time-to-hire), métrica clave del proyecto. |
| 3 | **US-03** | **AI Job Crafter** | **Could Have** | Aporta valor al Manager, pero no es crítico para el flujo principal de contratación. Se puede hacer manual en el MVP. |
| 4 | US-04 | Analytics Predictivo | Could Have | Funcionalidad avanzada para fases posteriores (Enterprise Tier). |

---

## 3. Desglose de Tickets Técnicos (Para US-01: Smart Screening)

He seleccionado la **US-01 (Smart Screening)** para el desglose técnico, ya que implica la integración de los componentes Core y AI Service definidos en la arquitectura.

### Tarea 1: Implementación de CV Parsing (Backend)
*   **ID:** TICK-01
*   **Tipo:** Backend / Infra
*   **Descripción:** Crear un servicio en el `Candidate Service` que acepte subidas de archivos (S3/MinIO), extraiga el texto plano de PDFs/DOCX y lo limpie de caracteres especiales.
*   **Dependencias:** Ninguna.
*   **Estimación:** 3 Puntos.

### Tarea 2: Pipeline de Generación de Embeddings (AI Service)
*   **ID:** TICK-02
*   **Tipo:** AI / Data
*   **Descripción:** Configurar el `AI Matching Service` para recibir texto limpio. Implementar conexión con OpenAI API (modelo `text-embedding-3-small`) para vectorizar el texto del CV y de la Job Description.
*   **Dependencias:** TICK-01.
*   **Estimación:** 5 Puntos.

### Tarea 3: Persistencia Vectorial y Búsqueda (Database)
*   **ID:** TICK-03
*   **Tipo:** Database
*   **Descripción:** Configurar Pinecone/Milvus. Crear índices para almacenar vectores de Candidatos. Implementar consulta de similaridad de coseno para devolver los "Top K" candidatos para una oferta.
*   **Dependencias:** TICK-02.
*   **Estimación:** 5 Puntos.

### Tarea 4: Generación de Resumen con LLM (AI Service)
*   **ID:** TICK-04
*   **Tipo:** AI / Prompt Engineering
*   **Descripción:** Crear el prompt para el LLM (GPT-4o/Claude) que tome el CV y la JD y genere el resumen de 3 bullets ("Pros" y "Cons").
*   **Dependencias:** TICK-01.
*   **Estimación:** 3 Puntos.

### Tarea 5: Visualización de Ranking en Dashboard (Frontend)
*   **ID:** TICK-05
*   **Tipo:** Frontend (React)
*   **Descripción:** Actualizar la vista de "Detalle de Oferta" para mostrar la lista de candidatos ordenados por Score. Incluir badge de score (color coded) y tooltip con el resumen generado.
*   **Dependencias:** TICK-03, TICK-04.
*   **Estimación:** 5 Puntos.

---

## 4. Estimación de Esfuerzo

Se ha utilizado la secuencia de **Fibonacci** (1, 2, 3, 5, 8, 13, 21) para los Puntos de Historia, asumiendo que 1 punto ~= medio día de trabajo ideal de un senior.

| Ticket | Título | Puntos | Justificación |
| :--- | :--- | :--- | :--- |
| TICK-01 | CV Parsing | 3 | Librerías estándar (PyPDF2, Textract) facilitan esto, pero manejar layouts complejos añade incertidumbre. |
| TICK-02 | Embeddings Pipeline | 5 | Requiere configurar llamadas asíncronas y manejo de errores con la API externa. |
| TICK-03 | Vector DB Logic | 5 | Complejidad media en la configuración de índices y optimización de queries. |
| TICK-04 | LLM Summary | 3 | Principalmente Prompt Engineering y parsing de la respuesta JSON del LLM. |
| TICK-05 | Frontend Dashboard | 5 | Requiere nuevos componentes de UI y conectar con múltiples endpoints (listado + detalles). |
| **TOTAL** | **US-01 Smart Screening** | **21** | La suma da 21, lo cual indica que es una historia grande (Epic candidata) que podría dividirse en dos sprints si fuera necesario. |
