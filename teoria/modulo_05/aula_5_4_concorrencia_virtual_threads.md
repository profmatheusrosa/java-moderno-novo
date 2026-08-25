# Aula 5.4: Concorrência e Virtual Threads: Desempenho e Escalabilidade

## Objetivos de Aprendizagem

Ao concluir este conteúdo, você deverá ser capaz de explicar os conceitos apresentados, executar os exemplos, reconhecer seus casos de uso e aplicar o conhecimento em uma solução pequena e verificável.

## Sumário

- [ Objetivos de Aprendizagem](#-objetivos-de-aprendizagem)
- [Conteúdo da Aula](#contedo-da-aula)
  - [1. Concorrência vs. Paralelismo](#1-concorrncia-vs-paralelismo)
  - [2. Threads Tradicionais (Platform Threads)](#2-threads-tradicionais-platform-threads)
  - [3. Desafios da Programação Concorrente](#3-desafios-da-programao-concorrente)
  - [4. Virtual Threads (Project Loom - Java 21+)](#4-virtual-threads-project-loom---java-21)
  - [5. ExecutorService para Gerenciamento de Threads](#5-executorservice-para-gerenciamento-de-threads)
- [Desafio de Fixação](#desafio-de-fixao)
  - [ Navegação](#-navegao)

## Introdução

Esta aula apresenta conceitos essenciais de **Tratamento de Erros e Tópicos Avançados**. Estude em pequenos ciclos: leia, execute os exemplos, altere uma parte e explique com suas próprias palavras.

##  Objetivos de Aprendizagem
- Compreender os conceitos fundamentais de concorrência e paralelismo em Java.
- Entender os desafios da programação concorrente (condições de corrida, deadlocks).
- Aprender sobre `Threads` tradicionais e seus custos.
- Explorar as **Virtual Threads** (Project Loom) introduzidas no Java 21 e seus benefícios.
- Implementar tarefas concorrentes usando `ExecutorService` e `Virtual Threads`.

##  Conteúdo da Aula

A programação concorrente é a capacidade de um programa executar várias tarefas "simultaneamente". Isso é crucial para aplicações modernas que precisam ser responsivas, eficientes e escaláveis, como servidores web, processamento de dados em massa e interfaces de usuário interativas.

### 1. Concorrência vs. Paralelismo

*   **Concorrência**: Lidar com muitas coisas ao mesmo tempo. Um único núcleo de CPU pode alternar rapidamente entre diferentes tarefas, dando a ilusão de simultaneidade (multitarefa). O foco é na *composição* de processos independentes.
*   **Paralelismo**: Executar muitas coisas ao mesmo tempo. Requer múltiplos núcleos de CPU ou múltiplos processadores para executar tarefas verdadeiramente simultâneas. O foco é na *execução* simultânea.

### 2. Threads Tradicionais (Platform Threads)

Historicamente, Java utiliza **Threads de Plataforma** (também conhecidas como threads do sistema operacional). Cada `Thread` Java é mapeada para uma thread do sistema operacional. Embora poderosas, elas têm desvantagens:

*   **Custo Elevado**: Criar e gerenciar threads do SO é caro em termos de memória e tempo de CPU.
*   **Limite de Escala**: O número de threads que um sistema pode suportar é limitado, o que restringe a escalabilidade de aplicações que precisam lidar com milhares de conexões simultâneas (ex: servidores web).
*   **Overhead de Context Switching**: A troca de contexto entre threads do SO é custosa.

```java
public class ThreadTradicionalExemplo {
    public static void main(String[] args) throws InterruptedException {
        Runnable tarefa = () -> {
            System.out.println("Iniciando tarefa em " + Thread.currentThread().getName());
            try {
                Thread.sleep(1000); // Simula trabalho
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
            System.out.println("Finalizando tarefa em " + Thread.currentThread().getName());
        };

        System.out.println("Criando 5 threads tradicionais...");
        for (int i = 0; i < 5; i++) {
            Thread thread = new Thread(tarefa, "Platform-Thread-" + (i + 1));
            thread.start();
            thread.join(); // Espera cada thread terminar antes de iniciar a próxima
        }
        System.out.println("Todas as threads tradicionais terminaram.");
    }
}
```

### 3. Desafios da Programação Concorrente

Programar com concorrência introduz complexidade e novos tipos de bugs:

*   **Condições de Corrida (Race Conditions)**: Quando múltiplas threads acessam e modificam dados compartilhados sem sincronização adequada, o resultado depende da ordem de execução das threads.
*   **Deadlocks**: Duas ou mais threads ficam bloqueadas indefinidamente, esperando por um recurso que a outra thread possui.
*   **Starvation**: Uma thread nunca consegue acesso a um recurso compartilhado porque outras threads sempre o monopolizam.
*   **Livelock**: Threads respondem umas às outras de forma a impedir o progresso, mas sem estarem bloqueadas.

Para lidar com esses problemas, Java oferece mecanismos como `synchronized`, `Lock`s, `Semaphore`s, `Atomic` classes, etc.

### 4. Virtual Threads (Project Loom - Java 21+)

As **Virtual Threads** (introduzidas como recurso final no Java 21) são threads leves, gerenciadas pelo JVM, que não são mapeadas 1:1 para threads do sistema operacional. Elas são projetadas para escalar drasticamente o número de tarefas concorrentes que uma aplicação pode lidar, especialmente em cenários de I/O intensivo (como chamadas de rede ou banco de dados).

#### Como funcionam?

Virtual Threads são executadas em **Platform Threads** (threads do SO), que atuam como "carregadores" (carriers). Quando uma Virtual Thread encontra uma operação de bloqueio (ex: I/O), ela é "desmontada" do seu carrier, liberando o carrier para executar outra Virtual Thread. Quando a operação de bloqueio é concluída, a Virtual Thread é "montada" novamente em um carrier (que pode ser o mesmo ou outro) e continua sua execução.

#### Benefícios:

*   **Alta Escalabilidade**: Milhões de Virtual Threads podem ser criadas com baixo custo de memória.
*   **Simplicidade**: Você programa como se estivesse usando threads tradicionais, sem a complexidade de programação assíncrona ou reativa.
*   **Melhor Utilização de Recursos**: Carriers (Platform Threads) são utilizados de forma mais eficiente.

#### Criando Virtual Threads:

You pode criar Virtual Threads usando `Thread.ofVirtual()` ou `Executors.newVirtualThreadPerTaskExecutor()`.

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.TimeUnit;

public class VirtualThreadExemplo {
    public static void main(String[] args) throws InterruptedException {
        Runnable tarefa = () -> {
            System.out.println("Iniciando tarefa em " + Thread.currentThread().getName());
            try {
                Thread.sleep(100); // Simula trabalho de I/O bloqueante
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
            System.out.println("Finalizando tarefa em " + Thread.currentThread().getName());
        };

        System.out.println("Criando 1000 Virtual Threads...");

        // Opção 1: Usando Thread.ofVirtual()
        // for (int i = 0; i < 1000; i++) {
        //     Thread.ofVirtual().name("Virtual-Thread-" + (i + 1)).start(tarefa);
        // }

        // Opção 2: Usando ExecutorService (recomendado para gerenciar o ciclo de vida)
        try (ExecutorService executor = Executors.newVirtualThreadPerTaskExecutor()) {
            for (int i = 0; i < 1000; i++) {
                executor.submit(tarefa);
            }
        }
        // O executor.close() implícito no try-with-resources espera todas as tarefas terminarem

        System.out.println("Todas as Virtual Threads foram submetidas e estão em execução/concluídas.");
        // Para garantir que todas as tarefas terminem antes do main, podemos usar um CountDownLatch ou esperar o executor
        // Como estamos usando try-with-resources com ExecutorService, ele já espera.
    }
}
```

### 5. `ExecutorService` para Gerenciamento de Threads

Para gerenciar o ciclo de vida de threads (criação, execução, término), é altamente recomendável usar `ExecutorService`. Ele abstrai a complexidade de gerenciar threads diretamente.

*   `Executors.newFixedThreadPool(int nThreads)`: Cria um pool de threads de plataforma com um número fixo de threads.
*   `Executors.newCachedThreadPool()`: Cria um pool de threads de plataforma que reutiliza threads existentes e cria novas conforme necessário.
*   `Executors.newVirtualThreadPerTaskExecutor()`: (Java 21+) Cria um `ExecutorService` que inicia uma nova Virtual Thread para cada tarefa submetida. **Esta é a forma recomendada de usar Virtual Threads na maioria dos casos.**

##  Desafio de Fixação

1.  Crie um método `public static void simularProcessamento(int idTarefa)` que simule um trabalho demorado (ex: `Thread.sleep(200)`).
2.  No método `main`, crie um `ExecutorService` usando `Executors.newVirtualThreadPerTaskExecutor()`.
3.  Submeta 1000 tarefas para este `ExecutorService`, cada uma chamando `simularProcessamento` com um `idTarefa` único.
4.  Meça o tempo total que leva para todas as 1000 tarefas serem concluídas e imprima o resultado.
5.  Compare o tempo com o que seria esperado se você usasse `Executors.newFixedThreadPool(10)` (não precisa implementar, apenas reflita sobre a diferença esperada).


## Referências

[1] Oracle. Virtual Threads. Disponível em: [https://docs.oracle.com/en/java/javase/21/core/virtual-threads.html](https://docs.oracle.com/en/java/javase/21/core/virtual-threads.html)
[2] Oracle. Exceptions Tutorial. Disponível em: [https://docs.oracle.com/javase/tutorial/essential/exceptions/](https://docs.oracle.com/javase/tutorial/essential/exceptions/)

---

[Voltar aos Links Rápidos](../../README.md#links-rapidos) | [Anterior](aula_5_3_lambdas.md) | [Próxima aula/módulo](../modulo_06_persistencia_de_dados_com_jpa_hibernate.md)
