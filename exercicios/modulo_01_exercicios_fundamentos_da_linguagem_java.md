# Exercícios: Fundamentos da Linguagem Java

> A base é tudo! Dominar os fundamentos te dá a confiança para construir qualquer sistema complexo no futuro.

Pegue sua xícara de café (ou chá) e vamos botar a mão na massa com código real!

## 1. Seu IMC na Tela

Operadores aritméticos e variáveis são o pão com manteiga de qualquer programa.

**Sua missão:**
Complete o método abaixo para calcular o IMC (Índice de Massa Corporal) usando tipos primitivos (`double`).
*Fórmula:* `peso / (altura * altura)`

```java
public class CalculadoraIMC {
    public static void main(String[] args) {
        double peso = 75.0; // em kg
        double altura = 1.75; // em metros
        
        // TODO: Calcule o imc
        double imc = 0.0;
        
        // TODO: Use ifs para imprimir a classificação:
        // Menos de 18.5 -> Abaixo do peso
        // 18.5 a 24.9 -> Peso ideal
        // 25.0 ou mais -> Acima do peso
        
    }
}
```

<details>
<summary>Ver gabarito</summary>

```java
public class CalculadoraIMC {
    public static void main(String[] args) {
        double peso = 75.0; // em kg
        double altura = 1.75; // em metros
        
        double imc = peso / (altura * altura);
        System.out.println("Seu IMC é: " + imc);
        
        if (imc < 18.5) {
            System.out.println("Abaixo do peso");
        } else if (imc < 25) {
            System.out.println("Peso ideal");
        } else {
            System.out.println("Acima do peso");
        }
    }
}
```
</details>

## 2. A Clássica Tabuada

Laços de repetição (loops) são perfeitos para tarefas repetitivas. 

**Sua missão:**
Usando um laço `for`, imprima a tabuada do 7, do 1 ao 10.

```java
public class Tabuada {
    public static void main(String[] args) {
        int numero = 7;
        
        // TODO: Crie um laço for de 1 a 10 e imprima a multiplicação
    }
}
```

<details>
<summary>Ver gabarito</summary>

```java
public class Tabuada {
    public static void main(String[] args) {
        int numero = 7;
        
        for (int i = 1; i <= 10; i++) {
            System.out.println(numero + " x " + i + " = " + (numero * i));
        }
    }
}
```
</details>

## 3. Caça aos Primos

Métodos nos permitem reaproveitar lógica!

**Sua missão:**
Crie um método chamado `ehPrimo` que recebe um `int` e retorna `boolean` indicando se o número é primo. Teste no método `main`.

```java
public class VerificadorPrimos {
    
    // TODO: Crie o método public static boolean ehPrimo(int numero) { ... }
    
    public static void main(String[] args) {
        // TODO: Chame o método com os números 7 e 10 e imprima o resultado
    }
}
```

<details>
<summary>Ver gabarito</summary>

```java
public class VerificadorPrimos {
    
    public static boolean ehPrimo(int numero) {
        if (numero <= 1) return false;
        
        for (int i = 2; i <= Math.sqrt(numero); i++) {
            if (numero % i == 0) {
                return false; // achou divisor, não é primo
            }
        }
        return true;
    }
    
    public static void main(String[] args) {
        System.out.println("7 é primo? " + ehPrimo(7)); // true
        System.out.println("10 é primo? " + ehPrimo(10)); // false
    }
}
```
</details>

## 4. Calculadora Moderna (Java 14+)

Switch expressions tornaram a escrita condicional muito mais limpa.

**Sua missão:**
Implemente o método `calcular` usando a nova sintaxe do `switch` introduzida no Java 14 (com as setinhas `->`).

```java
public class CalculadoraSimples {
    
    public static double calcular(double a, double b, String operacao) {
        // TODO: Use o switch expression para retornar o resultado
        // operacoes: "+", "-", "*", "/"
        return 0.0;
    }
    
    public static void main(String[] args) {
        System.out.println(calcular(10, 5, "+")); // 15
        System.out.println(calcular(10, 5, "/")); // 2
    }
}
```

<details>
<summary>Ver gabarito</summary>

```java
public class CalculadoraSimples {
    
    public static double calcular(double a, double b, String operacao) {
        return switch (operacao) {
            case "+" -> a + b;
            case "-" -> a - b;
            case "*" -> a * b;
            case "/" -> a / b;
            default -> throw new IllegalArgumentException("Operação inválida: " + operacao);
        };
    }
    
    public static void main(String[] args) {
        System.out.println(calcular(10, 5, "+"));
        System.out.println(calcular(10, 5, "/"));
    }
}
```
</details>

---

##  Desafio Bônus: FizzBuzz

A clássica questão de entrevista técnica!

**Sua missão:**
Imprima os números de 1 a 50. 
- Se o número for múltiplo de 3, imprima "Fizz".
- Se for múltiplo de 5, imprima "Buzz".
- Se for múltiplo de 3 e 5 ao mesmo tempo, imprima "FizzBuzz".
- Caso contrário, imprima o próprio número.

<details>
<summary>Ver gabarito</summary>

```java
public class FizzBuzz {
    public static void main(String[] args) {
        for (int i = 1; i <= 50; i++) {
            if (i % 3 == 0 && i % 5 == 0) {
                System.out.println("FizzBuzz");
            } else if (i % 3 == 0) {
                System.out.println("Fizz");
            } else if (i % 5 == 0) {
                System.out.println("Buzz");
            } else {
                System.out.println(i);
            }
        }
    }
}
```
</details>

---

[Voltar aos Links Rápidos](../README.md#links-rapidos)
