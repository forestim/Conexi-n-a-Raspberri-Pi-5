# 🍓 Raspberry Pi 5 — Configuración de servicio de Video para monitoreo

Este servicio permite visualizar el video de las cámaras en tiempo real y realizar capturas de pantalla.
> **Nota:** Este servicio es solo de visualización; no permite acceder a los controles PTZ de las cámaras.

## 1. Instalación

Descargar, descomprimir y mover los ejecutables a las carpetas del sistema.


# Descargar la versión 1.9.0 para ARM64
`wget https://github.com/bluenviron/mediamtx/releases/download/v1.15.5/mediamtx_v1.15.5_linux_arm64.tar.gz`

# Descomprimir el archivo
  `tar -xvzf mediamtx_v1.15.5_linux_arm64v8.tar.gz`

# Mover el ejecutable a /usr/local/bin
`sudo mv mediamtx /usr/local/bin/`

# Mover el archivo de configuración a /etc
`sudo mv mediamtx.yml /etc/`

`sudo chmod +x /usr/local/bin/mediamtx`


## 2. Configuración de Cámaras

Editar el archivo de configuración:


`sudo nano /etc/mediamtx.yml`

`sudo rm /etc/mediamtx.yml && sudo nano /etc/mediamtx.yml`

Certificados

`tailscale cert focoenobra3.tailb2be55.ts.net`

`tailscale serve https:8889 / http://127.0.0.1:8889`

`tailscale funnel 8889 on`


Agregar el siguiente contenido al final del archivo, en la sección `paths`:

```yaml
###############################################
# CONFIGURACIÓN - TAILSCALE FUNNEL
###############################################
logLevel: info
logDestinations: [stdout]

# --- WEBRTC ---
webrtc: yes
# Puerto de señalización (La página web)
webrtcAddress: :8889
webrtcEncryption: no

# CORS (Permisos web)
webrtcAllowOrigins:
  - "*"

# CRÍTICO: La dirección pública
# Esto le dice al navegador: "Búscame en esta URL, no en la IP local"
webrtcAdditionalHosts:
  - focoenobra0.tailb2be55.ts.net

# CRÍTICO: El canal de video TCP
# Corregimos el nombre del comando: es 'LocalTCPAddress'
webrtcLocalTCPAddress: :8443

# (Nota: No ponemos nada de UDP. Al no configurar UDP y sí TCP,
# el navegador intentará UDP, fallará, y saltará a este puerto TCP 8443)

# --- RTSP (Interno) ---
rtsp: yes
rtspAddress: :8554

# --- HLS (Backup) ---
hls: yes
hlsAddress: :8888

###############################################
# CÁMARAS
###############################################
paths:
  cam1_lq:
    source: rtsp://admin:feo2024!@192.168.1.108:554/cam/realmonitor?channel=1&subtype=1
    sourceOnDemand: yes
```

*Para guardar: `Ctrl + O`, `Enter`. Para salir: `Ctrl + X`.*

## 3. Crear Servicio en Segundo Plano (Systemd)

Crear el archivo de servicio para que arranque automáticamente:

```bash
sudo nano /etc/systemd/system/mediamtx.service
```

Pegar el siguiente contenido:

```ini
[Unit]
Description=MediaMTX Server
After=network.target

[Service]
ExecStart=/usr/local/bin/mediamtx /etc/mediamtx.yml
Restart=on-failure
User=root

[Install]
WantedBy=multi-user.target
```

## 4. Permisos y Ejecución

Asignar permisos, habilitar y arrancar el servicio.

```bash
# Dar permisos de ejecución
sudo chmod +x /usr/local/bin/mediamtx

# Habilitar el servicio al inicio del sistema
sudo systemctl enable mediamtx

# Arrancar el servicio
sudo systemctl start mediamtx
```

### Comandos de mantenimiento

```bash
# Verificar estado
sudo systemctl status mediamtx

# Reiniciar en caso de problemas
sudo systemctl restart mediamtx
```

## 5. Solución de Problemas (Firewall)
