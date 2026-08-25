# Aula 2.2: Encapsulamento: Protegendo seus Dados 🔒

## Objetivos de Aprendizagem

Ao concluir este conteúdo, você deverá ser capaz de explicar os conceitos apresentados, executar os exemplos, reconhecer seus casos de uso e aplicar o conhecimento em uma solução pequena e verificável.

## Sumário

- [ Objetivos de Aprendizagem](#-objetivos-de-aprendizagem)
- [Conteúdo da Aula](#contedo-da-aula)
  - [1. Por que Encapsular?](#1-por-que-encapsular)
  - [2. Modificadores de Acesso](#2-modificadores-de-acesso)
  - [3. Getters e Setters](#3-getters-e-setters)
- [Desafio de Fixação](#desafio-de-fixao)
  - [ Navegação](#-navegao)

## Introdução

Esta aula apresenta conceitos essenciais de **Introdução à Orientação a Objetos**. Estude em pequenos ciclos: leia, execute os exemplos, altere uma parte e explique com suas próprias palavras.

##  Objetivos de Aprendizagem
- Compreender o conceito de encapsulamento como um dos pilares da OOP.
- Aprender a usar modificadores de acesso (`public`, `private`, `protected`) para controlar a visibilidade de atributos e métodos.
- Implementar métodos *getters* e *setters* para acessar e modificar atributos privados de forma controlada.
- Entender a importância do encapsulamento para a segurança e manutenção do código.

##  Conteúdo da Aula

O **encapsulamento** é um dos quatro pilares da Programação Orientada a Objetos (junto com Herança, Polimorfismo e Abstração). Ele se refere à prática de agrupar dados (atributos) e os métodos que operam nesses dados dentro de uma única unidade (a classe), e de restringir o acesso direto a alguns dos componentes do objeto. Em outras palavras, o encapsulamento é sobre **esconder os detalhes internos** de uma classe e expor apenas o que é necessário para interagir com ela.

### 1. Por que Encapsular?

*   **Segurança dos Dados**: Impede que dados internos de um objeto sejam acessados ou modificados de forma inadequada por código externo, o que poderia levar a estados inconsistentes ou erros.
*   **Controle de Acesso**: Permite que a classe controle como seus dados são lidos e modificados, aplicando validações ou lógicas específicas.
*   **Manutenção e Flexibilidade**: Se a implementação interna de uma classe mudar, o código externo que a utiliza não será afetado, desde que a interface pública (métodos) permaneça a mesma. Isso facilita a manutenção e a evolução do sistema.

### 2. Modificadores de Acesso

Java oferece modificadores de acesso para controlar a visibilidade de classes, atributos e métodos:

*   **`private`**: O membro é acessível apenas dentro da própria classe onde foi declarado. É o modificador mais restritivo e é o mais usado para atributos, promovendo o encapsulamento.
*   **`default` (ou package-private)**: O membro é acessível apenas dentro do mesmo pacote. Se nenhum modificador for especificado, este é o padrão.
*   **`protected`**: O membro é acessível dentro do mesmo pacote e por subclasses (mesmo que estejam em pacotes diferentes).
*   **`public`**: O membro é acessível de qualquer lugar. É o modificador menos restritivo e é comumente usado para métodos que representam a interface pública da classe.

#### Exemplo de Modificadores de Acesso:

```java
package com.meuapp.modelo;

public class ExemploAcesso {
    public int publicVar; // Acessível de qualquer lugar
    protected int protectedVar; // Acessível no pacote e por subclasses
    int defaultVar; // Acessível apenas no pacote (package-private)
    private int privateVar; // Acessível apenas dentro desta classe

    public void setPrivateVar(int valor) {
        this.privateVar = valor;
    }

    public int getPrivateVar() {
        return this.privateVar;
    }
}
```

### 3. Getters e Setters

Para permitir que outras classes interajam com atributos `private` de forma controlada, utilizamos métodos públicos especiais:

*   **Getters (métodos de acesso)**: Métodos que permitem ler o valor de um atributo. Geralmente começam com `get` (ex: `getIdade()`). Para atributos booleanos, podem começar com `is` (ex: `isAtivo()`).
*   **Setters (métodos de modificação)**: Métodos que permitem modificar o valor de um atributo. Geralmente começam com `set` (ex: `setIdade(int novaIdade)`). Podem incluir lógica de validação.

#### Exemplo de Encapsulamento com Getters e Setters:

```java
public class ContaBancaria {
    private String numeroConta;
    private double saldo;
    private String titular;

    public ContaBancaria(String numeroConta, String titular) {
        this.numeroConta = numeroConta;
        this.titular = titular;
        this.saldo = 0.0; // Saldo inicial sempre zero
    }

    // Getter para numeroConta (apenas leitura)
    public String getNumeroConta() {
        return numeroConta;
    }

    // Getter para saldo (apenas leitura)
    public double getSaldo() {
        return saldo;
    }

    // Getter para titular (apenas leitura)
    public String getTitular() {
        return titular;
    }

    // Setter para depositar (com lógica de validação)
    public void depositar(double valor) {
        if (valor > 0) {
            this.saldo += valor;
            System.out.println("Depósito de R$" + valor + " realizado. Novo saldo: R$" + this.saldo);
        } else {
            System.out.println("Valor de depósito inválido.");
        }
    }

    // Setter para sacar (com lógica de validação)
    public void sacar(double valor) {
        if (valor > 0 && valor <= this.saldo) {
            this.saldo -= valor;
            System.out.println("Saque de R$" + valor + " realizado. Novo saldo: R$" + this.saldo);
        } else {
            System.out.println("Saldo insuficiente ou valor de saque inválido.");
        }
    }
}

public class TesteContaBancaria {
    public static void main(String[] args) {
        ContaBancaria minhaConta = new ContaBancaria("12345-6", "Ana Silva");

        System.out.println("Titular: " + minhaConta.getTitular());
        System.out.println("Saldo inicial: R$" + minhaConta.getSaldo());

        minhaConta.depositar(1000.0);
        minhaConta.sacar(200.0);
        minhaConta.sacar(1500.0); // Tentativa de saque inválida
        minhaConta.depositar(-50.0); // Tentativa de depósito inválida

        System.out.println("Saldo final: R$" + minhaConta.getSaldo());

        // minhaConta.saldo = 5000.0; // ERRO: atributo saldo é private!
    }
}
```

##  Desafio de Fixação

1.  Modifique a classe `Pessoa` criada na Aula 2.1 para encapsular seus atributos (`nome`, `idade`, `altura`). Torne-os `private`.
2.  Adicione métodos *getters* e *setters* para cada atributo.
3.  No método `setAltura()`, adicione uma validação: a altura não pode ser menor ou igual a zero. Se for, imprima uma mensagem de erro e não altere o valor.
4.  No método `main` da classe `TestePessoa`, utilize os *getters* e *setters* para interagir com os objetos, testando a validação da altura.


## Referências

[1] Oracle. Object-Oriented Concepts. Disponível em: [https://docs.oracle.com/javase/tutorial/java/concepts/](https://docs.oracle.com/javase/tutorial/java/concepts/)
[2] Oracle. Inheritance. Disponível em: [https://docs.oracle.com/javase/tutorial/java/IandI/subclasses.html](https://docs.oracle.com/javase/tutorial/java/IandI/subclasses.html)

---

[Voltar aos Links Rápidos](../../README.md#links-rapidos) | [Anterior](aula_2_1_classes_objetos.md) | [Próxima aula/módulo](aula_2_3_heranca.md)
