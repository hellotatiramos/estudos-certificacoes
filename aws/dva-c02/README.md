# DVA-C02 - AWS Certified Developer - Associate

## Amazon DynamoDB
<div align="center">
<img src="https://github.com/hellotatiramos/estudos-certificacoes/assets/158481113/8513ce66-f3fd-4d9f-9350-0e14b20292b9" width="700px" />
</div>

## Principais Conceitos
* **Tabelas:** A unidade básica de armazenamento no DynamoDB. Cada tabela contém múltiplos itens e cada item é um conjunto de atributos.
* **Itens:** Análogos às linhas em um banco de dados relacional. Cada item é identificado de forma única por uma chave primária.
* **Atributos:** Análogos às colunas em um banco de dados relacional. São os dados associados a um item.
* **Chave Primária:** Pode ser uma chave de partição (hash) ou uma combinação de chave de partição e chave de classificação (hash-range).
* **Índices Secundários:** Permitem consultas eficientes usando atributos que não fazem parte da chave primária.

## Operações Principais da API

### Control Plane 
O "control plane" (plano de controle) refere-se às operações administrativas e de gerenciamento de um serviço. No caso do DynamoDB, essas operações incluem:

* **CreateTable:** Cria uma nova tabela no DynamoDB.
* **DescribeTable:** Obtém informações sobre a estrutura e configurações de uma tabela.
* **ListTables:** Lista todas as tabelas em uma conta do DynamoDB.
* **UpdateTable:** Atualiza as configurações de uma tabela existente, como throughput provisionado, índices ou definições de atributo.
* **DeleteTable:** Exclui uma tabela do DynamoDB.

### Data Plane
O "data plane" (plano de dados) refere-se às operações relacionadas à manipulação dos dados propriamente ditos, ou seja, a leitura e escrita dos dados armazenados no DynamoDB. Essas operações são muito mais frequentes que as operações do control plane e incluem:

* **PutItem:** Insere um único item ou substitui um item existente em uma tabela.
* **BatchWriteItem:** Insere até 25 itens em uma ou mais tabelas em uma única chamada.
* **GetItem:** Recupera um item de uma tabela com base na chave primária.
* **BatchGetItem:** Recupera até 100 itens de uma ou mais tabelas em uma única chamada.
* **UpdateItem:** Atualiza um atributo de um item existente em uma tabela.
* **DeleteItem:** Exclui um item de uma tabela com base na chave primária.

## Tipos de dados que o DynamoDB suporta
O Amazon DynamoDB suporta diversos tipos de dados que podem ser categorizados em três grupos principais: tipos escalares, tipos de documento e tipos de conjunto. Cada grupo oferece diferentes formas de armazenar e manipular dados, permitindo uma grande flexibilidade na modelagem do banco de dados. Vamos explorar cada um desses grupos em detalhes.

### Tipos Escalares (Scalar Types)
Os tipos escalares são os tipos de dados mais básicos e incluem valores individuais e simples. DynamoDB suporta os seguintes tipos escalares:

**1. String (S)**

* Representa dados de texto.
* Pode conter até 400 KB de texto.
* Exemplo: `"Alice"`
  
**2. Number (N)**

* Representa dados numéricos.
* Pode ser inteiro ou ponto flutuante.
* Exemplo: `123`, `45.67`

**3. Binary (B)**

* Representa dados binários.
* Pode conter até 400 KB de dados binários.
* Exemplo: `b'\x01\x02\x03'`

**4. Boolean (BOOL)**

* Representa um valor booleano.
* Pode ser `true` ou `false`.
* Exemplo: `true`

**5. Null (NULL)**

* Representa um valor nulo.
* Usado para indicar a ausência de um valor.
* Exemplo: `NULL`

### Tipos de Documento (Document Types)

Os tipos de documento permitem armazenar estruturas de dados complexas, como JSON, que podem conter outros tipos de dados aninhados.

DynamoDB suporta os seguintes tipos de documento:

**1. Map (M)**

* Representa um objeto ou estrutura de dados semelhante a um dicionário.
* Pode conter pares chave-valor, onde as chaves são strings e os valores podem ser de qualquer tipo de dado suportado pelo DynamoDB.
* Exemplo:

