# Módulo 02: Orientação a Objetos
> A base de tudo no Java: entenda como modelar o mundo real com código e organizar suas ideias.

## O que você vai aprender neste módulo

Neste módulo, vamos mergulhar fundo no paradigma que dá vida ao Java: a Orientação a Objetos (OO). Você aprenderá a pensar em forma de objetos, descobrindo como:
- Criar suas próprias classes e objetos (o DNA do seu código).
- Proteger dados sensíveis usando Encapsulamento.
- Reutilizar código de forma inteligente com Herança.
- Programar de maneira flexível e extensível através do Polimorfismo.
- Criar contratos e modelos reutilizáveis usando Interfaces e Classes Abstratas.

## Sumário

- [Aula 2.1: Classes e Objetos](#aula-21-classes-e-objetos-o-dna-do-seu-programa)
- [Aula 2.2: Encapsulamento](#aula-22-encapsulamento-protegendo-seus-dados)
- [Aula 2.3: Herança](#aula-23-heranca-reutilizando-codigo-com-inteligencia)
- [Aula 2.4: Polimorfismo](#aula-24-polimorfismo-multiplas-formas-uma-interface)
- [Aula 2.5: Interfaces e Classes Abstratas](#aula-25-interfaces-e-classes-abstratas-contratos-e-modelos)
- [Exercícios de Fixação](#exercicios-de-fixacao)

---

## Aula 2.1: Classes e Objetos: O DNA do seu Programa

![Pilares da Orientação a Objetos](../imagens/pilares_oop.jpg)
*Figura 1: Representação visual dos 4 pilares da Orientação a Objetos.*


Na programação orientada a objetos (OOP) [1], tudo gira em torno de **classes** e **objetos**. Eles são os blocos fundamentais para construir programas que modelam o mundo real de forma mais intuitiva e organizada.

### O que é uma Classe?

Uma **classe** é como um projeto, um molde ou uma planta para criar objetos. Ela define as características (atributos) e os comportamentos (métodos) que os objetos desse tipo terão. Uma classe não é um objeto em si, mas sim a descrição de como um objeto deve ser.

Dica: **Pense em um carro.** A "Classe Carro" definiria que todo carro tem:
*   **Atributos**: cor, marca, modelo, ano, velocidade atual.
*   **Métodos**: ligar, desligar, acelerar, frear.

```java
// Definição da Classe Carro
public class Carro {
    // Atributos (variáveis de instância)
    String cor;
    String marca;
    String modelo;
    int ano;
    double velocidadeAtual;

    // Métodos (comportamentos)
    public void ligar() {
        System.out.println("O carro " + modelo + " está ligado.");
    }

    public void acelerar(double incremento) {
        velocidadeAtual += incremento;
        System.out.println("Acelerando. Velocidade atual: " + velocidadeAtual + " km/h.");
    }

    public void frear(double decremento) {
        velocidadeAtual -= decremento;
        if (velocidadeAtual < 0) {
            velocidadeAtual = 0;
        }
        System.out.println("Freando. Velocidade atual: " + velocidadeAtual + " km/h.");
    }

    public void desligar() {
        velocidadeAtual = 0;
        System.out.println("O carro " + modelo + " está desligado.");
    }
}
```

### O que é um Objeto?

Um **objeto** é uma instância concreta de uma classe. É a materialização do projeto. Você pode criar vários objetos a partir da mesma classe, e cada objeto terá seus próprios valores para os atributos, mas compartilharão os mesmos métodos.

Continuando com o exemplo do carro:
*   Um objeto seria "Meu Carro Azul" (uma instância específica da Classe Carro).
*   Outro objeto seria "Carro Vermelho do Vizinho" (outra instância).

```java
public class TesteCarro {
    public static void main(String[] args) {
        // Criando um objeto (instância) da Classe Carro
        Carro meuCarro = new Carro();

        // Atribuindo valores aos atributos do objeto meuCarro
        meuCarro.cor = "Azul";
        meuCarro.marca = "Toyota";
        meuCarro.modelo = "Corolla";
        meuCarro.ano = 2022;
        meuCarro.velocidadeAtual = 0;

        // Acessando atributos e chamando métodos do objeto meuCarro
        System.out.println("Meu carro é um " + meuCarro.marca + " " + meuCarro.modelo + " de cor " + meuCarro.cor);
        meuCarro.ligar();
        meuCarro.acelerar(50);
        meuCarro.frear(10);
        meuCarro.desligar();
        
        // Saída:
        // Meu carro é um Toyota Corolla de cor Azul
        // O carro Corolla está ligado.
        // Acelerando. Velocidade atual: 50.0 km/h.
        // Freando. Velocidade atual: 40.0 km/h.
        // O carro Corolla está desligado.
    }
}
```

### Construtores

Um **construtor** é um método especial usado para inicializar um objeto recém-criado. Ele tem o mesmo nome da classe e não possui tipo de retorno (nem mesmo `void`). Se você não definir nenhum construtor, Java fornece um construtor padrão (sem argumentos) implicitamente.

Boa Prática: Defina construtores para garantir que seus objetos nasçam com todos os dados essenciais preenchidos.

```java
public class CarroComConstrutor {
    String cor;
    String marca;
    String modelo;
    int ano;
    double velocidadeAtual;

    // Construtor com argumentos para inicializar os atributos
    public CarroComConstrutor(String cor, String marca, String modelo, int ano) {
        this.cor = cor; // 'this' refere-se ao atributo da instância
        this.marca = marca;
        this.modelo = modelo;
        this.ano = ano;
        this.velocidadeAtual = 0; // Atributo não passado, inicializado por padrão
    }

    public void ligar() {
        System.out.println("O carro " + modelo + " está ligado.");
    }

    public static void main(String[] args) {
        // Usando o construtor com argumentos
        CarroComConstrutor novoCarro = new CarroComConstrutor("Preto", "Ford", "Ka", 2020);
        System.out.println("Novo carro: " + novoCarro.marca + " " + novoCarro.modelo + " de cor " + novoCarro.cor);
        novoCarro.ligar();
    }
}
```

Agora que sabemos criar classes e instanciar objetos, precisamos entender que deixar atributos livres para modificação (como fizemos acima) pode ser perigoso. É aí que entra o Encapsulamento.

---

## Aula 2.2: Encapsulamento: Protegendo seus Dados 

O **encapsulamento** é um dos quatro pilares da Programação Orientada a Objetos (junto com Herança, Polimorfismo e Abstração). Ele se refere à prática de agrupar dados (atributos) e os métodos que operam nesses dados dentro de uma única unidade (a classe), e de restringir o acesso direto a alguns dos componentes do objeto. Em outras palavras, o encapsulamento é sobre **esconder os detalhes internos** de uma classe e expor apenas o que é necessário para interagir com ela.

### Por que Encapsular?

*   **Segurança dos Dados**: Impede que dados internos de um objeto sejam acessados ou modificados de forma inadequada por código externo.
*   **Controle de Acesso**: Permite que a classe controle como seus dados são lidos e modificados, aplicando validações.
*   **Manutenção e Flexibilidade**: Se a implementação interna mudar, o código externo não quebra, desde que a interface pública permaneça igual.

### Modificadores de Acesso

Java oferece modificadores de acesso para controlar a visibilidade:

*   **`private`**: O membro é acessível apenas dentro da própria classe. É o mais usado para atributos.
*   **`default` (package-private)**: Acessível apenas dentro do mesmo pacote (sem escrever nada).
*   **`protected`**: Acessível dentro do mesmo pacote e por subclasses (mesmo em pacotes diferentes).
*   **`public`**: Acessível de qualquer lugar. Comum para os métodos que a classe expõe.

### Getters e Setters

Para interagir com atributos `private`, usamos métodos especiais:
*   **Getters**: Para ler o valor (ex: `getIdade()`).
*   **Setters**: Para alterar o valor (ex: `setIdade(int novaIdade)`), geralmente com regras de validação.

```java
public class ContaBancaria {
    private String numeroConta; // Protegido!
    private double saldo;       // Protegido!
    private String titular;     // Protegido!

    public ContaBancaria(String numeroConta, String titular) {
        this.numeroConta = numeroConta;
        this.titular = titular;
        this.saldo = 0.0; // Saldo inicial sempre zero
    }

    // Apenas leitura, não queremos que alterem o número
    public String getNumeroConta() {
        return numeroConta;
    }

    // Apenas leitura do saldo
    public double getSaldo() {
        return saldo;
    }

    public String getTitular() {
        return titular;
    }

    // Lógica controlada de depósito
    public void depositar(double valor) {
        if (valor > 0) {
            this.saldo += valor;
            System.out.println("Depósito realizado. Novo saldo: R$" + this.saldo);
        } else {
            System.out.println("Valor de depósito inválido.");
        }
    }

    // Lógica controlada de saque
    public void sacar(double valor) {
        if (valor > 0 && valor <= this.saldo) {
            this.saldo -= valor;
            System.out.println("Saque realizado. Novo saldo: R$" + this.saldo);
        } else {
            System.out.println("Saldo insuficiente ou valor de saque inválido.");
        }
    }
}

public class TesteContaBancaria {
    public static void main(String[] args) {
        ContaBancaria minhaConta = new ContaBancaria("12345-6", "Ana Silva");

        minhaConta.depositar(1000.0);
        minhaConta.sacar(200.0);
        
        // minhaConta.saldo = 5000.0; // Atencao: ERRO DE COMPILAÇÃO: atributo saldo é private!
    }
}
```

Com nossos dados protegidos, podemos pensar em como não repetir código ao criar classes semelhantes. Para isso, usamos a Herança.

---

## Aula 2.3: Herança: Reutilizando Código com Inteligência 

A **herança** permite [2] que uma classe (**subclasse** ou classe filha) herde características (atributos) e comportamentos (métodos) de outra classe (**superclasse** ou classe pai). Isso modela relações do tipo "é um/uma" (e.g., "Um Carro é um Veículo").

### O Conceito de Herança

Imagine que você tem uma classe `Animal` com `nome`, `idade`, `comer()` e `dormir()`. Em vez de reescrever isso para `Cachorro` ou `Gato`, você faz com que eles herdem de `Animal` usando a palavra-chave `extends`.

```java
// Superclasse (Classe Pai)
class Animal {
    String nome;
    int idade;

    public Animal(String nome, int idade) {
        this.nome = nome;
        this.idade = idade;
    }

    public void comer() {
        System.out.println(nome + " está comendo.");
    }
}

// Subclasse (Classe Filha)
class Cachorro extends Animal {
    String raca;

    public Cachorro(String nome, int idade, String raca) {
        // 'super' chama o construtor da classe pai (Animal)
        super(nome, idade); 
        this.raca = raca;
    }

    public void latir() {
        System.out.println(nome + " está latindo: Au Au!");
    }
}
```

### Sobrescrita de Métodos (@Override)

Uma subclasse pode fornecer sua própria implementação para um método que já é definido na superclasse. Isso é a **sobrescrita de método**. Usamos a anotação `@Override` para segurança.

```java
class Gato extends Animal {
    public Gato(String nome, int idade) {
        super(nome, idade);
    }

    // Modificando o comportamento da classe pai
    @Override 
    public void comer() {
        System.out.println(nome + " (o gato) está comendo ração de peixe bem devagar.");
    }
}
```

A herança nos leva diretamente ao próximo pilar: o Polimorfismo, onde a sobrescrita brilha de verdade.

---

## Aula 2.4: Polimorfismo: Múltiplas Formas, Uma Interface 

O **polimorfismo** ("muitas formas") permite que objetos de diferentes classes sejam tratados de forma genérica como objetos de um tipo comum. Um único nome de método pode ter diferentes implementações dependendo do objeto que o invoca.

### O Conceito de Polimorfismo

Imagine uma lista de `Animal`. Cada animal faz um som diferente. Com polimorfismo, você chama `fazerSom()` no `Animal`, e o Java sabe qual versão executar (a do Gato, Cachorro, etc.). Isso se chama **Dynamic Method Dispatch** (resolvido em tempo de execução).

```java
class AnimalPolimorfico {
    String nome;
    public AnimalPolimorfico(String nome) { this.nome = nome; }
    
    public void fazerSom() {
        System.out.println(nome + " faz algum som genérico.");
    }
}

class Pato extends AnimalPolimorfico {
    public Pato(String nome) { super(nome); }

    @Override
    public void fazerSom() {
        System.out.println(nome + " está grasnando: Quack Quack!");
    }
}

public class TestePolimorfismo {
    public static void main(String[] args) {
        // Array de Animais
        AnimalPolimorfico[] animais = new AnimalPolimorfico[2];
        animais[0] = new AnimalPolimorfico("Bicho Genérico");
        animais[1] = new Pato("Pato Donald"); // Pato "é um" Animal

        for (AnimalPolimorfico a : animais) {
            a.fazerSom(); 
            // O Java decide qual método chamar com base no tipo real do objeto instanciado!
        }
        
        // Saída:
        // Bicho Genérico faz algum som genérico.
        // Pato Donald está grasnando: Quack Quack!
    }
}
```

Dica: **Vantagem:** Se você adicionar uma classe `Vaca` amanhã, você não precisa mudar o código que faz os animais emitirem som. É puro ganho de flexibilidade!

Mas e se não quisermos que a classe genérica (`AnimalPolimorfico`) possa ser instanciada, já que não existe um "animal genérico" na vida real? É para isso que usamos Classes Abstratas e Interfaces.

---

## Aula 2.5: Interfaces e Classes Abstratas: Contratos e Modelos 

A **abstração** foca nas características essenciais de um objeto, ignorando detalhes irrelevantes. 

### Classes Abstratas: Modelos Incompletos

Uma **classe abstrata** (marcada com `abstract`) não pode ser instanciada diretamente. Ela serve como modelo. Ela pode ter métodos normais (com código) e métodos abstratos (sem código, que as filhas são obrigadas a implementar).

```java
abstract class Veiculo {
    String marca;

    public Veiculo(String marca) {
        this.marca = marca;
    }

    public void exibirDetalhes() {
        System.out.println("Marca: " + marca);
    }

    // Método sem corpo. Quem herdar DEVE implementar!
    public abstract void ligar(); 
}

class Moto extends Veiculo {
    public Moto(String marca) {
        super(marca);
    }

    @Override
    public void ligar() {
        System.out.println("A moto " + marca + " ligou apertando o botão.");
    }
}
```

### Interfaces: Contratos Puros

Uma **interface** é um contrato absoluto. Ela diz "o que" a classe faz, não "como". Uma classe pode implementar várias interfaces (diferente da herança, onde só pode ter um pai).

```java
interface Ligavel {
    void ligar();    // É implicitamente public abstract
    void desligar(); 

    // A partir do Java 8, interfaces podem ter métodos default (com corpo)
    default void status() {
        System.out.println("Status: Operando.");
    }
}

class Computador implements Ligavel {
    @Override
    public void ligar() {
        System.out.println("Computador iniciando...");
    }

    @Override
    public void desligar() {
        System.out.println("Computador desligando...");
    }
}
```

Regra de ouro: Use Classes Abstratas quando houver forte relação "É UM" (Moto é um Veículo). Use Interfaces para relação "TEM COMPORTAMENTO" (Computador é Ligável, Lâmpada é Ligável - eles não têm a mesma família, mas têm o mesmo comportamento).

---

## Exercícios de Fixação

**Exercício 2:** Qual dos seguintes conceitos melhor descreve a prática de esconder os detalhes internos de uma classe e proteger seus atributos através de modificadores de acesso?
a) Polimorfismo

b) Herança

c) Encapsulamento

d) Abstração

<details>
<summary>Ver Resposta</summary>

**Resposta:** c) Encapsulamento

**Explicação:** O encapsulamento tem o objetivo de ocultar os detalhes de implementação de uma classe e proteger o estado do objeto (atributos), fornecendo métodos específicos (getters e setters) para acesso controlado.
</details>

**Exercício 2.1:** Quando uma subclasse fornece sua própria implementação para um método que já foi definido em sua superclasse, este processo é conhecido como:
a) Sobrecarga (Overloading)

