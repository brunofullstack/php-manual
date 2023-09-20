# The Request -> Response Cycle

## 1. The Front Controller

**Front Controller** é um padrão de design que atua como ponto de entrada único para uma aplicação web. Ele recebe todas as solicitações HTTP e encaminha essas solicitações para os controladores apropriados. Aqui está um exemplo básico de um front controller em PHP:

```php
// index.php (Front Controller)

// Inclui o carregador automático (autoloader) para carregar as classes automaticamente.
require_once 'autoloader.php';

// Recebe a URL da solicitação HTTP.
$requestUri = $_SERVER['REQUEST_URI'];

// Roteamento: Determina qual controlador deve lidar com a solicitação.
if ($requestUri === '/pagina-inicial') {
    $controller = new PaginaInicialController();
} elseif ($requestUri === '/perfil') {
    $controller = new PerfilController();
} else {
    $controller = new Erro404Controller();
}

// Chama o método apropriado do controlador com base na solicitação.
$response = $controller->handleRequest();

// Envia a resposta HTTP ao cliente.
$response->send();
```

Neste exemplo, o front controller (`index.php`) recebe todas as solicitações HTTP e encaminha para controladores específicos com base na URL da solicitação.

## 2. Autoloading

**Autoloading** é o processo de carregar automaticamente as classes PHP quando são necessárias, sem a necessidade de incluir manualmente os arquivos das classes. O uso de um autoloader é uma prática recomendada para manter o código organizado. Aqui está um exemplo de autoloading usando a função `spl_autoload_register`:

```php
// autoloader.php

spl_autoload_register(function ($className) {
    // Converte o namespace da classe para um caminho de arquivo.
    $filePath = str_replace('\\', DIRECTORY_SEPARATOR, $className) . '.php';

    // Verifica se o arquivo da classe existe e, se sim, inclui-o.
    if (file_exists($filePath)) {
        require_once $filePath;
    }
});
```

Neste exemplo, qualquer tentativa de criar uma instância de uma classe automaticamente chamará a função de autoloading, que carregará a classe a partir do arquivo correspondente.

## 3. Request Class

Uma **Request Class** é uma classe que encapsula informações sobre a solicitação HTTP atual, como métodos, cabeçalhos, parâmetros de consulta e dados do corpo. Aqui está um exemplo simples de uma classe de solicitação:

```php
// Request.php

class Request {
    private $method;
    private $uri;
    private $headers;
    private $queryParams;
    private $body;

    public function __construct() {
        $this->method = $_SERVER['REQUEST_METHOD'];
        $this->uri = $_SERVER['REQUEST_URI'];
        $this->headers = getallheaders();
        $this->queryParams = $_GET;
        $this->body = file_get_contents('php://input');
    }

    public function getMethod() {
        return $this->method;
    }

    public function getUri() {
        return $this->uri;
    }

    // Métodos getters para headers, queryParams e body
}
```

Você pode criar uma instância desta classe para acessar informações sobre a solicitação atual, como método, URI, cabeçalhos e muito mais.

## 4. Response Class

Uma **Response Class** é uma classe que encapsula informações sobre a resposta HTTP que será enviada ao cliente, como status, cabeçalhos e corpo. Aqui está um exemplo simples de uma classe de resposta:

```php
// Response.php

class Response {
    private $statusCode;
    private $headers;
    private $body;

    public function __construct($statusCode, $headers = [], $body = '') {
        $this->statusCode = $statusCode;
        $this->headers = $headers;
        $this->body = $body;
    }

    public function send() {
        // Define o código de status HTTP.
        http_response_code($this->statusCode);

        // Define os cabeçalhos HTTP.
        foreach ($this->headers as $name => $value) {
            header("$name: $value");
        }

        // Envia o corpo da resposta.
        echo $this->body;
    }
}
```

Você pode criar uma instância desta classe para definir o código de status, cabeçalhos e corpo da resposta HTTP e, em seguida, usar o método `send()` para enviá-la ao cliente.

## 5. Http Kernel

**Http Kernel** é uma parte central de muitos frameworks PHP, como Symfony e Laravel. Ele gerencia o fluxo de solicitação e resposta, incluindo a execução de middleware e a chamada de controladores. Aqui está um exemplo simplificado de um HTTP Kernel:

```php
// HttpKernel.php

class HttpKernel {
    private $middleware = [];

    public function __construct() {
        // Registra middleware no kernel.
        $this->registerMiddleware(new LoggerMiddleware());
        $this->registerMiddleware(new AuthenticationMiddleware());
    }

    public function handle(Request $request) {
        // Executa middleware.
        foreach ($this->middleware as $middleware) {
            $request = $middleware->handle($request);
        }

        // Chama o controlador apropriado com base na solicitação.
        $controller = $this->resolveController($request);
        $response = $controller->handleRequest($request);

        return $response;
    }

    public function registerMiddleware(Middleware $middleware) {
        $this->middleware[] = $middleware;
    }

    private function resolveController(Request $request) {
        // Lógica para determinar o controlador com base na solicitação.
        // Retorna uma instância do controlador apropriado.
    }
}
```

Neste exemplo, o HTTP Kernel gerencia a execução de middleware e a chamada de controladores com base na solicitação HTTP recebida.

Esses conceitos são fundamentais para entender o funcionamento interno de muitos frameworks PHP e para construir aplicativos web robustos. Eles formam a base para criar aplicativos web escaláveis e bem estruturados em PHP.