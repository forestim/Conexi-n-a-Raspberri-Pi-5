# 🍓 Raspberry Pi 5 (4GB) — Configuración de Acceso Remoto con TailScale

En esta guía configuraremos un túnel seguro mediante **TailScale**, permitiendo acceso SSH remoto a la Raspberry Pi, además de habilitar funciones avanzadas como *Exit Node* y desactivar la expiración de claves.

---

## 🚀 Instalación de TailScale (VPN)

### 1️⃣ Actualizar paquetes e instalar TailScale

sudo apt update && curl -fsSL https://tailscale.com/install.sh | sh

---

## 🔧 Configuración de red para TailScale

Estas configuraciones permiten el reenvío de paquetes IPv4 e IPv6, necesario para utilizar la Raspberry Pi como *Exit Node*.

echo 'net.ipv4.ip_forward = 1' | sudo tee -a /etc/sysctl.d/99-tailscale.conf  
echo 'net.ipv6.conf.all.forwarding = 1' | sudo tee -a /etc/sysctl.d/99-tailscale.conf

Aplicar los cambios:

sudo sysctl -p /etc/sysctl.d/99-tailscale.conf

Iniciar TailScale con acceso SSH y anuncio del *Exit Node*:

sudo tailscale up --ssh --advertise-exit-node

---

## 🛡️ Configuración necesaria en el panel de TailScale

Después de ejecutar los comandos anteriores, ingresa al panel web de TailScale y realiza las siguientes configuraciones:

### ✔️ Desactivar expiración de clave  
Evita que la Raspberry Pi pierda la conexión remota cada 180 días.

### ✔️ Aprobar el Exit Node  
Permite enrutar todo el tráfico de red a través de la Raspberry Pi, proporcionando una capa adicional de privacidad.

---

