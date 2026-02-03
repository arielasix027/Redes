# Servicios de Streaming

## Introducción
El streaming permite transmitir audio o vídeo en tiempo real sin necesidad de descargar el archivo completo. Este documento resume los conceptos fundamentales sobre topologías de red, protocolos, QoS, códecs y funcionamiento general de sistemas de streaming como Icecast2.

# 1. Descarga Directa vs Streaming

## Descarga directa
- El usuario solicita un archivo completo (ej. 100 MB).
- El servidor envía el fichero entero aunque el usuario no lo consuma.
- Se almacena localmente.
- Ineficiente en ancho de banda.

## Streaming
- El servidor envía datos en flujo continuo.
- No hay almacenamiento permanente.
- Solo se transmite lo que el usuario consume.
- Eficiente y adecuado para tiempo real.

# 2. Topologías de Red

## Unicast
- Conexión 1 a 1.
- Si hay 100 oyentes → 100 flujos independientes.
- **BW total = BW stream × Nº usuarios**
- Poco escalable.

## Multicast
- El servidor envía un único flujo a una dirección multicast (224.0.0.0–239.255.255.255).
- Los routers replican solo si hay suscriptores.
- Limitado a redes internas (muchos routers bloquean multicast).

# 3. Capa de Transporte: TCP vs UDP

## TCP
- Fiable: retransmite paquetes perdidos.
- Compatible con firewalls, NAT y proxies.
- Mayor latencia.

## UDP
- No hay retransmisión.
- Latencia mínima.
- Puede perder calidad si hay paquetes perdidos.

# 4. QoS: Jitter y Buffer

## Jitter
Variación en el tiempo de llegada de los paquetes.  
Si el jitter supera el tamaño del buffer → cortes en el audio.

## Buffer
Memoria temporal que almacena segundos de audio/vídeo.
- Más buffer → más estabilidad.
- Más buffer → más latencia.

## Burst-on-Connect
- El servidor envía una ráfaga inicial (ej. 64 KB a 10× velocidad).
- El buffer del cliente se llena casi instantáneamente.
- Reduce el *time-to-first-byte*.

# 5. Protocolos de Streaming

## 5.1. Capa de transporte
- TCP: calidad y compatibilidad.
- UDP: mínima latencia.

## 5.2. Capa de aplicación

### 1. HTTP Legacy (ICY – Icecast2)
- Conexión TCP continua.
- El servidor envía bytes sin parar.
- Puertos típicos: 80, 443, 8000.
- Formatos: MP3, OGG, AAC.

### 2. HTTP Adaptativo (HLS / MPEG-DASH)
- El vídeo se divide en *chunks* de 2–10 segundos.
- El cliente elige la calidad según su ancho de banda.
- Formatos: `.ts`, `.m4s`.
- Excelente para CDN.

# 6. Icecast2

Icecast2 es un servidor de streaming de código abierto que actúa como una “antena virtual”.  
Recibe audio de una fuente (Mixxx, Butt, etc.) y lo distribuye a múltiples oyentes.

### Características:
- Formatos: MP3, OGG.
- Gestión de oyentes.
- Puntos de montaje (ej. `/radio-asir`, `/radio-smr`).
- Compatible con navegadores, VLC y apps móviles.

# 7. Códecs de Audio

## Códecs con pérdida
- Eliminan información imperceptible.
- Muy eficientes.
- Ejemplos: MP3, AAC, Vorbis.

## Códecs sin pérdida
- No eliminan información.
- Menor compresión.
- Ejemplos: FLAC, WAV.

# 8. Parámetros de Audio

## Frecuencia de muestreo
- “Fotos” por segundo de la onda.
- Estándar: **44.1 kHz**.

## Profundidad de bits
- Calidad de cada muestra.
- Estándar: **16 bits (CD)**.

## Canales
- Mono, Estéreo, 5.1, etc.

# 9. Cálculo de Peso (Audio)

### Fórmula:Peso = Frecuencia × Bits × Canales × Segundos

Ejemplo WAV sin compresión (3 min, 44.1 kHz, 16 bit, estéreo):  
≈ **31.75 MB**

# 10. Vídeo: Conceptos Clave

## Contenedor
Formato que agrupa:
- Vídeo
- Audio
- Subtítulos
- Metadatos

#PRACTICA 1
![1](imagenes/1.png)
  
Ejemplos: MP4, MKV, MOV, OGG.

## Cálculo de peso sin compresión: Peso = (Ancho × Alto) × Profundidad de color × FPS × Tiempo
## Con compresión: Peso = Bitrate × Tiempo
