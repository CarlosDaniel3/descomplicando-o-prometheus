# Tipos de Dados

# Gauge (Medidor)
Utilizado para criar métricas com valores que podem aumentar ou diminuir.

Exemplos: Consumo de memória ou CPU e temperatura de uma cidade

Exemplo prático de uma métrica que mostra a utilização de memória:
```
memory_usage{instance="localhost:8899",job="Primeiro Exporter"}
```

# Counter (Contador)
Valor que será incrementado de maneira cumulativa no decorrer do tempo.

Exemplo: número total de solicitações HTTP recebidas por um servidor nas últimas 2 horas. O valor atual do mesmo raramente é importante porque na maioria das vezes são necessários apenas os valores em uma determinada janela de tempo. Geralmente as métricas counter apresentam o sufixo _total para mostrar que o total de valores foram contados

Exemplo: Counter que contabiliza o total de requisições:

```
requests_total{instance="localhost:8899",job="Primeiro Exporter"}
```

# Histogram (Histograma)
Tipo de dado que armazena os dados em buckets(baldes) com valores predefinidos. Os valores são em segundos e o valor máximo é de 10 segundos mas é possível a criação de buckets personalizados.

Exemplo: Tempo de execução de uma aplicação, todas as requisições que uma aplicação respondeu entre 0 e 0.5 segundos e requisições que tiveram respostas entre 1.5 e 3.0 segundos etc


É importante lembrar que o Prometheus irá contar cada item em cada bucket, e também a soma dos valores. Uma métrica do tipo histogram inclui alguns itens importantes que são adicionados ao final do nome da métrica para indicar o tipo de dado e o tamanho do bucket, por exemplo:

```
requests_duration_seconds_bucket{le="0.5"}
```

Onde: 
* "le" define o valor máximo do bucket
* 0.5 aponta que esse valor é até 0.5 segundos. Ou seja, requisições que tiveram respostas entre 0 e 0.5 segundos
* Como o próprio nome já diz, o sufixo _bucket aponta que o valor é um bucket

Também existem outros sufixos que podem ser importantes como:

_count (contador): o valor é incrementado a cada vez que a métrica é atualizada

_sum (soma): o valor é somado a cada vez que a métrica é atualizada. 

Vantagens:
* É bastante flexível porque os percentuais e as janelas de tempos podem ser definidas durante a criação das queries

Desvantagens:
* Possui uma baixa precisão

### Summary (Resumo)
É bem semelhante ao Histogram, a diferença é que os buckets nesse caso são chamados de quantiles e definidos por um valor entre 0 e 1. O valor do bucket é o valor que está entre os quantiles. Como no Histogram, é possível criar métricas do tipo Summary com alguns itens importantes adicionados ao final do nome da métrica.

Exemplo:

```
requests_duration_seconds_sum{instance="localhost:8899",job="Primeiro Exporter"}
```

Onde:
_ sum: aponta que o valor é uma soma, que significa que ele é somado a cada vez que a métrica é atualizada. 

_ count: aponta que o valor é um contador. que significa que ele é incrementado a cada vez que a métrica é atualizada

Vantagens:
* Alta precisão

Desvantagens:
* Baixa flexibilidade porque os percentuais e as janelas de tempos precisam ser definidos durante a criação da métrica e não é possível agregar métricas do tipo summary com outras métricas do tipo summary durante a criação das queries.  
