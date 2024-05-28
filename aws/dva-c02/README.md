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

### Leitura Eventualmente Consistente
A leitura eventualmente consistente é o padrão no DynamoDB e é a mais usada por causa de seu melhor desempenho e menor custo em comparação com a leitura fortemente consistente. Nesse modelo, quando uma leitura é realizada, a resposta pode não refletir imediatamente o resultado de uma operação de escrita recente. No entanto, os dados eventualmente se tornam consistentes em um curto período de tempo (normalmente dentro de um segundo).

#### Características:
* **Performance:** Maior throughput e menor latência.
* **Custo:** Menor custo de leitura em termos de unidades de leitura.
* **Consistência:** Pode retornar dados desatualizados se uma gravação recente foi realizada.

### Leitura Fortemente Consistente
A leitura fortemente consistente garante que uma leitura reflete o resultado de todas as gravações que receberam uma confirmação bem-sucedida antes da leitura. Ou seja, uma leitura fortemente consistente sempre retorna os dados mais atualizados e confirmados.

#### Características:
* **Performance: Menor throughput e maior latência em comparação com a leitura eventualmente consistente.
* **Custo: Maior custo de leitura em termos de unidades de leitura.
* **Consistência: Garante a leitura dos dados mais recentes e confirmados.
  
### Comparação dos Modelos
| Característica | Leitura Eventualmente Consistente |	Leitura Fortemente Consistente
| ------|-----|-----|
Performance |	Maior throughput, menor latência |	Menor throughput, maior latência |
Custo	| Menor	| Maior |
Consistência |	Pode ser desatualizada |	Sempre atualizada
Uso típico |	Aplicações que podem tolerar dados levemente desatualizados |	Aplicações que precisam de dados sempre consistentes |

### Considerações Práticas

* **Escolha de Consistência:** A escolha entre esses modelos deve ser baseada nos requisitos específicos de sua aplicação. Se a aplicação pode tolerar leituras que não estão imediatamente atualizadas (como em muitos cenários de análise de dados, dashboards, etc.), a leitura eventualmente consistente é mais adequada. No entanto, se a aplicação exige que as leituras reflitam as últimas gravações confirmadas (como em sistemas de transações financeiras, inventário em tempo real, etc.), a leitura fortemente consistente é a melhor opção.

* **Custo e Desempenho:** Em sistemas de alta carga, optar por leituras eventualmente consistentes pode resultar em economias significativas e maior desempenho, enquanto a leitura fortemente consistente, embora mais cara, oferece a garantia necessária para aplicações críticas.

### Exemplo de Uso
Ao configurar uma operação de leitura no DynamoDB, a escolha do modelo de consistência pode ser feita especificando o parâmetro ConsistentRead na API de leitura do DynamoDB. Por exemplo:

```python
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
```

### Conclusão
O DynamoDB oferece flexibilidade com seus modelos de consistência, permitindo que os desenvolvedores escolham entre leitura eventualmente consistente e leitura fortemente consistente conforme as necessidades específicas de suas aplicações. Essa flexibilidade ajuda a equilibrar a necessidade de desempenho, custo e integridade dos dados em diferentes cenários de uso.


## Transações no DynamoDB

As transações no DynamoDB são uma funcionalidade avançada que permite a execução de operações atômicas em múltiplos itens em uma ou mais tabelas do DynamoDB. Elas garantem que todas as operações em uma transação sejam bem-sucedidas ou nenhuma delas seja aplicada, proporcionando uma maneira robusta de manter a consistência dos dados em cenários complexos.

### Características das Transações no DynamoDB

1. **Atomicidade:** Todas as operações dentro de uma transação são atômicas. Isso significa que ou todas as operações são bem-sucedidas, ou nenhuma delas é aplicada. Isso é crucial para manter a consistência dos dados em aplicações complexas.

2. **Isolamento:** As transações são isoladas, o que garante que outras operações de leitura e gravação não possam interferir com uma transação em andamento. Isso proporciona uma visão consistente dos dados enquanto a transação está sendo processada.

3. **Consistência:** As transações no DynamoDB garantem consistência forte, assegurando que todas as leituras realizadas durante a transação reflitam todas as gravações que foram feitas com sucesso dentro dessa transação.

