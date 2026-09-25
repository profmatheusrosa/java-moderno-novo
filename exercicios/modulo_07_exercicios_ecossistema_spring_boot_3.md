# Exercícios: Ecossistema Spring Boot 3

> O Spring Boot revolucionou a forma de se construir aplicações corporativas. Ele elimina configurações complexas para você focar no negócio.

Vamos praticar a construção de APIs REST!

## 1. O Primeiro Endpoint (GET)

A porta de entrada da sua aplicação.

**Sua missão:**
Crie um `@RestController` chamado `ProdutoController`. Adicione um método mapeado para `GET /produtos` usando `@GetMapping`. Retorne uma `List<Produto>` estática (mock).

```java
// TODO: @RestController
public class ProdutoController {
    // TODO: @GetMapping
}
```

<details>
<summary>Ver gabarito</summary>

```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import java.util.List;

@RestController
public class ProdutoController {

    @GetMapping("/produtos")
    public List<Produto> listarProdutos() {
        return List.of(
            new Produto(1L, "Teclado", 150.0),
            new Produto(2L, "Mouse", 80.0)
        );
    }
}
```
</details>

## 2. Recebendo e Validando Dados (POST)

Dados vindos de clientes na web nunca são 100% confiáveis. Valide-os!

**Sua missão:**
Crie um endpoint `POST /produtos`. Use `@RequestBody` para receber os dados num DTO (Record `ProdutoRequestDTO`). No DTO, garanta que o `nome` não seja nulo nem vazio (`@NotBlank`) e o `preco` seja maior que zero (`@Positive`). Não esqueça o `@Valid` no Controller.

<details>
<summary>Ver gabarito</summary>

```java
import jakarta.validation.constraints.NotBlank;
import jakarta.validation.constraints.Positive;
import jakarta.validation.Valid;
import org.springframework.web.bind.annotation.*;
import org.springframework.http.HttpStatus;

// Record DTO para validação
public record ProdutoRequestDTO(
    @NotBlank(message = "O nome é obrigatório") String nome,
    @Positive(message = "O preço deve ser maior que zero") double preco
) {}

@RestController
@RequestMapping("/produtos")
public class ProdutoController {

    @PostMapping
    @ResponseStatus(HttpStatus.CREATED) // Retorna 201 em vez de 200
    public String criarProduto(@RequestBody @Valid ProdutoRequestDTO dto) {
        // Logica para salvar no banco chamando um Service...
        return "Produto " + dto.nome() + " criado!";
    }
}
```
</details>

## 3. Tratamento de Erros Moderno (ProblemDetail)

Lançar StackTraces na tela do usuário final é terrível.

**Sua missão:**
Crie uma classe anotada com `@RestControllerAdvice`. Adicione um método com `@ExceptionHandler(IllegalArgumentException.class)` que retorna o tipo padronizado `ProblemDetail` (introduzido no Spring Framework 6).

<details>
<summary>Ver gabarito</summary>

```java
import org.springframework.http.HttpStatus;
import org.springframework.http.ProblemDetail;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;
import java.net.URI;

@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(IllegalArgumentException.class)
    public ProblemDetail handleIllegalArgument(IllegalArgumentException ex) {
        ProblemDetail problem = ProblemDetail.forStatusAndDetail(HttpStatus.BAD_REQUEST, ex.getMessage());
        problem.setTitle("Requisição Inválida");
        problem.setType(URI.create("https://minhaapi.com/erros/bad-request"));
        return problem;
    }
}
```
</details>

## 4. O Segredo da Injeção de Dependências (IoC)

A arquitetura em camadas (Controller -> Service -> Repository) exige que passemos dependências corretamente.

**Sua missão:**
Como injetamos um `ProdutoService` em um `ProdutoController` na forma moderna recomendada pelo Spring? Escreva o esqueleto do Controller usando injeção por construtor (e sem usar `@Autowired`).

<details>
<summary>Ver gabarito</summary>

```java
import org.springframework.web.bind.annotation.RestController;

@RestController
public class ProdutoController {

    // A dependência é private e final
    private final ProdutoService service;

    // A partir do Spring 4.3, se a classe tiver apenas UM construtor, 
    // a anotação @Autowired é opcional. Essa é a prática mais recomendada hoje!
    public ProdutoController(ProdutoService service) {
        this.service = service;
    }
    
    // métodos de endpoint...
}
```
</details>

---

##  Desafio Bônus: Paginação Elegante

Retornar milhões de registros em um GET derrubará sua API.

**Sua missão:**
Refatore o método GET do Exercício 1 para receber a interface `Pageable` do Spring Data nos parâmetros, e retornar um `Page<Produto>`. O Service e Repository também precisam estar preparados!

<details>
<summary>Ver gabarito</summary>

```java
import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class ProdutoController {

    private final ProdutoRepository repository; // injetado via construtor

    // O Spring MVC automaticamente pega os parâmetros `page`, `size` e `sort` da URL 
    // ex: GET /produtos?page=0&size=10&sort=nome,asc
    @GetMapping("/produtos")
    public Page<Produto> listar(Pageable pageable) {
        return repository.findAll(pageable);
    }
}
```
</details>

---

[Voltar aos Links Rápidos](../README.md#links-rapidos)
