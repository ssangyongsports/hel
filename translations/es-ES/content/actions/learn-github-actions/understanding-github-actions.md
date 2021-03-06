---
title: Entender las GitHub Actions
shortTitle: Entendiendo las GitHub Actions
intro: 'Aprende lo básico de las {% data variables.product.prodname_actions %}, incluyendo los conceptos nucleares y la terminología esencial.'
redirect_from:
  - /github/automating-your-workflow-with-github-actions/core-concepts-for-github-actions
  - /actions/automating-your-workflow-with-github-actions/core-concepts-for-github-actions
  - /actions/getting-started-with-github-actions/core-concepts-for-github-actions
  - /actions/learn-github-actions/introduction-to-github-actions
versions:
  fpt: '*'
  ghes: '*'
  ghae: '*'
  ghec: '*'
type: overview
topics:
  - Fundamentals
---

{% data reusables.actions.enterprise-beta %}
{% data reusables.actions.enterprise-github-hosted-runners %}
{% data reusables.actions.ae-beta %}

## Resumen

Las {% data variables.product.prodname_actions %} te ayudan a automatizar tareas dentro de tu ciclo de vida de desarrollo de software. Las {% data variables.product.prodname_actions %} se manejan por eventos, lo cual significa que puedes ejecutar una serie de comandos después de que haya ocurrido un evento especificado. Por ejemplo, cada vez que alguien crea una solicitud de cambios para un repositorio, puedes ejecutar automáticamente un comando que ejecute un script de prueba de software.

Este diagrama ilustra como puedes utilizar las {% data variables.product.prodname_actions %} para ejecutar automáticamente tus scripts de pruebas de software. Un evento activa el _flujo de trabajo_ automáticamente, el cual contiene un _job_. Entonces, el job utiliza _pasos_ para controlar el orden en el que se ejecutan las _acciones_. Estas acciones son los comandos que automatizan las pruebas de tu software.

![Resumen del flujo de trabajo](/assets/images/help/images/overview-actions-simple.png)

## Los componentes de las {% data variables.product.prodname_actions %}

A continuación, encontrarás una lista de los diferentes componentes de las {% data variables.product.prodname_actions %} que funcionan en conjunto para ejecutar jobs. Puedes ver cómo dichos componentes interactúan entre ellos.

![Resumen de componentes y servicios](/assets/images/help/images/overview-actions-design.png)

### Flujos de trabajo

El flujo de trabajo es un procedimiento automatizado que agregas a tu repositorio. Los flujos de trabajo se componen de uno o más jobs y pueden programarse o activarse a través de un evento. El flujo de trabajo se puede utilizar para crear, probar, empacar, lanzar o desplegar un proyecto en {% data variables.product.prodname_dotcom %}. {% ifversion fpt or ghes > 3.3 or ghae-issue-4757 or ghec %}Puedes referenciar un flujo de trabajo dentro de otro flujo de trabajo, consulta la sección "[Reutilizar flujos de trabajo](/actions/learn-github-actions/reusing-workflows)".{% endif %}

### Eventos

