- Lo primero que voy a hacer es crear el archivo de configuracion docker-compose.yml
version: '3.8'

services:
  bind9:
    image: internetsystemsconsortium/bind9:9.18
    ports:
      - 53:54/tcp   (TCP)
      - 53:54/udp   (UDP)
      - 127.0.0.1:953:953/tcp
    volumes:
      - ./config:/etc/bind
      - ./data:/var/cache/bind  #
      - TZ=UTC
    restart: always

- Una vez tengo el archivo de configuracion procedo a crear los directorios de config y data. He creado el directorio data para que el contenedor tenga un lugar donde guardar los archivos de caché y datos temporales que Bind9 generará mientras eatá funcionando

- Ahora que tengo los directorio creados me dirijo al directorio config para crear el archivo de configuración named.conf.options

options {
    directory "/var/cache/bind";
    recursion yes;
    allow-query { any; };
    listen-on { any; };
    listen-on-v6 { any; };
};

- Despues de terminar con los archivos de configuración lo siguiente seria iniciar el contenedor con
docker-compose up -d

- Con el contenedor ya iniciado voy a comprobar que Bind9 funciona correctamente con el comando docker-compose logs -f bind9. El resultado es el siguiente:

Attaching to servidor_bind9
servidor_bind9 exited with code 1

- Por ultimo voy a realizar una consulta local para verificar que el servidor DNS responde correctamente con dig @localhost google.com. El servidor no responde correctamente... incluso despues de haberte preguntado sigo sin dar con la solucion para arreglarlo... Este es el resultado:

comunications error to 127.0.0.1#53: conection refused




