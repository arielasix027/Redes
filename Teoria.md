# Servicios de Streaming

## Introducción
El streaming permite transmitir audio o vídeo en tiempo real sin necesidad de descargar el archivo completo. Este documento resume los conceptos fundamentales sobre topologías de red, protocolos, QoS, códecs y funcionamiento general de sistemas de streaming como Icecast2.

# Descarga Directa vs Streaming

### Descarga directa
- El usuario solicita un archivo completo (ej. 100 MB).  
- El servidor envía el fichero entero aunque el usuario no lo consuma.  
- Se almacena localmente.  
- Ineficiente en ancho de banda.  

### Streaming
- El servidor envía datos en flujo continuo.  
- No hay almacenamiento permanente.  
- Solo se transmite lo que el usuario consume.  
- Eficiente y adecuado para tiempo real.  

# Topologías de Red

### Unicast
- Conexión 1 a 1.  
- Si hay 100 oyentes → 100 flujos independientes.  
- **BW total = BW stream × Nº usuarios**  
- Poco escalable.  

### Multicast
- El servidor envía un único flujo a una dirección multicast (224.0.0.0–239.255.255.255).  
- Los routers replican solo si hay suscriptores.  
- Limitado a redes internas (muchos routers bloquean multicast).  

# Capa de Transporte: TCP vs UDP

### TCP
- Fiable: retransmite paquetes perdidos.  
- Compatible con firewalls, NAT y proxies.  
- Mayor latencia.  

### UDP
- No hay retransmisión.  
- Latencia mínima.  
- Puede perder calidad si hay paquetes perdidos.  

# QoS: Jitter y Buffer

### Jitter
Variación en el tiempo de llegada de los paquetes.  
Si el jitter supera el tamaño del buffer → cortes en el audio.  

### Buffer
Memoria temporal que almacena segundos de audio/vídeo.  
- Más buffer → más estabilidad.  
- Más buffer → más latencia.  

### Burst-on-Connect
- El servidor envía una ráfaga inicial (ej. 64 KB a 10× velocidad).  
- El buffer del cliente se llena casi instantáneamente.  
- Reduce el *time-to-first-byte*.  

# Protocolos de Streaming

### Capa de transporte
- TCP: calidad y compatibilidad.  
- UDP: mínima latencia.  

### Capa de aplicación

**HTTP Legacy (ICY – Icecast2)**
- Conexión TCP continua.  
- El servidor envía bytes sin parar.  
- Puertos típicos: 80, 443, 8000.  
- Formatos: MP3, OGG, AAC.  

**HTTP Adaptativo (HLS / MPEG-DASH)**
- El vídeo se divide en *chunks* de 2–10 segundos.  
- El cliente elige la calidad según su ancho de banda.  
- Formatos: `.ts`, `.m4s`.  
- Excelente para CDN.  

# Icecast2

Icecast2 es un servidor de streaming de código abierto que actúa como una “antena virtual”.    
Recibe audio de una fuente (Mixxx, Butt, etc.) y lo distribuye a múltiples oyentes.

### Características:
- Formatos: MP3, OGG.  
- Gestión de oyentes.  
- Puntos de montaje (ej. `/radio-asir`, `/radio-smr`). 
- Compatible con navegadores, VLC y apps móviles.  

## Códecs de Audio

### Códecs con pérdida
- Eliminan información imperceptible.  
- Muy eficientes.  
- Ejemplos: MP3, AAC, Vorbis.  

### Códecs sin pérdida
- No eliminan información.  
- Menor compresión.  
- Ejemplos: FLAC, WAV.  

## Parámetros de Audio

### Frecuencia de muestreo
- “Fotos” por segundo de la onda.  
- Estándar: **44.1 kHz**.  

### Profundidad de bits
- Calidad de cada muestra.  
- Estándar: **16 bits (CD)**. 

### Canales
- Mono, Estéreo, 5.1, etc.  

# Vídeo

### Contenedor
Formato que agrupa: Vídeo, Audio, Subtítulos y Metadatos  
Ejemplos: MP4, MKV, MOV, OGG.



# FORMULAS

**Para pasar a bytes:**     
Bytes = Bits / 8  

**Para pasar a MB y GB:**  
MB = Bytes / 10^6
GB = Bytes / 10^9

### Cálculo de Peso (Audio sin compresión) --> WAC 
**Fórmula: Peso = Frecuencia × Bits × Canales × Segundos**  
*Frecuencia → Hz (44 100, 48 000…)  
*Bits → profundidad (16, 24…)  
*Canales → 1 mono, 2 estéreo  
*Segundos → duración total  

### Cálculo de Peso (Audio con compresión) --> MP3, AAC...    
**Fórmula: Peso = Bitrate × Tiempo**  
*Bitrate → en bits/s (ej: 128 kbps = 128 000 bits/s)  
*Tiempo → en segundos  

### Cálculo de Peso (Video sin compresión) --> RAW  
**Fórmula: Peso(bits) = (Ancho × Alto) × Profundidad × FPS × Tiempo**  
**Bitrate = (Ancho × Alto) × Profundidad × FPS**  
**Peso = Bitrate × Tiempo**  
*Ancho × Alto → resolución (ej: 1920×1080)  
*Profundidad → bits por pixel (24 bits = RGB 8+8+8)  
*FPS → frames por segundo  
*Tiempo → segundos  

