# Support_Logic_AI
Support_Logic_AI es un "empleado digital" que trabajará para ti 24/7 sin pedir vacaciones, sin dormir y respondiendo al instante.
Este pack contiene el cerebro de un Agente de Soporte Nivel 1 diseñado para integrarse en Slack, leer tu documentación interna y resolver dudas de tu equipo o clientes automáticamente. Si no sabe la respuesta, avisará a un humano.
Este pack es "Plug & Play": importa los archivos, pega tus claves y olvídate.

🛠️ REQUISITOS PREVIOS (ANTES DE EMPEZAR)
Para que este robot funcione, necesitas tener preparados estos ingredientes:
Una cuenta en Make (antes Integromat): Donde vive el cerebro del robot. (El plan gratuito suele ser suficiente para empezar).
https://www.make.com/en
Una cuenta en OpenAI (API Key): Necesaria para que el bot "piense". (Es una clave que permite a Make hablar con la IA).
https://platform.openai.com/settings/organization/api-keys
Permisos de Administrador en Slack: Necesitas poder crear una "App" en tu espacio de trabajo.
Google Drive y Sheets: Donde vivirá tu "Base de Conocimiento" (Manual de respuestas).

🗂️ CONTENIDO DEL PACK
Los archivos de esta descarga:
📄 Guia_Instalacion.pdf (Este documento).
🧠 Support_Logic_AI_v02.blueprint (El cerebro del robot para importar en Make).
📊 kb_incidencias_documentadas (La memoria del robot). Accede desde este enlace

🚜 PARTE 1: PREPARANDO EL TERRENO (SLACK)
Antes de tocar Make, necesitamos crear la identidad de tu robot en Slack.
PASO 1: Crear la App
Ve a api.slack.com/apps e inicia sesión.
Haz clic en Create New App > From Scratch.
Ponle un nombre (ej: "Soporte IA") y selecciona tu espacio de trabajo.
PASO 2: Permisos del Bot (Scope)
En el menú lateral izquierdo, ve a OAuth & Permissions.
Baja hasta la sección Bot Token Scopes.
Añade estos permisos (Scopes) obligatorios:
channels:read: Para poder leer la información básica del canal (nombre, ID) y saber dónde está.
chat:write: Para poder enviar respuestas.
chat:write.customize: 🎭 Crítico. Para cambiar su nombre y foto (Soporte IA) en cada mensaje.
chat:write.public: ✨ Vital. Permite al bot responder en canales donde no es miembro (ahorra tener que invitarlo).
users:read: Para saber quién es la persona que pregunta (y poder poner su ID en el fichero de registro).

Sube arriba del todo y dale al botón verde: Install to Workspace.

PASO 3: Obtener tu "Llave Maestra"
Una vez instalada, en esa misma página verás un código que empieza por xoxb-....
Cópialo y guárdalo. Esta es la llave de tu robot.

🧠 PARTE 2: INSTALANDO EL CEREBRO (MAKE)
Su misión: Leer Slack, buscar en el fichero, preguntar a la IA y responder.
PASO 1: Importar el Blueprint
Abre Make y crea un nuevo escenario.
Haz clic en los tres puntos (abajo) > Import Blueprint.

Selecciona el archivo Support_Logic_AI_v02.blueprint que has descargado.
Verás aparecer todo el esquema visual conectado.

PASO 2: La Base de Conocimiento
Abre el archivo kb_incidencias_documentadas en tu Google Drive.
Verás que contiene 2 pestañas. En la primera está la base de datos de incidencias/respuestas que el bot tomará como base de conocimientos. En la segunda es donde el bot registra automáticamente el log cada vez que un usuario le pregunta algo. Es vital para ti porque:
Control de Calidad: Puedes revisar si el bot ha respondido correctamente.
Detección de Necesidades: Si ves que 10 personas preguntan "Cómo borrar las cookies del navegador", quizás deberías enviar un recordatorio a todo el equipo.
Seguridad: Tienes un registro de quién pregunta qué y cuándo.
Consejo: No borres las columnas de esta pestaña, el bot las necesita para escribir.
Ábrelo y rellénalo con tus preguntas frecuentes (Columna A) y respuestas (Columna B).
En Make, abre los tres módulos de Google Sheets (tanto el de búsqueda como los de registro).
Conecta tu cuenta de Google y selecciona TU archivo que acabas de subir.
PASO 3: Conectar la Inteligencia (OpenAI)
Abre el módulo de OpenAI.
En "Connection", dale a "Add" y pega tu API Key de OpenAI.
Selecciona el modelo, te recomiendo gpt-5.2 (es rápido y barato para esto).
⚙️ PARTE 3: CONFIGURACIÓN CRÍTICA (EL SECRETO) ⚠️
Para garantizar la máxima seguridad y que el bot tenga personalidad propia, hemos configurado una conexión profesional vía HTTP.
PASO 1: Configurar la "Boca" del Robot
Ve al final del escenario en Make. Verás dos módulos verdes llamados HTTP Request.
Ábrelos (tendrás que hacerlo en los dos).
En la sección Headers, verás un campo que dice [PEGAR_TOKEN_AQUI].

