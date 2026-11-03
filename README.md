# TekoBot — tejuxi, tutor matemático en guaraní jopara

Proyecto desarrollado para el Hackathon GuaranIA.

> Aprendé matemática con tejuxi, la mascota de TekoBot: ojos morados, pañuelo verde y curiosidad por aprender.

---

##  Características principales

1. El Guaraní Jopara no es un accesorio: La IA enseña con una combinación natural de guaraní y español técnico cotidiano de las aulas paraguayas.
2. Enseñanza por estados: El profesor explica, resuelve ejemplos docentes y comprueba la comprensión antes de invitar a practicar. Las preguntas se pueden interrumpir y retomar sin reiniciar la lección.
3. Dual Coding (fórmula + gráfico sincronizado): Cada explicación vincula en tiempo real fórmulas en KaTeX con la circunferencia trigonométrica interactiva en Canvas y el graficador de ondas sinusoidales.
4. Detección exacta de errores y calidez pedagógica: Identifica si el error es de cuadrante, signo, confusión entre seno y coseno o cálculo del ángulo de referencia, sin emitir juicios negativos.
5. Andamiaje de 5 niveles de pistas: Desde un recordatorio conceptual sutil hasta procedimientos parciales guiados.
6. Resiliencia offline: El visualizador en Canvas, KaTeX y el banco de ejercicios funcionan incluso sin conexión a internet.
7. Evaluación diagnóstica: Módulos de Pre-Test y Post-Test para registrar la evolución del aprendizaje.

---

##  Cómo ejecutar el proyecto

### Opción 1: Con el servidor integrado de PHP (recomendado para pruebas rápidas)

Asegurate de tener PHP instalado (o abrir la terminal de XAMPP) y ejecutar en la carpeta del proyecto:

```bash
php -S localhost:8000
```

Luego, abrí tu navegador en: http://localhost:8000

### Opción 2: Con XAMPP / Apache / MySQL

1. Copiá esta carpeta dentro del directorio `htdocs` de XAMPP (por ejemplo, `C:\xampp\htdocs\math-tutor-ai`).
2. Iniciá los módulos de Apache y MySQL desde el panel de control de XAMPP.
3. Importá los archivos `database/schema.sql` y `database/seed_data.sql` desde phpMyAdmin (o dejá que la aplicación los inicialice automáticamente).
4. Abrí tu navegador en: http://localhost/math-tutor-ai

---

##  Configuración de la API Key de Gemini (opcional)

Para activar las respuestas generativas avanzadas con Google Gemini:

1. Abrí el archivo `.env` en la raíz del proyecto.
2. Ingresá tu clave:

```env
GEMINI_API_KEY="tu_clave_aqui"
```

Nota: Si no tenés una clave en este momento, la aplicación cuenta con un motor inteligente local en Guaraní Jopara que responderá todas las preguntas de la demo.

---

## Estructura del código

- `/api`: Endpoints REST (`chat.php`, `diagnostic.php`, `progress.php`, `exercises.php`).
- `/assets`: Estilos CSS modernos, KaTeX y scripts de interacción.
- `/config`: Conexión de base de datos PDO (MySQL con fallback automático a SQLite) y cargador `.env`.
- `/database`: Esquema SQL normalizado y datos iniciales de ejercicios.
- `/modules/trigonometry`: Circunferencia trigonométrica en Canvas (`unit_circle.js`), graficador de funciones (`graphs.js`) y banco local (`exercises_data.js`).
- `/prompts`: Prompt de profesor adaptativo en Guaraní Jopara y taxonomía de errores.
- `/services`: Servicios de backend (`ai_service.php`, `math_service.php`, `progress_service.php`).
- `/views`: Interfaz Split-Screen responsiva y modal de diagnóstico.

## Unidades y enseñanza (etapa 2)

El selector organiza 56 subtemas en cuatro unidades del índice proporcionado:

- Funciones.
- Funciones trigonométricas.
- Línea recta.
- Análisis combinatorio.

Cada conversación guarda el subtema y el punto de la explicación.

El catálogo fuente está en `database/curriculum.json`. Ejecutar `python scripts/build_curriculum.py` después de editarlo actualiza la copia local.

Ver `docs/etapa-2.md` para conocer el alcance de esta entrega y los comandos de pruebas.

La práctica adaptativa completa y el progreso por evidencias continúan en las etapas siguientes.

## Libro, problemas propios y exploración

El catálogo incorpora el libro MEC 2016 de 152 páginas, con enlaces a las páginas originales y extractos para contextualizar las explicaciones.

