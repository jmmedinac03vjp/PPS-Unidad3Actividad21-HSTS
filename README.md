# PPS-Unidad3Actividad21-HSTS
 Implementación de HTTP Strict Transport Security (HSTS)

> - Ver cómo implementar la política ImplementacionHSTS.pdf
>
> - Analizar el código de la aplicación que permite ataques de .
>
> - Implementar diferentes modificaciones del codigo para aplicar mitigaciones o soluciones.

# ¿Qué es HSTS?
HSTS (HTTP Strict Transport Security) obliga al navegador a usar únicamente HTTPS, reduciendo riesgos de ataques MITM y downgrade a HTTP.
---

Consecuencias de :
- 
# ACTIVIDADES A REALIZAR
---
> Lee detenidamente la sección de HSTS de mozilla <https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Strict-Transport-Security>
>
> Lee el siguiente [documento sobre Explotación y Mitigación de ataques de Remote Code Execution](./files/ImplementacionHSTS.pdf>
> 
> También y como marco de referencia, tienes [ la sección de correspondiente de ataque  del **Proyecto Web Security Testing Guide** (WSTG) del proyecto **OWASP**.](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/07-Test_HTTP_Strict_Transport_Security)
>


Vamos realizando operaciones:

## Iniciar entorno de pruebas

-Situáte en la carpeta de del entorno de pruebas de nuestro servidor LAMP e inicia el esce>

~~~
docker-compose up -d
~~~

- Para acceder al servidor Apache de nuestro escenario multicontenedor:

```bash
docker exec -it lamp-php83 /bin/bash
```

## Configurar HSTS en el servidor web
En Apache:
```apache
Header always set Strict-Transport-Security "max-age=31536000; includeSubDomains; preload"
```

Explicación:

- **Siempre activamos la Política Stric-Transport-Security.**

- ** max-age=31536000**: Obliga el uso de HTTPS durante 31,536,000 segundos (1 año).

- **includeSubDomains*: Aplica HSTS a todos los subdominios.

- **preload**: Solicita la inclusión en la lista de precarga de HSTS https://hstspreload.org/ de Google Chrome.

El parámetro preload hace que el dominio sea agregado a la lista de precarga de HSTS en navegadores como Chrome, Firefox y Edge. Una vez en esta lista, no se puede eliminar fácilmente y todos los visitantes siempre intentarán conectarse por HTTPS, incluso si se elimina el encabezado HSTS en el servidor.


Cuando **NO DEBEMOS** usar preload:

-  Si aún tienes subdominios que solo funcionan en HTTP.

-  Si no quieres un compromiso permanente con HTTPS en todo tu dominio.

-  Si acabas de implentar HSTS y quieres probarlo primero.


Cuando **SÍ** usar preload:

- Si todos los subdominios ya usan HTTPS correctamente.

- Si planeas mantener HTTPS de forma permanente y sin excepciones.

- Si quieres que los navegadores siempre accedan a tu dominio de manera segura, incluso en la primera visita.


## Pasos para Configurar HSTS con Apache

**Habilitar el módulo `headers´**

Si no está habilitado, ejecutar:

```bash
a2enmod headers
service apache2 reload
```


**Configurar SSL en el servidor**

Si no tienes habilitado **SSL** en Apache, configúralo. Tienes las indicaciones en el siguiente repositorio de git: <https://github.com/jmmedinac03vjp/PPS-Unidad3Actividad13-HardeningSevidorApache-HTTPS-HSTS>

**Configuración del sitio virtual**

Agregar o editar las siguientes líneas dentro de los archivos de configuración de tu sitio.

En este caso vamos a configurarlo sobre el archivo de configuración `ssl-default.conf`en el apartado de `ssl` puerto 443:

archivo `/etc/apache/sites-available/default-ssl.conf`
```apache
<VirtualHost *:80>

    ServerName www.pps.edu

    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>

<VirtualHost *:443>
    ServerName www.pps.edu

    DocumentRoot /var/www/html

    #activar uso del motor de protocolo SSL
    # Habilitar HSTS 
    Header always set Strict-Transport-Security "max-age=31536000"
    # Activar HSTS. Si queremos también en subdominios, comentamos la anterior y descomentamos éste
    #Header always set Strict-Transport-Security "max-age=63072000; includeSubDomains; preload"
    SSLEngine on
    SSLCertificateFile /etc/apache2/ssl/server.crt
    SSLCertificateKeyFile /etc/apache2/ssl/server.key
</VirtualHost>
```

![](images/HSTS.png)
![](images/HSTS.png)
![](images/HSTS.png)
![](images/HSTS.png)

NO usar **includeSubDomains** ni **preload**, ya que no aplican en localhost.


**Habilitar el sitio SSL y reiniciar Apache**

Si aún no se tiene habilitado el sitio SSL en Apache, ejecutar:

``` bash
a2ensite default-ssl
service apache2 reload
```
Probar que HSTS funciona correctamente
Ejecutar en la terminal:
curl -I https://localhost --insecure
Se debería obtener una respuesta con:
Strict-Transport-Security: max-age=31536000
o
 HSTS NO se aplicará en localhost en Chrome o Firefox por defecto.
o
 Solo servirá si accedes con https://localhost y confías en el certificado.
o
 Si se necesita una implementación real, es mejor probar en un dominio de desarrollo con HTTPS.
Mitigación y Mejores Prácticas
•
 Habilitar HSTS solo en sitios completamente migrados a HTTPS.
•
 Usar preload para asegurar que el navegador recuerde la configuración incluso después de cerrar la sesión.
## Código vulnerable
---





### **Código seguro**
---

Aquí está el código securizado:

🔒 Medidas de seguridad implementadas

- :

        - 

        - 



🚀 Resultado

✔ 

✔ 

✔ 

## ENTREGA

> __Realiza las operaciones indicadas__

> __Crea un repositorio  con nombre PPS-Unidad3Actividad6-Tu-Nombre donde documentes la realización de ellos.__

> No te olvides de documentarlo convenientemente con explicaciones, capturas de pantalla, etc.

> __Sube a la plataforma, tanto el repositorio comprimido como la dirección https a tu repositorio de Github.__

