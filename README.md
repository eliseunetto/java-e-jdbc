# JAVA e JDBC

## Trabalhando com um banco de dados

### Tópicos:

1. **Introdução ao JDBC e o padrão Factory** \
  1.1 Para acessar o banco de dados, precisamos de um **driver**
    * Um driver nada mais é do que uma biblioteca (JAR)

   1.2 **JDBC** significa *Java DataBase Conectivity*
   * JDBC define uma camada de abstração entre a sua aplicação e o driver do banco de dados
   * Essa camada possui, na sua grande maioria, interfaces que o driver implementa

   1.3 Para abrir uma conexão, devemos usar o método `getConnection`, da classe `DriverManager`
   * O método `getConnection` recebe uma string de conexão JDBC, que define a URL, usuário, senha, etc

2. **Executando comandos SQL no Java** \
  2.1 Para simplificar e encapsular a criação da conexão, devemos usar uma classe `ConnectionFactory`
    * A classe `ConnectionFactory` segue o padrão de criação *Factory Method*
    * O *Factory Method* encapsula a criação de um objeto

   2.2 Para executar um comando SQL, podemos usar a interface `java.sql.Statement`
    * O método `execute` envia o comando para o banco de dados
    * Dependendo do comando SQL, podemos recuperar a chave primária ou os registros selecionados

3. **Evitando SQL Injection** \
  3.1 Ao executar SQL como `Statement`, temos um risco de segurança, chamado de **SQL Injection**
    * *SQL Injection* nada mais é do que passar um novo comando SQL como parâmetro

   3.2   Para evitar *SQL Injection*, devemos usar a interface `PreparedStatement`
    * Diferentemente do `Statement`, o `PreparedStatement` trata (*sanitiza*) cada parâmetro do comando SQL

4. **Controle de transação** \
  4.1 O banco de dados oferece um recurso chamado de **transação**, para juntar várias alterações como unidade de trabalho
    * Se uma alteração falha, nenhuma alteração é aplicada (é feito um *rollback* da transação)
    * Todas as alterações precisam funcionar para serem aceitas (é feito um `commit`)

   4.2 `commit` e `rollback` são operações clássicas de transações \
   4.3 Para garantir o fechamento dos recursos, existe no Java uma cláusula *try-with-resources*
    * O recurso em questão deve usar a interface `Autoclosable`

5. **Escalabilidade com pool de conexões** \
  5.1 É boa prática usar um **pool de conexões** \
  5.2 Um *pool* de conexões administra/controla a quantidade de conexões abertas
    * Normalmente tem um mínimo e máximo de conexões

   5.3   Como existe uma interface que representa a conexão (`java.sql.Connection`), também existe uma interface que representa o *pool* de conexões (`javax.sql.DataSource`) \
   5.4   **C3PO** é uma implementação Java de um *pool* de conexão

6. **Camada de persistência** \
  6.1 Para cada tabela de domínio, temos uma classe de domínio
    * Por exemplo, a tabela `produtos` tem uma classe `Produto` associada
    * Objetos dessa classe representa um registro na tabela

   6.2   Para acessar a tabela, usaremos um padrão chamado **Data Access Object (DAO)**
    * Para cada classe de domínio, existe um DAO. Por exemplo, a classe `Produto` possui um `ProdutoDao`
    * Todos os métodos JDBC relacionados com o produto devem estar encapsulados no `ProdutoDao`

7. **Evitando queries N + 1** \
  7.1 Que quando temos um relacionamento, é preciso ter cuidado para não cair no problema de *queries* `N + 1`
    * `N + 1` significa executar uma *query* e mais uma nova *query* (´N`) para cada relacionamento
    * *Queries* `N + 1` podem gerar um problema no desempenho
    * *Queries* `N + 1` podem ser evitadas através de `joins` no SQL

   7.2   A criar a nossa própria camada de persistência

8. **Aplicação Desktop** \
  8.1 Uma aplicação é escrita em camadas \
    * Camadas clássicas são *view*, *controller*, *modelo* e *persistência*

   8.2   O fluxo entre as camadas segue a ordem:
   > `view <--> controller <--> persistencia` 

   8.3   Nesse curso focamos na camada de persistência \
   8.4   Uma camada não deve deixar "vazar" detalhes da implementação (por exemplo uma exceção como `SQLException`) \
   8.5   Em outras formações você aprenderá como criar a **view** ou **front-end** para Android (*mobile*) ou web (*html*)
