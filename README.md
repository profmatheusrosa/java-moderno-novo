# Curso de Programação Java Moderno

Curso completo de Java 21+, Spring Boot 3, persistência, arquitetura e práticas de entrega. O conteúdo combina microlearning, aprendizagem baseada em projetos e explicações conversacionais.

## Visão Geral do Curso

- **Carga Horária Total:** 80 horas
- **Duração por Aula:** aproximadamente 90 minutos
- **Tecnologias:** Java 21+, Spring Boot 3, JPA/Hibernate, Docker, Kubernetes e Git

## Estrutura de Pastas

```text
curso-java-moderno/
├── teoria/
├── exercicios/
├── projetos/
├── materiais_complementares/
├── imagens/
├── README.md
├── planejamento_aulas.md
└── LICENSE
```

## Conteúdo Programático

| Módulo | Tópicos Principais | Subtópicos | Detalhes |
|--------|-------------------|------------|----------|
| **Módulo 0** | Preparação e Mentalidade | 0.1 Ecossistema Java | Panorama do Java, versões LTS e aplicações no mercado |
|  |  | 0.2 Configuração do ambiente | JDK 21+, IDE, Maven, Git e verificação da instalação |
|  |  | 0.3 Técnicas de estudo | Aprendizagem ativa, prática deliberada e organização da rotina |
| **Módulo 1** | Fundamentos da Linguagem Java | 1.1 Variáveis e tipos de dados | Declaração, tipos primitivos, referências, conversões e inferência |
|  |  | 1.2 Operadores | Operadores aritméticos, relacionais, lógicos, atribuição e ternário |
|  |  | 1.3 Estruturas de controle | Decisões com if, else, switch e expressões condicionais |
|  |  | 1.4 Laços de repetição | for, while, do-while, break, continue e iteração |
|  |  | 1.5 Métodos e funções | Parâmetros, retorno, escopo, sobrecarga e organização do código |
| **Módulo 2** | Introdução à Orientação a Objetos | 2.1 Classes e objetos | Estado, comportamento, construtores e instanciação |
|  |  | 2.2 Encapsulamento | Modificadores de acesso, invariantes e proteção de estado |
|  |  | 2.3 Herança | Reutilização, especialização e relação entre superclasse e subclasse |
|  |  | 2.4 Polimorfismo | Substituição, sobrescrita e múltiplas formas de uma interface |
|  |  | 2.5 Interfaces e classes abstratas | Contratos, abstrações e composição de comportamentos |
| **Módulo 3** | Java Moderno: Novas Features | 3.1 Records | Classes de dados compactas, imutabilidade e métodos gerados |
|  |  | 3.2 Sealed classes | Controle da hierarquia de tipos com sealed, permits e final |
|  |  | 3.3 Pattern matching | Pattern matching para instanceof e switch |
|  |  | 3.4 Text blocks | Strings multilinhas legíveis e formatação de textos |
| **Módulo 4** | Coleções e Stream API | 4.1 List e Set | Listas, conjuntos, ordenação, igualdade e duplicidade |
|  |  | 4.2 Map | Estruturas chave-valor, busca, atualização e iteração |
|  |  | 4.3 Stream API | Pipeline, filter, map, reduce e coleta de resultados |
|  |  | 4.4 Streams avançadas | Agrupamento, particionamento, collectors e desempenho |
|  |  | 4.5 Optional | Modelagem da ausência de valor e redução de null |
| **Módulo 5** | Tratamento de Erros e Tópicos Avançados | 5.1 Exceções | try-catch, finally, exceções verificadas e exceções de domínio |
|  |  | 5.2 Generics | Tipos parametrizados, segurança de tipos e wildcards |
|  |  | 5.3 Expressões lambda | Interfaces funcionais, composição e programação funcional |
|  |  | 5.4 Concorrência e Virtual Threads | Tarefas concorrentes, executors e escalabilidade |
| **Módulo 6** | Persistência de Dados com JPA/Hibernate | 6.1 Introdução à persistência e JDBC | Conexões, comandos SQL, recursos e transações |
|  |  | 6.2 Fundamentos de JPA | Entidades, EntityManager, mapeamento e ciclo de vida |
|  |  | 6.3 Relacionamentos entre entidades | OneToOne, OneToMany, ManyToOne e ManyToMany |
|  |  | 6.4 JPQL e consultas nativas | Consultas orientadas a entidades, parâmetros e SQL nativo |
|  |  | 6.5 Spring Data JPA | Repositories, derived queries, paginação e integração com Spring |
| **Módulo 7** | Ecossistema Spring Boot 3 | 7.1 Introdução ao Spring Boot | Starters, auto-configuração, aplicação e configuração |
|  |  | 7.2 Injeção de dependência e IoC | Container, beans, componentes e injeção por construtor |
|  |  | 7.3 APIs RESTful com Spring MVC | Controllers, rotas, DTOs, HTTP e serialização |
|  |  | 7.4 Validação e tratamento de erros | Bean Validation, respostas de erro e ControllerAdvice |
| **Módulo 8** | Arquitetura Moderna e Cloud Native | 8.1 Introdução a microsserviços | Limites de serviço, comunicação e consistência distribuída |
|  |  | 8.2 Docker | Imagens, contêineres, volumes e redes |
|  |  | 8.3 Kubernetes | Pods, deployments, services e configurações |
|  |  | 8.4 CI/CD com GitHub Actions | Integração contínua, pipelines e entrega automatizada |
|  |  | 8.5 Observabilidade | Logs, métricas, traces e indicadores |
| **Módulo 9** | Projeto Final Capstone | 9.1 Revisão e integração | Integração dos fundamentos do curso |
|  |  | 9.2 Design do projeto final | Requisitos, arquitetura e critérios de aceite |
|  |  | 9.3 Implementação do backend | API, persistência, segurança e testes |
|  |  | 9.4 Implementação do frontend | Contratos HTTP, integração e experiência do usuário |
|  |  | 9.5 Deploy e apresentação | Entrega, validação, documentação e apresentação técnica |

