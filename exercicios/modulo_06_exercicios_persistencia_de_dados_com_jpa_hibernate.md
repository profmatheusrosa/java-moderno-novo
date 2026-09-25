# Exercícios: Persistência de Dados com JPA e Hibernate

> Banco de dados é onde a magia acontece. Aprender JPA e Spring Data te livra de muito código repetitivo SQL (embora saber SQL continue sendo fundamental!).

Vamos mapear tabelas e fazer consultas avançadas!

## 1. Conexão Raiz (Opcional Prático)

Entender o JDBC cru é importante para valorizar o JPA. 

**Sua missão:**
Apenas responda mentalmente ou descreva as 3 etapas básicas para fazer uma query com JDBC puro no Java:
1. ...
2. ...
3. ...

<details>
<summary>Ver gabarito</summary>

1. Pegar a conexão usando `DriverManager.getConnection()`.
2. Criar um `PreparedStatement` passando sua string SQL.
3. Executar o statement (`executeQuery()` para Selects, `executeUpdate()` para Inserts/Updates) e iterar sobre o `ResultSet` se for um select.

</details>

## 2. A Entidade Principal (@Entity)

O primeiro passo no JPA é mapear nossa classe para uma tabela.

**Sua missão:**
Mapeie a classe `Produto`. Ela deve ser uma `@Entity`. Use `@Id` e `@GeneratedValue` para criar uma chave primária auto-incremento (Id do tipo Long). Adicione campos `nome` e `preco`.

```java
// TODO: Escreva as anotações do JPA
public class Produto {
    // campos
}
```

<details>
<summary>Ver gabarito</summary>

```java
import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;

@Entity
public class Produto {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String nome;
    private double preco;

    // Getters e Setters, ou constructors, omitidos para brevidade.
}
```
</details>

## 3. O Dilema do 1-para-N (Relacionamentos)

Bancos de dados relacionais brilham quando relacionamos tabelas.

**Sua missão:**
Crie uma classe `Pedido` e um relacionamento `OneToMany` com a classe `ItemPedido`. O Pedido deve ter um `List<ItemPedido> itens`. Deixe a relação bidirecional (O `ItemPedido` aponta de volta pro `Pedido` com `@ManyToOne`).

<details>
<summary>Ver gabarito</summary>

```java
import jakarta.persistence.*;
import java.util.List;

@Entity
public class Pedido {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    // mappedBy indica que o "dono" da relação é a propriedade 'pedido' na classe ItemPedido.
    @OneToMany(mappedBy = "pedido", cascade = CascadeType.ALL)
    private List<ItemPedido> itens;
}

@Entity
public class ItemPedido {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @ManyToOne
    @JoinColumn(name = "pedido_id") // chave estrangeira na tabela
    private Pedido pedido;
}
```
</details>

## 4. O Poder do JPQL

JPQL permite consultar classes Java ao invés de tabelas do banco.

**Sua missão:**
Escreva uma string de consulta JPQL para selecionar todos os Produtos onde o `preco` seja maior que um parâmetro passado (ex: `:precoMinimo`), e ordene os resultados por nome de forma decrescente.

<details>
<summary>Ver gabarito</summary>

```java
// String da query JPQL
String jpql = "SELECT p FROM Produto p WHERE p.preco > :precoMinimo ORDER BY p.nome DESC";

// Como usaria com o EntityManager:
// em.createQuery(jpql, Produto.class)
//   .setParameter("precoMinimo", 100.0)
//   .getResultList();
```
</details>

---

##  Desafio Bônus: A Mágica do Spring Data JPA

No Spring, nós nem implementamos a interface dos Repositórios!

**Sua missão:**
Crie a interface `ProdutoRepository` que estende `JpaRepository<Produto, Long>`. Declare dentro dela um **Query Method** para buscar produtos por categoria, e outro método usando a anotação `@Query` para resolver o mesmo problema do Exercício 4 (produtos mais caros que um valor).

<details>
<summary>Ver gabarito</summary>

```java
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import org.springframework.data.repository.query.Param;

import java.util.List;

public interface ProdutoRepository extends JpaRepository<Produto, Long> {
    
    // 1. Query Method (Mágica pura pelo nome do método)
    List<Produto> findByCategoria(String categoria);
    
    // 2. Usando @Query (Para queries mais complexas ou otimizadas)
    @Query("SELECT p FROM Produto p WHERE p.preco > :valor ORDER BY p.nome DESC")
    List<Produto> buscarCarosOrdenados(@Param("valor") double valor);
}
```
</details>

---

[Voltar aos Links Rápidos](../README.md#links-rapidos)
