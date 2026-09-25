# Exercícios: Orientação a Objetos

> O mundo real é cheio de objetos interagindo. A Orientação a Objetos no Java te ajuda a representar esse mundo em código!

Vamos praticar os pilares da POO: Encapsulamento, Herança e Polimorfismo.

## 1. O Cofre Forte (Encapsulamento)

Proteger o estado de um objeto impede que ele fique inconsistente (como um saldo negativo inexplicável).

**Sua missão:**
Crie a classe `ContaBancaria` com atributo saldo privado. Crie métodos `depositar`, `sacar` (com validação para não sacar mais do que tem) e `getSaldo`.

```java
public class ContaBancaria {
    // TODO: Declarar saldo privado
    
    // TODO: Criar método depositar(double valor)
    
    // TODO: Criar método sacar(double valor) que retorna true se deu certo
    
    // TODO: Criar getter para saldo
}
```

<details>
<summary>Ver gabarito</summary>

```java
public class ContaBancaria {
    private double saldo = 0.0;
    
    public void depositar(double valor) {
        if (valor > 0) {
            saldo += valor;
        }
    }
    
    public boolean sacar(double valor) {
        if (valor > 0 && saldo >= valor) {
            saldo -= valor;
            return true;
        }
        return false;
    }
    
    public double getSaldo() {
        return saldo;
    }
}
```
</details>

## 2. Família Motorizada (Herança)

Herança ajuda a evitar código duplicado para conceitos similares.

**Sua missão:**
Crie uma classe base `Veiculo` com método `acelerar()`. Crie subclasses `Carro`, `Moto` e `Caminhao` que estendem `Veiculo` e sobrescrevam (`@Override`) o método `acelerar()`.

```java
class Veiculo {
    public void acelerar() {
        System.out.println("O veículo está acelerando!");
    }
}

// TODO: Criar Carro, Moto e Caminhao estendendo Veiculo
```

<details>
<summary>Ver gabarito</summary>

```java
class Veiculo {
    public void acelerar() {
        System.out.println("O veículo está acelerando!");
    }
}

class Carro extends Veiculo {
    @Override
    public void acelerar() {
        System.out.println("O carro acelera: Vrum vrum!");
    }
}

class Moto extends Veiculo {
    @Override
    public void acelerar() {
        System.out.println("A moto acelera: Randandandan!");
    }
}

class Caminhao extends Veiculo {
    @Override
    public void acelerar() {
        System.out.println("O caminhão acelera lentamente: POMMM!");
    }
}
```
</details>

## 3. O Contrato de Desconto (Interfaces)

Interfaces garantem que classes diferentes implementem um mesmo comportamento (um contrato).

**Sua missão:**
Crie a interface `Descontavel` com o método `double calcularDesconto(double valor)`. Implemente-a em classes `ClienteVIP` (15% de desconto) e `ClienteComum` (sem desconto).

```java
// TODO: Criar interface Descontavel

// TODO: Criar classes ClienteVIP e ClienteComum
```

<details>
<summary>Ver gabarito</summary>

```java
interface Descontavel {
    double calcularDesconto(double valor);
}

class ClienteVIP implements Descontavel {
    @Override
    public double calcularDesconto(double valor) {
        return valor * 0.15;
    }
}

class ClienteComum implements Descontavel {
    @Override
    public double calcularDesconto(double valor) {
        return 0.0; // Sem desconto
    }
}
```
</details>

## 4. O Coral de Animais (Polimorfismo)

Polimorfismo permite tratar objetos de tipos diferentes de forma genérica!

**Sua missão:**
Crie uma classe abstrata `Animal` com método abstrato `emitirSom()`. Implemente `Cachorro` e `Gato`. Na main, crie uma lista de `Animal`, adicione ambos e faça um laço for chamando `emitirSom()` de cada um.

```java
import java.util.List;

// TODO: Implementar classes

public class MainAnimais {
    public static void main(String[] args) {
        // TODO: Criar lista, adicionar animais e iterar emitindo som
    }
}
```

<details>
<summary>Ver gabarito</summary>

```java
import java.util.List;

abstract class Animal {
    public abstract void emitirSom();
}

class Cachorro extends Animal {
    @Override
    public void emitirSom() {
        System.out.println("Au au!");
    }
}

class Gato extends Animal {
    @Override
    public void emitirSom() {
        System.out.println("Miau!");
    }
}

public class MainAnimais {
    public static void main(String[] args) {
        List<Animal> animais = List.of(new Cachorro(), new Gato());
        
        for (Animal animal : animais) {
            animal.emitirSom(); // Polimorfismo agindo aqui!
        }
    }
}
```
</details>

---

##  Desafio Bônus: Ordenando Produtos

O Java usa a interface `Comparable<T>` para saber como ordenar listas de objetos customizados.

**Sua missão:**
Crie uma classe `Produto` com `nome` e `preco`. Implemente `Comparable<Produto>` de forma que produtos sejam ordenados do mais barato para o mais caro. Crie uma lista, ordene com `Collections.sort()` (ou `lista.sort(null)`) e imprima.

<details>
<summary>Ver gabarito</summary>

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

class Produto implements Comparable<Produto> {
    String nome;
    double preco;

    public Produto(String nome, double preco) {
        this.nome = nome;
        this.preco = preco;
    }

    @Override
    public int compareTo(Produto outro) {
        return Double.compare(this.preco, outro.preco);
    }

    @Override
    public String toString() {
        return nome + " ($" + preco + ")";
    }
}

public class DesafioOrdenacao {
    public static void main(String[] args) {
        List<Produto> lista = new ArrayList<>();
        lista.add(new Produto("Notebook", 3500.0));
        lista.add(new Produto("Mouse", 50.0));
        lista.add(new Produto("Teclado", 150.0));

        Collections.sort(lista);

        System.out.println(lista); 
        // Saída esperada: [Mouse ($50.0), Teclado ($150.0), Notebook ($3500.0)]
    }
}
```
</details>

---

[Voltar aos Links Rápidos](../README.md#links-rapidos)
