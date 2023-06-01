# Tutorial de Instalação e Configuração do Prometheus no Linux

O exemplo será feito em uma máquina Ubuntu, mas pode ser replicado em qualquer outra Distribuição Linux.

O primeiro passo será realizar o download dos 2 binários necessários (o do Prometheus e o do Promtool). O Promtool permite a execução de queries no Prometheus via terminal e o Prometheus server faz todo o processo acontecer. Além dos binários, também iremos trabalhar com o diretório consoles, console_libraries e com o arquivo de configuração do prometheus: o prometheus.yml. 
Agora basta seguir os seguintes passos:

Observação: Todos os arquivos de configuração do Prometheus estarão disponíveis no diretório conf desse repositório

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

Criar o arquivos de configuração do Prometheus:
```
sudo vim /etc/prometheus/prometheus.yml
```

Criar o diretório onde o Prometheus irá guardar seus dados
```
sudo mkdir /var/lib/prometheus
```

Criar um grupo e um usuário para o Prometheus
```
sudo addgroup --system prometheus
sudo adduser --shell /sbin/nologin --system --group prometheus
```

Já que o Prometheus será um serviço na máquina em que for instalado, agora é preciso criar o arquivo de service unit do SystemD:
```
sudo vim /etc/systemd/system/prometheus.service
```

Agora o próximo passo é mudar o dono desses diretórios e arquivos criados para que o usuário do prometheus seja o dono dos mesmos:
```
sudo chown -R prometheus:prometheus /var/log/prometheus

sudo chown -R prometheus:prometheus /etc/prometheus

sudo chown -R prometheus:prometheus /var/lib/prometheus

sudo chown -R prometheus:prometheus /usr/local/bin/prometheus

sudo chown -R prometheus:prometheus /usr/local/bin/promtool
```

Fazer um reload no systemd para que o serviço do Prometheus seja iniciado:
```
sudo systemctl daemon-reload
```

Iniciar o Prometheus:
```
sudo systemctl start prometheus
```

Configurar o serviço do Prometheus para que ele seja iniciado de forma automática ao iniciar o sistema:
```
sudo systemctl enable prometheus
```

Para garantir o funcionamento do serviço do Promethues, basta verificar o status do mesmo:
```
sudo systemctl status prometheus
```

Também é possível conferir por meio dos logs, que se tudo estiver funcionando corretamente, será a seguinte:
```
level=info msg="Server is ready to receive web requests."
```

Comando:
```
sudo journalctl -u prometheus
```

Para acessar a interface web do prometheus, digite o seguinte endereço em um navegador da sua preferência
```
http://localhost:9090
```
\
O resultado será essa linda interface mostrada abaixo:
\
![Interface do Prometheus](https://github.com/CarlosDaniel3/descomplicando-o-prometheus/blob/main/assets/prometheus-interface.png)