### Cálculo de Peso (Video con compresión) --> H.264, H.265…
**Fórmula: Peso = Bitrate × Tiempo**  
Igual que en audio comprimido.  

### ANCHO DE BANDA TOTAL (STREAMING)
**Fórmula: BW_total = Bitrate_stream × Número_de_usuarios**    
Ejemplo: 128 kbps × 25 usuarios = 3.2 Mbps  

### CÁLCULO DE USUARIOS MÁXIMOS
**Fórmula: Usuarios = BW_disponible / Bitrate_por_usuario**      
Si hay límite del 80%: 𝐵 𝑊 𝑢 𝑠 𝑎 𝑏 𝑙 𝑒 = 𝐵 𝑊 𝑡 𝑜 𝑡 𝑎 𝑙 × 0.8  

### CONVERSIONES IMPRESCINDIBLES
**Fórmula: Peso = Bitrate × Tiempo**  

**Unidades de almacenamiento**  
*1 byte = 8 bits   
*1 KB = 10^3 bytes  
*1 MB = 10^6 bytes  
*1 GB = 10^9 bytes  
*1 TB = 10^12 bytes  

**Unidades de velocidad**  
1 kbps = 10^3 bits/s  
1 Mbps = 10^6 bits/s  
1 Gbps = 10^9 bits/s  

**Conversión rápida**  
*MB → bits: × 8 × 10^6  
*GB → bits: × 8 × 10^9  
*kbps → Mbps: ÷ 1000  
*Segundos → horas: ÷ 3600  

### CÁLCULO DE PORCENTAJE DE USO DE LÍNEA  
**Fórmula: Porcentaje = (Bitrate / Capacidad_total) × 100**  
Ejemplo: 6 Mbps en una línea de 20 Mbps → ( 6 / 20 ) × 100 = 30 %  

### CÁLCULO DE RESOLUCIÓN  
**Fórmula: Pixels_por_frame = Ancho × Alto**  

### CÁLCULO DE BITRATE (AUDIO RAW)  
Bitrate de un audio sin comprimir:   
**Fórmula: Bitrate = Frecuencia × Bits × Canales**   
Ejemplo: 48000Hz × 24bits × 1 = 1.152.000 bits/s = 1.152 Mbps  

# EJERCICIOS DE PRÁCTICA  
### Peso de un audio WAV de 7 minutos**  
**Datos: 44.1 kHz, 16 bits, estéreo, 7 min.**  

**Bitrate RAW**  
44100 × 16 × 2 = 1411200 bits/s  

**Peso total**  
1411200 × 420 = 592704000 bits  

**Pasar a MB**  
592704000 / 8 = 74088000 bytes  
74088000 / 10^6 = 74.1 MB  
Resultado: ≈ 74 MB.  

### Ancho de banda total para 80 oyentes a 96 kbps  
**Bitrate por usuario**  
96 kbps = 96 000 bits/s  

**BW total**
96000 × 80 = 7680000 bits/s = 7.68 Mbps  
Resultado: 7.68 Mbps.  

### Bitrate de audio RAW 96 kHz, 24 bits, estéreo  
**Bitrate**  
96000 × 24 × 2 = 4608000 bits/s = 4.608 Mbps  
Resultado: 4.608 Mbps.  

### Vídeo RAW 1080p, 30 fps, 24 bits — 1 minuto  
**Pixels por frame**  
1920 × 1080 = 2073600  

**Bitrate RAW**  
2073600×24×30 =1492992000 bits/s ≈ 1.49 Gbps  

**Peso en 60 s**  
1.49 × 10^9 × 60 = 8.94 × 10^10 bits  

**Pasar a GB**
8.94 × 10^10 / 8 = 1.1175 × 10^10 bytes = 11.17 GB  
Resultado: ≈ 11.2 GB por minuto.  

### ¿Cuántos usuarios pueden ver un stream de 2 Mbps con 50 Mbps?  
Si cada usuario consume 2Mbps caben 25 usuarios ya que el servidor tiene 50Mbps disponibles.  
50/2 = 25 --> Resultado: 25 usuarios.  

### Porcentaje de uso de línea  
Enunciado: Emites a 5 Mbps en una línea de 20 Mbps.  
Formula de porcentaje -> porcentaje = (uso/capacidad total) x100  
(5/20) × 100 = 25% --> Resultado: 25% de uso.  

### Peso de un vídeo comprimido  
Enunciado: Bitrate 4 Mbps, duración 12 minutos.  

**Paso 1: Pasar 12 min a segundos**  
12×60 = 720 s  

**Peso**  
4×10^6×720 = 2.88×10^9 bits  

**Pasar a GB**  
2.88 × 10^9 / 8 = 360000000 bytes = 0.36 GB  
Resultado ≈ 0.36 GB.  

### ¿Cuántos minutos de vídeo 720p (3 Mbps) caben en 128 GB?  
**Paso 1: Convertir 128 GB a bits**  
128GB = 128×10^9 bytes
128 × 10^9 × 8 = 1.024 × 10^12 bits  --> pasamos a bits

**Paso 2: Dividir entre 3 Mbps**  
1.024 × 10^12 / (3 × 10^6) = 341333 s  

**Paso 3: Pasar a horas**  
341333 / 3600 = 94.8 h  
Resultado ≈ 95 horas de vídeo 720p.  