Borra ese texto y pega tu token de Slack (el que empieza por xoxb-...).
⚠️ OJO: Mantén la palabra Bearer y el espacio que hay delante. Debe quedar así: Bearer xoxb-1234...
PASO 2: Definir el Canal
Ve a Slack, haz clic derecho sobre el canal donde quieres que trabaje el bot y selecciona la opción “Ver información del canal”
Copia el código ID del canal (ej: C0ADZ2RW64A).
En los módulos HTTP de Make, busca el campo channel.
Sustituye el texto [PEGAR_CHANNEL_ID_AQUI] por el ID de tu canal. 

🚦 CÓMO PROBAR QUE FUNCIONA
Vamos a hacer un simulacro real:
En Make, dale al botón Run Once (Play).
Ve a Slack y escribe una pregunta que SÍ esté en tu fichero.
¡Magia! ✨ El bot debería responderte en el hilo con la solución.
Ahora escribe una pregunta que NO esté en el fichero (ej: "¿Cuál es la clave de Netflix?").
El bot debería responderte diciendo que no tiene esa información y mencionando al supervisor.
Por defecto, el mensaje predefinido para este supuesto es: “Incidencia no encontrada en mi base de conocimientos. Por favor, @team_leaders, revisad el caso.” pero se puede cambiar en el módulo HTTP “🚨 Respuesta: Escalar” dentro del campo Body content > Field 2 > Value:

🆘 SOLUCIÓN DE PROBLEMAS FRECUENTES (FAQ)
P: El bot responde con mi nombre de usuario en vez de como "Robot". 
R: Revisa la Parte 1, Paso 2. Es probable que te falte el permiso chat:write.customize en Slack o que no hayas reinstalado la App después de añadirlo.
P: Me da error "401 Not Authed" en el módulo HTTP. 
R: Revisa el campo Authorization. Asegúrate de que has escrito la palabra Bearer (con mayúscula inicial) seguida de un espacio y luego tu token xoxb.
P: La IA se inventa respuestas. 
R: En el módulo de OpenAI, asegúrate de que la Temperatura está baja (0.2). Esto obliga a la IA a ser estricta y basarse solo en los ejemplos de incidencias registradas en el fichero.

P: ¿Puedo añadir más columnas al Excel de la base de conocimientos? 
R: Sí, puedes usar las columnas C, D, E... para tus notas internas. Pero IMPORTANTE: No cambies el nombre de las columnas A (Pregunta) y B (Respuesta), ya que el escenario de Make las busca específicamente por ese nombre. Si las renombras, tendrás que volver a conectarlas en el módulo de Google Sheets.
P: Quiero que el bot responda en otro idioma o cambie su "tono". 
R: ¡Es muy fácil! Ve al módulo central OpenAI (Cerebro IA) en Make. En el campo System Message (donde le damos las instrucciones), puedes añadir: "Responde siempre en Inglés" o "Usa un tono muy formal/divertido". Tú controlas su personalidad desde ahí.
P: El bot no responde en mis canales privados (con el candado 🔒). 
R: Por diseño y privacidad, el bot no "cotillea" en canales privados automáticamente. Para que funcione ahí, tienes que invitarlo explícitamente. Entra al canal privado y escribe: /invite @NombreDeTuBot. Una vez dentro, te responderá igual que en los públicos.

🎓 ENTENDIENDO LA LÓGICA
Ruta de Resolución: Si la IA encuentra la respuesta en tu fichero, usa el camino superior y responde al usuario, registrando el éxito en la pestaña "Logs".
Ruta de Escalado: Si la IA no sabe la respuesta, usa el camino inferior ("Fallback"), avisa al usuario amablemente y etiqueta al humano responsable.
