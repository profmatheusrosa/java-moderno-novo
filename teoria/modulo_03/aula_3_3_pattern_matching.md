# Aula 3.3: Pattern Matching para instanceof e switch: Simplificando Condicionais 🧩

## Objetivos de Aprendizagem

Ao concluir este conteúdo, você deverá ser capaz de explicar os conceitos apresentados, executar os exemplos, reconhecer seus casos de uso e aplicar o conhecimento em uma solução pequena e verificável.

## Sumário

- [ Objetivos de Aprendizagem](#-objetivos-de-aprendizagem)
- [Conteúdo da Aula](#contedo-da-aula)
  - [1. Pattern Matching para instanceof](#1-pattern-matching-para-instanceof)
  - [2. Pattern Matching para switch](#2-pattern-matching-para-switch)
  - [3. Vantagens do Pattern Matching](#3-vantagens-do-pattern-matching)
- [Desafio de Fixação](#desafio-de-fixao)
  - [ Navegação](#-navegao)

## Introdução

Esta aula apresenta conceitos essenciais de **Java Moderno: Novas Features**. Estude em pequenos ciclos: leia, execute os exemplos, altere uma parte e explique com suas próprias palavras.

##  Objetivos de Aprendizagem
- Compreender o conceito de Pattern Matching em Java.
- Aprender a usar Pattern Matching para `instanceof` para simplificar verificações de tipo e casts.
- Empregar Pattern Matching para `switch` para tornar a lógica condicional mais concisa e segura.
- Entender como esses recursos melhoram a legibilidade e a manutenibilidade do código.

##  Conteúdo da Aula

O **Pattern Matching** é um recurso introduzido no Java para tornar o código mais conciso e legível, especialmente ao lidar com verificações de tipo e extração de componentes de objetos. Ele simplifica a lógica condicional que tradicionalmente envolvia `instanceof` e casts explícitos, e também aprimora a estrutura `switch`.

### 1. Pattern Matching para `instanceof`

Antes do Java 16, ao verificar o tipo de um objeto com `instanceof`, era comum ter que fazer um cast explícito em seguida para usar os métodos ou atributos específicos desse tipo. Isso resultava em código repetitivo e propenso a erros.

#### Antes do Pattern Matching:

```java
public class AntesPatternMatching {
    public static void processarObjeto(Object obj) {
        if (obj instanceof String) {
            String s = (String) obj; // Cast explícito necessário
            System.out.println("String: " + s.toUpperCase());
        } else if (obj instanceof Integer) {
            Integer i = (Integer) obj; // Cast explícito necessário
            System.out.println("Integer: " + (i * 2));
        }
    }

    public static void main(String[] args) {
        processarObjeto("hello");
        processarObjeto(10);
        processarObjeto(3.14);
    }
}
```

#### Com Pattern Matching para `instanceof` (Java 16+):

O Pattern Matching para `instanceof` permite que você declare uma variável de padrão diretamente na condição `if`. Se a condição for verdadeira, a variável de padrão é automaticamente inicializada com o objeto já convertido para o tipo correto, e seu escopo é limitado ao bloco `if`.

```java
public class ComPatternMatchingInstanceof {
    public static void processarObjeto(Object obj) {
        if (obj instanceof String s) { // Variável de padrão 's'
            System.out.println("String: " + s.toUpperCase());
        } else if (obj instanceof Integer i) { // Variável de padrão 'i'
            System.out.println("Integer: " + (i * 2));
        } else {
            System.out.println("Tipo desconhecido: " + obj.getClass().getSimpleName());
        }
    }

    public static void main(String[] args) {
        processarObjeto("hello");
        processarObjeto(10);
        processarObjeto(3.14);
    }
}
```

### 2. Pattern Matching para `switch`

O Pattern Matching para `switch` (introduzido como preview no Java 17 e padronizado no Java 21) estende a funcionalidade do `switch` para permitir que ele opere não apenas em valores exatos, mas também em tipos de objetos. Isso simplifica ainda mais a lógica condicional baseada em tipos, especialmente quando combinado com Sealed Classes.

#### Antes do Pattern Matching para `switch` (com `if-else if`):

```java
// Reutilizando as classes Forma, Circulo, Retangulo da Aula 2.5
interface FormaGeometrica {
    double calcularArea();
    double calcularPerimetro();
}

class Circulo implements FormaGeometrica {
    private final double raio;
    public Circulo(double raio) { this.raio = raio; }
    public double getRaio() { return raio; }
    @Override public double calcularArea() { return Math.PI * raio * raio; }
    @Override public double calcularPerimetro() { return 2 * Math.PI * raio; }
}

class Retangulo implements FormaGeometrica {
    private final double largura;
    private final double altura;
    public Retangulo(double largura, double altura) { this.largura = largura; this.altura = altura; }
    public double getLargura() { return largura; }
    public double getAltura() { return altura; }
    @Override public double calcularArea() { return largura * altura; }
    @Override public double calcularPerimetro() { return 2 * (largura + altura); }
}

public class AntesPatternMatchingSwitch {
    public static void descreverForma(FormaGeometrica forma) {
        if (forma instanceof Circulo) {
            Circulo c = (Circulo) forma;
            System.out.println("Círculo com raio " + c.getRaio() + ", área: " + c.calcularArea());
        } else if (forma instanceof Retangulo) {
            Retangulo r = (Retangulo) forma;
            System.out.println("Retângulo com largura " + r.getLargura() + " e altura " + r.getAltura() + ", área: " + r.calcularArea());
        } else {
            System.out.println("Forma desconhecida.");
        }
    }

    public static void main(String[] args) {
        descreverForma(new Circulo(10));
        descreverForma(new Retangulo(5, 8));
    }
}
```

#### Com Pattern Matching para `switch` (Java 21+):

O `switch` agora pode receber um objeto e usar padrões de tipo nos `case`s. A variável de padrão é automaticamente criada e tem o escopo do `case` correspondente. Isso elimina a necessidade de `instanceof` e casts explícitos.

```java
// Reutilizando as classes FormaGeometrica, Circulo, Retangulo
// (Definições omitidas para brevidade, assumindo que são as mesmas do exemplo anterior)

public class ComPatternMatchingSwitch {
    public static void descreverForma(FormaGeometrica forma) {
        switch (forma) {
            case Circulo c -> System.out.println("Círculo com raio " + c.getRaio() + ", área: " + c.calcularArea());
            case Retangulo r -> System.out.println("Retângulo com largura " + r.getLargura() + " e altura " + r.getAltura() + ", área: " + r.calcularArea());
            case null -> System.out.println("Forma nula."); // Tratamento de null explícito
            default -> System.out.println("Forma desconhecida: " + forma.getClass().getSimpleName());
        }
    }

    public static void main(String[] args) {
        descreverForma(new Circulo(10));
        descreverForma(new Retangulo(5, 8));
        descreverForma(null);
        // Se tivéssemos uma classe Triangulo que implementa FormaGeometrica,
        // mas não está no switch, cairia no default.
    }
}
```

**Observação**: Se a classe `FormaGeometrica` fosse uma `sealed interface` (como na Aula 3.2) e todos os seus implementadores fossem cobertos pelos `case`s do `switch`, o compilador poderia garantir a exaustividade, eliminando a necessidade do `default` em alguns cenários.

### 3. Vantagens do Pattern Matching

*   **Concisão**: Reduz a quantidade de código repetitivo (`instanceof` seguido de cast).
*   **Legibilidade**: Torna o código mais fácil de ler e entender, pois a intenção é mais clara.
*   **Segurança**: Elimina a chance de `ClassCastException` em tempo de execução, pois o cast é feito automaticamente e de forma segura.
*   **Exaustividade (com Sealed Types)**: Quando usado com Sealed Classes/Interfaces, o compilador pode verificar se todos os possíveis subtipos foram tratados em um `switch`.

##  Desafio de Fixação

1.  Crie uma superclasse abstrata `Animal` com um método abstrato `fazerBarulho()`.
2.  Crie as subclasses `Cachorro` e `Gato` que herdem de `Animal` e implementem `fazerBarulho()` de forma específica.
3.  No método `main`, crie um método `public static void inspecionarAnimal(Animal animal)`.
4.  Dentro de `inspecionarAnimal`, use **Pattern Matching para `instanceof`** para verificar se o `animal` é um `Cachorro` ou um `Gato`. Se for, chame o método `fazerBarulho()` e imprima uma mensagem adicional específica para cada tipo (ex: "O cachorro abana o rabo.").
5.  Crie um método `public static void descreverAnimal(Object obj)`.
6.  Dentro de `descreverAnimal`, use **Pattern Matching para `switch`** para:
    *   Tratar `Cachorro` e `Gato` (chamando `fazerBarulho()` e imprimindo uma mensagem específica).
    *   Tratar `String` (imprimindo o comprimento da string).
    *   Tratar `Integer` (imprimindo se é par ou ímpar).
    *   Ter um `default` para outros tipos.
7.  Teste ambos os métodos com diferentes objetos.


## Referências

[1] OpenJDK. Project Amber. Disponível em: [https://openjdk.org/projects/amber/](https://openjdk.org/projects/amber/)
[2] Oracle. Java Language Changes. Disponível em: [https://docs.oracle.com/en/java/javase/21/language/java-language-changes.html](https://docs.oracle.com/en/java/javase/21/language/java-language-changes.html)

---

[Voltar aos Links Rápidos](../../README.md#links-rapidos) | [Anterior](aula_3_2_sealed_classes.md) | [Próxima aula/módulo](aula_3_4_text_blocks.md)
