# 01. Datos y sistemas clínicos

## 1. Propósito de la capa

### 1.1. Qué cubre esta capa

Esta capa se centra en la base real sobre la que después se construyen el resto de componentes de una arquitectura de IA en sanidad: los datos clínicos disponibles y los sistemas que los generan, almacenan o transforman.

Su objetivo no es todavía resolver cómo se conectan todos los sistemas entre sí ni cómo se exponen hacia aplicaciones externas, sino entender con qué información se cuenta realmente, qué calidad tiene, qué limitaciones presenta y qué trabajo previo hace falta para convertirla en una base utilizable.

En otras palabras, esta capa intenta responder a una pregunta muy simple: antes de aplicar IA, ¿qué datos clínicos existen realmente y en qué estado se encuentran?

### 1.2. Por qué condiciona el resto de la arquitectura

Muchas iniciativas de IA en sanidad se enfocan muy pronto en el modelo, en el copiloto o en la interfaz. Sin embargo, en la práctica, una parte importante del éxito o del fracaso del sistema depende de algo más básico: la calidad, el contexto y la disponibilidad del dato clínico.

Si los datos son incompletos, inconsistentes, están desactualizados o carecen de contexto suficiente, el problema no se corrige después únicamente con mejores prompts, mejores modelos o más capacidad de cómputo. La arquitectura entera queda condicionada por esa base.

Por eso, esta capa no debe verse como un paso previo menor, sino como una condición de posibilidad para todo lo demás: analítica, soporte a decisión, sistemas RAG, agentes o cualquier otra aplicación posterior.

---

## 2. Fuentes clínicas habituales

### 2.1. Historia clínica electrónica

La historia clínica electrónica suele ser una de las principales fuentes de información longitudinal sobre el paciente. Puede incluir antecedentes, diagnósticos, episodios asistenciales, evolución clínica, listas de problemas, alergias y otra información relevante para contextualizar una recomendación o una predicción.

Su valor es alto, pero también suele presentar heterogeneidad en la forma de registrar la información, diferencias entre servicios y una combinación variable de campos estructurados y texto libre.

### 2.2. Laboratorio

Los sistemas de laboratorio aportan resultados objetivos, seriados y frecuentemente muy relevantes para el razonamiento clínico. Parámetros bioquímicos, microbiología, hematología o marcadores específicos pueden ser fundamentales para alimentar modelos o construir contexto clínico.

Aun así, un resultado de laboratorio rara vez se interpreta bien por sí solo. Su utilidad depende del momento en que se obtuvo, del episodio asistencial al que pertenece y del contexto clínico en el que se solicitó.

### 2.3. Imagen e informes asociados

La información procedente de radiología u otras áreas de imagen puede aportar evidencia diagnóstica muy valiosa. Sin embargo, en muchos entornos el valor utilizable para IA no se encuentra tanto en la imagen bruta como en el informe asociado, que con frecuencia está redactado en lenguaje natural.

Esto introduce un reto importante: una fuente muy rica clínicamente, pero muchas veces poco estructurada y difícil de reutilizar de manera directa.

### 2.4. Farmacia y prescripción

Los sistemas de prescripción y farmacia permiten conocer tratamientos activos, cambios terapéuticos, duración prevista de la medicación y, en algunos casos, información útil sobre administración o dispensación.

Son especialmente relevantes en casos de soporte a decisión, conciliación terapéutica o evaluación de riesgo, pero también pueden verse afectados por desfases temporales, tratamientos suspendidos no actualizados o diferencias entre prescripción y administración real.

### 2.5. Notas clínicas e informes en texto libre

Buena parte del contexto clínico relevante sigue viviendo en notas evolutivas, informes de alta, interconsultas, comentarios libres u otros documentos narrativos.

