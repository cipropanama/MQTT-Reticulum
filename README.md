<div align="center">
  <h1>🌐 Mesh-RNS Bridge</h1>
  <p><b>Una solución de nivel de producción para interconectar brokers MQTT (Meshtastic) mediante Reticulum Network Stack (RNS)</b></p>

  [![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
  [![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
  [![Reticulum](https://img.shields.io/badge/Network-Reticulum-orange.svg)](https://reticulum.network/)
  
  <br />
</div>

## 📌 Visión General

**Mesh-RNS Bridge** es un desarrollo de código abierto impulsado por la comunidad **CIPRO Panamá**. 

Permite establecer redes de malla de largo alcance o intercontinentales, puenteando el tráfico de dispositivos Meshtastic (vía MQTT) hacia otras redes de malla utilizando un ancho de banda extremadamente bajo gracias al protocolo RNS y a algoritmos de compresión y filtrado inteligente.

---

## ✨ Características Principales

- **⚙️ Configuración Desacoplada:** Cero variables hardcodeadas. Todo se gestiona desde un archivo `config.ini` limpio y centralizado.
- **🛡️ Filtrado Inteligente:** Control granular para enrutar solo la información que importa (Mensajes de Texto, Telemetría, Posiciones o NodeInfo).
- **🚦 Control de Tráfico (Rate Limiting):** Protege los enlaces RNS de bajo ancho de banda evadiendo ráfagas innecesarias de telemetría de un mismo nodo.
- **📦 Compresión Nativa:** Conversión binaria de alta eficiencia (msgpack) que reduce drásticamente el tamaño del payload.
- **🔄 Resiliencia:** Conexión y reconexión automática del cliente MQTT, diseñada para operar desatendida 24/7.
- **🐧 Integración Linux:** Operación nativa como servicio `systemd`.

---

## 🚀 Guía de Instalación Rápida

La instalación está automatizada para sistemas basados en Debian/Ubuntu.

**1. Clonar el repositorio:**
```bash
git clone https://github.com/cipropanama/MQTT-Reticulum.git
cd MQTT-Reticulum
```

**2. Ejecutar el instalador (requiere privilegios de superusuario):**
```bash
sudo ./install.sh
```
> *El script creará un entorno virtual aislado en `/opt/`, instalará las dependencias necesarias y preparará el servicio `systemd`.*

**3. Configurar el puente:**
Edite el archivo de configuración generado según las necesidades de su nodo:
```bash
sudo nano /etc/mesh-rns-bridge/config.ini
```

**4. Levantar el servicio:**
```bash
sudo systemctl start mesh-rns-bridge
```
*(Puede monitorear los logs en tiempo real utilizando `sudo journalctl -u mesh-rns-bridge -f`)*

---

## 🔗 Configuración de Nodos Distantes (Destination Hash)

Para crear un túnel entre dos brokers MQTT distantes (Nodo A y Nodo B) a través de RNS:

1. **Formato JSON:** Asegúrese de que el gateway emisor (Meshtastic) tenga habilitada la salida hacia MQTT en formato **JSON**. De lo contrario, los paquetes cifrados nativos serán ignorados para ahorrar ancho de banda.
2. **Obtener Hashes Locales:** Inicie el servicio en ambos servidores con el campo `destination_hash` en blanco. Revise los logs para encontrar el *Hash Local* de escucha de cada uno (ej. `9abc1234...`).
3. **Cruzar los Hashes:** 
   - En el `config.ini` del Servidor A, configure `destination_hash` con el Hash del Servidor B.
   - En el `config.ini` del Servidor B, configure `destination_hash` con el Hash del Servidor A.
4. **Reiniciar:** Ejecute `sudo systemctl restart mesh-rns-bridge` en ambos servidores. ¡El tráfico ahora fluirá bidireccionalmente!

---

## 🤝 Comunidad y Licencia

Este proyecto está liberado bajo la licencia **MIT**. Su uso es totalmente libre para fines experimentales, respuesta a emergencias y proyectos comunitarios solidarios. 

Si este desarrollo te ha sido útil, te invitamos a mantener el reconocimiento y el enlace hacia el proyecto original.

### Contacto CIPRO Panamá

- 🌐 **Sitio Web:** [www.cipropanama.org](https://www.cipropanama.org)
- ✉️ **Correo Electrónico:** [info@cipropanama.org](mailto:info@cipropanama.org)

<div align="center">
  <i>Desarrollado con ❤️ para las telecomunicaciones libres.</i>
</div>