b) Sobrescrita (Overriding)

c) Ocultação de informações (Information Hiding)

d) Instanciação

<details>
<summary>Ver Resposta</summary>

**Resposta:** b) Sobrescrita (Overriding)

**Explicação:** A sobrescrita de métodos (@Override) permite que a classe filha altere o comportamento herdado da classe pai, o que é fundamental para o polimorfismo.
</details>

<details>
<summary><strong>Exercício Prático 1: Modelando com Encapsulamento</strong></summary>

Crie uma classe `Produto` com os atributos privados `nome` (String), `preco` (double) e `quantidadeEmEstoque` (int). 
Crie os getters e setters, mas não permita que o preço seja negativo, nem que a quantidade fique menor que zero.

**Gabarito:**
```java
public class Produto {
    private String nome;
    private double preco;
    private int quantidadeEmEstoque;

    public Produto(String nome, double preco, int quantidade) {
        this.nome = nome;
        setPreco(preco);
        setQuantidadeEmEstoque(quantidade);
    }

    // Getters
    public String getNome() { return nome; }
    public double getPreco() { return preco; }
    public int getQuantidadeEmEstoque() { return quantidadeEmEstoque; }

    // Setters com validação
    public void setNome(String nome) { this.nome = nome; }
    
    public void setPreco(double preco) {
        if(preco >= 0) this.preco = preco;
    }
    
    public void setQuantidadeEmEstoque(int qtd) {
        if(qtd >= 0) this.quantidadeEmEstoque = qtd;
    }
}
```
</details>

