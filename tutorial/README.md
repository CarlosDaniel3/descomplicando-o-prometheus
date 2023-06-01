# Tutorial de Instalação do Prometheus no Linux

O exemplo será feito em uma máquina Ubuntu, mas pode ser replicado em qualquer outra Distribuição Linux.

O primeiro passo será realizar o download dos 2 binários necessários (o do Prometheus e o do Promtool). O Promtool permite a execução de queries no Prometheus via terminal e o Prometheus server faz todo o processo acontecer. Além dos binários, também iremos trabalhar com o diretório consoles, console_libraries e com o arquivo de configuração do prometheus: o prometheus.yml. 
Agora basta seguir os seguintes passos:

Download:

```
curl -LO https://github.com/prometheus/prometheus/releases/download/v2.38.0/prometheus-
2.38.0.linux-amd64.tar.gz
```

Extrair os arquivos:
```
tar -xvf prometheus-2.38.0.linux-amd64.tar.gz
```

Mover os binários para o diretório /usr/local/bin:
```
sudo mv prometheus-2.38.0.linux-amd64/prometheus /usr/local/bin/prometheus

sudo mv prometheus-2.38.0.linux-amd64/promtool /usr/local/bin/promtool
```

Verificar se o binário está funcionando:
```
prometheus --version
```

Criar o diretório de configuração do Prometheus:
```
sudo mkdir /etc/prometheus
```

Mover os diretórios consoles, console_libraries e o arquivo prometheus.yml para o diretório de configuração do Prometheus.
```
sudo mv prometheus-2.38.0.linux-amd64/prometheus.yml /etc/prometheus/prometheus.yml

sudo mv prometheus-2.38.0.linux-amd64/consoles /etc/prometheus

sudo mv prometheus-2.38.0.linux-amd64/console_libraries /etc/prometheus
```

Editar os arquivos de configuração do Prometheus:
```
sudo vim /etc/prometheus/prometheus.yml
```