En evento es una actividad específica que activa un flujo de trabajo. Por ejemplo, la actividad se puede originar desde {% data variables.product.prodname_dotcom %} cuando alguien sube una confirmación a un repositorio o cuando se crea una propuesta o solicitud de extracción. También puedes utilizar el [webhook de envío del repositorio](/rest/reference/repos#create-a-repository-dispatch-event) para activar un flujo de trabajo cuando ocurra un evento externo. Para encontrar una lista de eventos completa que puede utilizarse para activar flujos de trabajo, consulta los [Eventos que activan flujos de trabajo](/actions/reference/events-that-trigger-workflows).

### Jobs

Un job es un conjunto de pasos que se ejecutan en el mismo ejecutor. Predeterminadamente, un flujode trabajo con varios jobs los ejecutará en paralelo. También puedes configurar el flujo de trabajo para que los ejecute secuencialmente. Por ejemplo, un flujo de trabajo puede tener dos trabajos consecutivos para desarrollar y probar el código. El trabajo de prueba depende del estado del trabajo de desarrollo. Si el trabajo de desarrollo falla, no se ejecutará el trabajo de prueba.

### Pasos

Un paso es una tarea individual que puede ejecutar comandos en un job. Un paso puede ser tanto una _acción_ como un comando de shell. Cada paso en un job se ejecuta en el mismo ejecutor, lo cual permite que las acciones en dicho job compartan datos entre ellas.

### Acciones

Las _Acciones_ son comandos independientes que se combinan en _pasos_ para crear un _job_. Las acciones son el componente portable más pequeño de un flujo de trabajo. Puedes crear tus propias acciones, o utilizar acciones que haya creado la comunidad de {% data variables.product.prodname_dotcom %}. Para utilizar una acción en un flujo de trabajo, debes incluirla como paso.

### Ejecutores

{% ifversion ghae %}Un ejecutor es un servidor que tiene instalada la [{% data variables.product.prodname_actions %} aplicación de ejecutor](https://github.com/actions/runner). En el caso de {% data variables.product.prodname_ghe_managed %}, puedes utilizar los {% data variables.actions.hosted_runner %} con seguridad robustecida que están empaquetados con tu instancia en la nube. Un ejecutor escucha a los jobs disponibles, ejecuta un job a la vez, y reporta el progreso, bitácoras y resultados de vuelta a {% data variables.product.prodname_dotcom %}. Los {% data variables.actions.hosted_runner %} ejecutan cada job del flujo de trabajo en un ambiente virtual nuevo. Para obtener más información, consulta la sección "[Acerca de los {% data variables.actions.hosted_runner %}](/actions/using-github-hosted-runners/about-ae-hosted-runners)".
{% else %}
Un ejecutor es un servidor que tiene instalada la [aplilcación de ejecutor de {% data variables.product.prodname_actions %}](https://github.com/actions/runner). Puedes utilizar un ejecutor que esté hospedado en {% data variables.product.prodname_dotcom %}, o puedes hospedar el tuyo propio. Un ejecutor escucha a los jobs disponibles, ejecuta un job a la vez, y reporta el progreso, bitácoras y resultados de vuelta a {% data variables.product.prodname_dotcom %}. Los ejecutores hospedados en {% data variables.product.prodname_dotcom %} se basan en Ubuntu Linux, Microsoft Windows y macOS, y cada job en un flujo de trabajo se ejecuta en un ambiente virtual nuevo.  Para obtener más información sobre los ejecutores hospedados en {% data variables.product.prodname_dotcom %}, consulta la sección "[Acerca de los ejecutores hospedados en {% data variables.product.prodname_dotcom %}](/actions/using-github-hosted-runners/about-github-hosted-runners)". Si necesitas un sistema operativo diferente o si requieres de una configuración de hardware específica, puedes hospedar tus propios ejecutores. Para obtener información sobre los ejecutores auto-hospedados, consulta la sección "[Hospedar tus propios ejecutores](/actions/hosting-your-own-runners)".
{% endif %}

## Crear un flujo de trabajo de ejemplo

Las {% data variables.product.prodname_actions %} usan la sintaxis de YAML para definir los eventos, jobs y pasos. Estos archivos de YAML se almacenan en el repositorio de tu código, en un directorio que se llama `.github/workflows`.

Puedes crear un flujo de trabajo de ejemplo en tu repositorio que active automáticamente una serie de comandos cada que se suba código. En este flujo de trabajo, las {% data variables.product.prodname_actions %} verifican el código que se subió, instalan las dependencias de software, y ejecutan `bats -v`.

1. En tu repositorio, crea el directorio `.github/workflows/` para almacenar tus archivos de flujo de trabajo.
1. En el directorio `.github/workflows/`, crea un archivo nuevo que se llame `learn-github-actions.yml` y agrega el siguiente código.
    ```yaml
    name: learn-github-actions
    on: [push]
    jobs:
      check-bats-version:
        runs-on: ubuntu-latest
        steps:
          - uses: actions/checkout@v2
          - uses: actions/setup-node@v2
            with:
              node-version: '14'
          - run: npm install -g bats
          - run: bats -v
    ```
1. Confirma estos cambios y cárgalos a tu repositorio de {% data variables.product.prodname_dotcom %}.

Tu archivo de flujo de trabajo de {% data variables.product.prodname_actions %} nuevo estará ahora instalado en tu repositorio y se ejecutará automáticamente cada que alguien suba un cambio a éste. Para encontrar los detalles sobre el historial de ejecución un job, consulta la sección "[Visualizar la actividad del flujo de trabajo](/actions/learn-github-actions/introduction-to-github-actions#viewing-the-jobs-activity)".

## Entender el archivo de flujo de trabajo

Para ayudarte a entender cómo se utiliza la sintaxis de YAML para crear un flujo de trabajo, esta sección explica cada línea del ejemplo de la introducción:

<table>
<tr>
<td>

  ```yaml
  name: learn-github-actions
  ```
</td>
<td>
  <em>Opcional</em> - El nombre del flujo de trabajo ta como aparece en la pestaña de Acciones del repositorio de {% data variables.product.prodname_dotcom %}.
</td>
</tr>
<tr>
<td>

  ```yaml
  on: [push]
  ```
</td>
<td>
  Especifica el evento que activa automáticamente el archivo de flujo de trabajo. Este ejemplo utiliza el evento <code>push</code>, para que los jobs se ejecuten cada que alguien sube un cambio al repositorio. Puedes configurar el flujo de trabajo para que solo se ejecuten en ciertas ramas, rutas, o etiquetas. Para encontrar ejemplos de sintaxis que incluyan o excluyan ramas, rutas, o etiquetas, consulta la sección <a href="https://docs.github.com/actions/reference/workflow-syntax-for-github-actions#onpushpull_requestpaths">"Sintaxis de flujo de trabajo para {% data variables.product.prodname_actions %}".</a>
</td>
</tr>
<tr>
<td>

  ```yaml
  jobs:
  ```
</td>
<td>
 Agrupa los jobs que se ejecutan en el archivo de flujo de trabajo <code>learn-github-actions</code>.
</td>
</tr>
<tr>
<td>

  ```yaml
  check-bats-version:
  ```
</td>
<td>
  Define el nombre del job <code>check-bats-version</code> que se almacena en la sección <code>jobs</code>.
</td>
</tr>
<tr>
<td>

  ```yaml
    runs-on: ubuntu-latest
  ```
</td>
<td>
  Configura el job para ejecutarse en un ejecutor Ubuntu Linux. Esto significa que el job se ejecutará en una máquina virtual nueva que se hospede en GitHub. Para encontrar ejemplos de sintaxis que utilicen otros ejecutores, consulta la sección <a href="https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idruns-on">"Sintaxis de flujo de trabajo para {% data variables.product.prodname_actions %}".</a>
</td>
</tr>
<tr>
<td>

  ```yaml
    steps:
  ```
</td>
<td>
  Agrupa todos los pasos que se ejecutan en el job <code>check-bats-version</code>. Cada elemento anidado bajo esta sección es un comando de shell o acción separada.
</td>
</tr>
<tr>
<td>

  ```yaml
      - uses: actions/checkout@v2
  ```
</td>
<td>
  La palabra clave <code>uses</code> le dice al job que recupere la <code>v2</code> de la acción comunitaria que se llama <code>actions/checkout@v2</code>. Esta es una acción que revisa tu repositorio y lo descarga al ejecutor, lo que te permite ejecutar acciones contra tu código (tales como las herramientas de prueba). Debes utilizar la acción de verificación cada que tu flujo de trabajo se ejecute contra el código del repositorio o cada que estés utilizando una acción definida en el repositorio.
</td>
</tr>
<tr>
<td>

  ```yaml
      - uses: actions/setup-node@v2
        with:
          node-version: '14'
  ```
</td>
<td>
  Este paso utiliza la acción <code>actions/setup-node@v2</code> para instala la versión especificada del paquete de software del <code>node</code> en el ejecutor, lo cual te otorga acceso al comando <code>npm</code>.
</td>
</tr>
<tr>
<td>

  ```yaml
      - run: npm install -g bats
  ```
</td>
<td>
  La palabra clave <code>run</code> le dice al job que ejecute un comando en el ejecutor. Ene ste caso, estás utilizando <code>npm</code> para instalar el paquete de pruebas del software <code>bats</code>.
</td>
</tr>
<tr>
<td>

  ```yaml
      - run: bats -v
  ```
</td>
<td>
  Finalmente, ejecutarás el comando <code>bats</code> con un parámetro que producirá la versión del software.
</td>
</tr>
</table>

### Visualizar el archivo de flujo de trabajo

En este diagrama, puedes ver el archivo de flujo de trabajo que acabas de crear, así como la forma en que los componentes de {% data variables.product.prodname_actions %} se organizan en una jerarquía. Cada paso ejecuta una acción simple o un comando de shell. Los pasos 1 y 2 utilizan acciones comunitarias preconstruidas. Los pasos 3 y 4 ejecutan comandos de shell directamente en el ejecutor. Para encontrar más acciones preconstruidas para tus flujos de trabajo, consulta la sección "[Encontrar y personalizar acciones](/actions/learn-github-actions/finding-and-customizing-actions)".

![Resumen del flujo de trabajo](/assets/images/help/images/overview-actions-event.png)

## Visualizar la actividad de un job

Una vez que tu job comience a ejecutarse, podrás{% ifversion fpt or ghes > 3.0 or ghae or ghec %}ver una gráfica de visualización del progreso de dicha ejecución y {% endif %}ver la actividad de cada paso en {% data variables.product.prodname_dotcom %}.

{% data reusables.repositories.navigate-to-repo %}
1. Debajo del nombre de tu repositorio, da clic en **Acciones**. ![Navegar al repositorio](/assets/images/help/images/learn-github-actions-repository.png)
1. En la barra lateral izquierda, da clic en el flujo de trabajo que quieras ver. ![Impresión de pantalla de los resultados del flujo de trabajo](/assets/images/help/images/learn-github-actions-workflow.png)
1. Debajo de "Ejecuciones de flujo de trabajo", da clic en el nombre de la ejecución que quieres ver. ![Captura de pantalla de las ejecuciones del flujo de trabajo](/assets/images/help/images/learn-github-actions-run.png)
{% ifversion fpt or ghes > 3.0 or ghae or ghec %}
1. Debajo de **Jobs** o en la gráfica de visualización, da clic en el job que quieras ver. ![Seleccionar job](/assets/images/help/images/overview-actions-result-navigate.png)
{% endif %}
{% ifversion fpt or ghes > 3.0 or ghae or ghec %}
1. Ve los resultados de cada paso. ![Impresión de pantalla de los detalles de la ejecución del flujo de trabajo](/assets/images/help/images/overview-actions-result-updated-2.png)
{% elsif ghes %}
1. Da clic en el nombre del job para ver los resultados de cada paso. ![Impresión de pantalla de los detalles de la ejecución del flujo de trabajo](/assets/images/help/images/overview-actions-result-updated.png)
{% else %}
1. Da clic en el nombre del job para ver los resultados de cada paso. ![Impresión de pantalla de los detalles de la ejecución del flujo de trabajo](/assets/images/help/images/overview-actions-result.png)
{% endif %}

## Pasos siguientes

Para seguir aprendiendo sobre las {% data variables.product.prodname_actions %}, consulta la sección "[Encontrar y personalizar las acciones](/actions/learn-github-actions/finding-and-customizing-actions)".

Para entender cómo funciona la facturación de las {% data variables.product.prodname_actions %}, consulta la sección "[Acerca de la facturación para las {% data variables.product.prodname_actions %}](/actions/reference/usage-limits-billing-and-administration#about-billing-for-github-actions)".

## Contactar con soporte técnico

{% data reusables.github-actions.contacting-support %}