Estas fuentes suelen contener matices que no aparecen en los campos estructurados: hipótesis diagnósticas, justificaciones clínicas, cambios de criterio, dudas, evolución funcional o detalles que condicionan la decisión. Su problema es evidente: su riqueza semántica suele venir acompañada de mayor dificultad de procesamiento, extracción y validación.

### 2.6. Repositorios secundarios y bases analíticas

En algunos entornos existen repositorios secundarios, data warehouses, bases analíticas o entornos intermedios que consolidan información procedente de varios sistemas clínicos.

Estas capas pueden ser muy útiles para explotación de datos, analítica o preparación de casos de uso de IA, pero no deben asumirse automáticamente como una representación completa y fiel del dato clínico original. También pueden introducir transformaciones, pérdidas de granularidad o retrasos respecto a los sistemas fuente.

---

## 3. Problemas reales del dato clínico

### 3.1. Incompletitud

No toda la información necesaria está siempre disponible. Algunas variables no se registran, otras se registran solo en determinados contextos y otras existen, pero no son fácilmente accesibles para el caso de uso planteado.

Esta incompletitud no siempre es aleatoria. A veces depende del servicio, del profesional, del circuito asistencial o del tipo de paciente, lo que puede introducir sesgos relevantes.

### 3.2. Inconsistencia

La misma información puede aparecer de manera distinta en sistemas diferentes o incluso dentro del mismo sistema. Puede haber diferencias de formato, nomenclatura, unidades, estructura o codificación.

Esto dificulta tanto el análisis como la reutilización posterior, y obliga a revisar con cuidado qué significado tiene realmente cada variable antes de usarla en una aplicación de IA.

### 3.3. Fragmentación

El contexto clínico suele estar repartido entre varias fuentes. Ningún sistema por sí solo contiene toda la información relevante.

Un caso de uso puede necesitar antecedentes de la historia clínica, resultados del laboratorio, medicación activa de farmacia y matices presentes solo en texto libre. Esa fragmentación hace que trabajar con una sola fuente suela ser insuficiente.

### 3.4. Pérdida de contexto clínico

Un dato aislado puede ser técnicamente correcto y, aun así, resultar insuficiente o incluso engañoso si no se conoce su contexto. El valor de una variable clínica depende a menudo de cuándo se registró, en qué episodio, con qué sospecha diagnóstica y frente a qué evolución previa del paciente.

Sin ese contexto, el dato pierde parte de su capacidad interpretativa.

### 3.5. Desfase temporal

En sanidad, el tiempo importa. Una alergia desactualizada, una medicación antigua, una analítica no reciente o un diagnóstico pendiente de confirmación pueden afectar de forma importante a la interpretación.

La utilidad de una variable no depende solo de que exista, sino también de su actualidad y de su posición en la secuencia clínica.

### 3.6. Predominio del texto libre

En muchos sistemas, parte de la información más útil desde el punto de vista clínico sigue estando en formato narrativo. Eso enriquece la documentación asistencial, pero complica el uso automático del dato.

El texto libre no es un problema en sí mismo, pero sí obliga a asumir que parte del contexto no estará listo para consumo directo y requerirá procesamiento adicional.

### 3.7. Sesgos y diferencias de registro

No todo se registra igual, ni con el mismo nivel de detalle, ni en todos los pacientes. Algunos colectivos generan más documentación, ciertos procesos están mejor estructurados que otros y determinadas variables se recogen de forma desigual según el contexto asistencial.

Estos sesgos de registro pueden repercutir en la calidad del sistema resultante y deben identificarse, no asumirse como ruido menor.

### 3.8. Variabilidad semántica

Dos sistemas pueden referirse a conceptos muy parecidos con etiquetas distintas, y dos profesionales pueden documentar el mismo problema clínico de manera diferente.

La cuestión no es solo técnica, sino semántica: antes de usar el dato, hay que entender qué representa realmente, con qué alcance y con qué limitaciones.

---

## 4. Capacidades necesarias en esta capa

### 4.1. Inventario de fuentes

