# Route Middleware - Abordando Tópicos de Middleware de Rota, Injeção de Middleware, Refatoração do Roteador, Middleware de Autenticação, Middleware de Visitante, Conclusão do Registro, Link de Logout e Tratamento de Logout

Vamos abordar os tópicos de Middleware de Rota, Injeção de Middleware, Refatoração do Roteador, Middleware de Autenticação, Middleware de Visitante, Conclusão do Registro, Link de Logout e Tratamento de Logout em um contexto de aplicativo web PHP.

## 1. Middleware de Rota

O **Middleware de Rota** é usado para aplicar ações antes ou após o processamento de uma rota específica. Por exemplo, você pode usar middleware para autenticação, verificação de permissões ou registro de atividades. Aqui está um exemplo de middleware de rota:

```php
// Middleware de Autenticação
function authenticateMiddleware($request, $next) {
    if (!isLoggedIn()) {
        // Redirecionar para a página de login
        header('Location: /login');
        exit;
    }

    return $next($request);
}
```

## 2. Injeção de Middleware

Você pode injetar middleware em rotas específicas para executar ações antes ou após o processamento da rota. Por exemplo, no contexto de um framework ou sistema de roteamento, você pode definir middleware em uma rota específica. Exemplo:

```php
$route->middleware('authenticate')->get('/dashboard', 'DashboardController@index');
```

Isso significa que o middleware `authenticate` será executado antes do método `index` do `DashboardController`.

## 3. Refatoração do Roteador

Para tornar o sistema de roteamento mais flexível, é comum refatorar o roteador para suportar middleware de forma mais genérica. Aqui está um exemplo simplificado de um roteador com suporte a middleware:

```php
class Router {
    private $routes = [];
    private $middleware = [];

    public function get($path, $handler, $middleware = []) {
        $this->routes['GET'][$path] = $handler;
        $this->middleware['GET'][$path] = $middleware;
    }

    public function dispatch($request) {
        $path = $request->getPath();
        $method = $request->getMethod();

        if (isset($this->routes[$method][$path])) {
            $handler = $this->routes[$method][$path];
            $middleware = $this->middleware[$method][$path];

            // Executar middleware
            foreach ($middleware as $mw) {
                $mw->handle($request);
            }

            // Executar manipulador de rota
            $handler->handle($request);
        } else {
            // Rota não encontrada, lidar com isso
        }
    }
}
```

## 4. Middleware de Autenticação

O **Middleware de Autenticação** é usado para verificar se um usuário está autenticado antes de permitir o acesso a determinadas rotas. Se o usuário não estiver autenticado, ele será redirecionado para a página de login. Exemplo:

```php
// Middleware de Autenticação
function authenticateMiddleware($request, $next) {
    if (!isLoggedIn()) {
        // Redirecionar para a página de login
        header('Location: /login');
        exit;
    }

    return $next($request);
}
```

## 5. Middleware de Visitante

O **Middleware de Visitante** é usado para garantir que apenas usuários não autenticados tenham acesso a determinadas rotas. Se um usuário já estiver autenticado, ele será redirecionado para uma página diferente, como a página inicial. Exemplo:

```php
// Middleware de Visitante
function guestMiddleware($request, $next) {
    if (isLoggedIn()) {
        // Redirecionar para a página inicial
        header('Location: /home');
        exit;
    }

    return $next($request);
}
```

## 6. Conclusão do Registro

Após um usuário concluir o registro com sucesso, você pode redirecioná-lo para uma página de boas-vindas ou uma página inicial específica.

## 7. Link de Logout

Para permitir que os usuários façam logout, você deve criar um link de logout em sua interface de usuário. Por exemplo:

```html
<a href="/logout">Sair</a>
```

## 8. Tratamento de Logout

Ao processar o logout, você deve destruir a sessão ou o token de autenticação para efetivamente desautenticar o usuário. Aqui está um exemplo simplificado de como fazer isso:

```php
if ($_SERVER['REQUEST_METHOD'] === 'GET' && $_SERVER['REQUEST_URI'] === '/logout') {
    // Destruir a sessão ou token de autenticação
    session_destroy();

    // Redirecionar para a página de login ou outra página apropriada
    header('Location: /login');
    exit;
}
```

Lembre-se de que esses são exemplos simplificados e que a implementação real pode variar dependendo do framework ou biblioteca que você está usando. O uso de middleware é uma prática comum para adicionar funcionalidades intermediárias a uma aplicação web e organizar o código de forma modular.