## Links Rápidos

| Módulo | Teoria | Exercícios | Projeto |
|---|---|---|---|
| Módulo 00<br>Preparação e Mentalidade | [Teoria](teoria/modulo_00_preparacao_e_mentalidade.md) | [Exercícios](exercicios/modulo_00_exercicios_preparacao_e_mentalidade.md) | [Projeto](projetos/modulo_00_projeto_preparacao_e_mentalidade.md) |
| Módulo 01<br>Fundamentos da Linguagem Java | [Teoria](teoria/modulo_01_fundamentos_da_linguagem_java.md) | [Exercícios](exercicios/modulo_01_exercicios_fundamentos_da_linguagem_java.md) | [Projeto](projetos/modulo_01_projeto_fundamentos_da_linguagem_java.md) |
| Módulo 02<br>Introdução à Orientação a Objetos | [Teoria](teoria/modulo_02_orientacao_a_objetos.md) | [Exercícios](exercicios/modulo_02_exercicios_orientacao_a_objetos.md) | [Projeto](projetos/modulo_02_projeto_orientacao_a_objetos.md) |
| Módulo 03<br>Java Moderno: Novas Features | [Teoria](teoria/modulo_03_recursos_modernos_do_java.md) | [Exercícios](exercicios/modulo_03_exercicios_recursos_modernos_do_java.md) | [Projeto](projetos/modulo_03_projeto_recursos_modernos_do_java.md) |
| Módulo 04<br>Coleções e Stream API | [Teoria](teoria/modulo_04_collections_e_stream_api.md) | [Exercícios](exercicios/modulo_04_exercicios_collections_e_stream_api.md) | [Projeto](projetos/modulo_04_projeto_collections_e_stream_api.md) |
| Módulo 05<br>Tratamento de Erros e Tópicos Avançados | [Teoria](teoria/modulo_05_tratamento_de_erros_e_topicos_avancados.md) | [Exercícios](exercicios/modulo_05_exercicios_tratamento_de_erros_e_topicos_avancados.md) | [Projeto](projetos/modulo_05_projeto_tratamento_de_erros_e_topicos_avancados.md) |
| Módulo 06<br>Persistência de Dados com JPA/Hibernate | [Teoria](teoria/modulo_06_persistencia_de_dados_com_jpa_hibernate.md) | [Exercícios](exercicios/modulo_06_exercicios_persistencia_de_dados_com_jpa_hibernate.md) | [Projeto](projetos/modulo_06_projeto_persistencia_de_dados_com_jpa_hibernate.md) |
| Módulo 07<br>Ecossistema Spring Boot 3 | [Teoria](teoria/modulo_07_ecossistema_spring_boot_3.md) | [Exercícios](exercicios/modulo_07_exercicios_ecossistema_spring_boot_3.md) | [Projeto](projetos/modulo_07_projeto_ecossistema_spring_boot_3.md) |
| Módulo 08<br>Arquitetura Moderna e Cloud Native | [Teoria](teoria/modulo_08_arquitetura_moderna_e_cloud_native.md) | [Exercícios](exercicios/modulo_08_exercicios_arquitetura_moderna_e_cloud_native.md) | [Projeto](projetos/modulo_08_projeto_arquitetura_moderna_e_cloud_native.md) |
| Módulo 09<br>Projeto Final Capstone | [Teoria](teoria/modulo_09_projeto_final_capstone.md) | [Exercícios](exercicios/modulo_09_exercicios_projeto_final_capstone.md) | [Projeto](projetos/modulo_09_projeto_projeto_final_capstone.md) |

## Planejamento das Aulas

Consulte o [planejamento_aulas.md](planejamento_aulas.md) para a distribuição detalhada.

## Pré-requisitos e Ambiente de Desenvolvimento

JDK 21 ou superior, VS Code com Extension Pack for Java ou IntelliJ IDEA, Maven, Git, Docker e uma ferramenta para testar APIs HTTP. Para persistência, use PostgreSQL local ou em contêiner.

## Integridade Acadêmica

Use este curso para construir entendimento próprio. Consulte documentação e ferramentas de IA de forma responsável, registre quando uma ferramenta ajudou e valide cada resposta executando ou revisando o código. Não copie exercícios ou projetos sem compreender as decisões. O esforço, os erros e as revisões fazem parte da aprendizagem e da construção de caráter profissional.

## Como estudar

Leia a teoria, execute os exemplos, responda às questões sem consultar a resposta, implemente os exercícios e avance para o projeto. Ao concluir cada módulo, faça uma retrospectiva curta.

---

[Iniciar pelo Módulo 00](teoria/modulo_00_preparacao_e_mentalidade.md) | [Materiais Complementares](materiais_complementares/) | [Planejamento](planejamento_aulas.md)