Una arquitectura seria debería comenzar identificando con claridad qué sistemas contienen la información relevante para el caso de uso. No basta con asumir que “los datos están en el hospital”; hay que localizar qué fuente contiene qué tipo de dato y con qué nivel de utilidad.

### 4.2. Definición de variables mínimas

Antes de construir nada, conviene definir qué variables son realmente necesarias. Esta definición depende del caso de uso y evita trabajar con una idea difusa de “todos los datos disponibles”.

Pensar en variables mínimas ayuda a acotar necesidades, detectar ausencias y priorizar esfuerzos de preparación.

### 4.3. Evaluación de calidad del dato

No todas las fuentes tienen el mismo nivel de calidad ni todas las variables presentan la misma fiabilidad. Conviene evaluar completitud, consistencia, actualidad, formato y posibles anomalías antes de dar por válida una base de datos para IA.

### 4.4. Contextualización temporal

Los datos clínicos deben leerse en secuencia. Una arquitectura útil debe poder situar las variables en el tiempo y relacionarlas con episodios, eventos y evolución clínica.

Sin esta contextualización, la interpretación puede degradarse rápidamente.

### 4.5. Trazabilidad del origen

Debería ser posible saber de qué sistema procede cada dato, cuándo fue registrado y, en su caso, qué transformaciones ha sufrido antes de llegar a una capa de consumo.

Esta trazabilidad no solo mejora la gobernanza posterior, sino que también ayuda a depurar errores y a interpretar mejor los resultados.

### 4.6. Identificación de ausencias y sesgos

Una parte del trabajo en esta capa consiste en hacer explícito lo que falta, lo que no está bien representado y lo que podría introducir sesgos.

No se trata de aspirar a un dato perfecto, sino de conocer suficientemente bien sus límites.

### 4.7. Separación entre dato fuente y dato preparado

En muchos casos conviene distinguir entre el dato tal como vive en los sistemas fuente y una versión ya preparada para consumo analítico o para IA.

Esa separación permite aplicar transformaciones, validaciones, controles de calidad y reglas mínimas sin comprometer directamente la lógica de los sistemas operacionales.

---

## 5. Preparación y armonización del dato

### 5.1. Extracción y consolidación

Cuando la información relevante está repartida entre varias fuentes, suele ser necesario extraerla y consolidarla en una capa donde pueda analizarse o consumirse de forma más coherente.

Esto no significa todavía resolver toda la interoperabilidad del ecosistema, sino reunir la información necesaria para que el caso de uso sea viable.

### 5.2. Transformación y limpieza

Una vez reunidos los datos, normalmente hace falta transformarlos: ajustar formatos, resolver duplicidades, revisar campos anómalos, gestionar valores ausentes o preparar estructuras más adecuadas para explotación posterior.

Sin esta limpieza mínima, muchas aplicaciones de IA terminan operando sobre una base demasiado frágil.

### 5.3. Normalización terminológica

En determinados casos puede ser necesario alinear terminologías, codificaciones o formas de representar conceptos clínicos.

La normalización no elimina toda la variabilidad, pero ayuda a reducir ambigüedades y facilita análisis posteriores más consistentes.

### 5.4. Modelos comunes de datos

En algunos entornos, especialmente cuando se busca reutilización secundaria del dato, analítica avanzada, investigación o comparabilidad entre instituciones, puede ser útil transformar la información hacia modelos comunes de datos, como OMOP u otros enfoques similares según el contexto.

Este tipo de iniciativas puede ayudar a armonizar información procedente de fuentes heterogéneas y a hacer más reproducible su explotación posterior. Aun así, exige esfuerzo de mapeo, validación y mantenimiento, por lo que conviene plantearlo cuando responde a una necesidad real y no como un añadido cosmético.

### 5.5. Capas analíticas intermedias

En muchos proyectos resulta más razonable construir una capa intermedia preparada para análisis o IA que conectar directamente los sistemas fuente a modelos o asistentes.

