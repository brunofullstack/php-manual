# Database Abstraction Layer e Console Application

## 1. Database Abstraction Layer (Camada de Abstração de Banco de Dados)

Uma **Database Abstraction Layer** é uma camada de abstração que simplifica e padroniza o acesso a bancos de dados em PHP. Vamos abordar este tópico com exemplos usando o PDO (PHP Data Objects).

```php
// Exemplo de conexão usando PDO

try {
    $pdo = new PDO('mysql:host=localhost;dbname=meu_banco', 'usuario', 'senha');
    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
} catch (PDOException $e) {
    die('Erro na conexão: ' . $e->getMessage());
}
```

Aqui, criamos uma conexão PDO com um banco de dados MySQL.

## 2. Migration Files (Arquivos de Migração)

**Migration Files** são arquivos que definem as mudanças no esquema do banco de dados ao longo do tempo. Eles permitem que você versione e aplique essas mudanças de forma controlada. Um exemplo de estrutura de arquivo de migração:

```plaintext
migrations/
    ├── 20210920000001_create_users_table.php
    ├── 20210920000002_create_posts_table.php
    └── ...
```

## 3. Entrypoint da Aplicação de Console

O **Entrypoint da Aplicação de Console** é um arquivo que serve como ponto de entrada para a execução de comandos da linha de comando. Um exemplo de entrada de aplicação de console:

```php
// console.php

require_once 'vendor/autoload.php';

use Symfony\Component\Console\Application;

$application = new Application();

// Registre os comandos aqui...

$application->run();
```

## 4. Classes de Comando

As **Classes de Comando** são classes que definem os comandos específicos que podem ser executados na linha de comando. Exemplo de uma classe de comando:

```php
// CreateUserCommand.php

use Symfony\Component\Console\Command\Command;
use Symfony\Component\Console\Input\InputInterface;
use Symfony\Component\Console\Output\OutputInterface;

class CreateUserCommand extends Command {
    protected function configure() {
        $this->setName('user:create')
             ->setDescription('Cria um novo usuário');
    }

    protected function execute(InputInterface $input, OutputInterface $output) {
        // Lógica para criar um novo usuário...
        $output->writeln('Usuário criado com sucesso!');
    }
}
```

## 5. Registrando Comandos

Para registrar comandos em sua aplicação de console, você os adiciona ao objeto da aplicação no Entrypoint:

```php
// console.php

$application->add(new CreateUserCommand());
```

## 6. Executando a Aplicação de Console

Você pode executar sua aplicação de console a partir da linha de comando usando o seguinte comando:

```bash
php console.php
```

Isso listará todos os comandos disponíveis.

## 7. Executando um Comando de Console

Para executar um comando específico, use o nome do comando após `php console.php`:

```bash
php console.php user:create
```

## 8. Opções de Comando

Os comandos podem aceitar opções personalizadas. Por exemplo, adicione uma opção `--name` ao comando `user:create`:

```php
// CreateUserCommand.php

$this->addOption('name', null, InputOption::VALUE_REQUIRED, 'Nome do usuário');
```

Agora você pode passar `--name` ao executar o comando e acessar o valor na lógica do comando.

Esses são os conceitos básicos de uma aplicação de console em PHP usando uma camada de abstração de banco de dados e comandos personalizados. Aplicações de console são úteis para tarefas de automação e administração de sistemas, bem como para criar ferramentas de linha de comando para suas aplicações web.