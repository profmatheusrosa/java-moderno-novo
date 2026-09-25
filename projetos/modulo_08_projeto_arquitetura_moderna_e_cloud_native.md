<Projeto 8: Arquitetura Moderna e Cloud Native>
> "O sistema não existe apenas no seu computador. Um software só ganha vida de verdade quando é empacotado, orquestrado e ganha a nuvem."

## O que você vai aprender neste módulo

- Containerização usando Docker (Dockerfile Multi-stage)
- Orquestração local com Docker Compose
- Pipelines de Integração Contínua (CI/CD) com GitHub Actions
- Observabilidade básica com Spring Boot Actuator
- Manifestos Kubernetes essenciais

## Sumário

- [A História e Sua Missão](#a-história-e-sua-missão)
- [Requisitos Funcionais e Técnicos](#requisitos-funcionais-e-técnicos)
- [Critérios de Aceite](#critérios-de-aceite)
- [A Esteira de Deploy (Diagrama)](#a-esteira-de-deploy)
- [Embalando seu Código para Viagem](#embalando-seu-código-para-viagem)

---

## A História e Sua Missão

Sua API REST está fantástica e a equipe frontend está amando consumi-la. Mas há um problema crítico: *na sua máquina roda, na do servidor não*. A equipe de DevOps da faculdade disse que não vai instalar o Java 21 manualmente no servidor; eles exigem que você entregue uma "imagem Docker". Além disso, precisamos ver a saúde da aplicação para o caso dela travar de madrugada.

**Sua missão:** Preparar a sua aplicação para a nuvem. Containerize o sistema, crie uma rotina de integração contínua (CI) e exponha métricas de saúde com o Actuator.

---

## Requisitos Funcionais e Técnicos

1. **Docker:** Criar um `Dockerfile` em duas etapas (Multi-stage build): uma para compilar o `.jar` (usando Maven/Gradle) e outra menor apenas com o JRE para rodar o `.jar`.
2. **Compose:** Criar um `docker-compose.yml` que levante a sua API, o banco PostgreSQL e o pgAdmin de uma só vez, conectando-os na mesma rede.
3. **GitHub Actions:** Criar um arquivo na pasta `.github/workflows` que toda vez que você fizer `git push`, rode os testes automatizados da aplicação e se passarem, gere a imagem Docker.
4. **Actuator:** Adicionar a dependência do Actuator e expor as rotas `/actuator/health` (usada pelo Kubernetes para saber se a app tá viva) e `/actuator/metrics`.
5. **Kubernetes:** Criar os arquivos YAML básicos de `Deployment` e `Service` na pasta `/k8s`.

---

## Critérios de Aceite

- [ ] A aplicação sobe perfeitamente rodando `docker compose up` sem você precisar instalar banco de dados na sua máquina física.
- [ ] A rota `/actuator/health` retorna `{"status":"UP"}`.
- [ ] O pipeline do GitHub Actions tem um "check verde" demonstrando que o build ocorreu com sucesso.

---

## A Esteira de Deploy

A vida do código mudou. Ele não vai mais direto da sua máquina para a produção:

```mermaid
flowchart LR
    A[Seu PC (Commit & Push)] -->|GitHub| B(GitHub Actions)
    B --> C{Testes Passaram?}
    C -- Sim --> D[Build Docker Image]
    C -- Não --> E[Notificar Erro!]
    D --> F[(Docker Hub Registry)]
    F --> G[Servidor / Kubernetes]
```

---

## Embalando seu Código para Viagem

Um Dockerfile *Multi-stage* garante que o pacote final gerado não contenha lixo de compilação, deixando a imagem segura e super leve:

```dockerfile
# ESTÁGIO 1: Build da aplicação (Builder)
# Usamos uma imagem pesada que tem o JDK e ferramentas completas
FROM eclipse-temurin:21-jdk AS builder
WORKDIR /app
# Copiamos os arquivos do projeto
COPY . .
# Rodamos a compilação (Gerando o .jar)
RUN ./mvnw clean package -DskipTests

# ESTÁGIO 2: Execução em Produção (Runtime)
# Usamos uma imagem mais leve, apenas com o ambiente de execução (JRE)
FROM eclipse-temurin:21-jre
WORKDIR /app
# Copiamos apenas o .jar gerado na etapa anterior
COPY --from=builder /app/target/biblioteca-0.0.1-SNAPSHOT.jar app.jar

# Expomos a porta que o Spring vai rodar
EXPOSE 8080

# Comando de partida da aplicação
ENTRYPOINT ["java", "-jar", "app.jar"]
```

Com isso, ninguém da equipe de infraestrutura precisa saber programar Java; tudo que eles precisam é rodar: `docker run -p 8080:8080 my-java-app`.

---

## Exercícios de fixação do módulo

<details>
<summary>O que fazer se terminar cedo (Sugestões de Extensão)</summary>

1. **Métrica Customizada:** Com o Actuator e o *Micrometer*, injete um `MeterRegistry` no seu Service e crie um contador `counter("biblioteca.emprestimos.realizados")`. Incremente toda vez que um empréstimo acontecer com sucesso.
2. **Tratamento de Properties com Env Vars:** Certifique-se de que no `application.properties` você configure o banco referenciando variáveis de ambiente (ex: `spring.datasource.url=${DB_URL}`) em vez de chaves *hardcoded*. O Docker Compose passará as credenciais certas via `environment`.

</details>

## Referências

- [Spring Boot with Docker](https://spring.io/guides/gs/spring-boot-docker/)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Spring Boot Actuator: Production-ready features](https://docs.spring.io/spring-boot/docs/current/reference/html/actuator.html)

---
[Voltar aos Links Rápidos](../README.md#links-rapidos)
