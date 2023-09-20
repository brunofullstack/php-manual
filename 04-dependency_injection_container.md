# Dependency Injection Container Part 2

## 1. Dependency Injection (Injeção de Dependência)

**Dependency Injection** é um padrão de design em que as dependências de uma classe são injetadas em vez de serem criadas internamente pela classe. Isso promove um código mais flexível, testável e desacoplado. Vamos abordar esse conceito com exemplos.

## 2. Criando um Repositório de Framework

Um **Repositório de Framework** é um repositório de código que contém componentes comuns usados em vários projetos. A seguir, está um exemplo de estrutura de um repositório de framework:

```plaintext
framework-repository/
    ├── src/
    │   ├── Container.php
    │   └── ...
    ├── composer.json
    ├── README.md
```

## 3. Criando um Container (Contêiner)

Um **Container** é um componente que gerencia a criação e a resolução de dependências em um aplicativo PHP. Aqui está um exemplo simples de como criar um contêiner de injeção de dependência:

```php
// Container.php

class Container {
    private $bindings = [];

    public function bind($abstract, $concrete) {
        $this->bindings[$abstract] = $concrete;
    }

    public function resolve($abstract) {
        if (isset($this->bindings[$abstract])) {
            return $this->build($this->bindings[$abstract]);
        }

        throw new Exception("Classe não registrada no contêiner: $abstract");
    }

    private function build($concrete) {
        // Cria uma instância da classe e resolve suas dependências.
        $reflector = new ReflectionClass($concrete);
        $constructor = $reflector->getConstructor();

        if (!$constructor) {
            return new $concrete;
        }

        $parameters = $constructor->getParameters();
        $dependencies = [];

        foreach ($parameters as $parameter) {
            $dependency = $parameter->getClass();

            if ($dependency) {
                $dependencies[] = $this->resolve($dependency->name);
            } else {
                throw new Exception("Não é possível resolver a dependência: " . $parameter->name);
            }
        }

        return $reflector->newInstanceArgs($dependencies);
    }
}
```

## 4. Exceções do Contêiner

Lidar com exceções é uma parte importante do uso de um contêiner de injeção de dependência. O código acima já inclui tratamento de exceções para classes não registradas no contêiner.

## 5. Verificação de Existência no Contêiner

Você pode verificar se uma classe está registrada no contêiner usando um método `has`:

```php
public function has($abstract) {
    return isset($this->bindings[$abstract]);
}
```

## 6. Autowiring - Parte Um

O **Autowiring** é um recurso que permite ao contêiner resolver automaticamente as dependências com base nos tipos de argumentos nos construtores das classes. Aqui está um exemplo:

```php
// Container.php

class Container {
    // ...

    public function resolve($abstract) {
        if (isset($this->bindings[$abstract])) {
            return $this->build($this->bindings[$abstract]);
        }

        // Se a classe não estiver registrada, tente resolver automaticamente com autowiring.
        if (class_exists($abstract)) {
            return $this->build($abstract);
        }

        throw new Exception("Classe não registrada no contêiner: $abstract");
    }

    // ...
}
```

## 7. Autowiring - Parte Dois

Neste exemplo, se a classe não estiver registrada no contêiner, o contêiner tentará resolver automaticamente a classe com base em seu nome, se existir.

## 8. Autowiring - Parte Três

Você pode estender ainda mais o autowiring para considerar dependências de dependências. O exemplo acima só resolve diretamente as dependências. Para considerar dependências aninhadas, você precisaria de uma lógica mais complexa.

O código fornecido é simplificado para ilustrar os conceitos. Em aplicativos reais, é recomendável usar um contêiner de injeção de dependência já existente, como o Symfony Container ou o contêiner PSR-11, que oferecem funcionalidades avançadas e testadas em cenários de produção.