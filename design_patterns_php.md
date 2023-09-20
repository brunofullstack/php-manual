Os **Design Patterns** (Padrões de Projeto) são soluções recorrentes para problemas comuns que surgem durante o desenvolvimento de software. Eles são modelos testados e comprovados que ajudam os desenvolvedores a resolver problemas de forma eficiente e escalável. Vou descrever alguns dos padrões de design mais comuns com exemplos em PHP:

### 1. Padrão Singleton

**Descrição**: O Padrão Singleton garante que uma classe tenha apenas uma instância e fornece um ponto de acesso global a essa instância.

**Exemplo em PHP**:

```php
class Singleton {
    private static $instance;

    private function __construct() {
        // Construtor privado para evitar a criação direta de instâncias
    }

    public static function getInstance() {
        if (self::$instance === null) {
            self::$instance = new self();
        }
        return self::$instance;
    }
}

$singleton1 = Singleton::getInstance();
$singleton2 = Singleton::getInstance();

echo $singleton1 === $singleton2 ? "Mesma instância" : "Instâncias diferentes";
```

### 2. Padrão Factory Method

**Descrição**: O Padrão Factory Method define uma interface para criar objetos, mas permite que as subclasses escolham a classe concreta a ser instanciada.

**Exemplo em PHP**:

```php
interface Animal {
    public function falar();
}

class Cachorro implements Animal {
    public function falar() {
        return "Woof!";
    }
}

class Gato implements Animal {
    public function falar() {
        return "Meow!";
    }
}

// Factory Method
function criarAnimal($tipo) {
    if ($tipo === 'cachorro') {
        return new Cachorro();
    } elseif ($tipo === 'gato') {
        return new Gato();
    }
}

$animal = criarAnimal('cachorro');
echo $animal->falar();
```

### 3. Padrão Strategy

**Descrição**: O Padrão Strategy define uma família de algoritmos, encapsula cada um deles e permite que sejam intercambiáveis. Isso permite que você escolha o algoritmo a ser usado dinamicamente.

**Exemplo em PHP**:

```php
interface EstrategiaDeOrdenacao {
    public function ordenar(array $dados): array;
}

class OrdenacaoBubble implements EstrategiaDeOrdenacao {
    public function ordenar(array $dados): array {
        // Lógica de ordenação Bubble Sort
        return $dados;
    }
}

class OrdenacaoQuick implements EstrategiaDeOrdenacao {
    public function ordenar(array $dados): array {
        // Lógica de ordenação Quick Sort
        return $dados;
    }
}

class AlgoritmoDeOrdenacao {
    private $estrategia;

    public function __construct(EstrategiaDeOrdenacao $estrategia) {
        $this->estrategia = $estrategia;
    }

    public function executar(array $dados): array {
        return $this->estrategia->ordenar($dados);
    }
}

$dados = [3, 1, 2, 5, 4];
$algoritmoBubble = new AlgoritmoDeOrdenacao(new OrdenacaoBubble());
$resultado = $algoritmoBubble->executar($dados);
```

### 4. Padrão Observer

**Descrição**: O Padrão Observer define uma relação de um-para-muitos entre objetos, de modo que quando um objeto muda de estado, todos os seus observadores são notificados e atualizados automaticamente.

**Exemplo em PHP**:

```php
class Assunto {
    private $observadores = [];

    public function adicionarObservador(Observador $observador) {
        $this->observadores[] = $observador;
    }

    public function notificarObservadores() {
        foreach ($this->observadores as $observador) {
            $observador->atualizar();
        }
    }
}

interface Observador {
    public function atualizar();
}

class ObservadorA implements Observador {
    public function atualizar() {
        echo "Observador A foi notificado e atualizou.\n";
    }
}

class ObservadorB implements Observador {
    public function atualizar() {
        echo "Observador B foi notificado e atualizou.\n";
    }
}

$assunto = new Assunto();
$assunto->adicionarObservador(new ObservadorA());
$assunto->adicionarObservador(new ObservadorB());

$assunto->notificarObservadores();
```

Estes são apenas alguns dos padrões de design comuns em PHP. Cada padrão aborda um conjunto específico de problemas e pode ser aplicado de acordo com as necessidades do seu projeto. O uso adequado de padrões de design pode melhorar a manutenção, a extensibilidade e a legibilidade do código.