4. **Suporte a múltiplas tabelas:** É possível realizar operações em múltiplas tabelas dentro de uma única transação, oferecendo flexibilidade para manter a consistência dos dados em diferentes partes do seu banco de dados.

### Operações Transacionais
O DynamoDB suporta duas operações principais para transações: `TransactWriteItems` e `TransactGetItems`.

#### TransactWriteItems
Essa operação permite executar múltiplas operações de escrita em uma única transação. As operações suportadas incluem:

`Put`: Adicionar um novo item ou substituir um item existente.
`Update`: Atualizar um item existente.
`Delete`: Excluir um item.
`ConditionCheck`: Verificar se uma condição é verdadeira antes de executar uma operação de escrita.

Exemplo de uso do `TransactWriteItems`:

```python
import boto3

dynamodb = boto3.client('dynamodb')

response = dynamodb.transact_write_items(
    TransactItems=[
        {
            'Put': {
                'TableName': 'MinhaTabela',
                'Item': {
                    'PK': {'S': 'Chave1'},
                    'Atributo1': {'S': 'Valor1'}
                }
            }
        },
        {
            'Update': {
                'TableName': 'OutraTabela',
                'Key': {
                    'PK': {'S': 'Chave2'}
                },
                'UpdateExpression': 'SET Atributo2 = :val',
                'ExpressionAttributeValues': {
                    ':val': {'S': 'NovoValor'}
                }
            }
        }
    ]
)
```

#### TransactGetItems
Essa operação permite ler múltiplos itens de uma ou mais tabelas dentro de uma única transação, garantindo que as leituras são consistentes e refletindo as gravações feitas dentro da mesma transação.

Exemplo de uso do `TransactGetItems`:

```python
response = dynamodb.transact_get_items(
    TransactItems=[
        {
            'Get': {
                'TableName': 'MinhaTabela',
                'Key': {
                    'PK': {'S': 'Chave1'}
                }
            }
        },
        {
            'Get': {
                'TableName': 'OutraTabela',
                'Key': {
                    'PK': {'S': 'Chave2'}
                }
            }
        }
    ]
)
```

### Limitações e Considerações
* **Tamanho da Transação:** Cada transação pode conter até 25 operações de leitura ou escrita e até 4 MB de dados.
* **Consumo de Recursos:** Operações transacionais consomem mais unidades de capacidade de leitura e escrita em comparação com operações não transacionais.
* **Desempenho:** As transações podem introduzir uma latência adicional devido à complexidade de garantir atomicidade e consistência.

### Uso Comum
As transações no DynamoDB são úteis em cenários onde a consistência dos dados é crítica, como:

* Processamento de pagamentos
* Gerenciamento de inventário
* Aplicações de reserva de recursos
* Cenários de manutenção de integridade referencial entre tabelas

### Conclusão
As transações no DynamoDB fornecem uma poderosa ferramenta para garantir a consistência e integridade dos dados em operações complexas. Elas são especialmente úteis em aplicações onde operações atômicas e consistência forte são necessárias, oferecendo flexibilidade para lidar com múltiplos itens e tabelas dentro de uma única operação transacional.


## Desempenho e Throttling no DynamoDB

O desempenho e o gerenciamento de throttling no DynamoDB são aspectos cruciais para garantir que suas aplicações funcionem de maneira eficiente e confiável. Vamos explorar esses conceitos em detalhes.

### Desempenho no DynamoDB

#### Throughput Consistente
O DynamoDB é projetado para fornecer desempenho previsível e consistente em qualquer escala. Com capacidade provisionada, você especifica a quantidade de throughput necessária em termos de unidades de capacidade de leitura (RCU) e escrita (WCU). Com isso, a AWS garante que você terá a capacidade provisionada disponível para suas operações.

#### Latência Baixa
O DynamoDB é otimizado para operações de baixa latência, geralmente em milissegundos. Mesmo com grandes volumes de dados e altas taxas de solicitações, o DynamoDB mantém a latência baixa, o que é crítico para aplicações em tempo real.

#### Escalabilidade
O DynamoDB pode escalar automaticamente para lidar com picos de tráfego usando o recurso de auto scaling. Isso garante que as tabelas possam aumentar ou diminuir a capacidade de leitura e escrita conforme a demanda muda, sem intervenção manual.

