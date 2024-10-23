# Laboratorio Snyk con Github Actions

<br> 
En este laboratorio utilizaremos Snyk lo cual nos permuite realizar Software Composition Analisys o SCA, esto se utiliza para analizar las dependencias de software de código abierto o de terceros dentro de un proyecto. Su objetivo principal es identificar vulnerabilidades de seguridad, problemas de licencias y riesgos asociados con el uso de componentes externos.

<br>

## Creación de cuenta
<br>
Primero crearemos una cuenta en al plataforma de Snyk para pode realizar los demás pasos.

<br>

Ingresamos a la plataforma: 

<br> 

![image](https://github.com/user-attachments/assets/868668f3-17b7-4a86-823d-4a83a9a30ede)

<br>

Luego debes iniciar sesión con tu cuenta de Github. (Recordar que te solicitará usuario y contraseña de GitHub).

<br>

![image](https://github.com/user-attachments/assets/9a7f2676-57ad-4867-97a6-5eeaf141684a)

<br>

Una vez registrados en la plataforma, podremos visualizar nuestro nombre de usuario y el dashboard de Snyk:

<br>

![image](https://github.com/user-attachments/assets/3901ba9e-1f1c-4182-814d-b1d19345499c)

<br>

<br>


## Generar token de autenticación

<br>

Para trabajar con Snyk y Github Actions necesitamos crear un token en nuestra plataforma de Snyk.

<br>

![image](https://github.com/user-attachments/assets/e5ea94dd-cd3c-4818-8bbb-2659253a6235)

<br>

En esta sección podremos seleccionar el cuadro de texto (opción 1), haciendo click nos mostrará nuestro token. La otra opcioón si necesitas generar otro, presionas la opción 2 (Revoke y Regenerate).

<br>

![image](https://github.com/user-attachments/assets/cb64c73d-bc78-4d11-90f6-2a4a369d0506)

<br>



<br>



# Integración con Github Actions

<br>

Para integrar Snyk con Github Actions debemos tener un repositorio con algún proyecto, en este caso utilizaremos uno proporcionado por Snyk: https://github.com/snyk/java-reachability-playground

<br>

![image](https://github.com/user-attachments/assets/6abede89-6d09-4dfa-8862-e981fdb76ebc)

<br>

Luego iremos al MarketPlace de Github Actions para revisar a mas detalles lo que necesitamos utilizar: https://github.com/marketplace?query=Snyk

<br>

![image](https://github.com/user-attachments/assets/b8ad8bf1-605d-422e-9482-c6dff1ef0d2d)

<br>

Podremos visualizar multiples lenguajes para integrar Snyk, en este caso nosotros tenemos un proyecto de Java, entonces seleccionaremos "Maven".

<br>

![image](https://github.com/user-attachments/assets/da9b53ca-d20a-471a-a7bd-dfcd68d1bb28)

<br>

Podremos seleccionar el codigo en lenguaje de serialización YML proporcionado por Snyk.

<br>

<pre>
<code>
name: Example workflow for Maven using Snyk
on: push
jobs:
  security:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@master
      - name: Run Snyk to check for vulnerabilities
        uses: snyk/actions/maven@master
        env:
          SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}
</code>
</pre>
<button class="copy-btn" data-clipboard-target="#tuCodigo"></button>
<br>

<br>

Recordar que en GitHub, los archivos YAML que definen los flujos de trabajo de GitHub Actions deben ubicarse en un directorio específico dentro del repositorio. El directorio correcto es:

<br>

<pre>
<code>
.github/workflows/
</code>
</pre>
<br>


<br>

Ahora debemos configurar el token de Snyk en Github Actions.

<br>

![image](https://github.com/user-attachments/assets/8408420d-533b-46ae-b73c-e744d722dadf)

<br>

![image](https://github.com/user-attachments/assets/0d002964-0c6d-40ca-b786-c273e1897360)

<br>

![image](https://github.com/user-attachments/assets/aaa94146-6cd4-4ff5-86f4-cc37feec6ba5)

<br>

Luego al seleccionar Crear nuevo token, indicamos el nombre que debe ser SNYK_TOKEN y el valor correspondiente.

<br>

![image](https://github.com/user-attachments/assets/b5e483ba-9670-41cc-8915-0689367f87fb)

<br>


Luego de agregar nuestro archivo YAML al directorio, se ejecutará el actions correspondiente a tu fluo de trabajo.

<br>

![image](https://github.com/user-attachments/assets/15e6a7ad-e46c-463a-ae85-8c105b1a1646)

<br>
<br>

Dentro del log del Actions podremos visualizar que se encontraron vulnerabilidades en las dependencias del proyecto lo cual falló el workflow.

<br>

![image](https://github.com/user-attachments/assets/28518770-a0c8-4ce1-89da-b7e0d1f2f9aa)

<br>

En el caso que necesites que pase con exito el workflow debes agregar esta linea de codidgo para continuar aunque falle.

<br>

<pre>
<code>
name: Example workflow for Maven using Snyk
on: push
jobs:
  security:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@master
      - name: Run Snyk to check for vulnerabilities
        uses: snyk/actions/maven@master
        continue-on-error: true
        env:
          SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}
</code>
</pre>
<button class="copy-btn" data-clipboard-target="#tuCodigo"></button>
<br>

Se probaron 14 dependencias, y se encontraron 24 vulnerabilidades, 24 de las cuales tienen caminos vulnerables en el código.

<br>


## Detalles del informe

<br>

* Cantidad de vulnerabilidades encontradas: Se probaron 14 dependencias, y se encontraron 24 vulnerabilidades, 24 de las cuales tienen caminos vulnerables en el código.

* Actualizaciones sugeridas para solucionar problemas:

commons-collections: La versión 3.2.1 tiene varias vulnerabilidades de Deserialization of Untrusted Data (Deserialización de datos no confiables) con niveles de severidad que van desde medium hasta critical. Se recomienda actualizar a commons-collections 3.2.2.
<br>

* Problemas que no tienen actualización directa o parche disponible:

Por ejemplo, varias vulnerabilidades relacionadas con guava en la versión 20.0. Algunas de estas vulnerabilidades se resolvieron en versiones más recientes como 24.1.1 o 30.0, pero no se pueden arreglar directamente en tu código debido a dependencias transitivas (es decir, otras bibliotecas dependen de esta versión).
<br>

* Dependencias vulnerables adicionales:

Otras bibliotecas como commons-io, commons-codec, org.yaml:snakeyaml y org.apache.commons:commons-compress tienen problemas como Denial of Service (DoS), Buffer Overflows, y Zip Slip (escritura arbitraria de archivos).
<br>

* Soluciones posibles:

Actualizar las dependencias mencionadas a versiones seguras, como guava 30.0, commons-compress 1.21, snakeyaml 1.31, etc.
Si no hay una actualización disponible o es difícil actualizar debido a otras dependencias, es recomendable mitigar los problemas mediante medidas de seguridad adicionales, como deshabilitar la funcionalidad vulnerable o ajustar configuraciones de seguridad en tu aplicación.