```json
{
  "Name": {"S": "Alice"},
  "Age": {"N": "30"},
  "Address": {
    "M": {
      "Street": {"S": "123 Main St"},
      "City": {"S": "Wonderland"}
    }
  }
}
```

**2. List (L)**

* Representa uma lista ordenada de valores.
* Pode conter itens de qualquer tipo de dado suportado pelo DynamoDB.
* Exemplo:

```json
[
  {"S": "Apple"},
  {"N": "123"},
  {"BOOL": true}
]
```

### Tipos de Conjunto (Set Types)

Os tipos de conjunto permitem armazenar coleções não ordenadas de valores únicos. DynamoDB suporta três tipos de conjunto:

**1. String Set (SS)**

* Representa um conjunto de strings.
* Exemplo:
  
``` json
["Apple", "Orange", "Banana"]
```

**2. Number Set (NS)**

* Representa um conjunto de números.
* Exemplo:

```json
[1, 2, 3, 4.5]
```

**3. Binary Set (BS)**

* Representa um conjunto de dados binários.
* Exemplo:
  
`
[b'\x01', b'\x02', b'\x03']
`

**Resumo**

DynamoDB oferece uma variedade de tipos de dados para suportar diferentes necessidades de armazenamento e modelagem de dados:

* **Tipos Escalares:** Strings, Números, Binários, Booleanos, Nulos.
* **Tipos de Documento:** Mapas (objetos), Listas.
* **Tipos de Conjunto:** Conjuntos de Strings, Números, Binários.

Essa diversidade permite que os desenvolvedores escolham o tipo de dado mais apropriado para suas necessidades, garantindo uma estrutura de dados eficiente e flexível para suas aplicações.

## Chave de Partição (Partition Key)
A chave de partição é um identificador único que determina a partição na qual o item será armazenado. Esta chave é obrigatória e deve ser exclusiva para cada item em uma tabela que usa apenas uma chave de partição (tabela de chave simples). A escolha da chave de partição é importante para distribuir uniformemente os dados e a carga de tráfego entre as partições.

# Exemplo de Chave de Partição
Imagine uma tabela chamada Users que armazena informações sobre usuários, onde cada usuário é identificado exclusivamente pelo seu `UserID`.

| UserID        | Name     | Email |
| ------|-----|-----|
| 12345  	| Alice 	| alice@example.com	|
| 67890  	| Bob 	| bob@example.com 	|
| 11223 	| Carol 	| carol@example.com 	|

Neste caso, o `UserID` é a chave de partição. Cada item (ou linha) da tabela Users é identificado unicamente pelo `UserID`.

## Chave de Partição e Chave de Classificação (Partition Key and Sort Key)
Quando uma tabela tem uma chave de classificação, ela se torna uma tabela composta, permitindo múltiplos itens com a mesma chave de partição, desde que tenham chaves de classificação diferentes. A chave de classificação permite ordenar e buscar itens dentro da mesma partição.

### Exemplo de Chave de Partição e Chave de Classificação
Considere uma tabela chamada `Orders` que armazena informações sobre pedidos, onde cada pedido é identificado por um `OrderID` e pertence a um cliente identificado por `CustomerID`.

| CostumerID        | OrderID     | OrderDate | Amount | 
| ------|-----|-----| -----|
| 123  	| 001 	| 2023-05-01| $250 | 
| 123 	| 002 	| 2023-05-03 	| $450 | 
| 456 	| 003 	| 2023-05-02 	| $150 | 
| 456 	| 004 	| 2023-05-04	| $200 | 

Neste caso, `CustomerID` é a chave de partição e `OrderID` é a chave de classificação. Isso significa que os pedidos são organizados primeiro por `CustomerID` e, dentro de cada `CustomerID`, são classificados pelo `OrderID`.

## Consultas e Vantagens
* **Chave de Partição Única:** Consultas com base na chave de partição são rápidas e eficientes, pois o DynamoDB sabe exatamente em qual partição procurar.

**Exemplo de consulta:**

```python
response = table.get_item(Key={'UserID': '12345'})
```


* **Chave de Partição e Classificação:** Consultas podem filtrar e ordenar itens dentro da mesma partição, tornando-as mais flexíveis.

**Exemplo de consulta:**
```python
response = table.query(
    KeyConditionExpression=Key('CustomerID').eq('123') & Key('OrderID').begins_with('00')
)
```

