# Aula 7.3: Construindo APIs RESTful com Spring MVC 

## Objetivos de Aprendizagem

Ao concluir este conteúdo, você deverá ser capaz de explicar os conceitos apresentados, executar os exemplos, reconhecer seus casos de uso e aplicar o conhecimento em uma solução pequena e verificável.

## Sumário

- [ Objetivos de Aprendizagem](#-objetivos-de-aprendizagem)
- [Conteúdo da Aula](#contedo-da-aula)
  - [1. O que é uma API RESTful?](#1-o-que--uma-api-restful)
  - [2. Anotações Principais do Spring MVC para REST](#2-anotaes-principais-do-spring-mvc-para-rest)
  - [3. Extraindo Dados da Requisição](#3-extraindo-dados-da-requisio)
  - [4. Controlando o Status HTTP com ResponseEntity](#4-controlando-o-status-http-com-responseentity)
- [Desafio de Fixação](#desafio-de-fixao)
  - [ Navegação](#-navegao)

## Introdução

Esta aula apresenta conceitos essenciais de **Ecossistema Spring Boot 3**. Estude em pequenos ciclos: leia, execute os exemplos, altere uma parte e explique com suas próprias palavras.

##  Objetivos de Aprendizagem
- Compreender os princípios arquiteturais do estilo REST (Representational State Transfer).
- Aprender a utilizar anotações do Spring MVC para mapeamento de rotas e verbos HTTP (`@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping`).
- Manipular parâmetros de requisição (`@PathVariable`, `@RequestParam`, `@RequestBody`).
- Retornar status HTTP adequados usando `ResponseEntity`.

##  Conteúdo da Aula

O Spring Boot, em conjunto com o Spring MVC, fornece um ecossistema robusto e elegante para a construção de **APIs RESTful**. REST tornou-se o padrão da indústria para comunicação entre sistemas web, móveis e microsserviços.

### 1. O que é uma API RESTful?

**REST** (Representational State Transfer) é um estilo arquitetural que define um conjunto de restrições para a criação de serviços web. Uma API RESTful utiliza os verbos HTTP padrão para realizar operações sobre recursos (geralmente representados em formato JSON):

*   **GET**: Recupera um recurso ou uma lista de recursos (Seguro e Idempotente).
*   **POST**: Cria um novo recurso (Não seguro, Não idempotente).
*   **PUT**: Atualiza um recurso existente por completo (Idempotente).
*   **PATCH**: Atualiza parcialmente um recurso existente.
*   **DELETE**: Remove um recurso (Idempotente).

### 2. Anotações Principais do Spring MVC para REST

Para expor endpoints REST em uma aplicação Spring Boot, usamos anotações que mapeiam requisições HTTP para métodos Java:

*   **`@RestController`**: Combinação de `@Controller` e `@ResponseBody`. Indica que a classe é um controlador onde os dados retornados pelos métodos serão escritos diretamente no corpo da resposta HTTP (geralmente convertidos para JSON automaticamente pelo Jackson).
*   **`@RequestMapping("/api/recursos")`**: Define um prefixo de URL comum para todos os métodos da classe.
*   **`@GetMapping`**, **`@PostMapping`**, **`@PutMapping`**, **`@DeleteMapping`**: Atalhos para mapear requisições GET, POST, PUT e DELETE, respectivamente.

### 3. Extraindo Dados da Requisição

O Spring MVC oferece várias anotações para extrair dados da requisição HTTP:

*   **`@PathVariable`**: Extrai variáveis diretamente da URI (ex: `/livros/{id}`).
*   **`@RequestParam`**: Extrai parâmetros de consulta (query parameters) da URL (ex: `/livros?autor=Tolkien`).
*   **`@RequestBody`**: Extrai o corpo da requisição HTTP (JSON) e o desserializa automaticamente em um objeto Java.

```java
import org.springframework.web.bind.annotation.*;
import java.util.List;

@RestController
@RequestMapping("/api/livros")
public class LivroRestesController {

    // GET /api/livros (Lista todos ou filtra por autor via RequestParam)
    @GetMapping
    public List<String> listarLivros(@RequestParam(required = false) String autor) {
        if (autor != null) {
            return List.of("Livro de " + autor);
        }
        return List.of("O Senhor dos Anéis", "1984", "Dom Casmurro");
    }

    // GET /api/livros/{id} (Busca por ID via PathVariable)
    @GetMapping("/{id}")
    public String buscarPorId(@PathVariable Long id) {
        return "Detalhes do livro com ID: " + id;
    }

    // POST /api/livros (Cria um novo livro via RequestBody)
    @PostMapping
    public String criarLivro(@RequestBody LivroDto novoLivro) {
        return "Livro criado com sucesso: " + novoLivro.titulo();
    }
}

// Record para representar o DTO (Data Transfer Object)
record LivroDto(String titulo, String autor, int anoPublicacao) {}
```

### 4. Controlando o Status HTTP com `ResponseEntity`

Por padrão, quando um método de um `@RestController` retorna um objeto, o Spring responde com o status HTTP `200 OK` (ou `201 Created` se configurado, embora o padrão seja 200). No entanto, em aplicações reais, precisamos retornar status HTTP específicos (como `404 Not Found`, `400 Bad Request`, `204 No Content`).

Para isso, utilizamos a classe `ResponseEntity<T>`, que nos permite controlar o corpo da resposta, os headers e o código de status HTTP.

```java
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api/produtos")
public class ProdutoController {

    @GetMapping("/{id}")
    public ResponseEntity<String> buscarProduto(@PathVariable Long id) {
        if (id <= 0) {
            // Retorna status 400 Bad Request
            return ResponseEntity.badRequest().body("ID inválido.");
        }
        
        if (id == 999) {
            // Retorna status 404 Not Found
            return ResponseEntity.status(HttpStatus.NOT_FOUND).body("Produto não encontrado.");
        }

        // Retorna status 200 OK com o corpo
        return ResponseEntity.ok("Produto encontrado: ID " + id);
    }

    @PostMapping
    public ResponseEntity<String> criarProduto(@RequestBody String nomeProduto) {
        // Lógica de salvamento...
        // Retorna status 201 Created com header Location e corpo
        return ResponseEntity.status(HttpStatus.CREATED).body("Produto '" + nomeProduto + "' criado com sucesso!");
    }
}
```

##  Desafio de Fixação

1.  No projeto `gerenciador-biblioteca-api`, crie um `record LivroDto(String titulo, String autor)`.
2.  Crie um `LivroApiController` mapeado para o caminho `/api/v1/livros`.
3.  Implemente os seguintes endpoints:
    *   `GET /api/v1/livros`: Retorna uma lista estática de livros (use `ResponseEntity.ok()`).
    *   `GET /api/v1/livros/{id}`: Retorna um livro específico pelo ID. Se o ID for maior que 10, retorne `ResponseEntity.notFound()`.
    *   `POST /api/v1/livros`: Recebe um `LivroDto` no corpo da requisição e retorna o livro criado com o status HTTP `201 Created`.
4.  Teste os endpoints utilizando ferramentas como cURL, Postman ou Insomnia.


## Referências

[1] Spring. Spring Boot Reference. Disponível em: [https://docs.spring.io/spring-boot/docs/3.3.x/reference/html/](https://docs.spring.io/spring-boot/docs/3.3.x/reference/html/)
[2] Spring. Spring Framework Reference. Disponível em: [https://docs.spring.io/spring-framework/reference/](https://docs.spring.io/spring-framework/reference/)

---

[Voltar aos Links Rápidos](../../README.md#links-rapidos) | [Anterior](aula_7_2_injecao_dependencia.md) | [Próxima aula/módulo](aula_7_4_validacao_erros.md)
