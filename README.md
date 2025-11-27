# 📡 Raspberry Pi 5 + Tailscale + NoMachine  
Optimización de conexión remota en redes de baja velocidad

Este proyecto describe una configuración optimizada para entornos donde la velocidad de internet ronda **~5 Mbps de descarga y subida**.  
El objetivo es mantener una conexión **estable, de baja latencia y con entorno gráfico**, utilizando una Raspberry Pi 5 conectada mediante **Tailscale** y **NoMachine**.

---

## 🗺️ Topología de Conexión

        ┌─────────────┐
        │ Alimentación│
        └──────┬──────┘
               │
        ┌──────▼──────┐
        │   Switch    │
        └───┬────┬────┘
            │    │
            │    └───────────────┐
            │                    │
    ┌───────▼───────┐     ┌─────▼────────┐
    │ POE Converter │     │   Raspberry  │
    │   PS5724AT    │     │     PI 5     │
    └───────────────┘     └──────────────┘
            │
    ┌───────▼────────┐
    │    Router 4G   │
    │    Teltonika   │    
    └────────────────┘


## ⚙️ Especificaciones y Configuración Física

### 🛜 Router Teltonika **RUT200**
Principales limitaciones encontradas:

- Módulo **LTE Cat 4** → Máx. ~150 Mbps (bajada) / ~50 Mbps (subida)  
- Wi-Fi **802.11 b/g/n** (Wi-Fi 4) solo en 2.4 GHz  
- Puertos Ethernet **10/100 Mbps**  
- **128 MB de RAM**, limitando ciertas tareas

---

### 📶 Antenas Teltonika **PR1US440**

- Ganancia: **4 dBi**
- Frecuencias: **698–960 MHz / 1710–2690 MHz**
- Polarización: **Lineal**
- Conector: **SMA Male**

---

### 🔌 POE Converter **PS5724AT-RJ (Splitter)**

Utilizado para permitir la alimentación POE entre Switch → Raspberry y Switch → Router, evitando usar fuentes adicionales.

**Especificaciones:**
- SN: *07255724AT4228*
- Velocidad Máxima: **10/100 Mbps**

> **Nota:**  
> El cable integrado del convertidor no posee documentación técnica. Esto podría generar **cuellos de botella**, ruido o interferencias si el cable es de baja calidad o el diseño interno no es óptimo.

---

### 🔀 Switch Mercusys **MS105GP**

- **5 puertos Gigabit**
- **4 puertos POE+**
- **Modo Extend:**  
  - Aumenta la distancia POE  
  - *Reduce la velocidad a 10 Mbps*  
  - → Mantener **desactivado**
- **Modo Isolation:**  
  - Aísla puertos permitiendo comunicación sólo con el Puerto 5  
  - → Mantener **desactivado** para esta topología

---

### 🍓 Raspberry Pi 5 **(4GB)**

- Ancho de banda estimado: **~1 Gbps**
- Wi-Fi **802.11ac (Wi-Fi 5)**  
- Potencia suficiente para acceso remoto con entorno gráfico

---

## 🛠️ Configuraciones del Sistema

En este repositorio encontrarás dos archivos con todas las configuraciones necesarias para **exprimir al máximo la conexión** en entornos de bajo ancho de banda:

- 📄 **Configuración Raspberry Pi 5 (4GB)**
- 📄 **Configuración de acceso desde Windows 11**

Estas configuraciones permiten obtener una experiencia más **fluida**, **estable** y con **menor latencia** al utilizar Tailscale + NoMachine.

