# Projeto de Encurtador de URL com Microserviços AWS

Este projeto implementa um sistema de encurtamento de URLs usando microserviços, aproveitando a infraestrutura da AWS, incluindo S3, Lambda e API Gateway. O projeto consiste em dois microserviços principais: um para criação de URLs encurtadas e outro para redirecionamento dessas URLs.

---

## Arquitetura do Projeto

### Componentes Utilizados:
- **AWS Lambda**: Funções para criação e redirecionamento das URLs.
- **AWS S3**: Armazenamento dos metadados das URLs encurtadas.
- **API Gateway**: Exposição das APIs REST para interagir com as funções Lambda.
- **Java 17**: Linguagem utilizada para o desenvolvimento dos microserviços.
- **Maven**: Gerenciador de dependências e construção do projeto.

### Estrutura de Microserviços:

1. **Microserviço de Criação de URL**
   - **Descrição**: Gera uma URL encurtada a partir de uma URL original e define o tempo de expiração.
   - **Funcionalidade**:
     - Recebe uma URL original e um tempo de expiração.
     - Armazena os dados em um bucket S3.
     - Retorna o código encurtado.

2. **Microserviço de Redirecionamento**
   - **Descrição**: Redireciona o usuário para a URL original com base no código encurtado.
   - **Funcionalidade**:
     - Recebe o código encurtado.
     - Valida o tempo de expiração.
     - Redireciona o usuário ou retorna erro, caso a URL esteja expirada.

---

## Configuração e Deploy

### Pré-requisitos:
1. **AWS CLI** configurado.
2. **Bucket S3** criado (ex: `garciajops-url-shortener-storage`).
3. **IAM Role** com permissões de acesso ao S3 e Lambda.

### Etapas de Deploy:
1. **Construir o Projeto:**
   ```bash
   mvn clean package
   ```
2. **Criar as Funções Lambda**:
   - Faça o upload dos pacotes gerados para a AWS Lambda.
   - Defina o handler correto (`com.garciajops.redirecturlshortner.Main::handleRequest` e `com.garciajops.createurlshortener.Main::handleRequest`).

3. **Configurar o API Gateway**:
   - Crie duas rotas:
     - **POST /create**: Aponta para a função Lambda de criação.
     - **GET /{shortUrlCode}**: Aponta para a função Lambda de redirecionamento.

---

## Testando o Projeto

### Criar uma URL Encurtada:
- **Método**: POST
- **Endpoint**: `/create`
- **Corpo da Requisição**:
  ```json
  {
    "originalUrl": "https://www.exemplo.com",
    "expirationTime": "1700000000"
  }
  ```
- **Resposta**:
  ```json
  {
    "code": "abc12345"
  }
  ```

### Redirecionar uma URL:
- **Método**: GET
- **Endpoint**: `/{shortUrlCode}` (ex: `/abc12345`)
- **Resposta**:
  - **302**: Redirecionamento para a URL original.
  - **410**: URL expirada.

---

## Dependências

O projeto utiliza as seguintes dependências Maven:

```xml
<dependencies>
    <dependency>
        <groupId>com.amazonaws</groupId>
        <artifactId>aws-lambda-java-core</artifactId>
        <version>1.2.1</version>
    </dependency>
    <dependency>
        <groupId>software.amazon.awssdk</groupId>
        <artifactId>s3</artifactId>
        <version>2.17.106</version>
    </dependency>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.34</version>
    </dependency>
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>2.12.3</version>
    </dependency>
</dependencies>
```

---
![image](https://github.com/user-attachments/assets/63c36b3a-00f0-4672-880a-ac8f8fdbcd75)
![image](https://github.com/user-attachments/assets/e8c6e5ca-b049-42e3-abc4-288c35d978c3)



