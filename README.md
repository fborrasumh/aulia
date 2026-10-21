# AulIA

Aula multiagente: de un tema a una sesión completa, revisada y lista para el alumnado. Aplicación web de un solo fichero.

**Usar la app:** https://fborrasumh.github.io/aulia/

**Idiomas:** español (por defecto), inglés, portugués; selector en la barra superior (o `?lang=en` / `?lang=pt` en la URL).

## Qué hace

AulIA convierte un tema (y, si se quiere, los apuntes del profesorado) en una sesión de clase completa, diseñada por un equipo de agentes de IA y comprobada por código:

- **Plan editable de la sesión** como secuencia de escenas (diapositivas, cuestionarios y proyecto), con alineamiento constructivo. Se puede reordenar, añadir, quitar o rehacer con comentarios; el código cuadra los totales y los tiempos.
- **Diapositivas con guion oral** (80-150 palabras por diapositiva), ejemplos, preguntas para la clase y **diagramas que dibuja el código** a partir de la estructura que propone la IA (proceso, ciclo o jerarquía).
- **Cuestionarios** de respuesta única, múltiple, verdadero/falso y respuesta corta, con explicación, dificultad y nivel de Bloom.
- **Proyecto de Aprendizaje Basado en Proyectos** (Gold Standard PBL): pregunta impulsora, roles, hitos, rúbrica con cuatro niveles, medidas DUA y conexiones interdisciplinares.
- **Revisión pedagógica** con ciclos de corrección dirigida y **guía docente** con la secuencia minutada.
- **Edición a mano** de cada diapositiva y pregunta, **cambios pedidos al agente** de cada escena y **deshacer**.
- **Presentación a pantalla completa** en el navegador (N: notas, S: solución de las preguntas).
- **Descargas sin conexión:** PowerPoint editable con el guion en las notas del orador; Word (guía docente, guion, cuestionario para el alumnado, solucionario y proyecto); banco de preguntas para Moodle (GIFT y XML); JSON reproducible para seguir trabajando; ZIP con todo.
- **Página interactiva para el alumnado:** un único HTML con las diapositivas sin notas, cuestionarios que se corrigen solos con su explicación, autoevaluación de las preguntas abiertas y el proyecto con su rúbrica. Funciona sin clave y sin conexión.
- **Ejemplo completo sin clave** («La fotosíntesis») en español, inglés y portugués.

## El equipo de agentes

| Agente | Qué hace |
|---|---|
| Director (código, no IA) | Reparte el trabajo, ejecuta en paralelo lo independiente, sanea cada entrega y reenvía a corrección |
| Analista documental | Extrae del documento ideas, definiciones y datos, cada uno con cita literal |
| Planificador | Diseña la secuencia de escenas |
| Diseñador de diapositivas | Crea cada bloque de diapositivas y su guion oral |
| Evaluador | Redacta los cuestionarios con distractores basados en errores frecuentes |
| Diseñador ABP | Diseña el proyecto, la rúbrica y las medidas DUA |
| Revisor pedagógico | Señala errores, desalineaciones y problemas de nivel |
| Redactor de la guía docente | Redacta la guía con la secuencia minutada |

## Qué comprueba el código (la IA propone, el código comprueba)

- Que haya exactamente las diapositivas y preguntas pedidas (reparte el total si el planificador se desvía y recorta lo que sobra).
- Que los índices de las respuestas correctas existan, que una pregunta de respuesta única tenga una sola correcta, que verdadero/falso tenga dos opciones y que la respuesta corta no tenga opciones; opciones repetidas, «todas/ninguna de las anteriores» y respuesta correcta siempre en la misma posición.
- Que la rúbrica sume 100 (si no, la reescala por mayores restos) y que todos los criterios tengan sus niveles.
- Que la guía sume la duración de la sesión (recalcula los minutos de inicio) y que no cite escenas inexistentes.
- Que las citas del analista aparezcan literalmente en el documento y que las diapositivas no traigan cifras que el documento no tiene.
- Que el revisor no «apruebe» un aula con incidencias altas o medias: el código anula ese aprobado. Las incidencias altas que detecta el propio código también vuelven a corrección.
- Que una escena cuyo plan cambió después de generarla quede marcada como desactualizada.

