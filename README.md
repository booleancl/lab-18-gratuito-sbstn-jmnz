# Javascript-lab18

## Módulo 6: Herramientas del ecosistema Javascript
Conocer y manejar a nivel usuario las herramientas complementarias de Javascript.

### Unidad 4: Automatización

Utilizar herramientas basadas en node para automatizar pruebas simples		
Saber utilizar la documentación de Cypress para escribir buenas pruebas e2e )end-to-end) o pruebas de extremo a extremo

### Instalación del proyecto

- npm install
- npm run serve

Probar http://localhost:4000

#### 👀 Instrucciones 👀 

#### Cypress

###### ¿Qué es Cypress?
https://www.cypress.io/

###### ¿Quién lo usa?
https://www.cypress.io/case-studies

###### ¿Qué es lo que hace?

Cypress permite automatizar la experiencia de un usuario interactuando con un sitio web de manera de utilizar los conceptos de Pruebas de software para corroborar que los objetivos del sitio o aplicación se cumplen.

###### Instalación

```
npm install cypress --save-dev
```

> Para los porfiados que no programan con un entorno UNIX, pueden seguir esta guía y sufrir las consecuencias: https://shouv.medium.com/how-to-run-cypress-on-wsl2-989b83795fb6

Analicemos un poco que hace Cypress al instalarse:

👀 https://docs.cypress.io/guides/getting-started/installing-cypress#Installing 👀


Vamos a crear los siguientes scripts:

```
  "test": "cypress open",
  "test:ci": "cypress run"
```

Ahora ejecutamos:

```
npm test

```

y corremos algunos de los ejemplos para poder ver que posibilidades nos entrega Cypress.

ahora vamos a editar el directorio `cypress/1-getting-started` y le cambiaremos el nombre a `website`.

luego dejaremos solo el primer `it` y borraremos lo demás.
Ahora cambiaremos la instrucción que dice `cy.visit` y modificaremos por lo siguiente:

```javascript
  cy.visit('http://localhost:4000')
```
Luego de analizar porque falla vamos a cambiar el nombre del archivo a `home.spec.js` y el bloque de prueba debe quedar como lo siguiente:

```javascript
  it('should show right title', () => {
    cy.get('.hero-section h1').should('include.text', 'Programadores más influyentes de la historia')
  })
```

### Ejercicio 1

Vamos a integrar la librería [Pristine](https://github.com/sha256/Pristine) utilizando la metodología Test Driven Development (TDD).

Agregamos Pristine desde un CDN

`https://cdn.jsdelivr.net/npm/pristinejs@0.1.9/dist/pristine.min.js`

#### ¿Qué es TDD?

Iremos escribiendo distintas expectaciones relacionadas al objetivo que queremos lograr. Siempre partiremos de una prueba de software en estado rojo y la iremos iterando hasta que se vuelva verde. Haremos este proceso N cantidad de veces hasta que logremos la funcionalidad.

###### Objetivo

Hacer ambos entradas de texto del formulario requeridas.
Al presionar el boton "comentar" deberían validarse los campos y agregar el error "Este campo es obligatorio".

#### Ejercicio 2

Mejora al flujo con Cypress Cucumber Preprocessor
Ahora utilizaremos [Cucumber](https://cucumber.io/) para hacer más expresivas las pruebas de Software y permitir que otros roles relacionados a la construcción de productos digitales compartan sus impresiones al momento de definir un requerimiento. Utilizaremos esta tecnología de "Automatización" para escribir pruebas como por ejemplo:

```gerkhin
  Feature: Write comments in page in form
    Scenario: Send form without required fields
      Given the user in the homepage
      When press "comentar" button without fill the required fields
      Then the required fields show "Este campo es obligatorio"
```


###### Instalación y configuración

```
npm i -D cypress-cucumber-preprocessor
```

Agregar esta sección al `cypress.json`:

```
  "testFiles": "**/*.feature"
```

Agregar esto a `cypress/plugins/index.js`

```javascript
  const cucumber = require('cypress-cucumber-preprocessor').default

  module.exports = (on, config) => {
    on('file:preprocessor', cucumber())
  }
```

Y finalmente esto al `package.json`:

```json
  "cypress-cucumber-preprocessor": {
    "step_definitions": "cypress/step_definitions"
  }
```

###### Escribir archivo feature
Vamos a crear un archivo llamado `home.feature` en la ruta indicada en el siguiente trozo de texto:

** integration/website/home.feature
```
Feature: Write comments in page in form
  The user can write comments in page

  Scenario: Send form without required fields
    Given the user in the homepage
    When press "comentar" button without fill the required fields
    Then the required fields show "Este campo es obligatorio"
```

Ejecutamos `npm test` y vemos que sucede.

También podemos probar `npm run test:ci`


###### Escribir step definitions

Ahora vamos a migrar las pruebas ya escritas a la nueva forma de escribir pruebas. Para esto crearemos dentro del directorio `cypress` una carpeta llamada `step_definitions` y ahora vamos a crear un archivo `home.js` que se relacionará al `home.feature` escrito anteriormente.

Acá escribiremos el siguiente código:

```javascript

  import { Given } from "cypress-cucumber-preprocessor/steps";

  const url = 'http://localhost:4000'
  Given('the user in the homepage', () => {
    cy.visit(url)
  })

```

##### Integración continua

Si intentamos correr el comando `npm run test:ci` veremos problemas con correr las pruebas en un solo proceso.

Para esto utilizaremos el paquete "concurrently"

```
  npm install --save-dev concurrently wait-on
```

modificamos el script "test:ci" en el `package.json` con el comando anterior.

```
  concurrently 'npm run serve' 'wait-on http://localhost:4000 && cypress run' --kill-others --success first
```
y finalmente correr `npm run test:ci`

##### Desafío
Vamos a escribir pruebas para más casos analizando el directorio `2-advanced-examples` revisando el archivo relacionado a "Network requests" generando un flujo definido incluyendo una petición al servidor que pudiera o no existir.

¡Exito!
