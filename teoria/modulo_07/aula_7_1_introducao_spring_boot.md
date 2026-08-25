# Aula 7.1: Introdução ao Spring Boot

## Objetivos de Aprendizagem

Ao concluir este conteúdo, você deverá ser capaz de explicar os conceitos apresentados, executar os exemplos, reconhecer seus casos de uso e aplicar o conhecimento em uma solução pequena e verificável.

## Sumário

- [ Objetivos de Aprendizagem](#-objetivos-de-aprendizagem)
- [Conteúdo da Aula](#contedo-da-aula)
  - [1. O Problema Antes do Spring Boot](#1-o-problema-antes-do-spring-boot)
  - [2. O que é Spring Boot?](#2-o-que--spring-boot)
  - [3. Criando uma Aplicação Spring Boot com Spring Initializr](#3-criando-uma-aplicao-spring-boot-com-spring-initializr)
  - [4. Estrutura Básica de um Projeto Spring Boot](#4-estrutura-bsica-de-um-projeto-spring-boot)
  - [5. Executando a Aplicação](#5-executando-a-aplicao)
  - [6. Criando um Controller Simples](#6-criando-um-controller-simples)
- [Desafio de Fixação](#desafio-de-fixao)
  - [ Navegação](#-navegao)

## Introdução

Esta aula apresenta conceitos essenciais de **Ecossistema Spring Boot 3**. Estude em pequenos ciclos: leia, execute os exemplos, altere uma parte e explique com suas próprias palavras.

##  Objetivos de Aprendizagem
- Compreender o que é o Spring Boot e por que ele se tornou tão popular.
- Entender os princípios por trás do Spring Boot: `autoconfiguração`, `starter dependencies` e `fat JARs`.
- Criar uma aplicação Spring Boot básica do zero usando o Spring Initializr.
- Executar e testar uma aplicação Spring Boot simples.

##  Conteúdo da Aula

O **Spring Boot** é uma das ferramentas mais revolucionárias no ecossistema Java moderno. Lançado pela Pivotal (agora parte da VMware), ele foi projetado para simplificar e acelerar o desenvolvimento de aplicações baseadas no framework Spring, eliminando grande parte da configuração boilerplate que era comum em projetos Spring tradicionais.

### 1. O Problema Antes do Spring Boot

Antes do Spring Boot, configurar um projeto Spring era uma tarefa complexa e demorada. Envolvia:

*   **Configuração XML Extensa**: Muitos arquivos XML para configurar beans, injeção de dependência, transações, segurança, etc.
*   **Gerenciamento de Dependências**: Lidar com versões de bibliotecas e suas compatibilidades era um desafio constante.
*   **Servidores de Aplicação**: Era necessário configurar e implantar a aplicação em servidores de aplicação externos como Tomcat, JBoss ou WebLogic.
*   **Boilerplate Code**: Muito código repetitivo para tarefas comuns.

Isso tornava a curva de aprendizado do Spring íngreme e o desenvolvimento lento.

### 2. O que é Spring Boot?

Spring Boot é um projeto do ecossistema Spring que visa facilitar a criação de aplicações Spring autônomas, prontas para produção, que você pode "apenas executar". Ele se baseia em três princípios fundamentais:

*   **Autoconfiguração**: O Spring Boot tenta configurar automaticamente sua aplicação Spring com base nas bibliotecas que você adicionou ao seu projeto. Por exemplo, se você adiciona o Spring Data JPA e o H2 Database, ele automaticamente configura um `DataSource` e um `EntityManager` para você.
*   **Starter Dependencies**: São conjuntos de dependências convenientes que você pode adicionar ao seu projeto. Cada `starter` agrupa todas as dependências necessárias para uma funcionalidade específica (ex: `spring-boot-starter-web` para aplicações web, `spring-boot-starter-data-jpa` para persistência).
*   **Fat JARs (ou Executable JARs)**: O Spring Boot permite empacotar sua aplicação e todas as suas dependências em um único arquivo JAR executável. Isso facilita a implantação, pois você não precisa de um servidor de aplicação externo; o servidor (como Tomcat ou Jetty) já vem embarcado no JAR.

### 3. Criando uma Aplicação Spring Boot com Spring Initializr

A maneira mais fácil de iniciar um projeto Spring Boot é usando o **Spring Initializr**. Ele é uma ferramenta web (ou integrada a IDEs) que gera a estrutura básica de um projeto Spring Boot com as dependências que você escolher.

**Passos para criar um projeto:**

1.  Acesse [start.spring.io](https://start.spring.io/).
2.  **Project**: Escolha `Maven Project` ou `Gradle Project` (usaremos Maven neste curso).
3.  **Language**: `Java`.
4.  **Spring Boot**: Selecione a versão estável mais recente (ex: `3.x.x`).
5.  **Project Metadata**:
    *   **Group**: `com.example` (ou seu domínio reverso, ex: `com.devsuperior`)
    *   **Artifact**: `demo` (ou o nome do seu projeto, ex: `minha-primeira-api`)
    *   **Name**: `demo` (geralmente igual ao Artifact)
    *   **Description**: Uma breve descrição do projeto.
    *   **Package name**: `com.example.demo` (gerado automaticamente)
    *   **Packaging**: `Jar` (para aplicações autônomas)
    *   **Java**: Selecione a versão do Java que você está usando (ex: `21`).
6.  **Dependencies**: Adicione as dependências necessárias. Para uma aplicação web básica, adicione:
    *   `Spring Web`: Para construir aplicações web, incluindo RESTful.
    *   `Spring Boot DevTools`: Para ferramentas de desenvolvimento úteis (recarregamento automático).
7.  Clique em `Generate` para baixar o projeto compactado.
8.  Descompacte o arquivo e importe o projeto para sua IDE (IntelliJ IDEA, VS Code, Eclipse).

### 4. Estrutura Básica de um Projeto Spring Boot

Após importar o projeto, você verá uma estrutura semelhante a esta (para Maven):

```
minha-primeira-api/
├── .mvn/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/
│   │   │       └── example/
│   │   │           └── demo/
│   │   │               └── MinhaPrimeiraApiApplication.java  <-- Classe principal
│   │   └── resources/
│   │       ├── application.properties  <-- Arquivo de configuração
│   │       ├── static/               <-- Conteúdo estático (HTML, CSS, JS)
│   │       └── templates/            <-- Templates (Thymeleaf, FreeMarker)
│   └── test/
│       └── java/
│           └── com/
│               └── example/
│                   └── demo/
│                       └── MinhaPrimeiraApiApplicationTests.java <-- Testes
├── pom.xml                       <-- Arquivo de configuração do Maven
└── README.md
```

#### `MinhaPrimeiraApiApplication.java`

Esta é a classe principal da sua aplicação Spring Boot. Ela contém o método `main` que inicia a aplicação.

```java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class MinhaPrimeiraApiApplication {

    public static void main(String[] args) {
        SpringApplication.run(MinhaPrimeiraApiApplication.class, args);
    }

}
```

*   `@SpringBootApplication`: É uma anotação de conveniência que combina `@Configuration`, `@EnableAutoConfiguration` e `@ComponentScan`.
    *   `@Configuration`: Marca a classe como uma fonte de definições de beans.
    *   `@EnableAutoConfiguration`: Habilita a autoconfiguração do Spring Boot.
    *   `@ComponentScan`: Habilita a varredura de componentes para encontrar outros componentes, configurações e serviços.

### 5. Executando a Aplicação

Você pode executar sua aplicação Spring Boot de várias maneiras:

*   **Pela IDE**: Execute o método `main` na classe `MinhaPrimeiraApiApplication`.
*   **Pelo Maven**: Abra o terminal no diretório raiz do projeto e execute `mvn spring-boot:run`.
*   **Pelo JAR Executável**: Após empacotar o projeto (`mvn clean package`), você encontrará um JAR executável em `target/`. Execute-o com `java -jar nome-do-seu-projeto.jar`.

Ao iniciar, você verá logs do Spring Boot no console, indicando que o servidor embarcado (geralmente Tomcat) foi iniciado na porta 8080.

### 6. Criando um Controller Simples

Vamos adicionar um `Controller` para testar nossa aplicação. Crie um arquivo `HelloController.java` no mesmo pacote da classe principal:

```java
package com.example.demo;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController // Indica que esta classe é um controlador REST
public class HelloController {

    @GetMapping("/hello") // Mapeia requisições GET para /hello
    public String hello() {
        return "Hello, Spring Boot!";
    }

}
```

Reinicie a aplicação. Agora, abra seu navegador e acesse `http://localhost:8080/hello`. Você deverá ver a mensagem "Hello, Spring Boot!".

##  Desafio de Fixação

1.  Crie um novo projeto Spring Boot usando o Spring Initializr com as seguintes configurações:
    *   **Project**: Maven Project
    *   **Language**: Java
    *   **Spring Boot**: Versão estável mais recente (ex: 3.x.x)
    *   **Group**: `com.example.biblioteca`
    *   **Artifact**: `gerenciador-biblioteca-api`
    *   **Name**: `gerenciador-biblioteca-api`
    *   **Dependencies**: `Spring Web`, `Spring Boot DevTools`
2.  Importe o projeto para sua IDE.
3.  Crie um `RestController` chamado `LivroController`.
4.  Dentro de `LivroController`, crie um método que responda a requisições GET para `/livros` e retorne uma lista de strings com títulos de livros (ex: "O Senhor dos Anéis", "1984").
5.  Execute a aplicação e teste o endpoint no navegador ou com uma ferramenta como Postman/Insomnia.


## Referências

[1] Spring. Spring Boot Reference. Disponível em: [https://docs.spring.io/spring-boot/docs/3.3.x/reference/html/](https://docs.spring.io/spring-boot/docs/3.3.x/reference/html/)
[2] Spring. Spring Framework Reference. Disponível em: [https://docs.spring.io/spring-framework/reference/](https://docs.spring.io/spring-framework/reference/)

---

[Voltar aos Links Rápidos](../../README.md#links-rapidos) | [Anterior](../modulo_07_ecossistema_spring_boot_3.md) | [Próxima aula/módulo](aula_7_2_injecao_dependencia.md)
