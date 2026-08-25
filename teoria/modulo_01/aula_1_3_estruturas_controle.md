# Aula 1.3: Estruturas de Controle: Tomando Decisões 

## Objetivos de Aprendizagem

Ao concluir este conteúdo, você deverá ser capaz de explicar os conceitos apresentados, executar os exemplos, reconhecer seus casos de uso e aplicar o conhecimento em uma solução pequena e verificável.

## Sumário

- [ Objetivos de Aprendizagem](#-objetivos-de-aprendizagem)
- [Conteúdo da Aula](#contedo-da-aula)
  - [1. if-else: A Tomada de Decisão Básica](#1-if-else-a-tomada-de-deciso-bsica)
  - [2. switch: Múltiplas Escolhas](#2-switch-mltiplas-escolhas)
- [Desafio de Fixação](#desafio-de-fixao)
  - [ Navegação](#-navegao)

## Introdução

Esta aula apresenta conceitos essenciais de **Fundamentos da Linguagem Java**. Estude em pequenos ciclos: leia, execute os exemplos, altere uma parte e explique com suas próprias palavras.

##  Objetivos de Aprendizagem
- Compreender o conceito de fluxo de controle em programação.
- Utilizar a estrutura condicional `if-else` para tomar decisões no código.
- Empregar a estrutura `switch` para lidar com múltiplos casos de forma eficiente.
- Entender a importância de escrever código que reage a diferentes condições.

##  Conteúdo da Aula

Em programas de computador, raramente seguimos um caminho linear. Precisamos que nossos programas tomem decisões, executem diferentes blocos de código com base em certas condições. As **estruturas de controle** são as ferramentas que nos permitem direcionar o fluxo de execução do programa.

### 1. `if-else`: A Tomada de Decisão Básica

A estrutura `if-else` é a forma mais fundamental de tomar decisões. Ela permite que um bloco de código seja executado se uma condição for verdadeira, e outro bloco (opcional) se a condição for falsa.

```java
public class IfElseExemplo {
    public static void main(String[] args) {
        int idade = 20;

        if (idade >= 18) {
            System.out.println("Você é maior de idade.");
        } else {
            System.out.println("Você é menor de idade.");
        }

        // Exemplo com if aninhado e else if
        int pontuacao = 75;

        if (pontuacao >= 90) {
            System.out.println("Conceito A");
        } else if (pontuacao >= 70) {
            System.out.println("Conceito B");
        } else if (pontuacao >= 50) {
            System.out.println("Conceito C");
        } else {
            System.out.println("Conceito D");
        }
    }
}
```

### 2. `switch`: Múltiplas Escolhas

A estrutura `switch` é útil quando você precisa tomar decisões com base no valor de uma única variável ou expressão, que pode ter múltiplos valores possíveis. É uma alternativa mais limpa ao uso de vários `else if` encadeados.

O `switch` pode ser usado com tipos `byte`, `short`, `char`, `int`, seus Wrappers correspondentes, `enum`s e `String` (a partir do Java 7).

```java
public class SwitchExemplo {
    public static void main(String[] args) {
        int diaDaSemana = 3; // 1=Domingo, 2=Segunda, ..., 7=Sábado
        String nomeDia;

        switch (diaDaSemana) {
            case 1:
                nomeDia = "Domingo";
                break; // Importante para sair do switch após o case
            case 2:
                nomeDia = "Segunda-feira";
                break;
            case 3:
                nomeDia = "Terça-feira";
                break;
            case 4:
                nomeDia = "Quarta-feira";
                break;
            case 5:
                nomeDia = "Quinta-feira";
                break;
            case 6:
                nomeDia = "Sexta-feira";
                break;
            case 7:
                nomeDia = "Sábado";
                break;
            default:
                nomeDia = "Dia inválido";
                break;
        }
        System.out.println("O dia é: " + nomeDia);

        // Switch Expression (Java 14+): Retorna um valor
        String tipoDia = switch (diaDaSemana) {
            case 1, 7 -> "Fim de semana";
            case 2, 3, 4, 5, 6 -> "Dia útil";
            default -> "Inválido";
        };
        System.out.println("Tipo de dia: " + tipoDia);
    }
}
```

**Importante**: O `break` é fundamental nos `case`s do `switch` tradicional para evitar o "fall-through", onde o código dos `case`s seguintes seria executado. Com as `switch expressions` (Java 14+), o `break` não é necessário, e o `->` já indica que apenas aquele bloco de código será executado.

##  Desafio de Fixação

1.  Crie uma variável `int` chamada `nota` e atribua um valor entre 0 e 100.
2.  Usando `if-else if-else`, determine e imprima o conceito do aluno:
    *   90-100: "A"
    *   80-89: "B"
    *   70-79: "C"
    *   60-69: "D"
    *   Abaixo de 60: "F"
3.  Crie uma variável `String` chamada `mes` e atribua o nome de um mês (ex: "Janeiro").
4.  Usando `switch`, imprima o número correspondente ao mês (ex: "Janeiro" -> 1). Se o mês for inválido, imprima "Mês inválido".


## Referências

[1] Oracle. Java SE 21 API. Disponível em: [https://docs.oracle.com/en/java/javase/21/docs/api/](https://docs.oracle.com/en/java/javase/21/docs/api/)
[2] dev.java. Language Basics. Disponível em: [https://dev.java/learn/language-basics/](https://dev.java/learn/language-basics/)

---

[Voltar aos Links Rápidos](../../README.md#links-rapidos) | [Anterior](aula_1_2_operadores.md) | [Próxima aula/módulo](aula_1_4_lacos_repeticao.md)