### Throttling no DynamoDB

#### Conceito de Throttling
O throttling ocorre quando a taxa de solicitações excede a capacidade provisionada para leitura ou escrita. Quando isso acontece, o DynamoDB rejeita as solicitações adicionais, retornando erros de throughput excedido (HTTP 400). Throttling é uma medida de proteção para garantir que as tabelas do DynamoDB não sejam sobrecarregadas e possam manter um desempenho consistente.

#### Razões para Throttling

* **Capacidade Insuficiente:** A capacidade provisionada (RCUs e WCUs) não é suficiente para lidar com a carga atual.
* **Acessos Desiguais:** Padrões de acesso altamente desiguais, onde algumas chaves de partição são acessadas com muito mais frequência que outras (hot keys).
* **Operações em Lote:** Grandes operações em lote que excedem a capacidade de uma só vez.

### Mitigando Throttling

#### Monitoramento
Utilize o Amazon CloudWatch para monitorar métricas como `ConsumedReadCapacityUnits`, `ConsumedWriteCapacityUnits`, `ThrottledRequests`, e `ProvisionedThroughputExceeded`. Isso ajuda a identificar quando e onde o throttling está ocorrendo.

#### Auto Scaling
Configurar auto scaling para ajustar automaticamente a capacidade provisionada com base na utilização real pode ajudar a evitar throttling. Defina políticas de auto scaling que aumentem a capacidade quando a utilização atinge um certo limiar e reduza quando a demanda diminui.

#### Design de Partição
Projetar a tabela com uma chave de partição que distribua uniformemente a carga entre as partições. Isso evita hot keys e ajuda a manter o throughput distribuído de maneira uniforme.

#### Operações de Leitura/Escrita Otimizadas

* **Batch Operations:** Dividir operações em lote em partes menores para distribuir a carga.
* **Exponential Backoff:** Implementar lógica de retry com backoff exponencial para operações que falham devido a throttling. Isso ajuda a reduzir a carga imediata e espalhar as solicitações ao longo do tempo.

### Exemplo de Monitoramento e Auto Scaling
#### Monitoramento com CloudWatch:

```python
import boto3

cloudwatch = boto3.client('cloudwatch')

response = cloudwatch.get_metric_statistics(
    Namespace='AWS/DynamoDB',
    MetricName='ThrottledRequests',
    Dimensions=[
        {
            'Name': 'TableName',
            'Value': 'MinhaTabela'
        },
    ],
    StartTime=datetime.utcnow() - timedelta(minutes=5),
    EndTime=datetime.utcnow(),
    Period=60,
    Statistics=['Sum']
)

for point in response['Datapoints']:
    print(f"Time: {point['Timestamp']}, Throttled Requests: {point['Sum']}")
```

#### Configuração de Auto Scaling:

```python
import boto3

application_autoscaling = boto3.client('application-autoscaling')

response = application_autoscaling.register_scalable_target(
    ServiceNamespace='dynamodb',
    ResourceId='table/MinhaTabela',
    ScalableDimension='dynamodb:table:ReadCapacityUnits',
    MinCapacity=5,
    MaxCapacity=50
)

response = application_autoscaling.put_scaling_policy(
    PolicyName='MinhaTabelaReadAutoScalingPolicy',
    ServiceNamespace='dynamodb',
    ResourceId='table/MinhaTabela',
    ScalableDimension='dynamodb:table:ReadCapacityUnits',
    PolicyType='TargetTrackingScaling',
    TargetTrackingScalingPolicyConfiguration={
        'TargetValue': 70.0,
        'PredefinedMetricSpecification': {
            'PredefinedMetricType': 'DynamoDBReadCapacityUtilization'
        },
        'ScaleInCooldown': 60,
        'ScaleOutCooldown': 60
    }
)
```

### Conclusão
Entender e gerenciar o desempenho e throttling no DynamoDB é essencial para manter suas aplicações rodando de maneira eficiente e econômica. Ao usar monitoramento, auto scaling, e boas práticas de design de tabelas, você pode minimizar o impacto do throttling e garantir um desempenho consistente e previsível.

