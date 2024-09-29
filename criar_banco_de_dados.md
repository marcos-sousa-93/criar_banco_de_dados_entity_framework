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
<connectionStrings>
  <add name="CadastroPessoaBD" connectionString="Data Source=(LocalDB)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\CadastroPessoaBD.mdf;Integrated Security=True" providerName="System.Data.SqlClient"/>
</connectionStrings>

  </code>
</pre>
