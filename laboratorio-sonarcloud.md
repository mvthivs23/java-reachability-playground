# SonarCloud y Github Actions

<br>
En este laboratorio te presentaré como integrar SonarCloud mediante Github Actions.
<br>
<br>

## Creación de cuenta en SonarCloud
<br>
Para ingresar a la plataforma de SonarCloud debemos irnos a la siguiente URL: https://sonarcloud.io/login
<br>

Debemos ingresar y loguearnos a través de Github.

<br>

![image](https://github.com/user-attachments/assets/14aca858-586d-4379-90be-039b3996062c)

<br>

Luego en el panel de la cuenta debemos ir a "My Account", luego "Security" para poder generar el token necesario para integrar con Github Actions.

<br>

![image](https://github.com/user-attachments/assets/c2a128b0-9574-4d33-ae22-54effcff8a68)

<br>

Luego debemos generar el token necesario para incluirlo en nuestro Actions.

<br>

![image](https://github.com/user-attachments/assets/eaa1d127-dd68-44d0-bee1-0ffe38173578)

<br>

Posteriormente a la creación del token, debemos dirigirnos a Github para incluir nuestro token.

<br>

![image](https://github.com/user-attachments/assets/b4c75fbc-f71a-4820-baad-3a5d1b0bf377)

<br>

![image](https://github.com/user-attachments/assets/598878de-8d35-4e9c-804f-9ba066134bea)

<br>

![image](https://github.com/user-attachments/assets/77ae8a21-2208-402c-bfc2-02aff75c2008)

<br>

Una vez generado el token, debes crear una organización.

<br>

![image](https://github.com/user-attachments/assets/9f3093a4-fdfe-4cac-a366-88b3b1d639d0)

<br>
![image](https://github.com/user-attachments/assets/ccf54ca6-c159-42d8-9574-3625428e2e03)

<br>

![image](https://github.com/user-attachments/assets/456ecf8f-5236-433f-8f71-8da197c60ed3)

<br>

![image](https://github.com/user-attachments/assets/37344864-116e-4ff4-92a7-66ccfea57709)

<br>

![image](https://github.com/user-attachments/assets/449a4270-31b2-4808-9c64-e8e3af72ef47)

<br>

![image](https://github.com/user-attachments/assets/a41a0373-02d6-4f96-ac38-5b8f8c34dd27)

<br>

![image](https://github.com/user-attachments/assets/a759ea52-49ae-4c87-a671-f779486a36f2)

<br>

![image](https://github.com/user-attachments/assets/866e7ebc-d3e4-43a3-b5d0-4e05e8692589)

<br>
<br>

Luego podrás crear el archivo YAML correspondiente a nuestro flujo de trabajo.

<br>

<br>
<pre>
<code>
name: Run SonarQube with Maven
on: push

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    - name: Set up JDK 17
      uses: actions/setup-java@v3
      with:
        java-version: '17'
        distribution: 'adopt'
        cache: maven
    - name: Build with Maven cloud
      run:  mvn -B verify sonar:sonar -Dsonar.projectKey=java-project-reachabilitys_javavulnerabilities -Dsonar.organization=java-project-reachabilitys -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=$SONAR_TOKEN
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
</code>
</pre>
<button class="copy-btn" data-clipboard-target="#tuCodigo"></button>
<br>
<br>

Posteriormente al agregar tu archivo YAML comenzará a ejecutarse el flujo de trabajo en la sección "Actions" de Github.

<br>

![image](https://github.com/user-attachments/assets/8e146c72-6862-453f-81ab-aad007de4111)

<br>

Podremos ver el detalle de cada steps.
<br>

![image](https://github.com/user-attachments/assets/7aff26a3-939a-4aef-a297-523286c0f2f4)

<br>

Luego podremos visualizar el analisis en el panel de SonarCloud, con información de las issues, ramas analizadas, etc.

<br>

![image](https://github.com/user-attachments/assets/1c2fe746-3c4a-4689-b636-4f04ac73b803)
