AI Study Bot
Bot de Telegram con IA que lee tus notas de Obsidian desde Google Drive y responde preguntas sobre tu curso.

Mostrar imagen Mostrar imagen Mostrar imagen Mostrar imagen

✨ ¿Qué hace?
Tienes apuntes en Obsidian (.md) sincronizados en Google Drive. Le preguntas algo a tu bot de Telegram y él:

Busca en tus archivos el contenido relevante
Lo lee usando la API de Google Drive
Responde con Claude AI basándose SOLO en tus notas
Solo responde a ti (privado por Chat ID)
Tú → Telegram → Make → Google Drive → Claude AI → Telegram → Tú
🏗️ Arquitectura
┌─────────────┐     ┌──────────────┐     ┌──────────────┐
│  Telegram   │────▶│     Make     │────▶│ Google Drive │
│    Bot      │     │  AI Agent    │     │  API (HTTP)  │
└─────────────┘     └──────┬───────┘     └──────────────┘
                           │
                    ┌──────▼───────┐
                    │  Claude AI   │
                    │  Haiku 4.5   │
                    └──────────────┘
🧩 Componentes
Componente	Uso
Telegram Bot	Interfaz de chat
Make.com	Orquestador del flujo
Make AI Agent	Motor de IA (Claude Haiku 4.5)
Google Drive API	Lectura de archivos .md
Google OAuth 2.0	Autenticación segura
Obsidian	Donde escribes tus notas
🚀 Cómo montarlo tú mismo
Requisitos previos
Cuenta en Make.com (plan gratuito sirve)
Cuenta de Google con Google Drive
Telegram en tu móvil
Obsidian instalado (opcional pero recomendado)
PASO 1: Crear tu Bot de Telegram
Abre Telegram y busca @BotFather
Escribe /newbot
Dale un nombre y username (debe terminar en _bot)
Copia el TOKEN que te da
PASO 2: Preparar Google Drive
Instala Google Drive for Desktop
Crea una carpeta en Drive para tus notas, por ejemplo: Mi Bóveda/Curso/
Mete tus archivos .md dentro (o cualquier archivo de texto)
PASO 3: Configurar Google Cloud (OAuth)
Ve a console.cloud.google.com
Crea un proyecto nuevo
Activa la Google Drive API
Ve a "Credenciales" → "Crear credenciales" → "OAuth 2.0"
Tipo: Aplicación web
En URIs de redireccionamiento añade:
   https://www.integromat.com/oauth/cb/oauth2
   https://eu1.make.com/oauth/cb/oauth2
Copia el Client ID y Client Secret
En "Pantalla de consentimiento" → añádete como usuario de prueba
PASO 4: Montar el flujo en Make
Importa el blueprint bot-tajamar-blueprint.json de este repositorio:

En Make → "Scenarios" → "Create a new scenario"
Click en los tres puntos (···) → "Import Blueprint"
Sube el archivo bot-tajamar-blueprint.json
Luego configura las conexiones:

Módulo	Qué conectar
Telegram Bot	Tu token de BotFather
Google Drive	Tu cuenta de Google
HTTP OAuth	Tu Client ID y Client Secret
Make AI Agent	Se conecta automáticamente
PASO 5: Personalizar el escenario
En el módulo Make AI Agent, cambia las Instructions:

Eres un asistente de estudio especializado en [TU CURSO].

Tu rol:
- Responder preguntas sobre el curso de forma clara y educativa
- Usar las notas del estudiante como referencia
- Explicar conceptos de forma didáctica
- Si no encuentras información, di "No encontré eso en tus notas"

IMPORTANTE: Tienes dos herramientas HTTP:
1. Úsala primero para BUSCAR el archivo en Google Drive
2. Úsala después para DESCARGAR y leer su contenido
SIEMPRE usa ambas herramientas antes de responder.

Sé amable, paciente y didáctico. Responde en español.
PASO 6: Añadir seguridad (solo tú puedes usarlo)
Envía un mensaje a tu bot desde Telegram
En Make, ejecuta el escenario una vez y busca tu Chat ID en los outputs del módulo Telegram
En el filtro entre Telegram y AI Agent, pon tu Chat ID:
   {{6.message.chat.id}} == TU_CHAT_ID
PASO 7: Activar y probar
En Make, activa "Immediately as data arrives"
Abre Telegram y escribe a tu bot
¡Debería responder basándose en tus notas!
📁 Estructura de archivos recomendada en Drive
Mi Bóveda/
└── Mi Curso/
    ├── Módulo 1/
    │   ├── 01-introduccion.md
    │   └── 02-conceptos.md
    ├── Módulo 2/
    │   └── 03-practica.md
    └── Certificaciones/
        └── examen-prep.md
⚙️ Variables a configurar
Variable	Dónde	Descripción
BOT_TOKEN	Módulo Telegram	Token de BotFather
CHAT_ID	Filtro de seguridad	Tu Chat ID personal
CLIENT_ID	Conexión OAuth	Google Cloud Client ID
CLIENT_SECRET	Conexión OAuth	Google Cloud Client Secret
FOLDER_ID	URL de búsqueda HTTP	ID de tu carpeta en Drive
💰 Coste aproximado
Servicio	Plan	Coste
Make.com	Gratuito	0€ (1000 ops/mes)
Make AI Agent	Incluido en Make	0€
Google Drive	Gratuito	0€
Telegram Bot	Gratuito	0€
TOTAL		0€/mes
🔐 Seguridad
✅ Filtro por Chat ID (solo tú puedes usarlo)
✅ Google OAuth 2.0 (acceso seguro a Drive)
✅ App en modo prueba (solo usuarios autorizados)
✅ 2FA activado en Make
✅ Bot no puede unirse a grupos
🛠️ Posibles mejoras
 Soporte multi-idioma
 Comando /resumen para resumir la última clase
 Comando /quiz para generar preguntas de repaso
 Notificaciones programadas de repaso
 Soporte para imágenes y PDFs
📸 Demo
El bot buscando y leyendo notas reales de Obsidian y respondiendo en Telegram

🤝 Contribuciones
¡Las PRs son bienvenidas! Si lo montas y le añades mejoras, manda un pull request.

📄 Licencia
MIT — úsalo libremente para tu propio estudio.

Creado como proyecto personal para el Máster de IA y Data Engineering de Tajamar



