# IAM (Identity & Access Manager) Keycloak + Oauth2 Proxy + Apache HTTP

Este proyecto es un docker compose que arma un entorno de Identity & Access Manager (IAM) sin la necesidad de escribir codigo.

La finalidad es el de proteger una web app y/o una API REST, sin tener que codear un modulo de login/SSO.

## Entorno utilizado

- Docker version 20.10.12, build e91ed57
- Keycloak 18.0.0
- Oauth2 Proxy 7.3.0
- Apache HTTP 2.4


## Configura el archivo /etc/hosts

Agregar las siguientes lineas:

127.0.0.1 example.me
127.0.0.1 idp.example.me


## Crear docker network

desde la linea de comandos, ejecutar las siguiente linea:

$ docker network create proxy


## Iniciar IAM

desde la linea de comandos, ejecutar las siguiente linea:

$ docker-compose up -d

Salida por consola:

Creating keycloak ... done <br />
Creating apache_http ... done <br />
Creating oauth2-proxy ... done


## Detener IAM

desde la linea de comandos, ejecutar las siguiente linea:

$ docker-compose down

Salida por consola:

Stopping oauth2-proxy ... done <br />
Stopping apache_http  ... done <br />
Stopping keycloak     ... done <br />
Removing oauth2-proxy ... done <br />
Removing apache_http  ... done <br />
Removing keycloak     ... done <br />
Network proxy is external, skipping


## Ingresar a la consola de Keycloak

Desde un navegador, ingresar a la url http://idp.examplo.me:8080/admin/

<img src="readme_images/keycloak-login.png"  width="400" />

Utilizar las credenciales admin/admin


## Crear el usuario con permisos para ver la web protegida

Ya dentro de la consola de administracion de Keycloak, ir al menu:

- Realm Master -> Manage -> Users
- Click en el boton "Add user"
- Completar los campos

Username: test <br />
Email: test@example.me <br />
First Name: Test <br />
Last Name: Example <br />
User Enabled: ON <br />
Email verified: ON <br />
Groups:  <br />
Required User Actions:  <br />

- Click en el boton "Save"

Al guardar se igresa automaticamente al modo de edicion del usuario.

- Click en la solapa "Credentials"
- Completar el password y la confirmacion con "test"/"test"
- Temporary: OFF
- Click en el boton "Reset Password"


## Crear el cliente que protege nuestra web

Ya dentro de la consola de administracion de Keycloak, ir al menu:

- Realm Master -> Configure -> Clients
- Click en el boton "Create"
- Completar los campos

Client ID: oauth2-proxy <br />
Name: Oauth2 Proxy <br />
Enable: ON <br />
Client Protocol: openid-connect <br />
Access Type: confidential <br />
Standard Flow Enabled: ON <br />
Direct Access Grants Enabled: ON <br />
Valid Redirect URIs: http://example.me:80/oauth2/callback <br />

- Click en el boton "Save"
- Bajar y volver a ejecutar docker compose

## Ingresar a la web protegida

Ingresar a http://example.me/

<img src="readme_images/oauth2-proxy.png"  width="400" />

Click en el boton "Sign in with OpenID Connect"

<img src="readme_images/keycloak-login.png"  width="400" />

Igresar la credenciales:
- User: test
- Password: test

Al hacer click en el boton "Sign In", se debe presentar la pantalla de bienvenida del Apache HTTP

<img src="readme_images/apache-works.png"  width="400" />


