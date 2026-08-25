# Aula 7.4: Validação de Dados e Tratamento de Erros no Spring Boot 

## Objetivos de Aprendizagem

Ao concluir este conteúdo, você deverá ser capaz de explicar os conceitos apresentados, executar os exemplos, reconhecer seus casos de uso e aplicar o conhecimento em uma solução pequena e verificável.

## Sumário

- [ Objetivos de Aprendizagem](#-objetivos-de-aprendizagem)
- [Conteúdo da Aula](#contedo-da-aula)
  - [1. Validação de Dados com Bean Validation (Jakarta Validation)](#1-validao-de-dados-com-bean-validation-jakarta-validation)
  - [2. Tratamento Global de Erros (@ControllerAdvice)](#2-tratamento-global-de-erros-controlleradvice)
- [Desafio de Fixação](#desafio-de-fixao)
  - [ Navegação](#-navegao)

## Introdução

Esta aula apresenta conceitos essenciais de **Ecossistema Spring Boot 3**. Estude em pequenos ciclos: leia, execute os exemplos, altere uma parte e explique com suas próprias palavras.

##  Objetivos de Aprendizagem
- Compreender a importância da validação de dados de entrada em APIs REST.
- Utilizar Bean Validation (`jakarta.validation.constraints`) para validar DTOs (`@NotNull`, `@NotBlank`, `@Size`, `@Email`, etc.).
- Tratar exceções globalmente usando `@ControllerAdvice` e `@ExceptionHandler`.
- Retornar respostas de erro padronizadas e amigáveis para o cliente da API.

##  Conteúdo da Aula

Quando construímos APIs RESTful, nunca devemos confiar nos dados enviados pelo cliente. Dados inválidos, nulos ou malformados podem corromper o banco de dados ou causar falhas na aplicação. Além disso, quando erros ocorrem, a API precisa retornar respostas claras e estruturadas em vez de rastros de pilha (`stack traces`) confusos.

### 1. Validação de Dados com Bean Validation (Jakarta Validation)

O Spring Boot integra perfeitamente a especificação **Bean Validation** (Hibernate Validator), permitindo que você valide propriedades de objetos usando anotações diretamente nas classes DTO.

#### Principais Anotações de Validação:

*   **`@NotNull`**: O campo não pode ser `null`.
*   **`@NotBlank`**: O campo não pode ser `null`, nem vazio (`""`), nem conter apenas espaços em branco (aplicável a `String`).
*   **`@NotEmpty`**: O campo não pode ser `null` nem vazio (aplicável a `String`, `Collection`, `Map`, arrays).
*   **`@Size(min = x, max = y)`**: O tamanho do campo deve estar entre `min` e `max`.
*   **`@Min(value)` / `@Max(value)`**: Valores numéricos mínimos e máximos.
*   **`@Email`**: Valida se a string tem formato de email válido.
*   **`@Positive` / `@PositiveOrZero`**: Números positivos.

```java
import jakarta.validation.constraints.NotBlank;
import jakarta.validation.constraints.NotNull;
import jakarta.validation.constraints.Positive;
import jakarta.validation.constraints.Size;

public record UsuarioDto(
    @NotBlank(message = "O nome é obrigatório e não pode estar em branco.")
    @Size(min = 3, max = 100, message = "O nome deve ter entre 3 e 100 caracteres.")
    String nome,

    @NotBlank(message = "O email é obrigatório.")
    @jakarta.validation.constraints.Email(message = "Formato de email inválido.")
    String email,

    @NotNull(message = "A idade é obrigatória.")
    @Positive(message = "A idade deve ser um número positivo.")
    Integer idade
) {}
```

#### Ativando a Validação no Controller:

Para acionar a validação no momento em que o corpo da requisição é desserializado, devemos adicionar a anotação **`@Valid`** (ou `@Validated`) no parâmetro do método do controller que recebe o `@RequestBody`.

```java
import jakarta.validation.Valid;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api/usuarios")
public class UsuarioController {

    @PostMapping
    public ResponseEntity<String> criarUsuario(@Valid @RequestBody UsuarioDto usuarioDto) {
        // Se a validação falhar, o Spring lança uma MethodArgumentNotValidException automaticamente
        return ResponseEntity.status(HttpStatus.CREATED).body("Usuário " + usuarioDto.nome() + " criado com sucesso!");
    }
}
```

### 2. Tratamento Global de Erros (`@ControllerAdvice`)

Quando a validação falha (ou quando qualquer outra exceção mapeada ocorre), o Spring lança uma exceção (ex: `MethodArgumentNotValidException`). Se não tratada, o cliente receberá um erro padrão 500 ou 400 sem detalhes úteis.

Para padronizar as respostas de erro em toda a aplicação, usamos a anotação **`@ControllerAdvice`** (ou `@RestControllerAdvice`), combinada com **`@ExceptionHandler`**.

```java
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.validation.FieldError;
import org.springframework.web.bind.MethodArgumentNotValidException;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;

import java.time.LocalDateTime;
import java.util.HashMap;
import java.util.Map;

@RestControllerAdvice // Intercepta exceções em todos os controllers
public class GlobalExceptionHandler {

    // Trata erros de validação (@Valid)
    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<Map<String, Object>> handleValidationExceptions(MethodArgumentNotValidException ex) {
        Map<String, String> erros = new HashMap<>();
        
        // Coleta todos os erros de campos específicos
        ex.getBindingResult().getAllErrors().forEach((error) -> {
            String campo = ((FieldError) error).getField();
            String mensagem = error.getDefaultMessage();
            erros.put(campo, mensagem);
        });

        Map<String, Object> resposta = new HashMap<>();
        resposta.put("timestamp", LocalDateTime.now());
        resposta.put("status", HttpStatus.BAD_REQUEST.value());
        resposta.put("erros", erros);

        return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(resposta);
    }

    // Trata exceções personalizadas de recurso não encontrado (exemplo)
    @ExceptionHandler(RuntimeException.class)
    public ResponseEntity<Map<String, Object>> handleRuntimeException(RuntimeException ex) {
        Map<String, Object> resposta = new HashMap<>();
        resposta.put("timestamp", LocalDateTime.now());
        resposta.put("status", HttpStatus.NOT_FOUND.value());
        resposta.put("erro", ex.getMessage());

        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(resposta);
    }
}
```

##  Desafio de Fixação

1.  No projeto `gerenciador-biblioteca-api`, crie um DTO chamado `LivroCadastroDto` com as seguintes validações:
    *   `titulo`: `@NotBlank` (mensagem: "O título do livro não pode estar em branco.")
    *   `autor`: `@NotBlank` (mensagem: "O autor é obrigatório.")
    *   `anoPublicacao`: `@NotNull` e `@Positive` (mensagem: "O ano de publicação deve ser válido.")
2.  Atualize o endpoint POST `/api/v1/livros` para receber `LivroCadastroDto` anotado com `@Valid`.
3.  Crie uma classe `GlobalExceptionHandler` anotada com `@RestControllerAdvice` para capturar `MethodArgumentNotValidException` e retornar um JSON estruturado com os erros de validação.
4.  Teste enviando uma requisição POST com dados inválidos (ex: campos vazios ou nulos) e verifique se a API retorna o status `400 Bad Request` com as mensagens de erro detalhadas.


## Referências

[1] Spring. Spring Boot Reference. Disponível em: [https://docs.spring.io/spring-boot/docs/3.3.x/reference/html/](https://docs.spring.io/spring-boot/docs/3.3.x/reference/html/)
[2] Spring. Spring Framework Reference. Disponível em: [https://docs.spring.io/spring-framework/reference/](https://docs.spring.io/spring-framework/reference/)

---

[Voltar aos Links Rápidos](../../README.md#links-rapidos) | [Anterior](aula_7_3_api_rest.md) | [Próxima aula/módulo](../modulo_08_arquitetura_moderna_e_cloud_native.md)
