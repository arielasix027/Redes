# Práctica de Vídeo (FFmpeg, códecs, remuxing, bitrate…)

### Paso 1 --> Instalar icecast2 y vlc

Comando instalación --> sudo apt install icecast2 vlc

![1](Imagenes/6.png)

### Paso 2 --> Analizar el vídeo con ffprobe

![1](Imagenes/7.png)

### Paso 3 --> 



![1](Imagenes/8.png)


### Preguntas finales:

### Almacenamiento: Si tu servidor tiene un disco de 500 GB, ¿cuántas horas de vídeo del
### perfil "HD" (2 Mbps) podrías alojar?

Paso 1: Pasar 500 GB a bits
1 GB = 10^9 bytes
1 byte = 8 bits
500 GB = 500 × 10^9 bytes
500 × 10^9 bytes × 8 = 4 × 10^12 bits

Paso 2: Usar el bitrate de 2 Mbps
2 Mbps = 2 × 10 6 bits/s
Tiempo en segundos = 4 × 10 12 bits 2 × 10 6 bits/s = 2 × 10 6 s

Paso 3: Pasar segundos a horas
Horas=2×106s3600s/h --> 555.6h

Resultado: 556 horas de vídeo HD a 2 Mbps.

### Red: Tienes una línea de 100 Mbps simétricos. ¿Cuántos usuarios podrían ver el perfil
### "Móvil" (400 kbps) simultáneamente antes de saturar el 80% de la línea?

Paso 1: Calcular el 80% de 100 Mbps
100 Mbps × 0.8 = 80 Mbps

Paso 2: Pasar 400 kbps a Mbps
400 kbps = 0.4 Mbps

Paso 3: Dividir el ancho de banda disponible entre el consumo por usuario
Usuarios = 80 Mbps 0.4 Mbps = 200 

Resultado: 200 usuarios simultáneos con el perfil Móvil de 400 kbps.
