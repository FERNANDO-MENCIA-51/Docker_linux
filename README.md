# 🐳 Docker y Microservicios desde Cero

<div align="center">

![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![WSL2](https://img.shields.io/badge/WSL2-0078D4?style=for-the-badge&logo=windows&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)

### 🚀 Mi experiencia aprendiendo Docker con microservicios

*Documenté todo mi proceso de aprendizaje para que tú también puedas hacerlo*

</div>

---

## 📚 Lo que aprendí sobre Docker

Cuando empecé con Docker, me costó entender qué era exactamente. Después de mucho practicar, puedo decirte que **Docker** es como una "caja mágica" donde empaquetas tu aplicación con todo lo que necesita para funcionar. Lo genial es que esta caja funciona igual en cualquier computadora.

### 🎯 Por qué me enamoré de Docker:
- **Se acabó el "en mi máquina funciona"**: Si funciona en Docker, funciona en todas partes
- **Mismo entorno siempre**: No más problemas de "funciona en desarrollo pero no en producción"
- **Súper eficiente**: Los contenedores son mucho más ligeros que las máquinas virtuales
- **Fácil de escalar**: Necesitas más instancias? Solo ejecuta más contenedores
- **Cada app en su mundo**: Si una aplicación falla, no afecta a las demás

### 🏗️ Mi primer microservicio

Al principio pensaba que los microservicios eran súper complicados, pero resulta que es simplemente dividir una aplicación grande en pedacitos pequeños que hacen una sola cosa bien.

**Lo que me gustó de los microservicios:**
- Cada equipo puede trabajar en su pedacito sin molestar a otros
- Puedes usar Python para una parte y Node.js para otra
- Si necesitas más potencia en una función específica, solo escalas esa parte
- Si algo se rompe, solo se rompe esa función, no toda la aplicación

---

# Mi proceso paso a paso (que funcionó perfecto)

## 1. 🐧 Instalar Docker en Ubuntu (WSL2)

<div align="center">

![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=flat-square&logo=ubuntu&logoColor=white)
![WSL2](https://img.shields.io/badge/WSL2-0078D4?style=flat-square&logo=windows&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)

</div>

### Actualizar repositorios e instalar prerequisitos

```bash
sudo apt update
sudo apt install -y ca-certificates curl gnupg lsb-release
```

👉 **¿Qué hace este comando?**
- `apt update`: Actualiza la lista de paquetes disponibles en los repositorios
- `ca-certificates`: Instala certificados para conexiones HTTPS seguras
- `curl`: Herramienta para descargar archivos desde internet
- `gnupg`: Software para verificar firmas digitales y encriptar datos
- `lsb-release`: Proporciona información sobre la distribución de Linux

### Agregar clave y repositorio oficiales de Docker

```bash
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

👉 **¿Por qué es importante esto?**
- Añade la **firma oficial de Docker** para verificar que los paquetes son auténticos
- Configura el repositorio estable para que Ubuntu sepa de dónde descargar Docker
- La firma GPG garantiza que nadie ha modificado maliciosamente los paquetes

### Instalar Docker Engine

```bash
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io
```

👉 **¿Qué se instala?**
- `docker-ce`: Docker Community Edition (el motor principal de Docker)
- `docker-ce-cli`: La interfaz de línea de comandos (el comando `docker`)
- `containerd.io`: El runtime que gestiona los contenedores a bajo nivel

### Habilitar y arrancar el servicio de Docker

```bash
sudo systemctl enable --now docker
```

👉 **¿Qué hace este comando?**
- `enable`: Hace que Docker se inicie automáticamente cuando arranque el sistema
- `--now`: Arranca el servicio Docker inmediatamente
- Esto significa que Docker estará siempre disponible, incluso después de reiniciar

### Permitir usar Docker sin sudo

```bash
sudo usermod -aG docker $USER
newgrp docker
```

👉 **¿Por qué es necesario?**
- Te agrega al grupo `docker` para no tener que escribir `sudo` en cada comando
- `$USER` es tu nombre de usuario actual
- `newgrp docker` activa el grupo inmediatamente sin cerrar la sesión
- Sin esto, tendrías que escribir `sudo docker` en cada comando

## 2. 📁 Crear carpeta para el microservicio

```bash
mkdir ~/micro
cd ~/micro
```

👉 **¿Qué hace esto?**
- `mkdir ~/micro`: Crea un directorio llamado `micro` en tu carpeta home
- `cd ~/micro`: Cambia al directorio recién creado
- Aquí estarán todos los archivos de nuestro proyecto

## 3. 🐍 Crear archivo `app.py`

<div align="center">

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white)

</div>

```bash
cat > app.py <<'EOF'
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def hello():
    return {"message": "Hola desde mi microservicio en Docker"}
EOF
```

👉 **¿Qué es este código?**
- Es una aplicación web sencilla escrita en **FastAPI** (un framework moderno de Python)
- `@app.get("/")` define una ruta que responde a peticiones GET en la URL raíz
- Cuando alguien visite `http://localhost:8080/`, verá el mensaje JSON
- FastAPI automáticamente convierte el diccionario Python a formato JSON

## 4. 📋 Crear archivo `requirements.txt`

```bash
cat > requirements.txt <<'EOF'
fastapi
uvicorn
EOF
```

👉 **¿Para qué sirve este archivo?**
- Lista todas las librerías de Python que necesita nuestra aplicación
- `fastapi`: El framework web que usamos para crear la API
- `uvicorn`: El servidor web que ejecutará nuestra aplicación FastAPI
- Docker usará este archivo para instalar las dependencias dentro del contenedor

## 5. 🐳 Crear archivo `Dockerfile`

<div align="center">

![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![Alpine Linux](https://img.shields.io/badge/Alpine_Linux-0D597F?style=flat-square&logo=alpine-linux&logoColor=white)

</div>

```bash
cat > Dockerfile <<'EOF'
FROM python:3.12-alpine
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY app.py .
EXPOSE 8080
CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8080"]
EOF
```

👉 **¿Qué hace cada línea del Dockerfile?**
- `FROM python:3.12-alpine`: Usa como base una imagen de Python 3.12 en Alpine Linux (muy ligera)
- `WORKDIR /app`: Establece `/app` como directorio de trabajo dentro del contenedor
- `COPY requirements.txt .`: Copia el archivo de dependencias al contenedor
- `RUN pip install...`: Instala las librerías de Python listadas en requirements.txt
- `COPY app.py .`: Copia nuestro código de la aplicación al contenedor
- `EXPOSE 8080`: Informa que la aplicación usará el puerto 8080
- `CMD [...]`: Define el comando que se ejecutará cuando inicie el contenedor

## 6. 🔨 Construir la imagen Docker

```bash
docker build -t luis .
```

👉 **¿Qué hace este comando?**
- `docker build`: Construye una imagen Docker siguiendo las instrucciones del Dockerfile
- `-t luis`: Asigna el nombre "luis" a la imagen (puedes usar cualquier nombre)
- `.`: Indica que el Dockerfile está en el directorio actual
- Docker ejecutará cada línea del Dockerfile para crear la imagen

## 7. ✅ Verificar imagen creada

```bash
docker images
```

👉 **¿Qué verás?**
- Una lista de todas las imágenes Docker en tu sistema
- Deberías ver tu imagen "luis" junto con información como:
  - REPOSITORY: el nombre de la imagen
  - TAG: la versión (por defecto "latest")
  - IMAGE ID: identificador único
  - CREATED: cuándo se creó
  - SIZE: tamaño de la imagen

## 8. 🚀 Ejecutar el contenedor

```bash
docker run -d -p 8080:8080 --name microservicio luis
```

👉 **¿Qué significa cada parámetro?**
- `docker run`: Crea y ejecuta un nuevo contenedor
- `-d`: Ejecuta en modo "detached" (segundo plano), no bloquea la terminal
- `-p 8080:8080`: Mapea el puerto 8080 de tu máquina al puerto 8080 del contenedor
- `--name microservicio`: Asigna el nombre "microservicio" al contenedor
- `luis`: Usa la imagen "luis" que construimos anteriormente

### 🧪 Probar el microservicio

Una vez ejecutado el contenedor, puedes probar tu microservicio:

```bash
# Verificar que el contenedor está corriendo
docker ps

# Probar el endpoint
curl http://localhost:8080/

# Ver los logs del contenedor
docker logs microservicio
```

**Resultado esperado:**
```json
{"message": "Hola desde mi microservicio en Docker"}
```

También puedes abrir tu navegador y visitar: `http://localhost:8080/`

## 🔧 Comandos útiles de Docker

### Gestión básica del contenedor

```bash
# Ver contenedores en ejecución
docker ps

# Ver todos los contenedores (incluso los detenidos)
docker ps -a

# Detener el contenedor
docker stop microservicio

# Iniciar el contenedor nuevamente
docker start microservicio

# Reiniciar el contenedor
docker restart microservicio

# Ver los logs del contenedor
docker logs microservicio

# Seguir los logs en tiempo real
docker logs -f microservicio
```

### Limpieza y mantenimiento

```bash
# Eliminar el contenedor (debe estar detenido primero)
docker rm microservicio

# Eliminar la imagen
docker rmi luis

# Ver el espacio que ocupan las imágenes
docker images

# Limpiar imágenes no utilizadas
docker image prune

# Limpiar todo lo que no se está usando
docker system prune
```

### Debugging y exploración

```bash
# Ejecutar comandos dentro del contenedor
docker exec -it microservicio sh

# Ver información detallada del contenedor
docker inspect microservicio

# Ver el uso de recursos del contenedor
docker stats microservicio
```

---

## 🎓 Lo que me quedó súper claro después de practicar

### ¿Qué es una imagen Docker?
Una **imagen** es como una "plantilla" o "molde" que contiene:
- El sistema operativo base (Alpine Linux en nuestro caso)
- Tu aplicación (app.py)
- Las dependencias (FastAPI y Uvicorn)
- Las instrucciones de cómo ejecutar todo

### ¿Qué es un contenedor Docker?
Un **contenedor** es una "instancia en ejecución" de una imagen. Es como tomar el molde (imagen) y crear un objeto real que funciona. Puedes tener múltiples contenedores de la misma imagen.

### ¿Por qué usar Alpine Linux?
Alpine Linux es una distribución muy pequeña (solo ~5MB) que:
- Reduce el tamaño de las imágenes Docker
- Mejora la velocidad de descarga y despliegue
- Mantiene la seguridad con menos componentes

### Mapeo de puertos
Cuando usas `-p 8080:8080`:
- El primer 8080 es el puerto de tu máquina (host)
- El segundo 8080 es el puerto dentro del contenedor
- Esto permite que accedas desde tu navegador a `localhost:8080`

---

## 🚀 Lo que voy a aprender después

Ya que tengo esto dominado, mis próximos objetivos son:

1. **Docker Compose**: Para manejar múltiples contenedores
2. **Volúmenes**: Para persistir datos
3. **Redes**: Para comunicación entre contenedores
4. **Variables de entorno**: Para configuración flexible
5. **Multi-stage builds**: Para imágenes más eficientes

---

## 📚 Recursos que me ayudaron un montón

<div align="center">

### 🔗 Enlaces útiles

[![Docker Docs](https://img.shields.io/badge/Docker-Docs-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://docs.docker.com/)
[![FastAPI](https://img.shields.io/badge/FastAPI-Tutorial-009688?style=for-the-badge&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/tutorial/)
[![Docker Hub](https://img.shields.io/badge/Docker-Hub-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://hub.docker.com/)
[![Play with Docker](https://img.shields.io/badge/Play_with-Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://labs.play-with-docker.com/)

</div>

- 📘 [Documentación oficial de Docker](https://docs.docker.com/) - La fuente definitiva
- ⚡ [FastAPI Tutorial](https://fastapi.tiangolo.com/tutorial/) - Súper bien explicado
- 🐳 [Docker Hub](https://hub.docker.com/) - Aquí encuentras todas las imágenes
- 🧪 [Play with Docker](https://labs.play-with-docker.com/) - Para practicar sin instalar nada

---

<div align="center">

## 🎉 ¡Felicitaciones!

### ¡Ya tienes tu primer microservicio funcionando con Docker!

![Success](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)
![Docker](https://img.shields.io/badge/Powered_by-Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![FastAPI](https://img.shields.io/badge/Built_with-FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)

> *"El viaje de mil contenedores comienza con un solo `docker run`"* 🐳

---

## 👨‍💻 Sobre esta documentación

Esta guía la escribí mientras aprendía Docker desde cero. Documenté cada paso, cada error que cometí, y cada "¡ajá!" que tuve durante el proceso. 

Si algo no te funciona o tienes dudas, probablemente yo también las tuve. No dudes en abrir un issue o contactarme.

**¿Te sirvió esta guía?** Dale una ⭐ al repo, me ayuda mucho a saber que está siendo útil.

</div>
