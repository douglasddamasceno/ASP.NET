# ASP.NET com C#
> Roadmap de aprendizagem
1. Base Profunda
- Lógica de Programação
- POO
- Estrutura de Dados

2. Banco de Dados e Consultas
- LINQ
- SQL
- Dapper e EntityFrameowrkCore

3. Apps e Apis
- APP ASP.NET Core MVC
- API com controllers
- Minimal API

4. Bibliotecas .NET
- FluentValidation, MediatR, Refit e Polly
- Testes (xUnit, Moq e NSubstitute)
- Segurança a Autenticação: IdentityServer e JWT

### ASP.NET Profile
```json
{
    "workbench.colorTheme": "GitHub Dark",
    "editor.fontSize": 13,
    "editor.fontFamily": "Space Grotesk",
    "editor.formatOnSave": true,
    "editor.formatOnPaste": true,
    "workbench.iconTheme": "vscode-great-icons",
    "terminal.integrated.cursorStyle": "line",
    "files.exclude": {
        "**/*bin": true,
        "**/*obj": true
    },
    "terminal.integrated.fontWeight": "normal",
    "terminal.integrated.fontSize": 12,
    "terminal.integrated.fontFamily": "MesloLGM Nerd Font"
}
```
### ASP.NET Extensions
<img width="1366" height="720" alt="{5B941288-31DC-430E-837D-4EB03CF77790}" src="https://github.com/user-attachments/assets/f5dee39e-046a-4f54-8c99-f2e8a9bcf396" />

### Criar Projeto ASP.NET
1. Criar diretório da solução
```powershell
mkdir MeuApp
```

2. Navegar para o diretório
```powershell
cd .\MeuApp\
```

3. Criar arquivo de solução
```powershell
dotnet new sln -n MeuApp
```

4. Criar diretório `src`
```powershell
mkdir src
```

5. Navegar ao diretório `src`
```powershell
cd .\src\
```

6. Criar projeto - neste caso escolhemos um app mvc
```powershell
dotnet new mvc -n MeuWebApp`
```

7. Navegar para o diretório do projeto
```powershell
cd .\MeuWebApp\
```

8. Vincular projeto à solução
```powershell
dotnet sln ..\..\MeuApp.sln add .\MeuWebApp.csproj
```

### Instalar EntityFramework para SQL Server
```powershell
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet tool install --global dotnet-ef
dotnet add package Microsoft.EntityFrameworkCore.Tools
dotnet add package Microsoft.EntityFrameworkCore.Design
```

### Migrations
```powershell
dotnet ef migrations add initial
dotnet ef database update
```

### Annotations
```csharp
DataAnnotations && DataAnnotations.Schema || IEntityTypeConfiguration
```

### Authentication & Authorization:
- Adicionando dependências
```powershell
dotnet add package Microsoft.AspNetCore.Authentication
dotnet add package Microsoft.AspNetCore.Authentication.JwtBearer
```

- Configuração

Criar Settings.cs (class static) com atributo static secret: "VALOR".

```csharp
//Configure Services (depois do AddControllers):
var key = Encoding.ASCII.GetBytes(Settings.Secret);
    services.AddAuthentication(x =>
            {
                x.DefaultAuthenticateScheme = JwtBearerDefaults.AuthenticationScheme;
                x.DefaultChallengeScheme = JwtBearerDefaults.AuthenticationScheme;
            })
            .AddJwtBearer(x =>
            {
                x.RequireHttpsMetadata = false;
                x.SaveToken = true;
                x.TokenValidationParameters = new TokenValidationParameters
                {
                    ValidateIssuerSigningKey = true,
                    IssuerSigningKey = new SymmetricSecurityKey(key),
                    ValidateIssuer = false,
                    ValidateAudience = false
                };
            });

//Configure pipeline:
	app.UseAuthentication();
	app.UseAuthorization();
```
- Para Autenticar: No Header do Request >> `Authorization: Bearer VALOR_TOKEN`

### Gerar artefatos de Publicação
```powershell
dotnet publish -c Release -o C:\inetpub\wwwroot\dir
```


### Outros utilitários CLI .NET 
- Listar todas as versões instaladas do .NET
```powershell
dotnet --info
```

- Listar SDKs instalados
```powershell
dotnet --list-sdks
```

- Listar Runtimes instalados
```powershell
dotnet --list-runtimes
```

- Criar projeto com versão específica do .NET
```powershell
dotnet new web -o MyWebApp -f netX.X
```

- Criar arquivo `global.json` na raiz da pasta de projetos para rodar todos projetos com a versão uma única versão
```powershell
{
	"projects": ["src"],
	"sdk": {
		"version": "^6.0.201"
	}
}
```

- Migrations passando o contexto como parâmetro
```powershell
dotnet ef migrations add MYCOMMENT --context MYCONTEXT
dotnet ef database update --context MYCONTEXT
```

- Habiliar certificados de dev
```powershell
dotnet dev-certs https --check --trust
```
