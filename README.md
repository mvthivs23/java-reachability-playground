

# Laboratorio Devsecops con Github Actions: Snyk y SonarCloud

<br>



Este repositorio tiene como objetivo proporcionar una guía paso a paso para integrar herramientas de análisis de código, seguridad y calidad dentro de un flujo de trabajo de GitHub Actions. A continuación, se describen las principales herramientas que se utilizarán:


<br>
<br>

## Snyk

<br>

![image](https://github.com/user-attachments/assets/03722a6e-6d4e-4c50-9ce7-4cbbbbf2abec)

<br>


Snyk es una plataforma que permite detectar y corregir vulnerabilidades en las dependencias de código, contenedores, infraestructura como código y configuraciones de nube. Snyk ayuda a los desarrolladores a identificar fallas de seguridad antes de que el código llegue a producción.
<br>
<br>
### Funcionalidades clave de Snyk:

* Detección de Vulnerabilidades: Escanea los archivos de dependencias para identificar vulnerabilidades conocidas en bibliotecas de terceros.
* Correcciones Automáticas: Snyk puede generar automáticamente parches o sugerencias de actualización para remediar vulnerabilidades.
* Monitoreo Continuo: Permite monitorear continuamente las dependencias para detectar nuevas vulnerabilidades una vez que se publican.
* Soporte para Múltiples Lenguajes: Compatible con varios lenguajes y entornos, incluyendo JavaScript, Python, Java, y más.
<br>
<br>

### Integración con GitHub Actions:
Snyk se puede integrar con GitHub Actions para ejecutar escaneos de seguridad automáticamente durante el ciclo de desarrollo, lo que asegura que cualquier vulnerabilidad en las dependencias se identifique antes de fusionar cambios al código principal.
<br>
<br>

Aqui te propirciono un laboratorio para conocer mas detalles de la integración de Snyk con Github Actions: [AQUI](./laboratorio-snyk.md).



<br>
<br>

## SonarCloud 
<br>

![image](https://github.com/user-attachments/assets/43afe73d-9a94-41e8-ae1b-4d8c24360b9e)

<br>


SonarCloud es una herramienta de análisis estático que ayuda a medir y mantener la calidad del código mediante la identificación de errores, vulnerabilidades de seguridad, deuda técnica y duplicaciones de código. Es una solución en la nube basada en SonarQube y se integra fácilmente en flujos de CI/CD.
<br>
<br>
### Funcionalidades clave de SonarCloud:
<br>
* Análisis de Calidad del Código: Proporciona métricas sobre la mantenibilidad, confiabilidad y seguridad del código.
* Detección de Vulnerabilidades de Seguridad: Identifica fallas que podrían comprometer la seguridad de la aplicación.
* Métrica de Cobertura de Pruebas: Informa sobre el porcentaje de cobertura de pruebas unitarias.
* Informe de Duplicación de Código: Detecta y reporta la duplicación de segmentos de código que podrían indicar mala calidad.
* Soporte Multilenguaje: Soporta múltiples lenguajes de programación como Java, JavaScript, Python, C++, entre otros.
<br>

### Integración con GitHub Actions:
<br>
SonarCloud puede integrarse con GitHub Actions para realizar análisis automáticos de calidad y seguridad en cada commit o pull request. Esto permite verificar continuamente el estado de la calidad del código y prevenir la introducción de nuevas vulnerabilidades o problemas técnicos.

<br>
<br>

Aqui te propirciono un laboratorio para conocer mas detalles de la integración de SonarCloud con Github Actions: [AQUI](./laboratorio-sonarcloud.md).

<br>






# Java Reachability Playground

This is an intentionally vulnerable application. It was purposely designed to demonstrate the capabilities of Snyk's Reachable
Vulnerabilities feature and includes both a "Reachable" vulnerability (with a direct data flow to the vulnerable function) and a "Potentially Reachable" vulnerability (where only partial data exists for determining reachability).


## Included vulnerabilities
### [Arbitrary File Write via Archive Extraction](https://app.snyk.io/vuln/SNYK-JAVA-ORGND4J-72550)
An exploit is using a vulnerability called [ZipSlip](https://snyk.io/research/zip-slip-vulnerability) - a critical vulnerability discovered 
by Snyk, which typically results in remote command execution. As part of the exploit, a special zip archive is 
crafted (attached as `malicious_file.zip`). When this file is extracted by a vulnerable function, it will create a file 
called `good.txt` in the folder `unzipped`, but it will also create a file called `evil.txt` in the `/tmp/` folder. 
This example is not dangerous, of course, but demonstrates the risk the vulnerability poses - imagine overwriting `.ssh/authorized_keys` or another sensitive file.

### [Deserialization of Untrusted Data](https://app.snyk.io/vuln/SNYK-JAVA-COMMONSCOLLECTIONS-472711)
This vulnerability is not exploited. It demonstrates potentially vulnerable code, for which data about vulnerable functions
is not available.

## How to run the demo (Maven)
1. Checkout this repository (`git checkout git@github.com:snyk/java-reachability-playground.git`)
2. Install all the dependencies (`mvn install`)
3. Compile the project (`mvn compile`)
4. Run the main class (`mvn exec:java -Dexec.mainClass=Unzipper`); the application should throw an exception saying `Malicious file /tmp/evil.txt was created`.
5. Run snyk command with Reachable Vulnerabilities flag (`snyk test --reachable` or `snyk monitor --reachable`); you should see the vulnerability `SNYK-JAVA-ORGND4J-72550` marked as reachable
and the function call path to the vulnerability

## For Gradle 
1. Make sure you build the artifacts with `./gradlew build`
2. To see test results run `snyk test --file=build.gradle --reachable` or monitor: `snyk monitor --file=build.gradle --reachable`
---

*Note: Once the java application is run, `malicious_file.zip` will be deleted by it. To run it again, run `git checkout .` prior
to next java run.*

## Screenshots

### CLI
![Snyk CLI Reachable Vulnerabilities](CLI_reachable.png)

### Snyk UI
![Snyk UI Reachable Vulnerabilities](UI_reachable.png)