## Operações de leitura `Query` e `Scan`
No DynamoDB, as operações de leitura podem ser realizadas principalmente usando os métodos Query e Scan. Ambos têm propósitos distintos e são usados em diferentes cenários, dependendo das necessidades da aplicação. A seguir, vamos explorar cada um deles e destacar suas principais diferenças.

### Operação Query
A operação `Query` é usada para encontrar itens em uma tabela do DynamoDB usando apenas a chave de partição (e opcionalmente a chave de classificação, se existir). Essa operação é mais eficiente porque usa índices para buscar diretamente os itens, resultando em menor consumo de recursos e latência.

#### Características do Query:

* **Chave de Partição:** A Query requer que você especifique a chave de partição. Você pode filtrar adicionalmente por chave de classificação se a tabela usar uma chave de classificação composta.
* **Filtros:** Pode aplicar condições adicionais usando a expressão KeyConditionExpression para a chave de classificação e FilterExpression para outros atributos.
* **Eficiência:** Mais eficiente em termos de desempenho e custo, pois usa índices para acessar diretamente os itens.
* **Ordenação:** Suporta ordenação dos resultados com base na chave de classificação (ascendente ou descendente).

#### Exemplo de Query:

```python
import boto3

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('MinhaTabela')

response = table.query(
    KeyConditionExpression=Key('UserId').eq('1234') & Key('Timestamp').between('2023-01-01', '2023-01-31')
)

for item in response['Items']:
    print(item)
```

### Operação Scan

A operação `Scan` é usada para examinar todos os itens de uma tabela ou de um índice secundário global. `Scan` lê cada item na tabela e aplica quaisquer filtros especificados, o que pode resultar em alto consumo de recursos, especialmente para grandes tabelas.

#### Características do Scan:
* **Examina todos os itens:** A Scan examina todos os itens em uma tabela ou índice, independentemente da chave de partição.
* **Filtros:** Pode aplicar filtros com FilterExpression para retornar apenas itens que correspondam aos critérios especificados.
* **Consumo de Recursos:** Menos eficiente em termos de desempenho e custo, pois precisa ler todos os itens na tabela, potencialmente consumindo muitas unidades de capacidade de leitura (RCUs).
* **Uso de Paginação:** Em tabelas grandes, a operação Scan pode ser paginada para limitar a quantidade de dados processados em uma única operação.
  
#### Exemplo de Scan:

```python
import boto3

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('MinhaTabela')

response = table.scan(
    FilterExpression=Attr('Status').eq('Active')
)

for item in response['Items']:
    print(item)
```

### Comparação entre Query e Scan
| Característica |	Query |	Scan |
| ------|-----|-----|
| Eficiência |	Alta (usa índices) |	Baixa (lê todos os itens) |
| Uso de Índices	| Sim |	Não |
| Chave de Partição |	Requerida |	Não é necessária |
| Filtros | Suporta `KeyConditionExpression` e `FilterExpression`	| Suporta apenas `FilterExpression`|
| Latência |	Baixa	| Alta |
| Consumo de RCUs |	Baixo (eficiente)	| Alto (ineficiente) |
| Ordenação |	Suporta ordenação pela chave de classificação |	Não suporta ordenação| 
| Aplicação Típica |	Buscar itens específicos com base na chave de partição e classificação | Ler todos os itens de uma tabela, aplicar filtros |

### Quando Usar Cada Operação

#### Use `Query` quando:
* Você sabe a chave de partição dos itens que deseja buscar.
* Deseja obter resultados de maneira eficiente e rápida.
* Precisa ordenar os resultados ou fazer consultas complexas com base na chave de classificação.

#### Use `Scan` quando:
* Precisa ler todos os itens de uma tabela para análise completa.
* Está fazendo operações de manutenção ou migração de dados.
* Não tem uma chave de partição específica para suas consultas.

#### Conclusão
Entender as diferenças entre `Query` e `Scan` no DynamoDB é essencial para otimizar o desempenho e o custo de suas operações de leitura. `Query`  é a escolha preferida para acessos rápidos e eficientes quando se conhece a chave de partição, enquanto `Scan` é usado em cenários onde uma varredura completa da tabela é necessária. Escolher a operação correta pode fazer uma grande diferença na eficiência e escalabilidade de sua aplicação.
