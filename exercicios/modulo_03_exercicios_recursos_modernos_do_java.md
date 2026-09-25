# Exercícios: Recursos Modernos do Java

> Chega de código verboso! O Java moderno (do 14 ao 21) trouxe ferramentas incríveis para escrevermos código mais limpo e seguro.

Vamos testar Records, Text Blocks, Pattern Matching e Sealed Classes.

## 1. Dados Imuáveis com Records

Criar classes que só guardam dados (DTOs, Value Objects) costumava exigir muito código (getters, equals, hashCode). Records fazem isso em uma linha!

**Sua missão:**
Crie um Record `Ponto2D` com coordenadas `x` e `y` (doubles). Adicione a ele um método customizado `distanciaAte(Ponto2D outro)`.

```java
// TODO: Crie o record Ponto2D(double x, double y)
// Formula da distância: Math.sqrt(Math.pow(x2 - x1, 2) + Math.pow(y2 - y1, 2))
```

<details>
<summary>Ver gabarito</summary>

```java
public record Ponto2D(double x, double y) {
    
    public double distanciaAte(Ponto2D outro) {
        return Math.sqrt(Math.pow(outro.x() - this.x, 2) + Math.pow(outro.y() - this.y, 2));
    }
}

// Teste na main:
// Ponto2D p1 = new Ponto2D(0, 0);
// Ponto2D p2 = new Ponto2D(3, 4);
// System.out.println(p1.distanciaAte(p2)); // Saída: 5.0
```
</details>

## 2. Escrevendo JSON sem Sofrimento (Text Blocks)

Quem já concatenou Strings com `\n` e `\"` para montar um JSON no Java sabe a dor. Os Text Blocks (Java 15+) resolvem isso!

**Sua missão:**
Usando Text Blocks (`"""`), crie uma string que represente um JSON válido de um usuário (com id, nome e email) e imprima no console.

```java
public class JsonComTextBlock {
    public static void main(String[] args) {
        // TODO: Crie a String json aqui usando """
        
    }
}
```

<details>
<summary>Ver gabarito</summary>

```java
public class JsonComTextBlock {
    public static void main(String[] args) {
        String json = """
            {
                "id": 1,
                "nome": "Maria Silva",
                "email": "maria@email.com"
            }
            """;
            
        System.out.println(json);
    }
}
```
</details>

## 3. Pattern Matching para Notificações

O Java simplificou o `instanceof` e permitiu combiná-lo diretamente com o `switch` (Java 21).

**Sua missão:**
Dada uma interface `Notificacao`, e classes `Email` e `SMS` que a implementam. Crie um método que recebe uma `Notificacao`, usa switch expression com pattern matching e retorna uma mensagem amigável dependendo do tipo da notificação.

```java
interface Notificacao {}
record Email(String endereco) implements Notificacao {}
record SMS(String numero) implements Notificacao {}

public class ProcessadorNotificacao {
    public static String processar(Notificacao not) {
        // TODO: Retornar uma string usando switch(not)
        return "";
    }
}
```

<details>
<summary>Ver gabarito</summary>

```java
public class ProcessadorNotificacao {
    public static String processar(Notificacao not) {
        return switch (not) {
            case Email e -> "Enviando email para: " + e.endereco();
            case SMS s -> "Enviando SMS para o número: " + s.numero();
            default -> "Notificação desconhecida";
        };
    }
}
```
</details>

## 4. O Círculo Fechado (Sealed Classes)

Sealed classes permitem restringir QUAIS classes podem estender a sua. Excelente para domínios fechados!

**Sua missão:**
Crie uma interface *sealed* `Forma` que só permite `Circulo` e `Retangulo` a implementarem. Implemente essas duas classes. Em seguida, faça um método que calcule a área recebendo uma `Forma`.

```java
// TODO: interface sealed Forma permits...
// TODO: records Circulo e Retangulo
```

<details>
<summary>Ver gabarito</summary>

```java
sealed interface Forma permits Circulo, Retangulo {}

record Circulo(double raio) implements Forma {}
record Retangulo(double base, double altura) implements Forma {}

public class Geometria {
    public static double calcularArea(Forma forma) {
        // Aproveitando o Pattern Matching do exercício anterior!
        // Repare que não precisa do 'default', pois a interface é sealed! O compilador sabe que só existem 2 opções.
        return switch (forma) {
            case Circulo c -> Math.PI * c.raio() * c.raio();
            case Retangulo r -> r.base() * r.altura();
        };
    }
}
```
</details>

---

##  Desafio Bônus: Modelando Result (Sucesso ou Falha)

Na programação funcional, é comum retornarmos objetos que indicam Sucesso ou Erro, ao invés de lançar exceções para fluxos de negócio. 

**Sua missão:**
Use Sealed Interfaces e Records para criar um tipo `Result<T, E>`. Ele só pode ser `Sucesso<T, E>` (contendo o dado) ou `Erro<T, E>` (contendo a mensagem/código de erro).

<details>
<summary>Ver gabarito</summary>

```java
public sealed interface Result<T, E> permits Sucesso, Erro {}

record Sucesso<T, E>(T dado) implements Result<T, E> {}
record Erro<T, E>(E erro) implements Result<T, E> {}

// Testando
public class TesteResult {
    public static Result<String, String> processarIdade(int idade) {
        if (idade >= 18) {
            return new Sucesso<>("Maior de idade!");
        } else {
            return new Erro<>("Menor de idade não permitido.");
        }
    }
    
    public static void main(String[] args) {
        var resultado = processarIdade(15);
        
        switch (resultado) {
            case Sucesso s -> System.out.println("Ok: " + s.dado());
            case Erro e -> System.out.println("Falha: " + e.erro());
        }
    }
}
```
</details>

---

[Voltar aos Links Rápidos](../README.md#links-rapidos)
