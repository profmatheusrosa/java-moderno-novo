# Exercícios: Projeto Final (Capstone)

> A hora da verdade! Juntar tudo que você aprendeu num projeto do mundo real.

Aqui vamos pensar num escopo para um sistema de Gerenciamento de Biblioteca, que consolida todos os tópicos abordados.

## 1. Definindo o Escopo

Antes de codar, precisamos saber o que o sistema vai fazer.

**Sua missão:**
Liste 3 regras de negócio essenciais para a funcionalidade de "Empréstimo de Livro". O que deve acontecer para um empréstimo ser válido? (Ex: o que acontece com a quantidade do livro?)

<details>
<summary>Ver gabarito</summary>

Possíveis regras:
1. O livro deve ter estoque disponível (`quantidade > 0`).
2. Ao realizar o empréstimo, diminuir a quantidade do livro em 1 no banco de dados.
3. O usuário não pode pegar dois exemplares do mesmo livro simultaneamente.
4. Definir a data de devolução para 7 (ou 14) dias a partir da data de hoje.

</details>

## 2. A Arquitetura de Dados (Modelo ER)

A base de qualquer sistema transacional.

**Sua missão:**
Quais seriam as classes principais de Entidade JPA necessárias para este sistema? Indique quais as relações (OneToMany, ManyToMany, etc.) entre elas.

<details>
<summary>Ver gabarito</summary>

Entidades principais:
- `Usuario` (id, nome, email)
- `Livro` (id, titulo, isbn, quantidadeDisponivel)
- `Emprestimo` (id, dataEmprestimo, dataDevolucao, status)

Relacionamentos:
- O `Emprestimo` tem um relacionamento `ManyToOne` para `Usuario` (um usuário tem vários empréstimos, mas um registro de empréstimo pertence a um usuário).
- O `Emprestimo` também tem `ManyToOne` para `Livro`.
*(Se um empréstimo puder ter vários livros no "carrinho", seria um ManyToMany ou uma entidade associativa "ItemEmprestimo").*

</details>

## 3. A Lógica de Negócio (Service)

Aqui entra o Java moderno, validações e tratamentos de erros.

**Sua missão:**
Escreva o pseudo-código (ou código) da função `fazerEmprestimo(Long idUsuario, Long idLivro)` em um `EmprestimoService`. Lembre-se de lançar exceções se as regras do exercício 1 forem violadas!

<details>
<summary>Ver gabarito</summary>

```java
@Service
public class EmprestimoService {
    // dependências injetadas: LivroRepository, UsuarioRepository, EmprestimoRepository

    @Transactional
    public Emprestimo fazerEmprestimo(Long idUsuario, Long idLivro) {
        Usuario usuario = usuarioRepository.findById(idUsuario)
            .orElseThrow(() -> new RegraNegocioException("Usuário não encontrado"));
            
        Livro livro = livroRepository.findById(idLivro)
            .orElseThrow(() -> new RegraNegocioException("Livro não encontrado"));
            
        if (livro.getQuantidadeDisponivel() <= 0) {
            throw new RegraNegocioException("Livro sem estoque!");
        }
        
        // Verifica se o usuario ja tem este livro (lógica simplificada)
        if (emprestimoRepository.existeAtivo(idUsuario, idLivro)) {
            throw new RegraNegocioException("Usuário já tem este livro emprestado.");
        }
        
        livro.setQuantidadeDisponivel(livro.getQuantidadeDisponivel() - 1);
        
        Emprestimo emp = new Emprestimo();
        emp.setUsuario(usuario);
        emp.setLivro(livro);
        emp.setDataEmprestimo(LocalDate.now());
        emp.setDataDevolucao(LocalDate.now().plusDays(7));
        emp.setStatus(StatusEmprestimo.ATIVO);
        
        return emprestimoRepository.save(emp);
    }
}
```
</details>

## 4. Garantindo a Qualidade (Testes)

Testar garante que futuras alterações não quebrem o sistema (Regressão).

**Sua missão:**
Qual seria a estrutura básica de um teste de integração no Spring Boot (usando `@SpringBootTest`) para validar se o endpoint de Empréstimo retorna o HTTP Status 201 Created quando os dados estão corretos?

<details>
<summary>Ver gabarito</summary>

```java
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.http.MediaType;
import org.springframework.test.web.servlet.MockMvc;

import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.post;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

@SpringBootTest
@AutoConfigureMockMvc
class EmprestimoControllerIT {

    @Autowired
    private MockMvc mockMvc;

    @Test
    void deveRetornar201AoCriarEmprestimoValido() throws Exception {
        String jsonPayload = """
            {
                "idUsuario": 1,
                "idLivro": 2
            }
            """;

        mockMvc.perform(post("/api/emprestimos")
                .contentType(MediaType.APPLICATION_JSON)
                .content(jsonPayload))
            .andExpect(status().isCreated());
    }
}
```
</details>

---

##  Desafio Bônus: Entrega Contínua (Railway / Heroku)

Se o projeto não está na internet, ele não existe!

**Sua missão:**
Pesquise plataformas gratuitas (ou de baixo custo) como **Railway**, **Render** ou **Fly.io**. 
O desafio real é: Conecte seu repositório GitHub a um desses serviços e ative o *Auto-Deploy*. Assim, todo push na *main* atualiza a API em produção com uma URL pública (ex: `https://meu-projeto.up.railway.app/swagger-ui.html`). Compartilhe no seu portfólio! 

</details>

---

[Voltar aos Links Rápidos](../README.md#links-rapidos)
