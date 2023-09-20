# Registration (Abordando Tópicos de Autenticação de Usuário, Migração de Tabela de Usuários e Registro)

Vamos abordar os tópicos de Entidades de Usuário Autenticado, Migração de Tabela de Usuários, Formulário e Rota de Registro, Tratamento de Registro, Formulário de Registro, Validação de Entrada do Formulário, Redirecionamento com Erros e Conclusão do Registro.

## 1. Entidades de Usuário Autenticado

As **Entidades de Usuário Autenticado** representam os usuários em seu sistema de autenticação. Eles geralmente têm campos como nome de usuário, senha e informações de perfil. Aqui está um exemplo simplificado de uma entidade de usuário:

```php
class User {
    private $id;
    private $username;
    private $password;

    // Getters e setters...
}
```

## 2. Migração de Tabela de Usuários

Ao usar um banco de dados para armazenar informações de usuários, você precisará criar uma migração de tabela de usuários. Aqui está um exemplo de migração SQL:

```sql
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(255) NOT NULL,
    password VARCHAR(255) NOT NULL
);
```

## 3. Formulário e Rota de Registro

Você deve criar um **Formulário de Registro** em HTML e configurar uma rota para processá-lo. Aqui está um exemplo de formulário HTML:

```html
<form method="POST" action="/register">
    <label for="username">Nome de Usuário:</label>
    <input type="text" id="username" name="username" required>

    <label for="password">Senha:</label>
    <input type="password" id="password" name="password" required>

    <input type="submit" value="Registrar">
</form>
```

E uma rota em PHP para processar o formulário:

```php
if ($_SERVER['REQUEST_METHOD'] === 'POST' && $_SERVER['REQUEST_URI'] === '/register') {
    // Processar o registro aqui
}
```

## 4. Tratamento de Registro

Para tratar o registro, você deve receber os dados do formulário, validar os campos e criar um novo usuário. Aqui está um exemplo simplificado:

```php
if ($_SERVER['REQUEST_METHOD'] === 'POST' && $_SERVER['REQUEST_URI'] === '/register') {
    $username = $_POST['username'];
    $password = $_POST['password'];

    // Valide os dados do formulário...

    // Crie um novo usuário e salve-o no banco de dados...
}
```

## 5. Formulário de Registro

O **Formulário de Registro** deve incluir validação de entrada para garantir que os dados sejam corretos antes de criar um novo usuário. Você pode usar funções como `filter_var` ou bibliotecas de validação de formulário para isso.

## 6. Validação de Entrada do Formulário

Aqui está um exemplo de validação de entrada usando `filter_var` para verificar se o nome de usuário é um e-mail válido e se a senha tem pelo menos 8 caracteres:

```php
if ($_SERVER['REQUEST_METHOD'] === 'POST' && $_SERVER['REQUEST_URI'] === '/register') {
    $username = $_POST['username'];
    $password = $_POST['password'];

    if (!filter_var($username, FILTER_VALIDATE_EMAIL)) {
        // Erro: Nome de usuário inválido
    }

    if (strlen($password) < 8) {
        // Erro: Senha muito curta
    }

    // Crie um novo usuário e salve-o no banco de dados...
}
```

## 7. Redirecionamento com Erros

Se ocorrerem erros durante o registro, você deve redirecionar o usuário de volta ao formulário de registro e exibir mensagens de erro. Use redirecionamento HTTP com parâmetros de consulta para transmitir mensagens de erro:

```php
// Em caso de erro
header('Location: /register?error=1');
exit;
```

## 8. Conclusão do Registro

Após um registro bem-sucedido, você deve criar o usuário no banco de dados e redirecionar o usuário para uma página de boas-vindas ou página inicial:

```php
if ($_SERVER['REQUEST_METHOD'] === 'POST' && $_SERVER['REQUEST_URI'] === '/register') {
    // Processar o registro e criar o usuário

    // Redirecionar para a página de boas-vindas
    header('Location: /welcome');
    exit;
}
```

Lembre-se de que esses são exemplos simplificados. Em um aplicativo real, você deve usar técnicas de segurança, como hash de senhas e proteção contra ataques de injeção de SQL, e considerar a adoção de um framework ou biblioteca de autenticação confiável para lidar com o registro de usuários de forma segura.