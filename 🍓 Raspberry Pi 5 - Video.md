# 🍓 Raspberry Pi 5 — Configuración de servicio de Video para monitoreo

Este servicio permite visualizar el video de las cámaras en tiempo real y realizar capturas de pantalla.
> **Nota:** Este servicio es solo de visualización; no permite acceder a los controles PTZ de las cámaras.

## 1. Instalación

Descargar, descomprimir y mover los ejecutables a las carpetas del sistema.

```bash
# Descargar la versión 1.9.0 para ARM64
wget [https://github.com/bluenviron/mediamtx/releases/download/v1.9.0/mediamtx_v1.9.0_linux_arm64v8.tar.gz](https://github.com/bluenviron/mediamtx/releases/download/v1.9.0/mediamtx_v1.9.0_linux_arm64v8.tar.gz)

# Descomprimir el archivo
tar -xvzf mediamtx_v1.9.0_linux_arm64v8.tar.gz

# Mover el ejecutable a /usr/local/bin
sudo mv mediamtx /usr/local/bin/

# Mover el archivo de configuración a /etc
sudo mv mediamtx.yml /etc/
```

## 2. Configuración de Cámaras

Editar el archivo de configuración:

```bash
sudo nano /etc/mediamtx.yml
```

Agregar el siguiente contenido al final del archivo, en la sección `paths`:

```yaml
paths:
  # --- CÁMARA 1 (OBRA) ---

  # Ruta para Alta Calidad (Se usará al hacer clic para ver detalle)
  cam1_hq:
    source: rtsp://admin:feo2024!@192.168.1.108:554/cam/realmonitor?channel=1&subtype=0

  # Ruta para Baja Calidad (Se usará para el Dashboard fluido)
  cam1_lq:
    source: rtsp://admin:feo2024!@192.168.1.108:554/cam/realmonitor?channel=1&subtype=1
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

En caso de que el servicio no arranque o no sea accesible, puede deberse a un bloqueo de puertos. Ejecuta lo siguiente para permitirlos:

```bash
sudo ufw allow 8889/tcp
sudo ufw allow 8889/udp
```
