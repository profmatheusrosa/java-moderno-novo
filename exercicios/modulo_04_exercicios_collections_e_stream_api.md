# Exercícios: Collections e Stream API

> Adeus `for` loops gigantes e cheios de `if`! A Stream API e as Collections modernas mudaram para sempre a cara do Java.

Vamos manipular dados de forma funcional e declarativa.

## 1. O Filtro Mágico (Streams Básico)

Streams permitem processar coleções como se fossem um "tubo" de transformações.

**Sua missão:**
Dada uma lista de nomes, use Streams para:
1. Filtrar apenas os que começam com a letra 'A'.
2. Transformar todos em letras MAIÚSCULAS.
3. Ordenar alfabeticamente.
4. Coletar o resultado em uma nova lista.

```java
import java.util.List;

public class NomesStream {
    public static void main(String[] args) {
        List<String> nomes = List.of("Ana", "Pedro", "Amanda", "Carlos", "Alberto");
        
        // TODO: Crie a lista processada usando nomes.stream()...
    }
}
```

<details>
<summary>Ver gabarito</summary>

```java
import java.util.List;
import java.util.stream.Collectors;

public class NomesStream {
    public static void main(String[] args) {
        List<String> nomes = List.of("Ana", "Pedro", "Amanda", "Carlos", "Alberto");
        
        List<String> nomesProcessados = nomes.stream()
            .filter(n -> n.startsWith("A"))
            .map(String::toUpperCase)
            .sorted()
            .collect(Collectors.toList());
            // ou .toList() no Java 16+
            
        System.out.println(nomesProcessados); 
        // [ALBERTO, AMANDA, ANA]
    }
}
```
</details>

## 2. O Mestre do Agrupamento (Collectors)

O método `.collect()` é super poderoso, especialmente quando combinado com os `Collectors`.

**Sua missão:**
Dado o record `Produto(String nome, String categoria)`, agrupe uma lista de produtos por sua categoria. O resultado deve ser um `Map<String, List<Produto>>`.

```java
// TODO: Produto record

public class Agrupamento {
    public static void main(String[] args) {
        // TODO: Criar lista, usar stream().collect(Collectors.groupingBy(...))
    }
}
```

<details>
<summary>Ver gabarito</summary>

```java
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

record Produto(String nome, String categoria) {}

public class Agrupamento {
    public static void main(String[] args) {
        List<Produto> produtos = List.of(
            new Produto("Notebook", "Eletronicos"),
            new Produto("Mouse", "Eletronicos"),
            new Produto("Camisa", "Vestuario")
        );
        
        Map<String, List<Produto>> produtosPorCategoria = produtos.stream()
            .collect(Collectors.groupingBy(Produto::categoria));
            
        System.out.println(produtosPorCategoria);
    }
}
```
</details>

## 3. Em Busca do Mais Caro

Onde há agrupamentos e Streams, podemos encontrar máximos e mínimos facilmente.

**Sua missão:**
Usando o mesmo record Produto, mas agora com `preco` (`Produto(String nome, String categoria, double preco)`), use Streams para encontrar o Produto com o maior preço da lista! (Retornará um `Optional`).

```java
// TODO: Adicionar preço ao Produto e usar stream().max(...)
```

<details>
<summary>Ver gabarito</summary>

```java
import java.util.Comparator;
import java.util.List;
import java.util.Optional;

record Produto(String nome, String categoria, double preco) {}

public class MaiorPreco {
    public static void main(String[] args) {
        List<Produto> produtos = List.of(
            new Produto("Notebook", "Eletronicos", 4000.0),
            new Produto("Mouse", "Eletronicos", 50.0),
            new Produto("Monitor", "Eletronicos", 1200.0)
        );
        
        Optional<Produto> maisCaro = produtos.stream()
            .max(Comparator.comparingDouble(Produto::preco));
            
        maisCaro.ifPresent(p -> System.out.println("O mais caro é: " + p.nome()));
    }
}
```
</details>

## 4. O Fim do NullPointerException (Optional)

Evite o uso de `null` ao retornar valores de buscas. Use `Optional`.

**Sua missão:**
Crie um método `buscarUsuarioPorEmail` que procura em uma lista (mockada) e retorna um `Optional<Usuario>`. No `main`, pegue o resultado. Se existir, imprima o nome; se não, imprima "Usuário não encontrado".

```java
// TODO: Método buscarUsuarioPorEmail e teste com ifPresentOrElse ou orElse
```

<details>
<summary>Ver gabarito</summary>

```java
import java.util.List;
import java.util.Optional;

record Usuario(String nome, String email) {}

public class BuscaOptional {
    
    static List<Usuario> usuarios = List.of(
        new Usuario("Joao", "joao@email.com")
    );
    
    public static Optional<Usuario> buscar(String email) {
        return usuarios.stream()
            .filter(u -> u.email().equals(email))
            .findFirst();
    }
    
    public static void main(String[] args) {
        Optional<Usuario> resultado = buscar("maria@email.com");
        
        // Forma elegante de lidar com o resultado:
        resultado.ifPresentOrElse(
            u -> System.out.println("Achou: " + u.nome()),
            () -> System.out.println("Usuário não encontrado")
        );
    }
}
```
</details>

---

##  Desafio Bônus: O Contador de Palavras

**Sua missão:**
Dada uma frase (String), transforme-a em minúsculas, remova pontuação (se quiser) e conte a frequência de cada palavra usando um `Map<String, Long>`. Use Streams e `Collectors.groupingBy` com `Collectors.counting()`.

<details>
<summary>Ver gabarito</summary>

```java
import java.util.Arrays;
import java.util.Map;
import java.util.stream.Collectors;

public class ContadorDePalavras {
    public static void main(String[] args) {
        String texto = "java e incrivel porque java tem streams e streams sao incriveis";
        
        Map<String, Long> frequencia = Arrays.stream(texto.split(" "))
            .collect(Collectors.groupingBy(
                palavra -> palavra,      // Agrupa pela própria palavra
                Collectors.counting()    // Conta quantas tem em cada grupo
            ));
            
        System.out.println(frequencia);
        // Exemplo de saída: {sao=1, incriveis=1, incrivel=1, e=2, java=2, streams=2, porque=1}
    }
}
```
</details>

---

[Voltar aos Links Rápidos](../README.md#links-rapidos)
