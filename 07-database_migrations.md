# Database Migrations

## 1. Passos de Migração de Banco de Dados

Os **Passos de Migração de Banco de Dados** referem-se aos passos necessários para aplicar migrações em um sistema. Esses passos incluem criar uma tabela de controle de migração, usar transações de banco de dados, obter migrações aplicadas, determinar quais migrações aplicar, aplicar as migrações e registrar as migrações aplicadas.

## 2. Criando a Tabela de Migrações

A primeira etapa ao trabalhar com migrações é criar uma tabela no banco de dados para rastrear quais migrações foram aplicadas. Aqui está um exemplo de como criar essa tabela:

```sql
CREATE TABLE migrations (
    id INT AUTO_INCREMENT PRIMARY KEY,
    migration VARCHAR(255) NOT NULL,
    batch INT NOT NULL
);
```

Essa tabela manterá um registro de todas as migrações aplicadas e em qual "lote" (batch) elas foram aplicadas.

## 3. Transações de Banco de Dados

O uso de **Transações de Banco de Dados** é uma prática recomendada ao aplicar migrações. Isso garante que, se ocorrer algum erro durante a aplicação de uma migração, o banco de dados permanecerá em um estado consistente. Aqui está um exemplo de como usar transações em PHP com PDO:

```php
try {
    $pdo->beginTransaction();

    // Execute as instruções SQL da migração aqui.

    $pdo->commit();
} catch (PDOException $e) {
    $pdo->rollBack();
    echo "Erro na migração: " . $e->getMessage();
}
```

## 4. Obtendo Migrações Aplicadas

Para saber quais migrações já foram aplicadas, você pode consultar a tabela de migrações:

```php
$appliedMigrations = $pdo->query('SELECT migration FROM migrations')->fetchAll(PDO::FETCH_COLUMN);
```

Isso retornará uma lista de migrações já aplicadas.

## 5. Obtendo Arquivos de Migração a Serem Aplicados

Você pode verificar quais arquivos de migração estão disponíveis em um diretório e compará-los com as migrações já aplicadas para determinar quais migrações precisam ser aplicadas a partir de arquivos. Um exemplo de como fazer isso:

```php
$migrationFiles = glob('migrations/*.sql');
$pendingMigrations = array_diff($migrationFiles, $appliedMigrations);
```

## 6. Aplicando Migrações

Para aplicar as migrações pendentes, você pode ler o conteúdo de cada arquivo de migração e executar as instruções SQL no banco de dados:

```php
foreach ($pendingMigrations as $migrationFile) {
    $sql = file_get_contents($migrationFile);

    try {
        $pdo->exec($sql);

        // Registre a migração aplicada na tabela de migrações.
        $pdo->exec("INSERT INTO migrations (migration, batch) VALUES ('$migrationFile', 1)");
    } catch (PDOException $e) {
        echo "Erro na migração $migrationFile: " . $e->getMessage();
    }
}
```

## 7. Inserindo Migrações

Depois de aplicar com sucesso uma migração, você deve registrar essa migração na tabela de migrações. Isso impede que a mesma migração seja aplicada novamente. O exemplo acima demonstra como inserir uma migração após aplicá-la com sucesso.

## 8. Executando as Instruções SQL das Migrações

As migrações geralmente contêm instruções SQL para criar, modificar ou excluir tabelas e dados no banco de dados. Você pode executar essas instruções usando a função `exec` do PDO ou qualquer outro método de execução de SQL compatível com o seu banco de dados.

Certifique-se de criar migrações de forma cuidadosa e versioná-las adequadamente para manter o controle sobre as mudanças no esquema do banco de dados ao longo do tempo. As migrações são uma parte essencial do desenvolvimento de aplicativos web escaláveis e fáceis de manter.