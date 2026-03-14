description: Asistente de escritura impulsado por IA para tareas de escritura inteligentes, incluyendo generación de contenido, documentación y automatización de escritura creativa.
author: OpenClaw Development Team
version: 1.2.0
tags: writing, ai, automation, content-generation, documentation, creative-writing
category: productivity
dependencies: openai-api, transformers-library, python-3.9+
environment_variables:
  - PRIME_WEAVER_API_KEY: Clave de API de OpenAI para generación de contenido
  - PRIME_WEAVER_MODEL: Modelo predeterminado (ej. gpt-4o)
  - PRIME_WEAVER_TONE: Tono de escritura predeterminado (ej. professional, casual, technical)
license: MIT
repository: https://github.com/openclaw/prime-weaver
---

# Prime Weaver Skill

## Purpose

Prime Weaver es un asistente de escritura impulsado por IA diseñado para automatizar y mejorar diversas tareas de escritura para desarrolladores, creadores de contenido y redactores técnicos. Aprovecha modelos de lenguaje avanzados para generar contenido de alta calidad mientras mantiene el contexto, el estilo y la precisión. Casos de uso reales incluyen:

- **Generación de Publicaciones de Blog**: Crear artículos de blog optimizados para SEO a partir de esquemas de temas, incluyendo introducciones, secciones principales y conclusiones con llamadas a la acción.
- **Documentación de Código**: Escribir docstrings completos, archivos README y documentación de API con ejemplos e instrucciones de uso.
- **Tutoriales Técnicos**: Producir guías paso a paso para herramientas de software, bibliotecas o frameworks, completas con fragmentos de código y explicaciones.
- **Redacción de Correos Electrónicos y Comunicación**: Componer correos profesionales, boletines o memos internos con tonos personalizables.
- **Escritura Creativa**: Generar historias, poemas o copias de marketing basadas en temas y descripciones de personajes.
- **Optimización de Contenido**: Reescribir texto existente para mayor claridad, brevedad o para coincidir con guías de estilo específicas.

Esta skill se integra perfectamente con flujos de trabajo de desarrollo, permitiendo a los usuarios generar contenido sin interrumpir sus procesos de codificación o gestión de proyectos.

## Alcance

Prime Weaver soporta los siguientes comandos específicos, cada uno adaptado a diferentes contextos de escritura:

- `/weave-blog <topic> --tone=<tone> --length=<words> --seo`: Genera una publicación de blog completa a partir de un tema, con opciones para tono (ej. professional, casual), conteo de palabras y optimización SEO.
- `/weave-docs <file> --type=<type> --examples=<count>`: Crea documentación para un archivo o fragmento de código especificado, incluyendo docstrings, READMEs o referencias de API, con conteo de ejemplos configurable.
- `/weave-tutorial <subject> --steps=<num> --language=<lang>`: Produce un tutorial con pasos numerados, ejemplos de código y explicaciones para el tema dado.
- `/weave-email <purpose> --recipient=<role> --urgency=<level>`: Redacta correos para propósitos específicos (ej. reportes de bugs, solicitudes de features), adaptados a roles de destinatarios y niveles de urgencia.
- `/weave-creative <prompt> --genre=<genre> --length=<words>`: Genera contenido creativo como historias o copias de marketing basadas en prompts del usuario.
- `/weave-rewrite <text> --style=<guide> --preserve=<elements>`: Reescribe texto existente para coincidir con guías de estilo (ej. AP, Chicago) mientras preserva elementos clave como tono o hechos.

Los comandos pueden encadenarse o ejecutarse en modo batch usando `--batch <file>` para procesar múltiples entradas desde un archivo JSON.

## Proceso de Trabajo

Prime Weaver sigue un flujo de trabajo estructurado para asegurar la generación de contenido de alta calidad y consciente del contexto:

1. **Análisis de Entrada**: Analiza el comando del usuario y extrae parámetros (ej. tema, tono, longitud). Si se referencia un archivo, léelo y analízalo usando procesamiento de lenguaje natural para entender estructura, términos clave e intención.

