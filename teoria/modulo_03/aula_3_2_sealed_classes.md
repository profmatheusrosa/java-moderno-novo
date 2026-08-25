# Aula 3.2: Sealed Classes: Controlando a Herança 

## Objetivos de Aprendizagem

Ao concluir este conteúdo, você deverá ser capaz de explicar os conceitos apresentados, executar os exemplos, reconhecer seus casos de uso e aplicar o conhecimento em uma solução pequena e verificável.

## Sumário

- [ Objetivos de Aprendizagem](#-objetivos-de-aprendizagem)
- [Conteúdo da Aula](#contedo-da-aula)
  - [1. O Problema da Herança Aberta](#1-o-problema-da-herana-aberta)
  - [2. Introdução às Sealed Classes](#2-introduo-s-sealed-classes)
  - [3. Exemplo Prático](#3-exemplo-prtico)
  - [4. Vantagens das Sealed Classes](#4-vantagens-das-sealed-classes)
- [Desafio de Fixação](#desafio-de-fixao)
  - [ Navegação](#-navegao)

## Introdução

Esta aula apresenta conceitos essenciais de **Java Moderno: Novas Features**. Estude em pequenos ciclos: leia, execute os exemplos, altere uma parte e explique com suas próprias palavras.

##  Objetivos de Aprendizagem
- Compreender o propósito e os benefícios das Sealed Classes em Java.
- Aprender a declarar Sealed Classes e Interfaces.
- Entender como as Sealed Classes permitem um controle explícito sobre a herança e implementação.
- Conhecer as regras e restrições ao usar Sealed Classes.

##  Conteúdo da Aula

As **Sealed Classes** (classes seladas), introduzidas como um recurso de preview no Java 15 e padronizadas no Java 17, são uma poderosa adição à linguagem que permite aos desenvolvedores controlar quais classes ou interfaces podem estender ou implementar uma classe ou interface específica. Isso resolve um problema comum onde se deseja que uma hierarquia de classes seja finita e conhecida, mas sem restringir completamente a herança (como faria uma classe `final`).

### 1. O Problema da Herança Aberta

Tradicionalmente, em Java, uma classe pode ser estendida por qualquer outra classe, a menos que seja declarada como `final`. Isso pode levar a problemas de segurança, manutenção e design, pois não há controle sobre quem pode estender sua classe e como. Em muitos cenários, queremos uma hierarquia de classes fechada, onde apenas tipos específicos são permitidos.

### 2. Introdução às Sealed Classes

Uma **Sealed Class** ou **Sealed Interface** permite que você especifique explicitamente quais classes ou interfaces têm permissão para estendê-la ou implementá-la. Isso é feito usando a palavra-chave `permits`.

#### Sintaxe Básica:

```java
public abstract sealed class Forma permits Circulo, Retangulo, Triangulo {
    // ...
}

public final class Circulo extends Forma {
    // ...
}

public non-sealed class Retangulo extends Forma {
    // ...
}

public sealed interface Expressao permits Numero, Soma, Subtracao {
    // ...
}
```

#### Regras para Classes e Interfaces Permitidas:

As classes ou interfaces listadas no `permits` devem:

1.  Estar no mesmo módulo que a classe selada, ou no mesmo pacote se estiverem no módulo sem nome.
2.  Estender a classe selada ou implementar a interface selada.
3.  Ser declaradas com um dos seguintes modificadores:
    *   **`final`**: Não pode ser estendida.
    *   **`sealed`**: Pode ser estendida apenas pelas classes que ela explicitamente permite.
    *   **`non-sealed`**: Pode ser estendida por qualquer classe (comportamento padrão antes das Sealed Classes).

### 3. Exemplo Prático

Vamos considerar um sistema de processamento de formas geométricas. Queremos que apenas `Circulo`, `Retangulo` e `Triangulo` possam ser `Forma`s.

```java
// Classe Abstrata Selada
public abstract sealed class Forma
        permits Circulo, Retangulo, Triangulo {

    public abstract double calcularArea();
    public abstract double calcularPerimetro();

    public String getTipo() {
        return this.getClass().getSimpleName();
    }
}

// Subclasse final - não pode ser estendida
public final class Circulo extends Forma {
    private final double raio;

    public Circulo(double raio) {
        this.raio = raio;
    }

    @Override
    public double calcularArea() {
        return Math.PI * raio * raio;
    }

    @Override
    public double calcularPerimetro() {
        return 2 * Math.PI * raio;
    }
}

// Subclasse non-sealed - pode ser estendida por qualquer classe
public non-sealed class Retangulo extends Forma {
    private final double largura;
    private final double altura;

    public Retangulo(double largura, double altura) {
        this.largura = largura;
        this.altura = altura;
    }

    @Override
    public double calcularArea() {
        return largura * altura;
    }

    @Override
    public double calcularPerimetro() {
        return 2 * (largura + altura);
    }
}

// Subclasse sealed - pode ser estendida apenas por classes permitidas
public sealed class Triangulo extends Forma permits TrianguloEquilatero {
    private final double lado1, lado2, lado3;

    public Triangulo(double lado1, double lado2, double lado3) {
        this.lado1 = lado1;
        this.lado2 = lado2;
        this.lado3 = lado3;
    }

    @Override
    public double calcularArea() {
        // Fórmula de Heron para área de triângulo
        double s = (lado1 + lado2 + lado3) / 2;
        return Math.sqrt(s * (s - lado1) * (s - lado2) * (s - lado3));
    }

    @Override
    public double calcularPerimetro() {
        return lado1 + lado2 + lado3;
    }
}

// Subclasse de Triangulo (permitida)
public final class TrianguloEquilatero extends Triangulo {
    public TrianguloEquilatero(double lado) {
        super(lado, lado, lado);
    }
}

public class TesteSealedClasses {
    public static void main(String[] args) {
        Forma circulo = new Circulo(5);
        Forma retangulo = new Retangulo(4, 6);
        Forma triangulo = new Triangulo(3, 4, 5);
        Forma trianguloEquilatero = new TrianguloEquilatero(7);

        System.out.println("Área do " + circulo.getTipo() + ": " + String.format("%.2f", circulo.calcularArea()));
        System.out.println("Perímetro do " + retangulo.getTipo() + ": " + String.format("%.2f", retangulo.calcularPerimetro()));
        System.out.println("Área do " + triangulo.getTipo() + ": " + String.format("%.2f", triangulo.calcularArea()));
        System.out.println("Perímetro do " + trianguloEquilatero.getTipo() + ": " + String.format("%.2f", trianguloEquilatero.calcularPerimetro()));

        // Exemplo de como o compilador restringe a herança:
        // class Quadrado extends Forma { } // ERRO DE COMPILAÇÃO: Quadrado não está na lista permits de Forma
    }
}
```

### 4. Vantagens das Sealed Classes

*   **Controle Explícito**: Permite um controle preciso sobre a hierarquia de classes, tornando o design mais robusto.
*   **Segurança**: Impede que classes não autorizadas estendam ou implementem seus tipos, protegendo a integridade do modelo.
*   **Melhoria no Pattern Matching**: Trabalha em conjunto com o Pattern Matching para `switch` (Aula 3.3), permitindo que o compilador verifique exaustividade em blocos `switch` sobre tipos selados, o que pode eliminar a necessidade de um `default`.
*   **Clareza de Design**: Deixa claro para outros desenvolvedores quais são as subclasses esperadas e permitidas.

##  Desafio de Fixação

1.  Crie uma interface selada `OperacaoMatematica` que permita apenas as classes `Soma`, `Subtracao` e `Multiplicacao` implementá-la.
2.  A interface deve ter um método abstrato `double executar(double a, double b)`.
3.  Implemente as classes `Soma`, `Subtracao` e `Multiplicacao` como `final` classes, fornecendo a implementação do método `executar()`.
4.  No método `main`, crie instâncias de cada operação e execute-as, imprimindo os resultados.
5.  Tente criar uma classe `Divisao` que implemente `OperacaoMatematica` e observe o erro de compilação.


## Referências

[1] OpenJDK. Project Amber. Disponível em: [https://openjdk.org/projects/amber/](https://openjdk.org/projects/amber/)
[2] Oracle. Java Language Changes. Disponível em: [https://docs.oracle.com/en/java/javase/21/language/java-language-changes.html](https://docs.oracle.com/en/java/javase/21/language/java-language-changes.html)

---

[Voltar aos Links Rápidos](../../README.md#links-rapidos) | [Anterior](aula_3_1_records.md) | [Próxima aula/módulo](aula_3_3_pattern_matching.md)
