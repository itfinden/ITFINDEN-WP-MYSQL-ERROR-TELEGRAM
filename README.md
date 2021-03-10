# ITFINDEN WP db-error.php



## RESUMEN

Uno de los problemas más comunes a los que se enfrenta un webmaster de WordPress es la caída ocasional de la conectividad de la base de datos. Dejado a su suerte, WordPress simplemente muestra al usuario final el mensaje

![Error establishing a database connection](img/error.png)

El webmaster no tiene forma de saber que se ha producido un error.

WordPress nos permite abordar este problema de la siguiente manera: si no puede conectarse a su base de datos, ejecutará el plugin drop-in `/wp-content/db-error.php` ([documentation](https://developer.wordpress.org/reference/functions/dead_db/)) si existe. Smart WP db-error.php utiliza esa funcionalidad incorporada para servir una página 503 informando a los usuarios de la interrupción, al tiempo que envía un correo electrónico a los webmasters para alertarles del problema - pero sólo a intervalos especificados (por defecto: 5 minutos), para no saturar sus servidores de correo y sus buzones.

Traducción realizada con la versión gratuita del traductor www.DeepL.com/Translator

![Smart WP db-error.php Error Messager](img/example.png)

## Instalacion

Para instalar ITFINDEN WP db-error.php, ejecute lo siguiente:

```sh
cd /path/to/wp-content
git clone https://github.com/agkozak/smart-wp-db-error.git
cd smart-wp-db-error
cp db-error.php.dist ../db-error.php
cd ..
```

At this point it is vitally necessary that you edit the new `/wp-content/db-error.php` file so as to include installation-specific information. The defaults are

```php
define( 'MAIL_TO', 'noc@itfinden.com' );
define( 'MAIL_FROM', 'alerta@itfinden.com' );
define( 'ALERT_INTERVAL', 300 );        // In seconds.
define( 'SUPPRESS_CREDITS', false );
```

`MAIL_TO` and `MAIL_FROM` should be addresses chosen to cause the least trouble for spam filters (e-mail sent by PHP from a webserver is likely to need whitelisting). `ALERT_INTERVAL` is the number of seconds between attempts at mailing the webmaster.

## Notes

If `/wp-content/db-error.php` is accessed directly, the error page will be displayed, but only for the purposes of showing the webmaster what the error page looks like. No e-mail will be sent. A `noindex` meta tag reminds search engines never to index the error page (an unlikely event anyway, as the page is served with a 503 status).

If, on the other hand, `/wp-content/smart-wp-db-error/smart-wp-db-error.php` is accessed directly, the `MAIL_TO`, `MAIL_FROM`, and `ALERT_INTERVAL` constants will not have been defined, and the script will `die` quietly.
