# Aula 3.4: Text Blocks: Strings Multilinhas Mais Legíveis 📜

## Objetivos de Aprendizagem

Ao concluir este conteúdo, você deverá ser capaz de explicar os conceitos apresentados, executar os exemplos, reconhecer seus casos de uso e aplicar o conhecimento em uma solução pequena e verificável.

## Sumário

- [ Objetivos de Aprendizagem](#-objetivos-de-aprendizagem)
- [Conteúdo da Aula](#contedo-da-aula)
  - [1. O Problema das Strings Tradicionais](#1-o-problema-das-strings-tradicionais)
  - [2. Introdução aos Text Blocks](#2-introduo-aos-text-blocks)
  - [3. Regras e Dicas para Text Blocks](#3-regras-e-dicas-para-text-blocks)
  - [4. Vantagens dos Text Blocks](#4-vantagens-dos-text-blocks)
- [Desafio de Fixação](#desafio-de-fixao)
  - [ Navegação](#-navegao)

## Introdução

Esta aula apresenta conceitos essenciais de **Java Moderno: Novas Features**. Estude em pequenos ciclos: leia, execute os exemplos, altere uma parte e explique com suas próprias palavras.

##  Objetivos de Aprendizagem
- Compreender o propósito e os benefícios dos Text Blocks em Java.
- Aprender a declarar e utilizar Text Blocks para strings multilinhas.
- Entender como os Text Blocks melhoram a legibilidade do código, especialmente com JSON, XML e SQL.

##  Conteúdo da Aula

Antes do Java 15, lidar com strings multilinhas ou strings que continham caracteres especiais (como aspas duplas ou quebras de linha) era frequentemente complicado e resultava em código difícil de ler. Era necessário usar concatenação, caracteres de escape (`\n`, `\"`) e formatação manual. Os **Text Blocks**, introduzidos como um recurso de preview no Java 13 e padronizados no Java 15, vieram para resolver esse problema, oferecendo uma maneira mais limpa e legível de representar strings multilinhas.

### 1. O Problema das Strings Tradicionais

Considere a necessidade de representar um trecho de código HTML ou JSON em uma string Java:

```java
public class StringsTradicionais {
    public static void main(String[] args) {
        String htmlTradicional = "<html>\n" +
                                 "    <body>\n" +
                                 "        <h1>Olá, Mundo!</h1>\n" +
                                 "    </body>\n" +
                                 "</html>";
        System.out.println(htmlTradicional);

        String jsonTradicional = "{\n" +
                                 "  \"nome\": \"Alice\",\n" +
                                 "  \"idade\": 30,\n" +
                                 "  \"cidade\": \"São Paulo\"\n" +
                                 "}";
        System.out.println(jsonTradicional);
    }
}
```

Como você pode ver, o uso de `\n` para quebras de linha e `\"` para aspas duplas, além da concatenação, torna o código poluído e difícil de visualizar o conteúdo real da string.

### 2. Introdução aos Text Blocks

Um **Text Block** é uma literal de string multilinhas que começa com três aspas duplas (`"""`) seguidas de uma quebra de linha, e termina com três aspas duplas (`"""`). A grande vantagem é que você pode escrever o conteúdo da string como ele apareceria em um editor de texto, sem a necessidade de caracteres de escape para aspas duplas ou quebras de linha.

#### Sintaxe Básica:

```java
String meuTexto = """
    Conteúdo da string
    em múltiplas linhas
    com aspas "duplas" e 'simples'
""";
```

#### Exemplo com Text Blocks (Java 15+):

```java
public class TextBlocksExemplo {
    public static void main(String[] args) {
        String htmlComTextBlocks = """
<html>
    <body>
        <h1>Olá, Mundo!</h1>
    </body>
</html>
""";
        System.out.println(htmlComTextBlocks);

        String jsonComTextBlocks = """
{
  "nome": "Alice",
  "idade": 30,
  "cidade": "São Paulo"
}
""";
        System.out.println(jsonComTextBlocks);

        // O compilador remove espaços em branco indentados comuns
        String sqlQuery = """
                SELECT id, nome, email
                FROM usuarios
                WHERE ativo = true
                ORDER BY nome ASC;
                """;
        System.out.println(sqlQuery);
    }
}
```

### 3. Regras e Dicas para Text Blocks

*   **Delimitadores**: Começa com `"""` seguido de uma quebra de linha e termina com `"""`.
*   **Quebras de Linha**: As quebras de linha dentro do Text Block são preservadas.
*   **Aspas Duplas**: Aspas duplas (`"`) dentro do conteúdo não precisam ser escapadas.
*   **Indentação**: O compilador remove a indentação comum (mínima) de todas as linhas. Isso significa que você pode indentar seu Text Block para alinhar com o código Java sem que essa indentação apareça na string final. Você pode controlar a indentação final usando espaços no início das linhas dentro do bloco.
*   **`\s`**: Pode ser usado para preservar espaços em branco no final de uma linha.
*   **`\` (barra invertida no final da linha)**: Pode ser usado para suprimir uma quebra de linha, concatenando a linha atual com a próxima.

```java
public class TextBlocksAvancado {
    public static void main(String[] args) {
        String textoComEspacosFinais = """
Linha 1 com espaços no final   \s
Linha 2
""";
        System.out.println("--- Texto com espaços finais ---");
        System.out.println(textoComEspacosFinais);

        String textoSemQuebraDeLinha = """
Esta é a primeira linha \
esta é a continuação da primeira linha.
""";
        System.out.println("--- Texto sem quebra de linha ---");
        System.out.println(textoSemQuebraDeLinha);
    }
}
```

### 4. Vantagens dos Text Blocks

*   **Legibilidade Aprimorada**: Facilita a leitura e escrita de strings complexas, especialmente aquelas que contêm JSON, XML, HTML, SQL ou código de outras linguagens.
*   **Menos Caracteres de Escape**: Reduz a necessidade de usar `\n`, `\"`, etc., tornando o código mais limpo.
*   **Manutenibilidade**: Mais fácil de modificar strings complexas sem introduzir erros de escape.

##  Desafio de Fixação

1.  Crie um Text Block que represente um pequeno trecho de código Python:
    ```python
    def hello_world():
        print("Hello from Python!")

    if __name__ == "__main__":
        hello_world()
    ```
2.  Crie outro Text Block que represente uma carta simples, com quebras de linha e parágrafos, incluindo o nome do remetente e do destinatário.
3.  Imprima ambos os Text Blocks no console.


## Referências

[1] OpenJDK. Project Amber. Disponível em: [https://openjdk.org/projects/amber/](https://openjdk.org/projects/amber/)
[2] Oracle. Java Language Changes. Disponível em: [https://docs.oracle.com/en/java/javase/21/language/java-language-changes.html](https://docs.oracle.com/en/java/javase/21/language/java-language-changes.html)

---

[Voltar aos Links Rápidos](../../README.md#links-rapidos) | [Anterior](aula_3_3_pattern_matching.md) | [Próxima aula/módulo](../modulo_04_collections_e_stream_api.md)
