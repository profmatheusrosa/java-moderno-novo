# Exercícios: Preparação e Mentalidade

> Todo grande mestre já foi um iniciante. Vamos preparar o terreno para sua jornada no mundo Java!

Aqui estão os exercícios para consolidar sua base. Lembre-se, o objetivo agora é se familiarizar com o ecossistema.

## 1. O Mundo do Java

O Java possui diversas versões. É crucial saber quais usar em projetos reais.

**Sua missão:**
Liste as versões LTS (Long Term Support) do Java a partir da versão 8 e destaque qual é a versão LTS mais recente (no momento). Para cada versão, cite uma feature marcante introduzida nela.

<details>
<summary>Ver gabarito</summary>

- **Java 8 (LTS):** Lambdas e Stream API.
- **Java 11 (LTS):** `var` para variáveis locais (veio no 10, consolidado no 11) e HTTP Client padronizado.
- **Java 17 (LTS):** Records, Pattern Matching para `instanceof`, Sealed Classes.
- **Java 21 (LTS):** Virtual Threads, Record Patterns, Sequenced Collections. (Mais recente LTS atualmente).

</details>

## 2. Configurando a Casa

Antes de programar, precisamos garantir que as ferramentas estão instaladas.

**Sua missão:**
Abra o terminal do seu sistema operacional e execute os comandos para verificar as versões do Java e do Maven instalados. Quais comandos você deve usar?

<details>
<summary>Ver gabarito</summary>

No terminal, execute:
```bash
# Verifica a versão do Java
java -version

# Verifica a versão do Maven (se instalado)
mvn -version
```
</details>

## 3. O Mapa do Tesouro

O ecossistema Java é vasto. Entender o que cada sigla significa ajuda muito.

**Sua missão:**
Crie um mapa mental ou apenas defina brevemente a função das seguintes ferramentas/conceitos: JDK, JRE, JVM, Maven e Spring Boot.

<details>
<summary>Ver gabarito</summary>

- **JVM (Java Virtual Machine):** A máquina virtual que executa o bytecode Java. É o que faz o Java rodar em qualquer lugar.
- **JRE (Java Runtime Environment):** O ambiente de execução que contém a JVM e bibliotecas padrão. Necessário apenas para *rodar* programas Java.
- **JDK (Java Development Kit):** O kit completo de desenvolvimento. Contém o JRE, além de compiladores (javac) e outras ferramentas. Essencial para *programar* em Java.
- **Maven:** Ferramenta de build e gerenciamento de dependências. Baixa bibliotecas automaticamente.
- **Spring Boot:** Framework que simplifica a criação de aplicações Java, reduzindo a configuração necessária.

</details>

## 4. Planejando o Sucesso

Estudar programação exige constância e foco. A técnica Pomodoro é ótima para isso.

**Sua missão:**
Planeje um cronograma simples de estudos usando a técnica Pomodoro (25 min de foco, 5 min de pausa). Como você dividiria 2 horas de estudo diárias para aprender Java?

<details>
<summary>Ver gabarito</summary>

Uma sugestão de divisão em 4 ciclos (2 horas totais):
- **Pomodoro 1 (25m):** Ler/assistir teoria (ex: o que são variáveis).
- **Pausa (5m):** Água, alongamento.
- **Pomodoro 2 (25m):** Prática guiada (escrever o código do exemplo).
- **Pausa (5m)**
- **Pomodoro 3 (25m):** Resolução de exercícios sem olhar o gabarito.
- **Pausa (5m)**
- **Pomodoro 4 (25m):** Revisão dos erros, anotações finais e leitura de documentação.

</details>

---

##  Desafio Bônus: O Primeiro Passo

**Sua missão:**
Acesse o site do [Spring Initializr](https://start.spring.io/). Configure um projeto Maven com Java 21+ e Spring Boot 3+. Adicione a dependência `Spring Web`. Gere o projeto, extraia, e abra-o na sua IDE (IntelliJ ou Eclipse) para confirmar que ele compila!

<details>
<summary>Ver gabarito</summary>

Passos no Spring Initializr:
1. Project: Maven
2. Language: Java
3. Spring Boot: (Versão mais atual do 3.x)
4. Project Metadata: Preencha Group (ex: com.exemplo) e Artifact (ex: meuro-primeiro-projeto).
5. Java: 21 (ou superior)
6. Dependencies: Clique em "Add Dependencies" e pesquise por "Spring Web".
7. Clique em "Generate". Extraia o ZIP e abra a pasta raiz na sua IDE favorita. Se ele baixar as dependências e o botão "Run" ficar verde, sucesso total! OK:

</details>

---

[Voltar aos Links Rápidos](../README.md#links-rapidos)
