# Aula 7.2: Injeção de Dependência e Inversão de Controle (IoC) 

## Objetivos de Aprendizagem

Ao concluir este conteúdo, você deverá ser capaz de explicar os conceitos apresentados, executar os exemplos, reconhecer seus casos de uso e aplicar o conhecimento em uma solução pequena e verificável.

## Sumário

- [ Objetivos de Aprendizagem](#-objetivos-de-aprendizagem)
- [Conteúdo da Aula](#contedo-da-aula)
  - [1. Inversão de Controle (IoC - Inversion of Control) e Injeção de Dependência (DI)](#1-inverso-de-controle-ioc---inversion-of-control-e-injeo-de-dependncia-di)
  - [2. O Container Spring (IoC Container)](#2-o-container-spring-ioc-container)
  - [3. Tipos de Injeção de Dependência](#3-tipos-de-injeo-de-dependncia)
  - [4. O Escopo dos Beans](#4-o-escopo-dos-beans)
- [Desafio de Fixação](#desafio-de-fixao)
  - [ Navegação](#-navegao)

## Introdução

Esta aula apresenta conceitos essenciais de **Ecossistema Spring Boot 3**. Estude em pequenos ciclos: leia, execute os exemplos, altere uma parte e explique com suas próprias palavras.

##  Objetivos de Aprendizagem
- Compreender os conceitos fundamentais de **Inversão de Controle (IoC)** e **Injeção de Dependência (DI)**.
- Entender como o Container Spring (IoC Container) gerencia os componentes da aplicação (Beans).
- Aprender a utilizar anotações como `@Component`, `@Service`, `@Repository`, `@Autowired` e `@Bean`.
- Conhecer os diferentes tipos de injeção de dependência e por que a injeção via construtor é a mais recomendada.

##  Conteúdo da Aula

Um dos pilares do framework Spring é a inversão do controle de fluxo e o gerenciamento de dependências entre objetos. Se você já programou em Java tradicional, sabe que criar instâncias de classes usando `new` e gerenciar quem depende de quem pode rapidamente se tornar complexo e difícil de manter. O Spring resolve isso elegantemente com **IoC** e **DI**.

### 1. Inversão de Controle (IoC - Inversion of Control) e Injeção de Dependência (DI)

*   **Inversão de Controle (IoC)**: É um princípio de design onde o controle de execução e o ciclo de vida dos objetos são invertidos. Em vez de o seu código criar e controlar seus próprios objetos e dependências, um container externo (o Container Spring) assume esse controle. O container decide *quando* criar o objeto, *como* configurá-lo e *quando* destruí-lo.
*   **Injeção de Dependência (DI)**: É um padrão de design específico para implementar IoC. Em vez de uma classe criar diretamente as dependências que precisa para funcionar (usando `new`), essas dependências são "injetadas" (fornecidas) na classe pelo container IoC.

#### Exemplo Sem Injeção de Dependência:
```java
class ServicoEmail {
    public void enviar(String mensagem) {
        System.out.println("Enviando email: " + mensagem);
    }
}

class ClienteService {
    private ServicoEmail servicoEmail;

    public ClienteService() {
        // A própria classe cria sua dependência (acoplamento forte)
        this.servicoEmail = new ServicoEmail();
    }

    public void cadastrarCliente(String nome) {
        // ... lógica de cadastro ...
        servicoEmail.enviar("Bem-vindo, " + nome + "!");
    }
}
```

#### Exemplo Com Injeção de Dependência (Spring):
```java
import org.springframework.stereotype.Service;

@Service // Indica que é um componente gerenciado pelo Spring
class ServicoEmail {
    public void enviar(String mensagem) {
        System.out.println("Enviando email: " + mensagem);
    }
}

@Service
class ClienteService {
    private final ServicoEmail servicoEmail;

    // A dependência é injetada pelo construtor (Injeção de Dependência)
    public ClienteService(ServicoEmail servicoEmail) {
        this.servicoEmail = servicoEmail;
    }

    public void cadastrarCliente(String nome) {
        // ... lógica de cadastro ...
        servicoEmail.enviar("Bem-vindo, " + nome + "!");
    }
}
```

### 2. O Container Spring (IoC Container)

O coração do Spring Framework é o **Container IoC**. Ele é responsável por instanciar, configurar e montar os objetos chamados **Beans**, além de gerenciar todo o seu ciclo de vida.

Para que o Spring saiba quais classes gerenciar, nós usamos **anotações de estereótipo**:

*   **`@Component`**: Anotação genérica para qualquer bean gerenciado pelo Spring.
*   **`@Service`**: Indica que a classe pertence à camada de serviço (regras de negócio). É uma especialização de `@Component`.
*   **`@Repository`**: Indica que a classe pertence à camada de persistência (acesso a dados). Também traduz exceções de banco de dados e é uma especialização de `@Component`.
*   **`@Controller` / `@RestController`**: Indica que a classe é um controlador web / API REST. Especialização de `@Component`.

O Spring faz uma varredura (Component Scan) no classpath da aplicação em busca dessas anotações, cria instâncias das classes encontradas e as armazena no container.

### 3. Tipos de Injeção de Dependência

Existem três formas principais de injetar dependências em Spring:

1.  **Injeção por Construtor (Recomendada)**:
    As dependências são passadas como parâmetros para o construtor da classe.
    *   *Vantagens*: Permite que os campos sejam `final` (imutáveis), garante que o objeto não seja criado sem suas dependências obrigatórias e facilita testes unitários. A partir do Spring 4.3, se a classe tem apenas um construtor, a anotação `@Autowired` é opcional.
2.  **Injeção por Setter**:
    As dependências são injetadas através de métodos `set` após a criação do objeto.
    *   *Vantagens*: Útil para dependências opcionais ou mutáveis.
3.  **Injeção por Campo (Field Injection - Evitar)**:
    A anotação `@Autowired` é colocada diretamente no campo da classe.
    *   *Desvantagens*: Dificulta testes unitários (exige reflection ou inicialização manual), esconde dependências (viola o princípio da responsabilidade única) e não permite campos `final`.

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component
public class PedidoService {

    // 1. Injeção por Construtor (RECOMENDADA)
    private final NotificacaoService notificacaoService;

    @Autowired // Opcional se houver apenas um construtor
    public PedidoService(NotificacaoService notificacaoService) {
        this.notificacaoService = notificacaoService;
    }

    /*
    // 2. Injeção por Campo (NÃO RECOMENDADA, mas muito vista em códigos legados)
    @Autowired
    private NotificacaoService notificacaoServiceField;
    */
}
```

### 4. O Escopo dos Beans

Por padrão, todos os beans gerenciados pelo Spring são **Singletons**. Isso significa que o container cria apenas uma instância daquele bean e a reutiliza em toda a aplicação.

Outros escopos disponíveis incluem:
*   **`prototype`**: Cria uma nova instância do bean toda vez que ele é solicitado.
*   **`request`**: Cria uma nova instância para cada requisição HTTP (usado em aplicações web).
*   **`session`**: Cria uma nova instância para cada sessão HTTP.

Para alterar o escopo, usamos a anotação `@Scope`:
```java
import org.springframework.context.annotation.Scope;
import org.springframework.stereotype.Component;

@Component
@Scope("prototype")
public class TransacaoUnica {
    // ...
}
```

##  Desafio de Fixação

1.  No projeto `gerenciador-biblioteca-api`, crie uma interface chamada `AutorService` com um método `String getAutorDestaque()`.
2.  Crie uma classe `AutorServiceImpl` que implemente `AutorService`, anotada com `@Service`, retornando o nome de um autor famoso (ex: "J.R.R. Tolkien").
3.  Crie um `LivroController` que receba `AutorService` via **injeção por construtor**.
4.  Crie um endpoint GET `/autor-destaque` no `LivroController` que chame o método `getAutorDestaque()` do serviço injetado e retorne o resultado.
5.  Execute a aplicação e teste o novo endpoint para garantir que a injeção de dependência funcionou corretamente.


## Referências

[1] Spring. Spring Boot Reference. Disponível em: [https://docs.spring.io/spring-boot/docs/3.3.x/reference/html/](https://docs.spring.io/spring-boot/docs/3.3.x/reference/html/)
[2] Spring. Spring Framework Reference. Disponível em: [https://docs.spring.io/spring-framework/reference/](https://docs.spring.io/spring-framework/reference/)

---

[Voltar aos Links Rápidos](../../README.md#links-rapidos) | [Anterior](aula_7_1_introducao_spring_boot.md) | [Próxima aula/módulo](aula_7_3_api_rest.md)
