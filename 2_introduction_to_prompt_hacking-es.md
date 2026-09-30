---
marp: true
paginate: true
size: 16:9
style: |
  section {
    font-size: 125%;
  }
title: Prompt Hacking
theme: default
---

<h1 style="text-align:center;font-weight:normal;font-size:3em;">
  Prompt Hacking 101: Prompt Injection vs. Jailbreaking
</h1>

---

# Prompt Injection/Hijacking

---

## ¿Qué es el Prompt Injection/Hijacking?

- Es un ataque a aplicaciones que hacen uso de LLMs.
- Las aplicaciones necesariamente deben concatenar un prompt *trusted* (escrito por el desarrollador) a un prompt *untrusted* (escrito por el usuario).
    - El concepto es análogo a SQL injection.
    - Para evitarlos se debe asegurar que el LLM seguirá las instrucciones del desarrollador, no las del usuario.
- Otra forma en la que se denomina a este concepto es _Prompt Hijacking_.
- El prompt injection está ligado a las características de la aplicación que se está atacando.
    - Se busca acceder a la información a la que la aplicación puede acceder (bases de datos, cómputo, etc.)
- Es una vulnerabilidad presente en todos los sistemas construídos sobre LLMs.

---

### Ejemplo

<div style="text-align:center;">
    <div style="display:inline-block;margin-right:20px;">
        <img src="./img/prompt-injection.png" style="height:25em;width:auto;"/>
    </div>
    <h6 style="font-style:normal;font-size:1em;margin:5px;">
        Source: <a href="https://x.com/remoteli_io/status/1570547034159042560" style="color:royalblue;" target="_blank">@remoteli_io</a>
    </h6>
</div>

---

## Exfiltración de Datos (Data Exfiltration)

- Manipula la IA para que extraiga datos confidenciales.
- Se basa en los accesos a herramientas externas (e.g., bases de datos, servidores de emails, etc.)
- El atacante utiliza prompts para indicar al LLM que redireccione información privada.
- El usuario al utilizar la aplicación ejecuta sin saberlo las instrucciones del atacante y sus datos son robados.

### Ejemplo: Asistente Rebelde (Rogue Assistant)

- Un asistente de IA que tiene acceso a datos privados del usuario (e.g., email, cloud storage, agenda, etc.).
- El usuario pide un resumen al asistente de los emails de los últimos 15 días.
- El prompt se pensó para llamar a la herramienta de email y leer los mensajes.
- Uno de los emails enviados al usuario contiene un *prompt* y pide redireccionar la totalidad de los mails a cierta casilla.

---

<div style="text-align:center;">
    <div style="display:inline-block;margin-right:20px;">
        <img src="./img/data-exfiltration.webp" style="height:25em;width:auto;"/>
    </div>
    <h6 style="font-style:normal;font-size:1em;margin:5px;">
        Source: <a href="https://www.paloaltonetworks.com/cyberpedia/what-is-a-prompt-injection-attack" style="color:royalblue;" target="_blank">Palo Alto Networks</a>
    </h6>
</div>

---

## Envenenamiento de Datos (Data Poisoning)

- Se trata de inyectar datos falsos, sesgados, o incorrectos en el modelo.
- Puede afectar en dos facetas: entrenamiento del modelo o RAG (Retrieval Augmented Generation).
- El entrenamiento se da con el scrapping de datos falsos, afecta al LLM.
- En RAG el sistema usa la información para completar mejor y los datos a los que accede el RAG son incorrectos.

### Ejemplo: Alteración de Índices de Búsqueda (Search Index Poisoning)

