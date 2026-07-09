Entregable Final: Publicación de Blog Técnico y Post-Mortem
Estudiante: [Leandro Weimann]
Curso: Mentalidad de Crecimiento y Comunicación en Entornos Digitales

Crónica de una Caída y cómo un Error nos hizo un Mejor Equipo
¿Alguna vez has sentido ese frío en el estómago cuando un sistema deja de funcionar y los usuarios no pueden ingresar? Hace unas semanas, nuestro equipo vivió un desafío técnico que parecía caótico, pero que terminó transformándose en una de nuestras mayores oportunidades de aprendizaje.
Hoy quiero contarte qué pasó, cómo lo solucionamos sin buscar culpables y qué aprendimos en el proceso.
1. Contexto
Nuestro proyecto consiste en una plataforma web de gestión escolar donde los docentes suben calificaciones y los alumnos consultan sus promedios en tiempo real. El entorno de desarrollo utiliza Git para el control de versiones y desplegamos de manera continua. Es un sistema crítico, especialmente durante las semanas de exámenes finales, donde el tráfico se triplica.
2. El Problema (¿Qué falló?)
Durante el último día de carga de notas, la base de datos colapsó y la plataforma quedó completamente inaccesible durante 45 minutos.
Para nuestra audiencia no técnica: imaginen que la escuela tiene un solo gran libro de registros donde todos los profesores intentan escribir al mismo tiempo. El libro es tan pesado y la fila tan larga que el pasillo se bloqueó y nadie pudo entrar al edificio. Técnicamente, una consulta mal optimizada (un bucle anidado que consultaba la base de datos por cada alumno individualmente en lugar de traer los datos en lote) saturó las conexiones disponibles de nuestro servidor SQL.
3. Acciones Tomadas y Post-Mortem Constructivo
Siguiendo la filosofía de Google SRE, realizamos un análisis post-mortem constructivo enfocado en el proceso y no en las personas.
Cronología del Incidente (Descripción Objetiva)
14:00 – Se detecta una degradación del tiempo de respuesta (las páginas tardan más de 10 segundos en cargar).
14:15 – La plataforma se cae por completo (Error 504 Gateway Timeout). El equipo técnico es alertado.
14:30 – Identificamos que el pico de tráfico coincidió con un cambio de código reciente en el módulo de reportes.
14:45 – Aplicamos un rollback (volver a la versión anterior del código que funcionaba bien). La plataforma vuelve a estar en línea.
Análisis de Causa Raíz
La causa no fue "el desarrollador que subió el código", sino la falta de una prueba de rendimiento con alta carga de usuarios y la ausencia de una revisión cruzada (Peer Review) obligatoria en ese cambio específico de código.
Acciones Correctivas y Preventivas
Optimización inmediata: Se reescribió la consulta de base de datos para traer los registros agrupados (reduciendo las consultas en un 90%).
Protección de Ramas: Configuramos el repositorio en GitHub para que nadie pueda subir cambios directamente a la rama principal (main) sin la aprobación de al menos un compañero.

4. Aprendizajes Clave
El error es un activo: En lugar de señalar con el dedo, el equipo se unió para encontrar la falla. Descubrimos una debilidad en nuestro flujo de trabajo que ahora está solucionada.
Documentación viva: Registramos este incidente en nuestra base de conocimientos en Notion para que cualquier desarrollador nuevo que se sume al equipo entienda cómo prevenir este cuello de botella en el futuro.



5. Reflexión: Feedback Radicalmente Sincero en Acción
Durante la resolución de este problema, aplicamos el concepto de Feedback Radicalmente Sincero (basado en el modelo de Kim Scott: Cuidar de forma personal, desafiar de manera directa).
Al analizar la falla, un compañero me señaló de forma directa que mi mensaje de confirmación de cambios (commit) anterior era demasiado vago ("cambios varios") y que eso retrasó la identificación del error por 10 minutos.
Recibir esa crítica dolió un poco al principio, pero entendí que venía desde un lugar de cuidado mutuo para mejorar el equipo. Acepté el feedback con empatía, y a partir de ahí establecimos una plantilla obligatoria para documentar cada cambio en Git. La transparencia y la honestidad directa nos ahorraron horas de frustración.
