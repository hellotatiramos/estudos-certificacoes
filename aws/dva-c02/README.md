# DVA-C02 - AWS Certified Developer - Associate

## Amazon DynamoDB
<div align="center">
<img src="https://github.com/hellotatiramos/estudos-certificacoes/assets/158481113/8513ce66-f3fd-4d9f-9350-0e14b20292b9" width="700px" />
</div>

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