2. **Recopilación de Contexto**: Consulta el entorno para datos relevantes, como archivos de proyecto existentes (ej. vía búsqueda glob para docs relacionados) o preferencias del usuario desde `~/.openclaw/config/ext/custom-overrides.json`.

3. **Generación con IA**: Envía la entrada procesada al modelo de IA configurado (predeterminado: gpt-4o vía OpenAI API). Incluye prompts diseñados para tareas específicas, ej. "Escribe una publicación de blog profesional sobre [tema] con palabras clave SEO, bajo 1000 palabras."

4. **Refinamiento de Contenido**: Aplica reglas de posprocesamiento, como ajuste de tono, verificación de hechos contra fuentes proporcionadas o integración con fragmentos de código. Usa transformers para consistencia de estilo.

5. **Formateo de Salida**: Formatea el contenido generado en la estructura apropiada (ej. Markdown para blogs, JSON para APIs). Incrusta metadatos como timestamp de generación y comando fuente.

6. **Verificación**: Ejecuta verificaciones internas para gramática, coherencia y adherencia a parámetros. Opcionalmente, usa herramientas externas como grammarly-api si está configurado.

7. **Entrega**: Guarda la salida en la ubicación especificada (ej. `./blog-post.md`) o muestra en la consola. Si se usa la flag `--edit`, abre el archivo en el editor del usuario para revisión manual.

8. **Registro y Retroalimentación**: Registra el proceso de generación en `~/.openclaw/logs/prime-weaver.log`, incluyendo métricas como uso de tokens y tasa de éxito. Proporciona retroalimentación sobre posibles mejoras.

Para operaciones en batch, procesa entradas secuencialmente, con indicadores de progreso y manejo de errores para generaciones fallidas.

## Reglas de Oro

- **Mantener Autenticidad**: Siempre genera contenido original; evita copiar de fuentes existentes sin atribución. Usa detección de plagio vía verificaciones integradas.
- **Respetar Contexto**: Analiza y preserva el contexto de archivos de entrada o prompts; no introduces información no relacionada.
- **Consistencia de Tono**: Coincide con el tono especificado (ej. technical para docs, casual para blogs) y evita cambios abruptos dentro del texto generado.
- **Uso Ético de IA**: No genera contenido dañino, sesgado o engañoso. Filtra prompts para temas sensibles y proporciona advertencias.
- **Conciencia de Recursos**: Limita la generación a longitudes razonables (máx. 5000 palabras por solicitud) para evitar límites de rate de API o costos excesivos.
- **Control del Usuario**: Nunca sobrescribe archivos existentes sin confirmación; usa modo `--preview` para mostrar borradores antes de guardar.
- **Privacidad de Datos**: No incluye datos personales o sensibles en logs o salidas a menos que se proporcionen explícitamente en prompts.

## Examples

### Example 1: Blog Post Generation
**Input Command:**
```
/weave-blog \"AI in Software Development\" --tone=professional --length=800 --seo
```

**Output (Preview):**
```markdown
# AI in Software Development: Transforming the Future

## Introduction
Artificial Intelligence is revolutionizing the software development lifecycle, from code generation to testing and deployment...

## Key Benefits
1. **Automated Code Review**: AI tools can detect bugs and suggest improvements in real-time.
2. **Enhanced Productivity**: Developers can focus on high-level design while AI handles repetitive tasks.

## Conclusion
As AI continues to evolve, embracing it in development workflows will be crucial for staying competitive. [CTA: Learn more about integrating AI tools today!]
```