Estas capas analíticas permiten consolidar, documentar y validar mejor la información antes de utilizarla en aplicaciones más sensibles.

### 5.6. Validaciones previas al uso por IA

Antes de alimentar un sistema de IA, conviene comprobar al menos aspectos básicos: presencia de campos obligatorios, coherencia de identificadores, actualidad de la información, consistencia temporal y plausibilidad mínima de determinadas variables.

La idea no es bloquear cualquier uso hasta tener un dato perfecto, sino reducir errores evitables y dejar claro el nivel de confianza con el que se está operando.

---

## 6. Ejemplo práctico

### 6.1. Caso de uso

Supongamos un sistema de apoyo a la prescripción antibiótica empírica que intenta estimar patógenos probables y orientar una recomendación inicial en función de la situación clínica del paciente.

### 6.2. Datos necesarios

Para ello, el sistema podría necesitar:

* edad
* alergias
* comorbilidades relevantes
* foco infeccioso o diagnóstico de sospecha
* tipo de adquisición
* gravedad clínica
* resultados analíticos recientes
* microbiología previa
* exposición antibiótica reciente
* medicación activa
* notas clínicas recientes

### 6.3. Qué puede fallar

Incluso antes de cualquier razonamiento, pueden aparecer problemas como:

* diagnósticos poco específicos o no consolidados
* información crítica repartida entre varias fuentes
* antecedentes microbiológicos no recuperados a tiempo
* comorbilidades mal estructuradas
* diferencias entre tratamiento prescrito y situación clínica actual
* notas evolutivas con información relevante que no aparece en campos estructurados

### 6.4. Implicaciones para la IA

En una recomendación empírica, pequeños fallos de contexto pueden tener mucho impacto.

Si faltan datos sobre alergias, microbiología previa, comorbilidades o situación actual, la recomendación puede perder precisión o volverse clínicamente menos segura.

Por eso, en este tipo de sistemas, la capa de datos no es un soporte secundario: es parte central de la fiabilidad del sistema.

---

## 7. Checklist mínimo

### 7.1. Fuentes

* [ ] Se han identificado los sistemas que contienen la información relevante
* [ ] Se sabe qué fuente aporta cada variable necesaria
* [ ] Se conocen las limitaciones básicas de cada sistema fuente

### 7.2. Calidad

* [ ] Se ha revisado la completitud de las variables mínimas
* [ ] Se han identificado inconsistencias relevantes
* [ ] Se conoce el nivel de fiabilidad de los datos principales

### 7.3. Contexto

* [ ] Existe contexto temporal suficiente
* [ ] Las variables pueden relacionarse con episodios o eventos clínicos
* [ ] Se ha evaluado el impacto del texto libre en el caso de uso

### 7.4. Trazabilidad

* [ ] Puede identificarse el origen de cada dato
* [ ] Se conoce, al menos de forma básica, cuándo se registró cada información relevante
* [ ] Se han documentado las transformaciones aplicadas en capas intermedias

### 7.5. Preparación previa al consumo por IA

* [ ] Se ha definido un conjunto mínimo de datos para el caso de uso
* [ ] Se han aplicado validaciones básicas antes del consumo por IA
* [ ] Se ha diferenciado entre dato fuente y dato preparado
* [ ] Se han identificado ausencias, sesgos y limitaciones relevantes

---

## 8. Referencias y enlaces de interés

### 8.1. Marcos generales

* World Health Organization. *Ethics and governance of artificial intelligence for health*.
* NIST. *AI Risk Management Framework*.

### 8.2. Recursos técnicos útiles

* OHDSI / OMOP Common Data Model
* Documentación y recursos sobre calidad del dato en entornos clínicos
* Iniciativas de normalización terminológica y modelado secundario del dato

---

## Idea clave

Una IA no empieza en el modelo.

Empieza en la calidad del dato clínico, en su contexto y en la comprensión de los sistemas que lo producen.
