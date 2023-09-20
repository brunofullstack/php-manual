# Saving and Retrieving Data

Vamos abordar os tópicos de envio de um formulário, acesso à solicitação (request), criação de uma entidade "Post", uso de um Data Mapper, repositórios e o método "Find or Fail". 

## 1. Enviando um Formulário

Ao enviar um formulário em uma página da web, você geralmente faz uma solicitação HTTP POST. Aqui está um exemplo de um formulário HTML simples:

```html
<form method="POST" action="/processar-formulario.php">
    <label for="titulo">Título:</label>
    <input type="text" id="titulo" name="titulo">

    <label for="conteudo">Conteúdo:</label>
    <textarea id="conteudo" name="conteudo"></textarea>

    <input type="submit" value="Enviar">
</form>
```

Neste exemplo, o formulário será enviado para o arquivo `processar-formulario.php` usando o método POST.

## 2. Acessando a Solicitação

Para acessar os dados enviados pelo formulário, você pode usar a variável `$_POST` no PHP. Por exemplo, em `processar-formulario.php`:

```php
$titulo = $_POST['titulo'];
$conteudo = $_POST['conteudo'];
```

## 3. Criando uma Entidade "Post"

Uma **entidade** representa um objeto do mundo real em seu aplicativo. Para criar uma entidade "Post", você pode criar uma classe PHP correspondente:

```php
class Post {
    private $id;
    private $titulo;
    private $conteudo;

    // Métodos getters e setters...
}
```

## 4. Data Mapper

Um **Data Mapper** é um padrão de design que separa a lógica de mapeamento de objetos de negócios para registros no banco de dados. Aqui está um exemplo simplificado de um Data Mapper para a entidade "Post":

```php
class PostMapper {
    public function map(array $data) {
        $post = new Post();
        $post->setId($data['id']);
        $post->setTitulo($data['titulo']);
        $post->setConteudo($data['conteudo']);
        return $post;
    }

    public function unmap(Post $post) {
        return [
            'id' => $post->getId(),
            'titulo' => $post->getTitulo(),
            'conteudo' => $post->getConteudo(),
        ];
    }
}
```

## 5. Repositórios

Um **Repositório** é uma classe responsável por armazenar e recuperar entidades de um banco de dados. Aqui está um exemplo de um repositório para a entidade "Post":

```php
class PostRepository {
    private $db;
    private $mapper;

    public function __construct(PDO $db, PostMapper $mapper) {
        $this->db = $db;
        $this->mapper = $mapper;
    }

    public function findById($id) {
        $stmt = $this->db->prepare('SELECT * FROM posts WHERE id = ?');
        $stmt->execute([$id]);
        $data = $stmt->fetch(PDO::FETCH_ASSOC);
        return $data ? $this->mapper->map($data) : null;
    }

    // Outros métodos de consulta...
}
```

## 6. Método "Find or Fail"

O método "Find or Fail" é uma prática comum em repositórios para buscar uma entidade por ID e lançar uma exceção se a entidade não for encontrada. Exemplo:

```php
public function findOrFail($id) {
    $post = $this->findById($id);
    if (!$post) {
        throw new NotFoundException("Post não encontrado.");
    }
    return $post;
}
```

Este método ajuda a simplificar o tratamento de entidades não encontradas em seu aplicativo.

Esses conceitos representam a estrutura básica de como trabalhar com formulários, entidades, Data Mappers, repositórios e o método "Find or Fail" em PHP. Eles formam a base para construir aplicativos que lidam com entrada de dados, armazenamento em banco de dados e recuperação de informações.