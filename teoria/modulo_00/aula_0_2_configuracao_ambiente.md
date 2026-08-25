# Aula 0.2: Configurando o Ambiente de Desenvolvimento (JDK 21+, IDE) 

## Objetivos de Aprendizagem

Ao concluir este conteúdo, você deverá ser capaz de explicar os conceitos apresentados, executar os exemplos, reconhecer seus casos de uso e aplicar o conhecimento em uma solução pequena e verificável.

## Sumário

- [ Objetivos de Aprendizagem](#-objetivos-de-aprendizagem)
- [Conteúdo da Aula](#contedo-da-aula)
  - [1. Java Development Kit (JDK)](#1-java-development-kit-jdk)
  - [2. Integrated Development Environment (IDE)](#2-integrated-development-environment-ide)
- [Desafio de Fixação](#desafio-de-fixao)
  - [ Navegação](#-navegao)

## Introdução

Esta aula apresenta conceitos essenciais de **Preparação e Mentalidade**. Estude em pequenos ciclos: leia, execute os exemplos, altere uma parte e explique com suas próprias palavras.

##  Objetivos de Aprendizagem
- Instalar e configurar o Java Development Kit (JDK) versão 21 ou superior.
- Escolher e configurar uma Integrated Development Environment (IDE) para desenvolvimento Java.
- Executar o primeiro programa "Hello World" em Java.

##  Conteúdo da Aula

Para começar a programar em Java, você precisará de duas ferramentas essenciais: o **JDK (Java Development Kit)** e uma **IDE (Integrated Development Environment)**.

### 1. Java Development Kit (JDK)

O JDK é um conjunto de ferramentas que permite desenvolver, compilar e executar aplicações Java. Ele inclui o JRE (Java Runtime Environment), o compilador Java (`javac`), o depurador (`jdb`) e outras ferramentas. Para este curso, utilizaremos o **JDK 21 ou superior**, que é uma versão LTS (Long Term Support) e inclui as últimas funcionalidades da linguagem.

#### Passos para Instalação do JDK:

1.  **Download**: Acesse o site oficial da Oracle ou o OpenJDK (como Adoptium Temurin, Amazon Corretto, Azul Zulu) e baixe a versão mais recente do JDK 21+ compatível com seu sistema operacional.
2.  **Instalação**: Siga as instruções do instalador. Geralmente, é um processo de "próximo, próximo, finalizar".
3.  **Configuração da Variável de Ambiente `JAVA_HOME`**: É uma boa prática configurar a variável de ambiente `JAVA_HOME` para apontar para o diretório de instalação do seu JDK. Isso é crucial para muitas ferramentas Java e frameworks.
    *   **Windows**: Adicione `JAVA_HOME` com o caminho da sua instalação (ex: `C:\Program Files\Java\jdk-21`) e adicione `%JAVA_HOME%\bin` ao `Path` do sistema.
    *   **Linux/macOS**: Adicione `export JAVA_HOME=/path/to/your/jdk-21` e `export PATH=$PATH:$JAVA_HOME/bin` ao seu arquivo de perfil (`.bashrc`, `.zshrc`, etc.).
4.  **Verificação**: Abra o terminal e execute `java -version` e `javac -version`. Se a versão correta for exibida, sua instalação está pronta.

### 2. Integrated Development Environment (IDE)

Uma IDE é um software que fornece um ambiente de desenvolvimento abrangente para programadores. Ela integra um editor de código, um compilador, um depurador e outras ferramentas. As IDEs mais populares para Java são:

*   **VS Code (Visual Studio Code)**: Leve, rápido e altamente personalizável com extensões. É a IDE recomendada para este curso devido à sua flexibilidade e ao nosso foco em ferramentas modernas.
    *   **Instalação**: Baixe e instale o VS Code. Em seguida, instale o "Extension Pack for Java" da Microsoft, que inclui o Language Support for Java, Debugger for Java, Maven for Java, entre outros.
*   **IntelliJ IDEA**: Considerado por muitos como a IDE mais poderosa para Java, com recursos avançados de refatoração, análise de código e integração com frameworks. Possui uma versão Community gratuita e uma Ultimate paga.
*   **Eclipse**: Uma IDE tradicional e robusta, muito utilizada em projetos corporativos. É gratuita e de código aberto.

#### Nosso Primeiro "Hello World" no VS Code:

1.  Abra o VS Code.
2.  Crie uma nova pasta para o seu projeto (ex: `meu-primeiro-projeto-java`).
3.  Dentro dessa pasta, crie um arquivo `HelloWorld.java`.
4.  Cole o seguinte código:

    ```java
    public class HelloWorld {
        public static void main(String[] args) {
            System.out.println("Hello, World!");
        }
    }
    ```

5.  Salve o arquivo.
6.  O VS Code, com as extensões Java instaladas, deve reconhecer o arquivo como um programa Java. Você verá um botão "Run" acima do método `main` ou poderá usar o atalho `Ctrl+F5` (ou `Cmd+F5` no macOS) para executar.
7.  A saída "Hello, World!" aparecerá no terminal integrado do VS Code.

Parabéns! Você configurou seu ambiente e executou seu primeiro programa Java. Agora você está pronto para mergulhar no código!

##  Desafio de Fixação

1.  Configure seu ambiente de desenvolvimento com o JDK 21+ e sua IDE preferida (recomendamos VS Code).
2.  Crie um novo projeto e um programa Java que imprima seu nome completo no console.
3.  Tire um "print" do seu terminal com a saída do programa e guarde-o para referência futura.


## Referências

[1] dev.java. Learn Java. Disponível em: [https://dev.java/learn/](https://dev.java/learn/)
[2] Oracle. Java Tutorials. Disponível em: [https://docs.oracle.com/javase/tutorial/](https://docs.oracle.com/javase/tutorial/)

---

[Voltar aos Links Rápidos](../../README.md#links-rapidos) | [Anterior](aula_0_1_ecossistema_java.md) | [Próxima aula/módulo](aula_0_3_tecnicas_estudo.md)
