<Projeto 4: Collections e Stream API>
> "Dados sem organização são apenas ruído. Com Coleções e Streams, transformamos dados em respostas."

## O que você vai aprender neste módulo

- Estruturas de dados avançadas (List, Set, Map)
- Programação Funcional no Java (Stream API)
- Transformação, Filtros e Reduções
- Uso de `Optional` para evitar NullPointerExceptions

## Sumário

- [A História e Sua Missão](#a-história-e-sua-missão)
- [Requisitos Funcionais](#requisitos-funcionais)
- [Requisitos Técnicos](#requisitos-técnicos)
- [O Pipeline de Relatórios (Diagrama)](#o-pipeline-de-relatórios)
- [Streams em Ação](#streams-em-ação)

---

## A História e Sua Missão

A biblioteca está crescendo e agora gerencia milhares de itens. A diretoria pediu um painel de relatórios urgentes. Eles querem saber quais são os livros mais populares, quantos itens temos de cada categoria, e pior: querem uma lista imediata de todos os alunos que estão com livros atrasados. Fazer isso usando `for` e `if` vai gerar um código gigantesco e ineficiente.

**Sua missão:** Substituir as manipulações de listas manuais pelo poder do Framework Collections e construir os relatórios utilizando a API de Streams do Java.

---

## Requisitos Funcionais

O sistema deve expor as seguintes buscas/relatórios:
1. Buscar livro por título ou ISBN de forma segura (retornando algo que indique ausência caso não exista).
2. Gerar uma lista contendo os nomes dos livros em caixa alta, ordenados alfabeticamente.
3. Listar apenas os livros que estão disponíveis (não emprestados).
4. Agrupar os livros por Categoria/Gênero (ex: "TI", "Ficção").

## Requisitos Técnicos

- Usar um `Set` para garantir que o acervo nunca tenha um livro duplicado (mesmo ISBN).
- A busca de livros deve retornar um `Optional<Livro>` para obrigar o chamador a tratar o caso de não encontrar.
- Usar `stream().filter()`, `stream().map()`, e `stream().sorted()` para os relatórios.
- Usar `Collectors.groupingBy` para o agrupamento de categorias (gerando um `Map<String, List<Livro>>`).

---

## Critérios de Aceite

- [ ] A inclusão de dois livros com o mesmo código identificador em um `Set` é rejeitada automaticamente (verifique a sobrescrita do `equals` e `hashCode`!).
- [ ] Nenhum método retorna `null` diretamente; a ausência de dados usa `Optional`.
- [ ] Os relatórios são gerados usando encadeamento funcional (Stream pipeline).

---

## O Pipeline de Relatórios

Veja como os dados fluem em uma Stream até se tornarem um relatório útil:

```mermaid
flowchart TD
    A[Acervo Completo (List)] --> B[filter: Apenas não emprestados]
    B --> C[map: Extrair apenas o Título]
    C --> D[sorted: Ordem Alfabética]
    D --> E[collect: Nova Lista Limpa]
    
    style A fill:#f9f,stroke:#333,stroke-width:2px
    style E fill:#bbf,stroke:#333,stroke-width:2px
```

---

## Streams em Ação

Esquecer os loops antigos pode ser assustador, mas a legibilidade paga o preço. Veja um exemplo real de aplicação:

```java
import java.util.*;
import java.util.stream.Collectors;

public class ServicoRelatorios {

    private List<Livro> acervo;

    public ServicoRelatorios(List<Livro> acervo) {
        this.acervo = acervo;
    }

    // Dica: Optional força quem chama a lidar com o caso do livro não existir!
    public Optional<Livro> buscarPorIsbn(String isbn) {
        return acervo.stream()
                     .filter(livro -> livro.isbn().equals(isbn))
                     .findFirst();
    }

    public List<String> obterNomesLivrosDisponiveis() {
        return acervo.stream()
                     .filter(Livro::isDisponivel) // Filter: só passa se for true
                     .map(Livro::titulo)          // Map: transforma Livro em String (título)
                     .map(String::toUpperCase)    // Transforma tudo em Maiúsculo
                     .sorted()                    // Ordena de A-Z
                     .collect(Collectors.toList());// Empacota em uma nova lista
    }

    public Map<String, Long> contarLivrosPorCategoria() {
        // OK: Um clássico GROUP BY igual no banco de dados!
        return acervo.stream()
                     .collect(Collectors.groupingBy(Livro::categoria, Collectors.counting()));
    }
}
```

---

## Exercícios de fixação do módulo

<details>
<summary>O que fazer se terminar cedo (Sugestões de Extensão)</summary>

1. **Top 3:** Use a combinação de `sorted()` (com um Comparator customizado) e `limit(3)` na stream para buscar os 3 livros mais recentes da biblioteca.
2. **AnyMatch:** Crie um método booleano rápido `existeAlgumLivroAtrasado()` usando `stream().anyMatch(...)`.
3. **Mapas Rápidos:** Use os métodos de fábrica `Map.of()` e `List.of()` do Java moderno para criar dados de testes rapidamente no seu método `main`.

</details>

## Referências

- [Guia Definitivo de Java Streams (Baeldung)](https://www.baeldung.com/java-8-streams)
- [Entendendo Optionals em Java](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html)

---
[Voltar aos Links Rápidos](../README.md#links-rapidos)