## Características Avançadas

* **Escalabilidade Automática:** DynamoDB ajusta automaticamente a capacidade de leitura e escrita da tabela para acomodar mudanças de tráfego.
* **Consistência:** Oferece consistência eventual e consistência forte nas leituras.
* **Streams:** DynamoDB Streams captura uma sequência de mudanças em uma tabela, permitindo processamentos de eventos em tempo real.
* **Transações:** Permite realizar operações atômicas em múltiplos itens em uma ou mais tabelas.
* **Segurança:** Integração com AWS Identity and Access Management (IAM) para controle de acesso e criptografia de dados em trânsito e em repouso.

## Modelos de consistência de leitura

O DynamoDB, um serviço de banco de dados NoSQL gerenciado pela AWS, oferece dois modelos de consistência de leitura para garantir a integridade dos dados: a leitura fortemente consistente (strongly consistent read) e a leitura eventualmente consistente (eventually consistent read). Esses modelos diferem em termos de desempenho, custo e garantias de consistência dos dados.

Leitura Eventualmente Consistente
A leitura eventualmente consistente é o padrão no DynamoDB e é a mais usada por causa de seu melhor desempenho e menor custo em comparação com a leitura fortemente consistente. Nesse modelo, quando uma leitura é realizada, a resposta pode não refletir imediatamente o resultado de uma operação de escrita recente. No entanto, os dados eventualmente se tornam consistentes em um curto período de tempo (normalmente dentro de um segundo).

Características:
Performance: Maior throughput e menor latência.
Custo: Menor custo de leitura em termos de unidades de leitura.
Consistência: Pode retornar dados desatualizados se uma gravação recente foi realizada.
Leitura Fortemente Consistente
A leitura fortemente consistente garante que uma leitura reflete o resultado de todas as gravações que receberam uma confirmação bem-sucedida antes da leitura. Ou seja, uma leitura fortemente consistente sempre retorna os dados mais atualizados e confirmados.

Características:
Performance: Menor throughput e maior latência em comparação com a leitura eventualmente consistente.
Custo: Maior custo de leitura em termos de unidades de leitura.
Consistência: Garante a leitura dos dados mais recentes e confirmados.
Comparação dos Modelos
Característica	Leitura Eventualmente Consistente	Leitura Fortemente Consistente
Performance	Maior throughput, menor latência	Menor throughput, maior latência
Custo	Menor	Maior
Consistência	Pode ser desatualizada	Sempre atualizada
Uso típico	Aplicações que podem tolerar dados levemente desatualizados	Aplicações que precisam de dados sempre consistentes
Considerações Práticas
Escolha de Consistência: A escolha entre esses modelos deve ser baseada nos requisitos específicos de sua aplicação. Se a aplicação pode tolerar leituras que não estão imediatamente atualizadas (como em muitos cenários de análise de dados, dashboards, etc.), a leitura eventualmente consistente é mais adequada. No entanto, se a aplicação exige que as leituras reflitam as últimas gravações confirmadas (como em sistemas de transações financeiras, inventário em tempo real, etc.), a leitura fortemente consistente é a melhor opção.

Custo e Desempenho: Em sistemas de alta carga, optar por leituras eventualmente consistentes pode resultar em economias significativas e maior desempenho, enquanto a leitura fortemente consistente, embora mais cara, oferece a garantia necessária para aplicações críticas.

Exemplo de Uso
Ao configurar uma operação de leitura no DynamoDB, a escolha do modelo de consistência pode ser feita especificando o parâmetro ConsistentRead na API de leitura do DynamoDB. Por exemplo:

python
Copiar código
import boto3

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('MinhaTabela')

# Leitura eventualmente consistente
response = table.get_item(
    Key={'MeuID': '123'}
)

# Leitura fortemente consistente
response = table.get_item(
    Key={'MeuID': '123'},
    ConsistentRead=True
)
Conclusão
O DynamoDB oferece flexibilidade com seus modelos de consistência, permitindo que os desenvolvedores escolham entre leitura eventualmente consistente e leitura fortemente consistente conforme as necessidades específicas de suas aplicações. Essa flexibilidade ajuda a equilibrar a necessidade de desempenho, custo e integridade dos dados em diferentes cenários de uso.
