# Lista de Imagens a Gerar

Abaixo está o detalhamento de cada imagem conceitual necessária para ilustrar os tópicos do curso, caso precisem ser recriadas ou substituídas por versões mais ricas.

## 1. Módulo 00
- **Módulo**: Módulo 00: Preparação e Mentalidade
- **Placeholder**: `[IMAGEM_CONCEITO_ECOSSISTEMA_JAVA]`
- **Descrição**: Um diagrama mostrando a diferença e a hierarquia entre JRE, JDK, JVM e GraalVM. Deve ficar claro que o JDK contém o JRE, que por sua vez contém a JVM.
- **Prompt de Criação**: A technical diagram illustrating the Java ecosystem hierarchy. Show an outer box labeled 'JDK (Java Development Kit)' containing a box labeled 'JRE (Java Runtime Environment)', which contains 'JVM (Java Virtual Machine)'. Add 'GraalVM' as an alternative high-performance runtime. All text in the image must be in Brazilian Portuguese.

## 2. Módulo 01
- **Módulo**: Módulo 01: Fundamentos da Linguagem Java
- **Placeholder**: `[IMAGEM_CONCEITO_FLUXO_EXECUCAO]`
- **Descrição**: Um fluxograma simples mostrando o caminho de compilação: Código Fonte (.java) -> Compilador (javac) -> Bytecode (.class) -> JVM executando em qualquer sistema operacional (Windows, Mac, Linux).
- **Prompt de Criação**: A flowchart showing the Java compilation process. It starts with 'Código Fonte (.java)' going through a gear icon labeled 'Compilador (javac)' resulting in 'Bytecode (.class)'. Then arrows pointing from Bytecode to 'JVM', which then branches to three laptop icons showing 'Windows', 'Mac', and 'Linux'. All text in the image must be in Brazilian Portuguese.

## 3. Módulo 02
- **Módulo**: Módulo 02: Orientação a Objetos
- **Placeholder**: `[IMAGEM_CONCEITO_PILARES_OOP]`
- **Descrição**: Uma ilustração dos 4 pilares da Orientação a Objetos (Abstração, Encapsulamento, Herança e Polimorfismo) usando analogias visuais (ex: cápsula para encapsulamento, árvore genealógica para herança).
- **Prompt de Criação**: An educational illustration showing the four pillars of Object-Oriented Programming: Abstraction, Encapsulation (represented by a capsule or shield), Inheritance (represented by a family tree structure), and Polymorphism (represented by a shape shifting into different forms). All text in the image must be in Brazilian Portuguese.

## 4. Módulo 03
- **Módulo**: Módulo 03: Recursos Modernos do Java
- **Placeholder**: `[IMAGEM_CONCEITO_RECORD_PATTERN]`
- **Descrição**: Uma comparação visual entre uma classe tradicional verbosa (com getters, setters, equals, hashcode) e um Java Record compacto e limpo.
- **Prompt de Criação**: A side-by-side visual comparison. On the left side, a large block of code representing a traditional Java class with getters, setters, equals, hashcode. On the right side, a very small and clean block representing a 'Java Record'. Use arrows to show that the Record encapsulates all that boilerplate. All text in the image must be in Brazilian Portuguese.

## 5. Módulo 04
- **Módulo**: Módulo 04: Coleções e Stream API
- **Placeholder**: `[IMAGEM_CONCEITO_STREAM_PIPELINE]`
- **Descrição**: Uma analogia de uma linha de montagem industrial. Os dados brutos entram (List), passam por filtros (filter), são transformados (map) e empacotados no final (collect).
- **Prompt de Criação**: An illustration of a factory assembly line representing a Java Stream pipeline. Raw materials enter on a conveyor belt (List), pass through a filtering machine (filter), then a transforming machine (map), and finally get packaged into boxes at the end (collect). All text in the image must be in Brazilian Portuguese.

## 6. Módulo 05
- **Módulo**: Módulo 05: Tratamento de Erros e Tópicos Avançados
- **Placeholder**: `[IMAGEM_CONCEITO_VIRTUAL_THREADS]`
- **Descrição**: Uma visualização mostrando a diferença entre Platform Threads (pesadas, ocupando muito espaço) e Virtual Threads (leves, mapeadas de muitas para uma Platform Thread no Project Loom).
- **Prompt de Criação**: A technical diagram comparing OS Threads and Virtual Threads in Java. Show a few large, heavy blocks representing 'Platform Threads', and then show thousands of tiny, lightweight dots representing 'Virtual Threads' multiplexed onto a single carrier thread. All text in the image must be in Brazilian Portuguese.

## 7. Módulo 06
- **Módulo**: Módulo 06: Persistência de Dados com JPA e Hibernate
- **Placeholder**: `[IMAGEM_CONCEITO_CICLO_VIDA_JPA]`
- **Descrição**: O diagrama de transição de estados de uma entidade JPA: Transient, Managed, Detached e Removed, com as setas indicando os métodos do EntityManager (persist, merge, remove).
- **Prompt de Criação**: A state transition diagram for JPA entities. Show four states: 'Transient', 'Managed', 'Detached', and 'Removed'. Draw arrows between them labeled with methods like 'persist()', 'merge()', 'remove()', and 'detach()'. All text in the image must be in Brazilian Portuguese.

## 8. Módulo 07
- **Módulo**: Módulo 07: Ecossistema Spring Boot 3
- **Placeholder**: `[IMAGEM_CONCEITO_SPRING_IOC]`
- **Descrição**: Uma ilustração do contêiner de Inversão de Controle (IoC) do Spring gerenciando o ciclo de vida dos Beans e injetando dependências automaticamente.
- **Prompt de Criação**: An architecture diagram showing a central 'Spring IoC Container'. Inside, show puzzle pieces labeled 'Beans' connecting to each other automatically, representing Dependency Injection. All text in the image must be in Brazilian Portuguese.

## 9. Módulo 08
- **Módulo**: Módulo 08: Arquitetura Moderna e Cloud Native
- **Placeholder**: `[IMAGEM_CONCEITO_CONTAINERS_VS_VM]`
- **Descrição**: Uma imagem comparativa clássica: de um lado Máquinas Virtuais (com Guest OS pesado) e do outro Containers Docker (compartilhando o SO hospedeiro, mais leves).
- **Prompt de Criação**: A side-by-side comparison diagram. On the left, 'Virtual Machines' showing hardware, hypervisor, and multiple boxes each with a 'Guest OS' and App. On the right, 'Containers' showing hardware, OS, Docker Engine, and multiple apps sharing the OS. All text in the image must be in Brazilian Portuguese.

## 10. Módulo 09
- **Módulo**: Módulo 09: Projeto Final Capstone
- **Placeholder**: `[IMAGEM_CONCEITO_ARQUITETURA_FINAL]`
- **Descrição**: O diagrama de arquitetura do projeto final: Cliente -> API REST Spring Boot -> Spring Data JPA -> Banco de Dados PostgreSQL, tudo rodando em containers.
- **Prompt de Criação**: A software architecture diagram for a web application. Show a Client (browser/mobile) connecting to a 'Spring Boot REST API', which connects to 'Spring Data JPA', which stores data in a 'PostgreSQL Database'. Show a Docker logo encapsulating the backend components. All text in the image must be in Brazilian Portuguese.
