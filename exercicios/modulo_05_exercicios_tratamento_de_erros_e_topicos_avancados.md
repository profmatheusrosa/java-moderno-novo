# Exercícios: Tratamento de Erros e Tópicos Avançados

> Sistemas reais falham. E quando falham, precisam fazê-lo de forma controlada! 

Vamos praticar Exceções, Lambdas a fundo, Generics e as modernas Virtual Threads!

## 1. Regras de Negócio com Exceções

Lançar exceções genéricas (`Exception` ou `RuntimeException`) esconde o real motivo do erro.

**Sua missão:**
Crie uma exceção customizada `SaldoInsuficienteException` (que estende `RuntimeException`). No método `sacar` da sua classe Conta, lance essa exceção se o valor for maior que o saldo. Teste no `main` envolvendo a chamada em um bloco `try-catch`.

```java
// TODO: Criar SaldoInsuficienteException

// TODO: Atualizar Conta e testar try-catch
```

<details>
<summary>Ver gabarito</summary>

```java
class SaldoInsuficienteException extends RuntimeException {
    public SaldoInsuficienteException(String mensagem) {
        super(mensagem);
    }
}

class Conta {
    private double saldo = 100.0;
    
    public void sacar(double valor) {
        if (valor > saldo) {
            throw new SaldoInsuficienteException("Saldo de " + saldo + " não cobre saque de " + valor);
        }
        saldo -= valor;
    }
}

public class TesteErro {
    public static void main(String[] args) {
        Conta c = new Conta();
        try {
            c.sacar(200.0);
        } catch (SaldoInsuficienteException e) {
            System.err.println("Erro ao sacar: " + e.getMessage());
        }
    }
}
```
</details>

## 2. A Pilha Genérica (Generics)

Generics (os famosos `<T>`) permitem criar classes que funcionam de forma segura com qualquer tipo de dado!

**Sua missão:**
Crie uma classe genérica `Pilha<T>`. Ela deve ter um método `push(T item)` e um método `pop()` que retorna um `T`. Você pode usar um `ArrayList` internamente para guardar os itens.

```java
// TODO: public class Pilha<T> { ... }
```

<details>
<summary>Ver gabarito</summary>

```java
import java.util.ArrayList;
import java.util.List;

public class Pilha<T> {
    private List<T> elementos = new ArrayList<>();
    
    public void push(T item) {
        elementos.add(item);
    }
    
    public T pop() {
        if (elementos.isEmpty()) {
            throw new IllegalStateException("Pilha vazia");
        }
        return elementos.remove(elementos.size() - 1);
    }
}

// Teste
// Pilha<String> pNomes = new Pilha<>();
// pNomes.push("Java");
// String topo = pNomes.pop();
```
</details>

## 3. O Poder das Method References

Method references (`Classe::metodo`) deixam lambdas ainda mais curtas.

**Sua missão:**
Refatore o código imperativo abaixo para usar `forEach` de Listas, passando um method reference de `System.out.println`.

```java
import java.util.List;

public class MethodRef {
    public static void main(String[] args) {
        List<String> frutas = List.of("Maçã", "Banana", "Uva");
        
        // TODO: Refatore este for:
        for (String fruta : frutas) {
            System.out.println(fruta);
        }
    }
}
```

<details>
<summary>Ver gabarito</summary>

```java
import java.util.List;

public class MethodRef {
    public static void main(String[] args) {
        List<String> frutas = List.of("Maçã", "Banana", "Uva");
        
        // Refatorado com Method Reference
        frutas.forEach(System.out::println);
    }
}
```
</details>

## 4. O Futuro da Concorrência: Virtual Threads (Java 21)

As Virtual Threads do Project Loom são extremamente leves, permitindo criar milhões de threads sem derrubar sua máquina.

**Sua missão:**
Crie 10.000 Virtual Threads em um loop. Cada uma deve apenas dormir (`Thread.sleep(1000)`) por 1 segundo e depois imprimir o nome da thread. Use `Thread.ofVirtual().start(...)` ou `Executors.newVirtualThreadPerTaskExecutor()`.

```java
// TODO: Executar 10.000 Virtual Threads
```

<details>
<summary>Ver gabarito</summary>

```java
import java.util.concurrent.Executors;

public class VirtualThreadDemo {
    public static void main(String[] args) {
        // Usando Executor Service focado em Virtual Threads (Java 21+)
        try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
            
            for (int i = 0; i < 10_000; i++) {
                final int id = i;
                executor.submit(() -> {
                    try {
                        Thread.sleep(1000);
                        System.out.println("Tarefa " + id + " executada na " + Thread.currentThread());
                    } catch (InterruptedException e) {
                        Thread.currentThread().interrupt();
                    }
                });
            }
        } // O try-with-resources espera todas finalizarem
        System.out.println("Todas finalizaram (em quase 1 segundo!).");
    }
}
```
</details>

---

##  Desafio Bônus: Cache Thread-Safe (Supplier + ConcurrentHashMap)

**Sua missão:**
Implemente uma classe `CacheLocal` usando `ConcurrentHashMap<String, String>`. Crie o método `String obter(String chave, Supplier<String> computacaoPesada)`. Se a chave existir, retorne do mapa. Se não, execute o *Supplier* simulando um cálculo pesado, salve no mapa e retorne. Você pode usar o método `.computeIfAbsent` do Map!

<details>
<summary>Ver gabarito</summary>

```java
import java.util.concurrent.ConcurrentHashMap;
import java.util.function.Supplier;

public class CacheLocal {
    private final ConcurrentHashMap<String, String> cache = new ConcurrentHashMap<>();

    public String obter(String chave, Supplier<String> computacaoPesada) {
        return cache.computeIfAbsent(chave, k -> computacaoPesada.get());
    }

    public static void main(String[] args) {
        CacheLocal meuCache = new CacheLocal();

        Supplier<String> calculo = () -> {
            System.out.println("Processando algo pesado no banco de dados...");
            try { Thread.sleep(2000); } catch (Exception e) {}
            return "Resultado Caro!";
        };

        // Primeira chamada (Demora e calcula)
        System.out.println(meuCache.obter("relatorio_x", calculo));
        
        // Segunda chamada (Rápida, pega do cache)
        System.out.println(meuCache.obter("relatorio_x", calculo));
    }
}
```
</details>

---

[Voltar aos Links Rápidos](../README.md#links-rapidos)
