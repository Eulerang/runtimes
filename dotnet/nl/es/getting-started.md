---

copyright:
  years: 2017
lastupdated: "2017-12-15"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:app_name: data-hd-keyref="app_name"}

# Guía de aprendizaje de iniciación
{: #getting_started}

* {: download} Enhorabuena, ha desplegado una aplicación de ejemplo Hello World en {{site.data.keyword.Bluemix}}.  Para empezar a trabajar, siga los pasos de esta guía. O bien <a class="xref" href="http://bluemix.net" target="_blank" title="(Descargue el código de ejemplo)"><img class="hidden" src="../../images/btn_starter-code.svg" alt="Descargue el código de aplicación" />descargue el código de ejemplo</a> y explore por su cuenta.

Si sigue la guía de aprendizaje de iniciación de Dotnet, configurará un entorno de desarrollo, desplegará una app localmente y en {{site.data.keyword.Bluemix}} e integrará un servicio de base de datos de {{site.data.keyword.Bluemix}} en su app.

## Antes de empezar
{: #prereqs}

Necesitará lo siguiente:
* [Cuenta de {{site.data.keyword.Bluemix_notm}}](https://console.ng.bluemix.net/registration/)
* [CLI de Cloud Foundry ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/cloudfoundry/cli#downloads){: new_window}
* [Git ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://git-scm.com/downloads){: new_window}
* Instale .NET Core 1.1 SDK 1.0.4 desde las instrucciones del sitio web [dot.net ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.microsoft.com/net/download/core).

## Paso 1: Clone la app de ejemplo
{: #clone}

Ahora está listo para empezar a trabar con la app. Clone el repositorio y vaya al directorio donde se encuentra la app de ejemplo.
  ```
git clone https://github.com/IBM-Bluemix/get-started-aspnet-core
  ```
  {: pre}
  ```
cd get-started-aspnet-core
  ```
  {: pre}

## Paso 2: Ejecute la app localmente
{: #run_locally}

Ejecute la app.
  ```
cd src/GetStartedDotnet
  ```
  {: pre}
  ```
dotnet restore
  ```
  {: pre}
  ```
dotnet run
  ```
  {: pre}

Visualice la app en: http://localhost:5000/

## Paso 3: Prepare la app para el despliegue
{: #prepare}

Para desplegar en {{site.data.keyword.Bluemix_notm}}, puede resultarle útil configurar un archivo manifest.yml. El archivo manifest.yml incluye información básica sobre la app, como por ejemplo el nombre, la cantidad de memoria a asignar para cada instancia y la ruta. Encontrará un archivo manifest.yml de ejemplo en el directorio `get-started-dotnet`.

Abra el archivo manifest.yml y cambie el valor de `name` de `GetStartedDotnet` por el nombre de su app, <var class="keyword varname" data-hd-keyref="app_name">app_name</var>.
{: download}

  ```
 applications:
 - name: GetStartedDotnet
   random-route: true
   memory: 512M
  ```
  {: codeblock}

En este archivo manifest.yml, **random-rout: true** genera una ruta aleatoria para la app a fin de evitar que su ruta entre en conflicto con otras.  Si lo desea, puede sustituir **random-route: true** por **host: myChosenHostName**, especificando el nombre de host que elija. [Más información...](/docs/manageapps/depapps.html#appmanifest)
{: tip}

## Paso 4: Despliegue la app
{: #deploy}

Puede utilizar la CLI de Cloud Foundry para desplegar apps.

Para empezar, inicie una sesión en su cuenta de {{site.data.keyword.Bluemix_notm}}:
  ```
cf login
  ```
  {: pre}

  Si no puede iniciar sesión utilizando los mandatos `cf login` o `bx login` porque tiene un ID de usuario federado, utilice los mandatos `cf login --sso` o `bx login --sso` para iniciar sesión con el ID de inicio de sesión único. Consulte [Inicio de sesión con un ID federado](https://console.bluemix.net/docs/cli/login_federated_id.html#federated_id) para obtener más información.

Elija el punto final de la API
  ```
cf api <API-endpoint>
  ```
  {: pre}

Sustituya *API-endpoint* en el mandato por un punto final de API de la siguiente lista.

| **Nombre de la región** | **Ubicación geográfica** | **Punto final de la API** |
|-----------------|-------------------------|-------------------|
| Región EE.UU. sur| Dallas, EE.UU.| api.ng.bluemix.net |
|  Región EE.UU este | Washington, DC, EE.UU | api.us-east.bluemix.net |
| Región del Reino Unido| Londres, Inglaterra| api.eu-gb.bluemix.net |
| Región de Sídney| Sídney, Australia | api.au-syd.bluemix.net |
|  Región de Alemania | Frankfurt, Alemania | api.eu-de.bluemix.net |
{: caption="Tabla 1. Lista de regiones de {{site.data.keyword.cloud_notm}}" caption-side="top"}

**Asegúrese de que está en el directorio principal, `get-started-aspnet-core`, para la aplicación** que envíe la aplicación a {{site.data.keyword.Bluemix_notm}}
  ```
cf push
  ```
  {: pre}

Esto puede tardar un minuto. Si hay algún error en el proceso de despliegue, puede utilizar el mandato `cf logs <Your-App-Name> --recent` para resolver problemas.

Cuando finalice el despliegue, verá un mensaje que indica que la app se está ejecutando.  Visualice la app en el URL que aparece en la salida del mandato push.  También puede emitir el mandato
  ```
cf apps
  ```
  {: pre}
  para ver el estado de su app y ver el URL.

## Paso 5: Conecte una base de datos MySQL
{: connect_mysql}

A continuación, añadiremos una base de datos ClearDB MySQL a esta aplicación y configuraremos la aplicación para que se pueda ejecutar localmente y en {{site.data.keyword.Bluemix_notm}}.

1. Inicie una sesión en {{site.data.keyword.Bluemix_notm}} en su navegador. Vaya al `Panel de control`. Seleccione su aplicación pulsando su nombre en la columna `Nombre`.
2. Pulse `Conexiones` y, a continuación, `Crear conexión`.
2. En la sección `Data & Analytics`, seleccione `Base de datos ClearDB MySQL gestionada` y `Crear` para crear el servicio.
3. Seleccione `Volver a transferir` cuando se le solicite. {{site.data.keyword.Bluemix_notm}} reiniciará la aplicación y proporcionará las credenciales de base de datos para la aplicación mediante la variable de entorno `VCAP_SERVICES`. Esta variable de entorno sólo está disponible para la aplicación cuando se ejecuta en {{site.data.keyword.Bluemix_notm}}.

Las variables de entorno le permiten separar valores de despliegue del código fuente. Por ejemplo, en lugar codificar una contraseña de base de datos, puede guardarla en una variable de entorno a la que haga referencia en el código fuente. [Más información...](/docs/manageapps/depapps.html#app_env)
{: tip}

## Paso 6: Utilice la base de datos localmente
{: #use_database}

Ahora vamos a actualizar el código local para que apunte a esta base de datos. Guardaremos las credenciales correspondientes a los servicios en un archivo json. Este archivo SOLO se utilizará cuando la aplicación se ejecute localmente. Cuando se ejecute en {{site.data.keyword.Bluemix_notm}}, las credenciales se leerán de la variable de entorno VCAP_SERVICES.

1. Cree el archivo src/GetStartedDotnet/vcap-local.json

2. En el navegador, abra la interfaz de usuario de {{site.data.keyword.Bluemix_notm}}, seleccione la app > Conexiones -> Base de datos ClearDB MySQL gestionada -> Ver credenciales

3. Copie y pegue todo el objeto json de las credenciales en el archivo `vcap-local.json` y guarde los cambios.  El resultado se parecerá al siguiente:
  ```
  {
  "cleardb": [
    {
      "credentials": {
        ...
        "uri": "mysql://user:password@some-hostname.cleardb.net:3306/database-name?reconnect=true",
        ...
      },
      ...
      "name": "My ClearDB service instance name",
      ...
    }
  ]
}
  ```

4. Desde el directorio `get-started-aspnet-core/src/GetStartedDotnet`, reinicie la aplicación con el mandato `dotnet run`.

  Renueve la vista del navegador en http://localhost:5000/. Cualquier nombre que especifique en la app se añadirá ahora a la base de datos.

  La app local y la app {{site.data.keyword.Bluemix_notm}} comparten la base de datos.  Visualice la app {{site.data.keyword.Bluemix_notm}} en el URL que aparece en la salida del mandato push anterior.  Los nombres que añada desde cualquiera de las apps deberían aparecer cuando renueve los navegadores.

Recuerde que, si no necesita la app en directo, debe detenerla para no incurrir en cargos inesperado.
{: tip}

## Siguientes pasos

* [Guías de aprendizaje](/docs/tutorials/index.html)
* [Ejemplos ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://ibm-cloud.github.io){: new_window}
* [Centro de arquitectura ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/cloud/garage/category/architectures){: new_window}
