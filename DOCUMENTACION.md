# Apartado 1: Fundamentos del servicio FTP

## 1. ¿Qué es FTP?

FTP (File Transfer Protocol) es un protocolo de red estándar utilizado para transferir archivos entre un cliente y un servidor. Forma parte del conjunto de protocolos TCP/IP y utiliza los puertos 21 (control) y 20 (datos) en su forma clásica.

Su funcionamiento se basa en un modelo cliente-servidor:

- El servidor FTP gestiona las conexiones y controla los accesos.
- El cliente FTP (línea de comandos, FileZilla, navegador, etc.) se conecta al servidor para subir o descargar archivos.

---

## 2. Puertos y funcionamiento

| Tipo de conexión | Puerto | Descripción |
|------------------|---------|--------------|
| Control | 21/TCP | Se usa para comandos y autenticación |
| Datos | 20/TCP (activo) o dinámico (pasivo) | Se usa para enviar o recibir archivos |

FTP utiliza dos canales separados:

- Canal de control: donde se envían los comandos como `USER`, `PASS`, `LIST`, `RETR`, `STOR`.
- Canal de datos: por donde circulan los archivos.

---

## 3. Modos de conexión: activo y pasivo

### Modo Activo

En este modo:
- El cliente abre un puerto efímero (>1023) y se lo comunica al servidor.
- El servidor inicia la conexión de datos desde su puerto 20 hacia el cliente.

Ventajas:
- Rápido y sencillo de configurar.

Inconvenientes:
- Puede verse bloqueado por NAT o cortafuegos modernos.

```
Cliente (puerto >1023)  ← conexión datos ←  Servidor (puerto 20)
Cliente (puerto >1023)  → conexión control →  Servidor (puerto 21)
```

---

### Modo Pasivo

En este modo:
- El servidor abre un puerto aleatorio (>1023) y se lo indica al cliente.
- El cliente inicia la conexión de datos hacia ese puerto.

Ventajas:
- Funciona bien tras NAT o firewalls restrictivos.

Inconvenientes:
- Requiere abrir un rango de puertos en el servidor.

```
Cliente (puerto >1023)  → conexión control →  Servidor (puerto 21)
Cliente (puerto >1023)  → conexión datos →  Servidor (puerto >1023)
```

---

## 4. FTP, FTPS y SFTP: diferencias

| Protocolo | Capa de seguridad | Puerto | Método de cifrado | Comentario |
|------------|-------------------|---------|--------------------|-------------|
| FTP | Ninguna | 21 | Texto plano | Inseguro, datos y contraseñas visibles |
| FTPS | TLS/SSL | 21 (explícito) o 990 (implícito) | Cifrado | FTP con seguridad añadida |
| SFTP | SSH | 22 | SSH | No es FTP real, usa otro protocolo |

En esta práctica se trabajará con FTP y FTPS (no SFTP), ya que se busca mantener compatibilidad con routers y clientes antiguos.

---

## 5. Casos de uso y utilidad

FTP es útil en múltiples contextos, especialmente en entornos donde se requiere transferir archivos entre sistemas de forma sencilla:

- Copias de seguridad (por ejemplo, de routers o servidores).
- Transferencia de archivos entre servidores o departamentos.
- Repositorios de software o imágenes ISO.
- Automatización de tareas con scripts o cron jobs.

Aunque FTP no es seguro por defecto, puede usarse FTPS (FTP sobre TLS/SSL) para cifrar la comunicación y proteger las credenciales y los datos transferidos.

---

## 6. Conceptos clave

| Concepto | Descripción |
|-----------|--------------|
| Control Channel | Puerto 21, comandos y autenticación |
| Data Channel | Puerto 20 (activo) o dinámico (pasivo) |
| Anonymous FTP | Acceso sin usuario real, solo lectura |
| Chroot Jail | Carpeta donde se "enjaula" al usuario |
| Quotas | Límite de espacio en disco asignado a cada usuario |
| LDAP Auth | Autenticación centralizada de usuarios |
| FTPS | FTP con TLS/SSL (seguro) |
| S3FS | Sistema para montar un bucket S3 como almacenamiento local |

---
