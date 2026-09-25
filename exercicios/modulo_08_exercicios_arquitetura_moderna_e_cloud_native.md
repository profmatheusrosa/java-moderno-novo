# Exercícios: Arquitetura Moderna e Cloud Native

> Escrever código funciona na sua máquina. O verdadeiro desafio é fazê-lo funcionar em qualquer lugar (Docker), suportar milhões de acessos (Escalabilidade) e monitorar quando cai (Observabilidade)!

## 1. Empacotando para Viagem (Dockerfile)

O Docker empacota sua aplicação e suas dependências (o Java inclusive).

**Sua missão:**
Escreva um `Dockerfile` simples, em duas etapas (multi-stage). A primeira etapa (build) usa uma imagem Maven para rodar `mvn clean package`. A segunda etapa usa uma imagem leve com JRE (ex: `eclipse-temurin:21-jre-alpine`) para rodar o `.jar`.

<details>
<summary>Ver gabarito</summary>

```dockerfile
# Stage 1: Build
FROM maven:3.9-eclipse-temurin-21 AS build
WORKDIR /app
COPY pom.xml .
COPY src ./src
RUN mvn clean package -DskipTests

# Stage 2: Runtime
FROM eclipse-temurin:21-jre-alpine
WORKDIR /app
# Copia o JAR do estágio de build para o de runtime
COPY --from=build /app/target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```
</details>

## 2. A Dupla Dinâmica (Docker Compose)

Apps do mundo real quase sempre dependem de banco de dados.

**Sua missão:**
Escreva o arquivo `docker-compose.yml` que sobe 2 serviços: um banco de dados PostgreSQL e uma aplicação Spring Boot (do seu Dockerfile). Lembre-se de definir as variáveis de ambiente de senha e user do banco.

<details>
<summary>Ver gabarito</summary>

```yaml
version: '3.8'
services:
  db:
    image: postgres:15-alpine
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: mydb
    ports:
      - "5432:5432"

  app:
    build: .
    ports:
      - "8080:8080"
    environment:
      - SPRING_DATASOURCE_URL=jdbc:postgresql://db:5432/mydb
      - SPRING_DATASOURCE_USERNAME=user
      - SPRING_DATASOURCE_PASSWORD=password
    depends_on:
      - db
```
</details>

## 3. O Robô Trabalhador (CI/CD com GitHub Actions)

Automação é o segredo de equipes de alto nível.

**Sua missão:**
Crie a estrutura básica de um workflow do GitHub Actions (em `.github/workflows/build.yml`) que rode em todo *push* para a `main`. O job deve configurar o Java 21 e rodar `mvn test`.

<details>
<summary>Ver gabarito</summary>

```yaml
name: Java CI with Maven

on:
  push:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v3
    
    - name: Set up JDK 21
      uses: actions/setup-java@v3
      with:
        java-version: '21'
        distribution: 'temurin'
        cache: maven
        
    - name: Build with Maven
      run: mvn test
```
</details>

## 4. O Coração Batendo (Spring Boot Actuator)

Como saber se sua aplicação Spring Boot está viva em produção (sem ser tentando dar login)?

**Sua missão:**
Adicione a dependência do Actuator (mentalmente ou no pom.xml). Como você configuraria o `application.properties` para expor APENAS os endpoints `health` e `info` pela web? E qual seria a URL para checar a saúde?

<details>
<summary>Ver gabarito</summary>

- No pom.xml: adicionar `spring-boot-starter-actuator`.
- No properties:
  ```properties
  # Exibe os detalhes do health check (db status, espaço em disco) apenas para acessos permitidos ou sempre
  management.endpoint.health.show-details=always
  
  # Habilita apenas health e info via HTTP (esse é o comportamento padrão na verdade, mas é bom reforçar)
  management.endpoints.web.exposure.include=health,info
  ```
- URL: `http://localhost:8080/actuator/health`

</details>

---

##  Desafio Bônus: Observabilidade com Micrometer

**Sua missão:**
Injete o bean `MeterRegistry` (do Micrometer) na sua classe Service de vendas. Toda vez que o método `realizarVenda()` for chamado, incremente um contador chamado `vendas.realizadas.total`. 

<details>
<summary>Ver gabarito</summary>

```java
import io.micrometer.core.instrument.Counter;
import io.micrometer.core.instrument.MeterRegistry;
import org.springframework.stereotype.Service;

@Service
public class VendasService {

    private final Counter vendasCounter;

    public VendasService(MeterRegistry registry) {
        // Registra o contador no micrometer
        this.vendasCounter = registry.counter("vendas.realizadas.total", "tipo", "online");
    }

    public void realizarVenda() {
        // Lógica de negócio da venda aqui...
        
        // Incrementa a métrica (pode ser coletada por Prometheus/Grafana)
        vendasCounter.increment();
    }
}
```
</details>

---

[Voltar aos Links Rápidos](../README.md#links-rapidos)
