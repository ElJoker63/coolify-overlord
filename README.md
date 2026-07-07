# 😈 Coolify Overlord

**Domina tu Coolify desde Hermes Agent**

Un skill potente para gestionar tu instancia Coolify directamente desde tu asistente Hermes. Sin límites, sin complicaciones.

---

## 🚀 ¿Qué hace?

Coolify Overlord te permite controlar tu PaaS auto-hospedado usando solo comandos naturales:

- **📊 Monitoreo en tiempo real** — Estado de apps, servicios y servidores
- **🔄 Gestión completa** — Restart, stop, start, deploy de cualquier app
- **🔍 Búsqueda inteligente** — Encuentra apps por nombre sin saber el UUID
- **📋 Variables de entorno** — Lee, edita y gestiona env vars
- **🖥️ Servidores** — Información y estado de tus servidores
- **🚀 Deployments** — Trigger builds y monitorea el progreso
- **📈 Auditoría** — Reportes completos de tu infraestructura

---

## 📦 Instalación

### Opción 1: Git clone (recomendado)

```bash
cd ~/.hermes/skills/
git clone https://github.com/ElJoker63/coolify-overlord.git
```

### Opción 2: Copia manual

```bash
cp -r coolify-overlord/coolify ~/.hermes/skills/
```

### Opción 3: Via Hermes CLI

```bash
hermes skill install coolify-overlord
```

---

## ⚙️ Configuración

### 1. Obtén tu API token de Coolify

1. Abre tu dashboard de Coolify
2. Ve a **Security → API Tokens**
3. Crea un token con permisos de lectura/escritura
4. Copia el token

### 2. Configura las variables de entorno

Agrega a tu archivo `~/.env`:

```bash
COOLIFY_URL=http://tu-ip:8000
COOLIFY_TOKEN=tu-api-token-aqui
```

O exporta directamente:

```bash
export COOLIFY_URL="http://tu-ip:8000"
export COOLIFY_TOKEN="tu-api-token-aqui"
```

### 3. Verifica la conexión

```bash
curl -s -H "Authorization: Bearer $COOLIFY_TOKEN" "$COOLIFY_URL/api/v1/version"
```

Debería devolver la versión de Coolify (ej: `4.1.2`).

---

## 💬 Uso

Una vez instalado, simplemente pregúntale a Hermes:

### Estado general
- "¿Qué apps están caídas en Coolify?"
- "Muéstrame el estado de todos los servicios"
- "¿Cuántos servidores tengo?"

### Gestión de apps
- "Reinicia la app UDYAT BOT"
- "Detén la app ANIME-BOT-AUTO"
- "Inicia la app DOWNTIFY"
- "Haz deploy de APIUDYAT"

### Búsqueda
- "Busca la app que se llama Music"
- "¿Qué UUID tiene BOT CARLOS?"

### Información
- "¿Qué env vars tiene UDYAT STORE?"
- "Dame los detalles de la app XFilmo"

### Servidores
- "¿Cuál es la IP de mi servidor?"
- "Muéstrame los servidores configurados"

---

## 🔧 Comandos Disponibles

### Aplicaciones
| Comando | Descripción |
|---------|-------------|
| `listar apps` | Lista todas las aplicaciones |
| `estado app [nombre]` | Muestra estado de una app |
| `reiniciar app [nombre]` | Reinicia una aplicación |
| `detener app [nombre]` | Detiene una aplicación |
| `iniciar app [nombre]` | Inicia una aplicación |
| `deploy [nombre]` | Trigger de build/deploy |

### Servicios
| Comando | Descripción |
|---------|-------------|
| `listar servicios` | Lista todos los servicios |
| `reiniciar servicio [nombre]` | Reinicia un servicio |
| `detener servicio [nombre]` | Detiene un servicio |

### Servidores
| Comando | Descripción |
|---------|-------------|
| `listar servidores` | Lista todos los servidores |
| `detalles servidor [nombre]` | Info detallada del servidor |

### Variables de entorno
| Comando | Descripción |
|---------|-------------|
| `env vars [app]` | Lista variables de una app |

---

## 📋 Ejemplos

### Ver todas las apps con estado

```
Hermes: ¿Qué apps tengo en Coolify?

Resultado:
✅ UDYAT BOT — running:healthy
✅ BOT CARLOS — running:healthy
❌ ANIME-BOT-AUTO — exited:unhealthy
✅ APIUDYAT — running:unknown
...
```

### Reiniciar una app

```
Hermes: Reinicia BOT CONFESIONES

Resultado:
🔍 Buscando BOT CONFESIONES...
UUID: tg8o8w0s...
🔄 Reiniciando BOT CONFESIONES...
✅ BOT CONFESIONES reiniciada correctamente
```

---

## 🛡️ Seguridad

- Las credenciales se almacenan en variables de entorno (nunca en el código)
- El skill solo realiza operaciones autorizadas
- Todas las llamadas API usan HTTPS cuando está disponible
- Los tokens se transmiten de forma segura en headers de autorización

---

## 🐛 Troubleshooting

### Conexión fallida
- Verifica que la URL sea correcta (incluye puerto si no es 80/443)
- Comprueba que el token sea válido
- Prueba manualmente: `curl -H "Authorization: Bearer $TOKEN" $URL/api/v1/version`

### Permiso denegado
- Asegúrate de que el token tenga permisos de lectura/escritura
- Verifica que el token no haya sido revocado

### App no inicia
- Revisa los logs en el dashboard de Coolify
- Verifica que las variables de entorno estén correctas
- Comprueba si el puerto está en uso

---

## 📚 API Reference

Documentación completa de la API de Coolify: https://coolify.io/docs/api

---

## 🤝 Contribuir

Las contribuciones son bienvenidas. Abre un PR o issue en GitHub.

---

## 📄 Licencia

MIT License - VER [LICENSE](coolify/LICENSE)

---

## 👨‍💻 Autor

**ElJoker63** - [GitHub](https://github.com/ElJoker63)

---

> *"Domina tu nube, domina tu destino"* 😈