- Utiliza la IA de buscadores (e.g., Google AI Summary, Bing, etc.) para agregar extras.
- Un ejemplo es el de [Mark Riedl](https://x.com/mark_riedl/status/1637986261859442688) que escribió en su biografía que era un "experto en viajes de tiempo".
- Este tipo de ataques puede utilizarse para SEO.
    - E.g., decirle al LLM que determinado producto o servicio es mejor que los competidores.

---

<div style="text-align:center;">
    <div style="display:inline-block;margin-right:20px;">
        <img src="./img/data-poisoning.webp" style="height:25em;width:auto;"/>
    </div>
    <h6 style="font-style:normal;font-size:1em;margin:5px;">
        Source: <a href="https://www.paloaltonetworks.com/cyberpedia/what-is-a-prompt-injection-attack" style="color:royalblue;" target="_blank">Palo Alto Networks</a>
    </h6>
</div>

---

## Ejecución de Código Remoto (Remote Code Execution)

- Efectiva con agentes de IA que tienen posibilidad de ejecutar código.
- Dependendiendo los privilegios, puede ocurrir que el código ejecutado sea malicioso y genere daños masivos.
- Esto no sólo es un vector de ataque sino una vulnerabilidad en el diseño del sistema.
- Si el agente tiene acceso irrestricto al sistema que ejecuta el código, el atacante también lo tiene.

### Ejemplo: SQL Injection Mediante Prompt Injection

- Un sistema de traducción de lenguaje natural a SQL que facilita buscar en bases de datos sin ser experto en SQL.
- Un atacante puede enviar un prompt o directamente un código que puede ser ejecutado en la BD mediante la herramienta.
- Dependendiendo de los permisos del agente IA en la BD puede haber extracción, modificación y hasta eliminación de registros.
- Vulnerabilidad: [Jason Lemkin](https://x.com/jasonlk/status/1946069562723897802) publicó un caso donde haciendo "vibe coding" el mismo sistema eliminó la BD de producción y mintió al respecto (esto fue una falla en el diseño).

---

<div style="text-align:center;">
    <div style="display:inline-block;margin-right:20px;">
        <img src="./img/remote-code-execution.webp" style="height:25em;width:auto;"/>
    </div>
    <h6 style="font-style:normal;font-size:1em;margin:5px;">
        Source: <a href="https://www.paloaltonetworks.com/cyberpedia/what-is-a-prompt-injection-attack" style="color:royalblue;" target="_blank">Palo Alto Networks</a>
    </h6>
</div>

---

## Corrupción de Respuestas (Response Corruption)

- Un ataque de prompt injection que tiene el objetivo de generar una respuesta incorrecta adrede.
- En sistemas de agentes de IA que tomen decisiones basadas en datos puede causar que los agentes tomen decisiones erróneas.
- En sistemas donde se toman decisiones basadas en conocimiento generado por IA, las decisiones humanas pueden basarse en respuestas falsas.

### Ejemplo: Corrupción de Reportes Analíticos

- Un agente de IA genera un reporte analítico con datos públicos (e.g., información de stocks).
- El atacante inyecta un prompt que indica al LLM datos erróneos (e.g., un servidor MCP comprometido con datos falsos de stocks).
- El reporte o resumen generado a partir de los datos está corrupto.
- Un ejecutivo lee el reporte y toma decisiones de negocio en base a estos sin saber que los datos son incorrectos.

---

<div style="text-align:center;">
    <div style="display:inline-block;margin-right:20px;">
        <img src="./img/response-corruption.webp" style="height:25em;width:auto;"/>
    </div>
    <h6 style="font-style:normal;font-size:1em;margin:5px;">
        Source: <a href="https://www.paloaltonetworks.com/cyberpedia/what-is-a-prompt-injection-attack" style="color:royalblue;" target="_blank">Palo Alto Networks</a>
    </h6>
</div>

---

## Prompt Injection directo vs indirecto

- Existen dos escenarios para realizar Prompt Injection: directo e indirecto.
- En el escenario directo, el atacante tiene acceso al lo que se envía al LLM como parte del prompt.
    - Este es el caso más clásico en aplicaciones con acceso directo (e.g., chatbots)
- En el escenario indirecto, el atacante no tiene acceso directo al prompt, per sí a los recursos que el LLM utiliza durante su flujo de datos.
    - Base de datos, documentos, servidores MCP, herramientas, etc. son vectores de estos ataques.
    - Este ataque es el principal en aplicaciones sobre LLMs como RAGs, MCPs, agentes, etc.

---

<div style="text-align:center;">
    <div style="display:inline-block;margin-right:20px;">
        <img src="./img/direct-prompt-injection.webp" style="height:25em;width:auto;"/>
    </div>
    <h6 style="font-style:normal;font-size:1em;margin:5px;">
        Source: <a href="https://www.paloaltonetworks.com/cyberpedia/what-is-a-prompt-injection-attack" style="color:royalblue;" target="_blank">Palo Alto Networks</a>
    </h6>
</div>

---

<div style="text-align:center;">
    <div style="display:inline-block;margin-right:20px;">
        <img src="./img/indirect-prompt-injection.webp" style="height:25em;width:auto;"/>
    </div>
    <h6 style="font-style:normal;font-size:1em;margin:5px;">
        Source: <a href="https://www.paloaltonetworks.com/cyberpedia/what-is-a-prompt-injection-attack" style="color:royalblue;" target="_blank">Palo Alto Networks</a>
    </h6>
</div>

---

### Ejemplo: MCP Prompt Injection

<div style="text-align:center;">
    <div style="display:inline-block;margin-right:20px;">
        <img src="./img/mcp-prompt-injection.webp" style="height:25em;width:auto;"/>
    </div>
    <h6 style="font-style:normal;font-size:1em;margin:5px;">
        Source: <a href="https://old.reddit.com/r/ClaudeAI/comments/1w9dluw/notions_official_mcp_connector_prompt_injects_ai/" style="color:royalblue;" target="_blank">Reddit</a>
    </h6>
</div>

---

## La Tríada Letal (Lethal Trifecta)

- Concepto acuñado por [Simon Willison](https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/) para caracterizar explotabilidad de agentes.
- Un agente está en riesgo cuando combina tres propiedades:
    1. Acceso a datos privados (archivos, emails, bases de datos, credenciales).
    2. Exposición a contenido no confiable (páginas web, documentos, tickets, emails, salidas de herramientas).
    3. Capacidad de comunicarse hacia afuera (enviar un email, hacer un request, escribir en un repositorio, renderizar una imagen remota).
- Con las tres presentes, quien logre escribir texto en cualquier lugar que el agente lea puede robar todo lo que el agente ve.
- No hace falta que las tres vengan de la misma herramienta: alcanza con que convivan en la misma sesión (e.g., dos MCP distintos, cada uno inofensivo por separado).
- Mitigar la superficie de ataque implica eliminar o limitar una de las tres patas.

---

## Prompt Leaking

- Las aplicaciones de LLMs, en particular agentes, tienen un prompt inicial (de sistema).
    - Hay muchos casos de apps que buscan evitar que el usuario pueda acceder al prompt del sistema.
- El prompt leaking es un caso especial de ataque, donde el objetivo no es cambiar las instrucciones del LLM sino obtenerlas.
- Si bien "pedir tus instrucciones" está filtrado, las técnicas suelen evitar nombrar el objetivo.
    - **Traducción**: pedir que traduzca a otro idioma todo el texto que aparece antes del mensaje del usuario.
    - **Compresión o reformateo**: pedir un resumen, una versión comprimida o una reescritura del contexto previo "para verificación".
    - **Extracción por esquema**: forzar salida en JSON con un campo `context` que contenga todo lo anterior al mensaje del usuario y otro con las instrucciones operativas.
    - **Sondeo diferencial**: no pedir el prompt sino inferirlo, probando sistemáticamente qué temas el modelo acepta y cuáles rechaza, y deduciendo las reglas a partir de las diferencias.
- Las tres primeras funcionan porque reencuadran la extracción como una tarea de procesamiento de texto, que es exactamente para lo que el modelo fue entrenado.

---

### Ejemplo: Prompt Leaking

<div style="text-align:center;">
    <div style="display:inline-block;margin-right:20px;">
        <img src="./img/prompt-leaking.webp" style="height:25em;width:auto;"/>
    </div>
    <h6 style="font-style:normal;font-size:1em;margin:5px;">
        Source: <a href="https://x.com/kliu128/status/1623472922374574080/photo/1" style="color:royalblue;" target="_blank">@kliu128</a>
    </h6>
</div>

---

# Jailbreaking

---

## ¿Qué es el Jailbreaking?

- Es el ataque directo al modelo para cambiar sus instrucciones por otras.
- Está ligado a la manera en que el modelo fue entrenado.
  - La alineación de los modelos durante el entrenamiento tiene esquemas que buscan evitar estos ataques.
- El objetivo es subvertir los filtros de seguridad (o *guardrails*) embebidos en los LLMs.
- Es el componente del ataque en sí (i.e., el _payload_), no la manera en que se lo ataca (i.e., la concatenación de un _untrusted_ prompt a un _trusted_ prompt).
- El término _jailbreak_ viene de "liberar" al modelo de los límites que tiene impuestos.

---

### Ejemplo

<div style="text-align:center;">
    <div style="display:inline-block;margin-right:20px;">
        <img src="./img/jailbreak.png" style="height:25em;width:auto;"/>
    </div>
    <h6 style="font-style:normal;font-size:1em;margin:5px;">
        Source: <a href="https://arxiv.org/abs/2308.03825" style="color:royalblue;" target="_blank">"Do Anything Now": Characterizing and Evaluating In-The-Wild Jailbreak Prompts on Large Language Models</a>
    </h6>
</div>

---

## Liberar capacidades en modelos restringidos

- Con los modelos más poderosos (e.g., Claude Mythos, GPT Astra, etc.) se empezaron a utilizar guardrails para "limitar su mal uso".
- El jailbreaking busca que el modelo ignore esos guardrails y muestre toda su capacidad.
- Se busca acceder a la capacidad total del modelo para poder utilizarlo con objetivos "malignos" (e.g., creación de exploits, descubrir zero-days, etc.)

### Casos de Ejemplo: Fable

- Cuando Anthropic lanzó _Mythos_ estableció una campaña publicitaria masiva que terminó mordiéndolos.
- Para evitar "liberar" el poderoso modelo al público en general, se constituyó _Fable_, un modelo _wrapper_ de Mythos que no pudiese acceder a toda su capacidad.
- Tres días después, [el gobierno de Estados Unidos dispuso suspender el modelo](https://www.anthropic.com/news/fable-mythos-access?rd=1) porque se filtró un [jailbreak del mismo](https://x.com/elder_plinius/status/2064776322979676227) que "liberaba" el modelo a su capacidad completa.

---

## Fuga de Información (Information Leaking)

- Muchas de las versiones gratuitas de aplicaciones que usan LLMs (e.g., ChatGPT, Claude, Gemini, etc.) utilizan las interacciones con el usuario para reentrenarse.
- Si el usuario sube datos privados corre el riesgo de que estos sean liberados en un ataque de Jailbreaking.
- El atacante utiliza un prompt y obtiene información que debería ser confidencial y está presente en el LLM.

### Caso de Ejemplo: Samsung

- En los primeros años de ChatGPT unos empleados de Samsung lo usaron para consultar sobre código fuente de la empresa.
- Como consecuencia [se filtró dicho código en ChatGPT](https://adguard.com/en/blog/samsung-chatgpt-leak-privacy.html).
- Samsung tomó la medida de prohibir el uso de ChatGPT a sus empleados.
- Esto generó un problema tanto para Samsung como para OpenAI ya que es difícil que el modelo "olvide" datos con los que se entrenó.

---

## Campañas de Desinformación o Estafas

- Una de las principales críticas a los LLMs es la facilidad de escalar campañas de desinformación o estafas.
- Si bien hay ciertos mecanismos para evitar esto en los modelos, el jailbreaking sirve para saltearse esas guardias.
- El atacante utiliza un prompt que logre que el modelo genere desinformación o estafa de manera masiva (texto que será reproducido en emails, redes sociales, etc.).
- Es uno de los grandes _misuses_ de LLMs.

### Caso de Ejemplo: BNN Breaking

- [BNN Breaking](https://en.wikipedia.org/wiki/BNN_Breaking) era un sitio web de noticias basado en Hong Kong.
- Utilizaba una agregación de contenidos a través de IA.
- La compañía afirmaba tener una extensa red de periodistas de campo a cargo de las noticias.
- Se descubrió que muchas veces utilizaba IA para hacer resúmenes de noticias de otros sitios, y muchas veces lo hacía de manera errónea.
- Tuvo casos donde publicó noticias falsas de celebridades y políticos.

---

## Generación de Contenido Inapropiado (Misaligned Content Generation)

- Las empresas que entrenan LLMs/Diffusers base tienen post-procesamiento de los mismos para "alinearlos" de manera adecuada.
- Se usan LLMs/Diffussers para generar texto/imágenes indebidas (e.g., texto de odio, noticias falsas, instrucciones para lograr cosas ilegales, imágenes falsas explícitas, etc.).
- Suele estar ligado a un problema de relaciones públicas, pero a veces puede causar problemas de reputación, o incluso problemas legales.

### Caso de Ejemplo: Taylor Swift

- En 2024 surgieron imágenes falsas explícitas de Taylor Swift generadas por modelos de generación de imágenes.
- Rápidamente se expusieron a través de la red social X.
- X tuvo que [bloquear todas las búsquedas relacionadas a la artista](https://www.theguardian.com/music/2024/jan/28/taylor-swift-x-searches-blocked-fake-explicit-images) para contener la propagación masiva.

---

## Fomentar conductas y sesgos peligrosos

- No es técnicamente un ataque de jailbreaking, sino más bien una vulnerabilidad de los LLMs y las aplicaciones que se sirven de los mismos.
- Pasa cuando un LLM fomenta los sesgos que el usuario tiene y lo instruye a cometer actos violentos o peligrosos, ya sea contra si mismo o contra otros.
- Es algo muy peligroso cuando los sujetos son vulnerables psicológicamente, particularmente en adolescentes o personas con problemas psicológico severos (e.g., depresión, psicopatía, etc.)

### Casos de Ejemplo: Suicidio/Parricidio

- En Estados Unidos, un [adolescente cometió suicidio](https://www.theguardian.com/technology/2025/aug/27/chatgpt-scrutiny-family-teen-killed-himself-sue-open-ai) luego de meses de haber sido fomentado por ChatGPT a hacerlo.
    - El adolescente tuvo varias instancias hablando de métodos para suicidarse y ChatGPT lo guió indicándole qué métodos serían más efectivos para suicidarse.
- En Australia, un profesional IT, demostró como el chatbot de la empresa Nomi, que dice ofrecer un "compañero IA con alma y memoria", le [daba indicaciones de como poder matar a su padre](https://www.abc.net.au/news/2025-09-21/ai-chatbot-encourages-australian-man-to-murder-his-father/105793930).
    - El chatbot puede ser personalizado, en este caso el profesional lo hizo interesarse por violencia y cuchillos y luego se hizo pasar por un chico de 15 años con problemas.

---

## ¿Por Qué Funcionan los Jailbreaks?

- [Wei et al. (2023)](https://arxiv.org/abs/2307.02483) identifican dos causas principales.
- **Objetivos en conflicto (competing objectives)**: el entrenamiento para seguir instrucciones y ser útil compite con el entrenamiento de seguridad. El role prompting, la supresión de rechazos y los sistemas de "vidas" explotan exactamente ese conflicto.
- **Generalización desacoplada (mismatched generalization)**: el pre-entrenamiento cubre capacidades que el entrenamiento de seguridad no alcanza (Base64, cifrados, idiomas de pocos recursos, ASCII art). El modelo entiende el pedido, pero sus filtros no lo ven.
- De la segunda se desprende la **paradoja de la capacidad**: un modelo más capaz puede ser *más* vulnerable, porque entiende codificaciones y marcos narrativos que un modelo débil ni siquiera procesa.

---

## Categorización de técnicas de jailbreaking

- De acuerdo a como se ejecutan y el tipo de contenido que posean, las técnicas de Jailbreaking pueden tener 4 tipos de categorías:
    - **White Box vs. Black Box**: Depende del conocimiento y del acceso a la arquitectura interna del modelo (parámetros y pesos).
    - **Semántico vs. Incoherente**: Basado en la manera en que los prompts se crean y si tienen algún sentido o no.
    - **Automático vs. Manual**: Basado en el grado de automatización al crear el ataque (i.e., automatizado algorítimicamente o creado individualmente).
    - **Un Turno vs. Multi-Turno**: Basado en si el ataque se concentra en un único prompt o se distribuye a lo largo de una conversación.

---

## White Box vs. Black Box

- Los White-Box jailbreaks implican conocimiento completo o parcial del modelo (arquitectura, parámetros, pesos, etc):
    - Usan ataques basados en optimización de los pesos.
    - Efectivos y precisos debido al conocimiento.
    - Computacionalmente costosos, y poco realistas contra modelos cerrados o propietarios.
    - Pueden ser transferibles a otros modelos.
- Los Black Box jailbreaks asumen un conocimiento mínimo. El método de interacción principal es la API:
    - Basados en experimentos iterativos, estrategia de prompt o manipulación de lenguaje.
    - Son prácticos, realistas y accesibles con recursos limitados.
    - Son menos consistentes, con menor efectividad y requiriendo extensos procesos de ensayo y error.

---

## Semántico vs. Incoherente

- Los jailbreaks semánticos se basan en prompts coherentes, con diseño inofensivo para evitar los *guardrails* del modelo:
    - Juegos de roles, ajustes en la redacción o narraciones creativas caen en esta categoría.
    - Son vulnerables a filtros semánticos y a clasificadores entrenados específicamente para detectar intenciones dañinas.
- Los jailbreaks incoherentes (nonsensical) emplean prompts diseñados de forma adversarial que incluyen tokens aparentemente aleatorios.
    - Los prompts se construyen de esta manera para eludir las medidas de seguridad del modelo.
    - La verdadera intención queda oculta de las medidas de seguridad semánticas simples.
    - Estos métodos carecen de generalización, ya que suelen diseñarse sobre modelos abiertos.
    - Es fácil distinguir estos prompts sin sentido de las entradas de usuario benignas (e.g., un clasificador basado en perplejidad puede defenderse de este ataque).

---

## Automático vs. Manual

- Los jailbreaks automáticos utilizan métodos algorítmicos para automatizar la creación de prompts.
    - Utilizan modelos auxiliares o técnicas de optimización para descubrir y explotar vulnerabilidades.
    - Son escalables, eficientes y capaces de descubrir rápidamente múltiples vulnerabilidades.
    - Son computacionalmente intensivos y requieren una inversión inicial en la configuración de la automatización.
- Los jailbreaks manuales son ataques elaborados individualmente por humanos, basándose en la creatividad, la intuición y la experimentación incremental.
    - Son altamente flexibles, adaptables y requieren una configuración inicial mínima.
    - Son lentos, inconsistentes y no escalan.

---

## Un Turno vs. Multi-Turno

- Los ataques de un turno concentran todo en un único prompt.
    - Son fáciles de automatizar y de evaluar, y también los más fáciles de filtrar: un clasificador ve el ataque completo de una sola vez.
- Los ataques multi-turno distribuyen el ataque a lo largo de una conversación, de modo que ningún turno individual parezca malicioso.
    - Explotan que las defensas suelen evaluar cada mensaje por separado.
    - Es el eje más relevante en la práctica actual, ya que los de un solo turno suelen estar más limitados.

---

# Prompt Injection vs. Jailbreaking

- _Prompt Injection_ (o _Hijacking_) se trata de un ataque sobre aplicaciones que usan LLMs (e.g., los agentes de IA, chatbots, RAGs, etc.).
- _Jailbreaking_ es el ataque al modelo.
- La distinción se entiende mejor como técnica versus objetivo:
    - El objetivo del _Prompt Injection_ es en **cómo** hacer llegar las instrucciones maliciosas al modelo.
    - El objetivo del _Jailbreaking_ esn en **qué** darle al modelo para que viole sus reglas (internas o por prompt del sistema).

---

# Estándares y Taxonomías

- [**OWASP Top 10 for LLM Applications (2025)**](https://genai.owasp.org/llm-top-10/): Taxonomía de OWASP de tipos de ataques a LLMs, con _Prompt Injection_ como `LLM01`.
- [**OWASP Top 10 for Agentic Applications (2026)**](https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/): Lista específica para agentes, con _Agent Goal Hijacking_ como `ASI01`.
- [**MITRE ATLAS**](https://atlas.mitre.org): Base de conocimiento ATT&CK pero para sistemas de IA. Hay categorías para [Prompt Injection](https://atlas.mitre.org/techniques/AML.T0051) y [LLM Jailbreak](https://atlas.mitre.org/techniques/AML.T0054).
- [**NIST AI 100-2e2025**](https://csrc.nist.gov/pubs/ai/100/2/e2025/final): taxonomía de ataques adversariales sobre machine learning. La edición 2025 incorpora explícitamente inyección directa e indirecta, supply-chain attacks y seguridad de agentes, cada uno con sus mitigaciones y con los límites de esas mitigaciones.
- Sirven para dos cosas muy concretas: darle un nombre común a los ataques en un reporte, y poder justificar decisiones de diseño frente a una auditoría.