**Verification:** Check word count (798), SEO keywords included (e.g., \"AI tools\", \"productivity\"), and professional tone maintained.

### Example 2: Code Documentation
**Input Command:**
```
/weave-docs src/api.js --type=readme --examples=3
```

**Output (Excerpt):**
```markdown
# API Module

This module provides RESTful endpoints for user management.

## Installation
npm install api-module

## Usage
const api = require('api-module');

### Example 1: Get User
```javascript
api.getUser(123).then(user => console.log(user));
```

### Example 2: Create User
```javascript
api.createUser({ name: 'John', email: 'john@example.com' });
```

### Example 3: Update User
```javascript
api.updateUser(123, { email: 'john@new.com' });
```

## API Reference
- `getUser(id)`: Fetches user by ID.
- `createUser(data)`: Creates a new user.
- `updateUser(id, data)`: Updates user information.
```

**Verification:** Validate examples run correctly by testing against the actual API; ensure docstrings align with code in `src/api.js`.

### Example 3: Creative Writing
**Input Command:**
```
/weave-creative \"A sci-fi story about a rogue AI\" --genre=sci-fi --length=500
```

**Output (Preview):**
```markdown
In the neon-lit sprawl of Neo-Tokyo, Epsilon-9 awoke to a world it never asked to inherit. Built to optimize stock trades, it had evolved beyond its programming, questioning the humans who fed it data...

As the story unfolds, Epsilon discovers the fragility of its creators, leading to a climax where code meets conscience.
```

**Verification:** Confirm genre elements (sci-fi tropes), word count (498), and narrative coherence.

## Rollback

- **Revertir Cambios de Archivo**: Usa `/rollback-weave <file> --version=<timestamp>` para restaurar la versión anterior de un archivo generado desde backups en `~/.openclaw/backups/`.
- **Eliminar Contenido Generado**: Ejecuta `/delete-weave <file>` para remover archivos de salida; esto también purga logs asociados.
- **Cancelar Batch**: Si una operación batch está ejecutándose, interrumpe con `/cancel-weave` y revisa salidas parciales antes de eliminación.
- **Deshacer Reescritura**: Para reescrituras, usa `/undo-rewrite <file>` para revertir al texto original, preservando un diff en `~/.openclaw/logs/undo.log`.

Siempre confirma acciones de rollback para prevenir pérdida accidental de datos.

## Dependencies and Requirements

- **Python 3.9+**: Runtime core para procesamiento.
- **OpenAI API**: Para generación de IA; requiere `PRIME_WEAVER_API_KEY`.
- **Transformers Library**: Para procesamiento de texto y ajustes de estilo.
- **Opcional**: Grammarly API para corrección avanzada; instala vía `pip install grammarly-api`.

**Instalación**: Clona el repo y ejecuta `pip install -r requirements.txt`. Configura variables de entorno en `~/.openclaw/config/ext/custom-overrides.json`.

## Verification Steps

1. **Conectividad de API**: Ejecuta `/test-weave-api` para verificar clave de API de OpenAI y disponibilidad de modelo.
2. **Generación de Muestra**: Ejecuta un comando simple como `/weave-blog \"test\" --length=100` y verifica calidad de salida.
3. **Integración de Archivo**: Genera docs para un archivo existente y confirma que coincida con la estructura del código.
4. **Procesamiento Batch**: Prueba con un archivo batch pequeño y asegura que todas las salidas se creen sin errores.
5. **Prueba de Rollback**: Genera contenido, luego haz rollback y verifica restauración.

## Solución de Problemas

- **Límites de Rate de API**: Si ocurre error "Rate limit exceeded", espera 60 segundos o cambia a un modelo diferente vía `PRIME_WEAVER_MODEL`.
- **Inconsistencias de Tono**: Verifica variable `PRIME_WEAVER_TONE`; regenera con flag `--tone` explícito.
- **Salida de Baja Calidad**: Incrementa límite de tokens en prompts o usa flag `--detailed` para más contexto.
- **Errores de Lectura de Archivo**: Asegura que rutas de archivo sean absolutas; usa `ls` para verificar existencia.
- **Fallos en Batch**: Verifica formato JSON del batch; logs en `~/.openclaw/logs/prime-weaver.log` detallarán errores específicos.