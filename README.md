# Validação de CPF com Azure Functions

Este repositório contém um projeto de validação de CPF utilizando o Visual Studio Code (VSCode) conectado ao portal Azure. O objetivo é criar uma Azure Function com um HttpTrigger, permitindo a validação de números de CPF enviados no corpo da requisição (body) no formato JSON.

## Pré-requisitos
- Conta no Azure.
- Visual Studio Code com a extensão **Azure Functions** instalada.
- Azure CLI instalada.
- Node.js instalado.

## Comandos Utilizados

### 1. Criar a Function com HttpTrigger
No terminal do VSCode, execute os comandos abaixo para criar o projeto e a Function:

```bash
# Criar um novo diretório para o projeto
mkdir validacao-cpf
cd validacao-cpf

# Inicializar o projeto Azure Functions
func init . --worker-runtime node

# Criar a Function com HttpTrigger
dotnet new func --template "HttpTrigger" --name "ValidaCPF" --authlevel "function"
```

### 2. Alterações no Código
Edite o arquivo gerado da Function (`ValidaCPF/index.js`) para obrigar o uso do método POST e para validar o CPF recebido no corpo da requisição. O código modificado deverá ter a seguinte estrutura:

```javascript
module.exports = async function (context, req) {
    const cpf = req.body && req.body.cpf;

    if (!cpf) {
        context.res = {
            status: 400,
            body: "Por favor, insira o CPF no formato JSON, exemplo: { \"cpf\": \"12345678901\" }"
        };
        return;
    }

    // Validação simples do CPF (substituir pela lógica desejada)
    const isValidCPF = cpf.length === 11 && !isNaN(cpf);

    context.res = {
        status: isValidCPF ? 200 : 400,
        body: isValidCPF ? "CPF válido" : "CPF inválido"
    };
};
```

### 3. Executar e Testar Localmente
Para iniciar a Azure Function localmente, utilize os seguintes comandos:

```bash
# Iniciar o servidor local da Function
func start
```

Após iniciar, a URL gerada será exibida no terminal, algo como:

```
http://localhost:7071/api/ValidaCPF
```

Teste a validação de CPF utilizando o Postman:
- Método: POST
- URL: `http://localhost:7071/api/ValidaCPF`
- Body (JSON):
  ```json
  {
      "cpf": "12345678901"
  }
  ```

### 4. Fazer o Deploy para o Azure
Para publicar a Function no Azure, utilize os comandos abaixo:

```bash
# Fazer login no Azure
az login

# Publicar a Function no Azure
func azure functionapp publish <NOME_DA_SUA_FUNCTION_APP>
```

Após o deploy, a URL da Function será atualizada para um domínio Azure, como:

```
https://<NOME_DA_SUA_FUNCTION_APP>.azurewebsites.net/api/ValidaCPF
```

### 5. Observação: Autenticação no Postman
Depois do deploy, é necessário configurar a autenticação no Postman para realizar a validação do CPF. Inclua o **valor padrão** nos parâmetros de autenticação do Postman:

- Aba **Authorization**
  - Tipo: **Function Key**
  - Valor: Copie a chave de autenticação disponível no portal Azure.

Sem a chave de autenticação correta, as requisições retornarão erro.

## Conclusão
Este projeto demonstra como criar, testar e publicar uma Azure Function para validação de CPF utilizando o VSCode e o portal Azure. Sinta-se à vontade para personalizar a lógica de validação de CPF conforme necessário!

## REFERÊNCIAS 
DIO AZ204