<details>
<summary><strong>Exercício Prático 2: Herança e Polimorfismo</strong></summary>

Crie uma classe base `Funcionario` com o método `calcularSalario()` retornando `2000.0`. Crie a classe `Gerente` que herda de `Funcionario` e sobrescreve o método `calcularSalario()` para retornar o salário base mais 1500.0 de bônus.

**Gabarito:**
```java
class Funcionario {
    public double calcularSalario() {
        return 2000.0;
    }
}

class Gerente extends Funcionario {
    @Override
    public double calcularSalario() {
        return super.calcularSalario() + 1500.0; // Reutiliza a lógica do pai
    }
}
```
</details>

<details>
<summary><strong>Exercício Prático 3: Interfaces</strong></summary>

Crie uma interface `Pagavel` com um método `processarPagamento(double valor)`. Faça uma classe `CartaoCredito` e outra `Pix` implementarem essa interface, imprimindo lógicas diferentes.

**Gabarito:**
```java
interface Pagavel {
    void processarPagamento(double valor);
}

class CartaoCredito implements Pagavel {
    @Override
    public void processarPagamento(double valor) {
        System.out.println("Pagamento de R$" + valor + " processado no crédito (taxa de 2%).");
    }
}

class Pix implements Pagavel {
    @Override
    public void processarPagamento(double valor) {
        System.out.println("Pagamento de R$" + valor + " processado no PIX na hora.");
    }
}
```
</details>

## Referências

[1] Oracle. Object-Oriented Concepts. Disponível em: [https://docs.oracle.com/javase/tutorial/java/concepts/](https://docs.oracle.com/javase/tutorial/java/concepts/)
[2] Oracle. Inheritance. Disponível em: [https://docs.oracle.com/javase/tutorial/java/IandI/subclasses.html](https://docs.oracle.com/javase/tutorial/java/IandI/subclasses.html)

---

[Voltar aos Links Rápidos](../README.md#links-rapidos) | [Próximo módulo: Módulo 03 — Recursos Modernos do Java](./modulo_03_recursos_modernos_do_java.md)

