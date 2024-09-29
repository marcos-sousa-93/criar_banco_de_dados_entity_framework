## Criando o banco de dados
### Instale o Entity Framework no seu projeto usando o NuGet:
<pre>
  <code>
Install-Package EntityFramework
  </code>
</pre>
### Crie a classe do modelo:
<pre>
  <code>
public class Pessoa
{
    public int PessoaId { get; set; } // Chave primária
    public string Nome { get; set; } // Nome da pessoa
    public string Endereco { get; set; } // Endereço da pessoa
}

  </code>
</pre>
### Crie a classe de contexto:
<pre>
  <code>
using System.Data.Entity;

public class CadastroPessoaContext : DbContext
{
    // O nome passado para o construtor é o nome da connection string
    public CadastroPessoaContext() : base("CadastroPessoaBD") { }

    // DbSet representa a tabela no banco de dados
    public DbSet<Pessoa> Pessoas { get; set; }
}

  </code>
</pre>
### Crie a connection string no arquivo App.config ou Web.config:
<pre>
  <code>
&#60connectionStrings&#62
  &#60add name="CadastroPessoaBD" connectionString="Data Source=(LocalDB)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\CadastroPessoaBD.mdf;Integrated Security=True" providerName="System.Data.SqlClient"/&#62
&#60/connectionStrings&#62

  </code>
</pre>
### Inicialize o banco de dados no código:
<pre>
  <code>
using System;
using System.Linq;

class Program
{
    static void Main()
    {
        using (var context = new CadastroPessoaContext())
        {
            // Adiciona uma nova pessoa no banco de dados
            context.Pessoas.Add(new Pessoa { Nome = "Carlos", Endereco = "Rua Principal, 123" });
            context.SaveChanges(); // Salva as alterações no banco de dados

            // Recupera e exibe todas as pessoas
            var pessoas = context.Pessoas.ToList();
            foreach (var pessoa in pessoas)
            {
            Console.WriteLine($"ID: {pessoa.PessoaId}, Nome: {pessoa.Nome}, Endereço:{pessoa.Endereco}");
            }
        }
    }
}

  </code>
</pre>
### Abordagem 2: Usando ADO.NET
### Outra maneira de criar um banco de dados é através do ADO.NET, onde você tem controle direto sobre os comandos SQL.

### Adicione uma referência ao System.Data.SqlClient para manipular o banco de dados.

### Crie o banco de dados com comandos SQL no código:
<pre>
  <code>
using System;
using System.Data.SqlClient;

class Program
{
    static void Main()
    {
        // Connection String para se conectar ao SQL Server LocalDB
        string connectionString = @"Data Source=(LocalDB)\MSSQLLocalDB;Integrated Security=True;";

        // Comando SQL para criar o banco de dados
        string createDbQuery = "CREATE DATABASE CadastroPessoaBD";

        // Executa o comando para criar o banco de dados
        using (SqlConnection connection = new SqlConnection(connectionString))
        {
            SqlCommand command = new SqlCommand(createDbQuery, connection);
            try
            {
                connection.Open();
                command.ExecuteNonQuery();
                Console.WriteLine("Banco de dados criado com sucesso.");
            }
            catch (Exception ex)
            {
                Console.WriteLine("Erro: " + ex.Message);
            }
        }

        // Connection String para se conectar ao novo banco de dados criado
        connectionString = @"Data Source=(LocalDB)\MSSQLLocalDB;Initial Catalog=CadastroPessoaBD;Integrated Security=True;";

        // Comando SQL para criar a tabela Pessoa
        string createTableQuery = @"
            CREATE TABLE Pessoa (
                PessoaId INT PRIMARY KEY IDENTITY,
                Nome NVARCHAR(100) NOT NULL,
                Endereco NVARCHAR(200) NOT NULL
            )";

        // Executa o comando para criar a tabela
        using (SqlConnection connection = new SqlConnection(connectionString))
        {
            SqlCommand command = new SqlCommand(createTableQuery, connection);
            try
            {
                connection.Open();
                command.ExecuteNonQuery();
                Console.WriteLine("Tabela Pessoa criada com sucesso.");
            }
            catch (Exception ex)
            {
                Console.WriteLine("Erro: " + ex.Message);
            }
        }
    }
}

  </code>
</pre>
