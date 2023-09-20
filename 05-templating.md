# Templating

## 1. Templating (Modelo)

**Templating** é a prática de separar a lógica de apresentação de um aplicativo, tornando-o mais organizado e legível. Vamos abordar esse conceito com exemplos usando a **Twig Templating Engine**.

## 2. Twig Templating Engine

**Twig** é uma engine de template popular em PHP que permite criar templates de forma mais eficiente e segura. Você pode instalá-lo usando o Composer:

```bash
composer require twig/twig
```

## 3. Controlador Abstrato

Um **Controlador Abstrato** é uma classe base que contém lógica comum para vários controladores. Isso ajuda a evitar a duplicação de código. Vamos criar um exemplo de controlador abstrato:

```php
// AbstractController.php

abstract class AbstractController {
    protected $twig;

    public function __construct(\Twig\Environment $twig) {
        $this->twig = $twig;
    }
}
```

## 4. Renderizando Templates

Agora, podemos criar controladores específicos que estendem o controlador abstrato e usam o Twig para renderizar templates. Aqui está um exemplo:

```php
// Exemplo de um controlador específico

class ExemploController extends AbstractController {
    public function index() {
        $data = ['nome' => 'João'];
        $html = $this->twig->render('index.html.twig', $data);
        echo $html;
    }
}
```

## 5. Criando Templates de Visualização

Os templates de visualização são arquivos que definem a estrutura e o conteúdo das páginas HTML. Aqui está um exemplo de um arquivo de template Twig `index.html.twig`:

```twig
<!DOCTYPE html>
<html>
<head>
    <title>Meu Site</title>
</head>
<body>
    <h1>Bem-vindo, {{ nome }}!</h1>
</body>
</html>
```

## 6. Templates Reutilizáveis

O Twig permite criar **templates reutilizáveis** usando blocos e herança de templates. Por exemplo, você pode criar um template base comum e herdar dele em outros templates específicos.

## 7. Criando um Formulário de Entrada

Você pode criar um **Formulário de Entrada** em um template Twig da seguinte forma:

```twig
<form action="/processar-formulario" method="post">
    <label for="nome">Nome:</label>
    <input type="text" id="nome" name="nome">
    <input type="submit" value="Enviar">
</form>
```

## 8. Prevenção de Ataques de Cross-Site Scripting (XSS)

Para prevenir ataques XSS em templates, o Twig automaticamente escapa todas as variáveis de saída por padrão. Isso significa que qualquer conteúdo gerado pelo usuário é tratado como texto simples e não como código HTML. Portanto, o Twig ajuda a prevenir ataques de injeção de código malicioso.

Por exemplo, se você exibir uma variável em um template Twig, ela será automaticamente escapada:

```twig
{{ nome }}
```

Se o valor da variável `nome` contiver código HTML malicioso, ele será exibido como texto simples, não como HTML.

O Twig oferece recursos poderosos de manipulação de templates e proteção contra XSS, tornando-o uma escolha sólida para a criação de templates em aplicativos PHP. Certifique-se de seguir as práticas recomendadas ao criar e renderizar templates para garantir a segurança e o desempenho adequados.