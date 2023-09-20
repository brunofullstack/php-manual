# Routing and Controllers

## 1. Routing (Roteamento)

**Roteamento** é o processo de direcionar solicitações HTTP para as ações ou controladores apropriados em seu aplicativo PHP com base na URL da solicitação. Vamos explorar o uso do **FastRoute Router** para criar rotas em seu aplicativo.

## 2. FastRoute Router

O **FastRoute Router** é uma biblioteca eficiente para gerenciar roteamento em aplicativos PHP. Você pode instalá-lo usando o Composer:

```bash
composer require nikic/fast-route
```

## 3. Adicionando Rotas

Aqui está um exemplo de como adicionar rotas usando o FastRoute Router:

```php
// index.php

require_once 'vendor/autoload.php';

use FastRoute\RouteCollector;

$dispatcher = FastRoute\simpleDispatcher(function (RouteCollector $r) {
    $r->addRoute('GET', '/', 'HomeController');
    $r->addRoute('GET', '/produto/{id:\d+}', 'ProdutoController');
    $r->addRoute('POST', '/criar-produto', 'ProdutoController@criar');
});
```

Neste exemplo, definimos rotas para as páginas inicial, de detalhes do produto e uma rota POST para criar um novo produto.

## 4. Recuperando Informações da URL

Agora, vamos ver como recuperar informações da URL em seus controladores:

```php
// ProdutoController.php

class ProdutoController {
    public function show($params) {
        $id = $params['id'];
        // Lógica para exibir os detalhes do produto com base no $id.
    }

    public function criar($params) {
        // Lógica para criar um novo produto com base nos dados do POST.
    }
}
```

## 5. Definindo Rotas Flexíveis

Você pode definir rotas flexíveis usando placeholders na URL. No exemplo anterior, usamos `{id:\d+}` para aceitar somente números inteiros na URL. Você pode definir placeholders personalizados para atender às suas necessidades.

## 6. Manipulando Exceções

Para melhorar o tratamento de erros de roteamento, você pode adicionar uma lógica para lidar com exceções, como quando uma rota não é encontrada:

```php
// index.php

$dispatcher = FastRoute\simpleDispatcher(function (RouteCollector $r) {
    // Rotas definidas aqui...
});

$httpMethod = $_SERVER['REQUEST_METHOD'];
$uri = $_SERVER['REQUEST_URI'];

$routeInfo = $dispatcher->dispatch($httpMethod, $uri);

switch ($routeInfo[0]) {
    case FastRoute\Dispatcher::NOT_FOUND:
        // Rota não encontrada - Lógica de tratamento de erro.
        break;
    case FastRoute\Dispatcher::METHOD_NOT_ALLOWED:
        // Método HTTP não permitido - Lógica de tratamento de erro.
        break;
    case FastRoute\Dispatcher::FOUND:
        // Rota encontrada - Execute a ação apropriada.
        break;
}
```

Agora você pode implementar um tratamento adequado para lidar com exceções ou erros de roteamento.

## 7. Quando Criar um Repositório

A decisão de criar um repositório de código (por exemplo, no GitHub) depende de vários fatores, como o desejo de compartilhar seu código com outros desenvolvedores, facilitar a colaboração, controlar versões e muito mais. Normalmente, você pode considerar criar um repositório quando:

- Seu código atinge um estado estável e usável.
- Você deseja colaborar com outros desenvolvedores.
- Deseja rastrear alterações e versões do código.
- Deseja tornar seu código acessível publicamente.

No entanto, criar um repositório é uma decisão pessoal e depende dos objetivos de seu projeto e de suas preferências de desenvolvimento.

Lembre-se de que os exemplos acima são simplificados para fins didáticos. Em aplicativos web do mundo real, você geralmente usará um framework PHP completo, como Laravel ou Symfony, que oferece funcionalidades avançadas de roteamento e abstrai muitos detalhes técnicos.