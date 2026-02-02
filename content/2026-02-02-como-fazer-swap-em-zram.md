---
title: "Como fazer swap em Zram"
slug: como-fazer-swap-em-zram
date: 2026-02-02
tags: linux, tutorial
author: carlos


## Como Fazer Swap em Zram

Swap é muito útil no mundo Linux. Como dito em Como fazer swap
, do Fabrício, o artigo apenas ensina como fazer swap via swapfile em distribuições médias. Porém, aqui serei mais complexo no ramo de swap. Sugiro que vejam primeiro o blog do Fabrício antes deste.

## Zram

O Zram (anteriormente compcache) é um módulo do kernel Linux que cria um dispositivo de bloco comprimido na RAM, funcionando como um “disco” de troca (swap) virtual. Ao comprimir dados em tempo real na memória, ele evita o uso do disco rígido ou SSD, que são mais lentos.

- Em resumo, o Zram é muito mais rápido do que swapfile.

- É muito útil em sistemas rolling release, pois faz bastante diferença na velocidade do sistema, como no Fedora ou Arch Linux.

## Arch Linux

No Arch Linux temos o zram-generator. Certifique-se de que ele está instalado:
```

sudo pacman -S zram-generator
```

Crie o arquivo de configuração em `/etc/systemd/zram-generator.conf`:
```
sudo vim /etc/systemd/zram-generator.conf
```

Ou, se preferir o Nano:
```
sudo nano /etc/systemd/zram-generator.conf
```

Dentro do arquivo zram-generator.conf, coloque:

Para abordagem automática
```
[zram0]
``` 
## Sugestão de tamanhos de Zram

Se você tem 4 GB de RAM, use 2,5 GB de Zram ou igual
Se tem 8 GB de RAM, use 4 GB de swap ou igual
Se tem 16 GB de RAM, use 8 GB ou 12 GB de Zram
Se tem 32 GB de RAM, use 4 GB ou 8 GB, apenas para manter

``` 
Obs.: O Zram é útil em máquinas com mais de 16 GB de RAM, pois quando a RAM chega a 100%, o Zram ou swapfile entra em ação para evitar travamentos. Quando o sistema não tem swap, ele pode simplesmente travar e fechar processos.
``` 
Exemplo de configuração manual:
``` 
[zram0]
zram-size = 4096  # exemplo de 4 GB de swap
```

Sempre use:
```
zram-size = {RAM} x 1024
```

Exemplo: `8 GB = 8192`

## 50% da ram

No mesmo arquivo, use:
```
zram-size = ram / 2
``` 
Isso define o Zram como 50% da RAM total.
## Config recomendada
```
[zram0]
zram-size = ram / 2
compression-algorithm = zstd
swap-priority = 100

```
rapido e util 


## Aplicar

```
sudo systemctl daemon-reexec
sudo systemctl start systemd-zram-setup@zram0.service
```

apos o reinicio execute:

```
swapon --show
```