Las tres lecciones de funciones inversas son ampliaciones didácticas explícitas (59 subtemas en total).

Para regenerar el catálogo, ejecutar los siguientes comandos:

1. `scripts/import_mec_book.py` con el PDF original.
2. `scripts/upgrade_learning.py`.
3. `scripts/build_curriculum.py`.

Marcá «Che ejercicio · Mi problema» para escribir un enunciado libre. La guía conserva el enunciado al pedir pistas y reserva el resultado y la operación inmediatamente anterior.

La IA recibe una política específica y sus pasos pasan por un filtro que rechaza expresiones numéricas y algebraicas. Si la respuesta no cumple, se utiliza una guía conceptual local.

Sin conexión o sin Gemini, la guía indica su alcance y orienta por tema, sin simular una resolución personalizada.

La opción «Volver a la lección» termina este modo.

El gráfico permite modificar la pendiente y la ordenada. El selector trigonométrico incluye seno, coseno, tangente, arcsen, arccos y arctan; las funciones inversas muestran radianes en el eje vertical.

La barra lateral contiene tablas de valores, signos y recorridos principales.

La entrada del chat permite utilizar texto y fotografías de ejercicios. Las funciones de voz, dictado y lectura en voz alta fueron retiradas.

Pruebas adicionales: `node tests/learning_upgrade_browser.cjs`, con el servidor de pruebas descrito en `tests/web_router.php`.

## Preguntas adaptadas al concepto

Las consultas utilizan un catálogo compartido (`question_bank` en `database/curriculum.json`), generado mediante `scripts/build_question_bank.py` y `scripts/build_curriculum.py`.

El navegador y PHP reconocen conceptos y alias, comparaciones, ejemplos, causas, aplicaciones, dominios, signos y referencias al concepto anterior.

Una consulta no cambia la lección seleccionada ni su progreso. La fórmula y el gráfico corresponden al concepto consultado cuando hay un recurso visual disponible.

Ante una pregunta no reconocida, el modo local solicita una aclaración; no pretende comprender cualquier enunciado libre.

Gemini recibe los conceptos de la consulta, en lugar de quedar limitado al subtema abierto.

Los problemas propios conservan su política de ayuda sin mostrar resultados finales.

Pruebas: `node tests/question_resolver_test.cjs` comprueba respuestas y estados equivalentes entre PHP y JavaScript; `node tests/question_browser.cjs` verifica el flujo en servidor y offline, incluyendo la recarga y el regreso a la lección.

## Registros lingüísticos y JOPAMATH

El tutor distingue entre jopara pedagógico, español paraguayo coloquial y español neutro, tanto con servidor como offline.

El adaptador aprovecha el glosario, los contextos pedagógicos y el principio de protección matemática de JOPAMATH.

Las fórmulas y los datos se conservan mientras cambia la redacción.

Ver `docs/jopamath-integration.md` para consultar las fuentes, el alcance y las pruebas.

El catálogo editable es `database/language_profiles.json`. Después de modificarlo, ejecutar `python scripts/build_languages.py`.

## Respuestas locales adaptadas a la pregunta

El motor distingue definiciones breves, explicaciones sencillas, ejemplos, causas, usos, gráficos, signos, dominio y recorrido (también admite consultas combinadas).

Las repreguntas como «me lo explicás más fácil», «¿y su recorrido?» o «¿me das otro ejemplo?» conservan el concepto anterior. Nombrar otro concepto cambia la consulta sin modificar la lección.

Se admiten abreviaturas como `q`, `pq`, `xq` y variantes ortográficas acotadas del catálogo.

Las definiciones ya no agregan automáticamente bloques generales de la unidad. Si falta una explicación específica, se solicita una aclaración y no se sustituye por contenido ajeno.

Los estados antiguos de preguntas sin contexto también solicitan aclaraciones.

Las preguntas conceptuales con ángulos no se convierten automáticamente en problemas de cálculo, y las dudas durante la práctica conservan el ejercicio pendiente.

Esto sigue siendo un sistema de recuperación local de un catálogo, no una comprensión irrestricta de cualquier pregunta.

Para agregar contenidos, editar `scripts/build_question_bank.py` y regenerar con ese script y `scripts/build_curriculum.py`.

Pruebas: `node tests/adaptive_questions_test.cjs` y `node tests/question_browser.cjs` (servidor de pruebas en el puerto 8127), además de las pruebas de paridad e idiomas.
