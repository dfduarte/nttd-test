# Teste - NTT Data

Essa codebase serve para demonstrar o funcionamento de um CRUD simples usando uma API, conforme solicitado.

## How it works

Esse teste foi desenvolvido usando o serverless framework, e 100% dos recursos necessarios sao deployados usando um simples "serverless deploy", assumindo que as credenciais da AWS estejam setadas corretamente e como default (voce pode passa-las como parametro tambem)

## Uso

### Pré-requisitos

* Voce precisa ter o Python 3.9 instalado localmente para fazer as dependencias funcionarem. Vc pode criar um virtualenv com tudo rodando:

```bash
python3.9 -m venv ./venv
source ./venv/bin/activate
```

Como alternativa, você também pode usar a configuração `dockerizePip` de `serverless-python-requirements`. Para obter detalhes sobre isso, consulte o repositório GitHub correspondente (https://github.com/UnitedIncome/serverless-python-requirements).

* Voce tambem vai precisar do AWS CLI instalado e com as credenciais de seguranca corretamente setadas, ou pelo menos chaves AWS disponiveis para deployment, com policies Lambda, DynamoDB, e APIGateway setadas, no minimo.

### Implantação

Este exemplo foi feito para funcionar com o painel do Serverless Framework.

Primeiro instale as dependencias do Serverless e plugins:

```
npm install
```

E, em seguida, execute o deployment com:

```
serverless deploy
```

Depois de executar o deploy, você deve ver uma saída semelhante a:

```bash
% serverless deploy 
Running "serverless" from node_modules
	
Deploying nttdata-interview to stage dev (us-east-1)
Using Python specified in "runtime": python3.9
Packaging Python WSGI handler...

✔ Service deployed to stack nttdata-interview-dev (126s)

endpoint: ANY - https://xyzabcfoobar.execute-api.us-east-1.amazonaws.com
functions:
  api: nttdata-interview-dev-api (1.5 MB)

Monitor all your API routes with Serverless Console: run "serverless --console"
```

_Nota_: Na forma atual, após a implantação, sua API é pública e pode ser invocada por qualquer pessoa. Para implantações de produção, convém configurar um authorizer. Para detalhes sobre como fazer isso, consulte [httpApi event docs](https://www.serverless.com/framework/docs/providers/aws/events/http-api/).


### Invocação

Após o deploy bem-sucedido, você pode criar um novo usuário chamando o endpoint correspondente:

Para criar (Create): 

```bash
curl --request POST 'https://xyzabcfoobar.execute-api.us-east-1.amazonaws.com/users' --header 'Content-Type: application/json' --data-raw '{"username": "joao.maria", "userId": "someUserId"}'
```

O que deve resultar na seguinte resposta:

```bash
{"userId":"someUserId","username":"joao.maria"}
```

Para buscar o usuario (Retrieve)

```bash
curl https://xyzabcfoobar.execute-api.us-east-1.amazonaws.com/users/someUserId
```

O que deve resultar na seguinte resposta:

```bash
{"userId":"someUserId","username":"joao.maria"}
```

Se você tentar buscar um usuário que não existe, deverá receber a seguinte resposta:

```bash
{"error":"Não foi possível encontrar o usuário com o \"userId\""} 
```

Para modificar o usuario (update):

```bash
curl --request PUT 'https://xyzabcfoobar.execute-api.us-east-1.amazonaws.com/users/joao.maria-userId' --header 'Content-Type: application/json' --data-raw '{"username": "joao.maria.newusername", "userId": "joao.maria-userId"}'
```

O que deve resultar na seguinte resposta:

```bash
{"userId":"someUserId","username":"joao.maria"}
```

Para deletar o usuario (delete):

```bash
curl --request DELETE 'https://xyzabcfoobar.execute-api.us-east-1.amazonaws.com/users/joao.maria-userId'
```

O que deve resultar na seguinte resposta:

```bash
{"userId":"someUserId","username":"joao.maria has been removed"} 
```

### Remover o ambiente:

digite o seguinte comando para remover:

```
serverless remove
```

E aguarde a finalizacao