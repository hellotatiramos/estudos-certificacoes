# DVA-C02 - AWS Certified Developer - Associate

## Amazon DynamoDB
<div align="center">
<img src="https://github.com/hellotatiramos/estudos-certificacoes/assets/158481113/8513ce66-f3fd-4d9f-9350-0e14b20292b9" width="700px" />
</div>

## Principais Conceitos
Tabelas: A unidade básica de armazenamento no DynamoDB. Cada tabela contém múltiplos itens e cada item é um conjunto de atributos.
Itens: Análogos às linhas em um banco de dados relacional. Cada item é identificado de forma única por uma chave primária.
Atributos: Análogos às colunas em um banco de dados relacional. São os dados associados a um item.
Chave Primária: Pode ser uma chave de partição (hash) ou uma combinação de chave de partição e chave de classificação (hash-range).
Índices Secundários: Permitem consultas eficientes usando atributos que não fazem parte da chave primária.

# Operações Principais da API

## Control Plane 
O "control plane" (plano de controle) refere-se às operações administrativas e de gerenciamento de um serviço. No caso do DynamoDB, essas operações incluem:

* **CreateTable:** Cria uma nova tabela no DynamoDB.
* **DescribeTable:** Obtém informações sobre a estrutura e configurações de uma tabela.
* **ListTables:** Lista todas as tabelas em uma conta do DynamoDB.
* **UpdateTable:** Atualiza as configurações de uma tabela existente, como throughput provisionado, índices ou definições de atributo.
* **DeleteTable:** Exclui uma tabela do DynamoDB.

## Data Plane
O "data plane" (plano de dados) refere-se às operações relacionadas à manipulação dos dados propriamente ditos, ou seja, a leitura e escrita dos dados armazenados no DynamoDB. Essas operações são muito mais frequentes que as operações do control plane e incluem:

* **PutItem:** Insere um único item ou substitui um item existente em uma tabela.
* **BatchWriteItem:** Insere até 25 itens em uma ou mais tabelas em uma única chamada.
* **GetItem:** Recupera um item de uma tabela com base na chave primária.
* **BatchGetItem:** Recupera até 100 itens de uma ou mais tabelas em uma única chamada.
* **UpdateItem:** Atualiza um atributo de um item existente em uma tabela.
* **DeleteItem:** Exclui um item de uma tabela com base na chave primária.

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

```response = table.get_item(Key={'UserID': '12345'})```



* **Chave de Partição e Classificação:** Consultas podem filtrar e ordenar itens dentro da mesma partição, tornando-as mais flexíveis.

**Exemplo de consulta:**

`response = table.query(
    KeyConditionExpression=Key('CustomerID').eq('123') & Key('OrderID').begins_with('00')
)`

## Características Avançadas

Escalabilidade Automática: DynamoDB ajusta automaticamente a capacidade de leitura e escrita da tabela para acomodar mudanças de tráfego.
Consistência: Oferece consistência eventual e consistência forte nas leituras.
Streams: DynamoDB Streams captura uma sequência de mudanças em uma tabela, permitindo processamentos de eventos em tempo real.
Transações: Permite realizar operações atômicas em múltiplos itens em uma ou mais tabelas.
Segurança: Integração com AWS Identity and Access Management (IAM) para controle de acesso e criptografia de dados em trânsito e em repouso.