Cada corrección del código queda anotada y visible («Corregido por el código»).

## Cómo se usa la IA

Con la propia clave de OpenAI, Google Gemini o Anthropic Claude; el desplegable muestra los modelos disponibles para esa clave. La clave se guarda solo en el navegador. No hace falta servidor. Antes de generar se muestra una estimación del número de llamadas; una sesión estándar (12 diapositivas, 8 preguntas, proyecto y un ciclo de revisión) hace unas 10-14. Lo ya generado se guarda en cuanto llega: se puede detener el trabajo y continuar después.

## Privacidad

- Se guarda solo en el navegador (IndexedDB): la sesión, el documento, los materiales y la clave de IA.
- Sale hacia el proveedor de IA elegido: la descripción de la sesión y, si se sube, el documento (hasta 60 000 caracteres), con correos, teléfonos y DNI/NIE enmascarados. Antes del primer envío se muestra una muestra de lo que sale y hay que confirmarlo.
- El JSON exportado no contiene la clave. La página del alumnado no se conecta a ningún sitio y las respuestas de cada estudiante no salen de su ordenador.

## Límites

- Los agentes pueden equivocarse en el contenido. El código comprueba lo que se puede contar o cotejar, pero **no juzga el rigor científico ni la calidad didáctica**: eso lo orienta el revisor (también una IA) y lo decide siempre el profesorado. Revisa todo antes de usarlo en clase.
- La comprobación de cifras solo funciona si se sube un documento de referencia, e ignora los números de 12 o menos.
- En los PDF escaneados no se extrae texto (se pide pegarlo). La lectura de Word, PDF y PowerPoint necesita conexión la primera vez para cargar las librerías de lectura; escribir los ficheros no.
- No hay agentes que hablen ni pizarra en tiempo real (a diferencia de OpenMAIC): el destinatario es el profesorado, como en el cuaderno original.
- Probada con IA simulada de los tres proveedores (incluidas respuestas erróneas a propósito) y con el ejemplo; no se ha probado aún con claves reales en producción.

## Origen de la idea

AulIA generaliza el cuaderno de Colab **«MAIC-26 · Aula interactiva multiagente con la API de OpenAI»** de Federico Botella Beviá (Universidad Miguel Hernández de Elche), una reimplementación con el OpenAI Agents SDK inspirada en [OpenMAIC](https://github.com/THU-MAIC/OpenMAIC) (Universidad de Tsinghua). Conserva su arquitectura de agentes, su pipeline en dos etapas y su validación determinista, y la lleva a una app de navegador con tres proveedores, tres idiomas, edición, verificación ampliada y nuevas salidas (página del alumnado, Moodle, presentación).

## In English

AulIA turns a topic (and optionally the teacher's notes) into a complete class session designed by a team of AI agents and checked by code: editable plan, slides with a speaking script and code-drawn diagrams, quizzes, a project-based learning task with rubric, pedagogical review and a timed teacher's guide. It exports PowerPoint, Word, Moodle (GIFT/XML), JSON and a self-contained interactive student page. Interface in Spanish, English and Portuguese. Bring your own OpenAI, Gemini or Claude key; everything stays in the browser.

## Autoría

Fernando Borrás Rocher (Universidad Miguel Hernández de Elche) y Federico Botella Beviá (Universidad Miguel Hernández de Elche).

A partir del cuaderno «MAIC-26 · Aula interactiva multiagente» de Federico Botella Beviá, inspirado en OpenMAIC (Universidad de Tsinghua).

ORCID: Fernando Borrás Rocher [0000-0002-5519-4573](https://orcid.org/0000-0002-5519-4573) · Federico Botella Beviá [0000-0003-3631-5066](https://orcid.org/0000-0003-3631-5066)

## Cómo citar

Borrás Rocher, F. y Botella Beviá, F. (2026). *AulIA* (v1.0.0) [Software]. (DOI en trámite)

## Licencia

MIT. Véase [LICENSE](LICENSE).
