---
title: "Como fazer SWAP"
slug: como-fazer-SWAP
date: 2026-02-02
tags: tutorial, linux
author: fabricio
---

# Como fazer SWAP

## O que é SWAP?

A swap é uma extensão temporária da sua memória RAM, feita usando o espaço do seu SSD ou HD tipo uma "memória RAM fake".

Quando a RAM enche, o Linux começa a jogar dados "menos usados" pra esse espaço da swap, pra liberar RAM pra o que é mais urgente.

"E se eu não fizer isso?", você pode questionar. O Linux é um sistema que obedece cegamente, caso você esgote a memória RAM e não tenha uma swap configurada, ele simplismente vai travar o sistema e você vai ter que reiniciar sua máquina. Digo isso por experiência própria...

"Por qual motivo eu nunca ouvi falar disso?", talvez você esteja se questionando. Se você está nesse artigo então é grande a possibilidade de você ser recém-chegado no mundo Linux migrando do Ruindows, bem provavelmente. Os outros sistemas fazem esse gerenciamento de forma automática e você não precisa nem saber que isso existe.

## Como a SWAP funciona na prática?

O sistema monitora o uso da RAM.

Quando a RAM se aproxima do limite (dependendo do valor de `swappiness`), ele começa a mover partes da memória pouco usadas pra swap. Essas partes continuam acessíveis, mas ficam no SSD/HD (muito mais lento que a RAM). Isso evita travamentos e aumenta a estabilidade, principalmente em multitarefa.

### Observações importantes!

**Swap é mais lenta que RAM.**\
Porque seu SSD (ou pior, um HD) é muito mais lento que RAM. Se o sistema começar a depender demais da swap, você sente um lag violento.

**Swap constante = desgaste do SSD.**\
Swap em SSD não é o fim do mundo, mas se ela for usada o tempo todo, vai acelerar o desgaste. Por isso é bom manter a RAM "limpa" e limitar o uso da swap com `swappiness`.

## Configurando SWAP

Rode o comando:
```
swapon --show
```

Para verificar ter certeza se você tem uma swap, mas 90% de chance de não ter.\
Se você não tem swap não terá nenhum retorno, mas se você tiver swap irá ter retorno semelhante a este:
```
NAME      TYPE SIZE USED PRIO
/swapfile file   8G   3G   -2
```

#### Criar/ativar SWAP file (se não tiver)

##### Comando completo:
```
# Cria um swap de 4GB (ajuste se quiser)
sudo fallocate -l 4G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
```

Depois adicione no fstab para ativar automaticamente:
```
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

##### Comando com explicações:


Criar um arquivo de swap com 4 GB
```
sudo fallocate -l 4G /swapfile
```
Criamos um arquivo especial de 4 GB chamado `/swapfile`, que será usado como “RAM falsa” quando a memória real estiver cheia.

Restringir o acesso ao arquivo de swap
```
sudo chmod 600 /swapfile
```
Demos permissão de leitura/escrita só pro root, por segurança. Isso impede que outro processo leia ou altere esse arquivo diretamente.

Formatar esse arquivo como uma área de swap
```
sudo mkswap /swapfile
```

Dizemos ao sistema:
“Esse arquivo aqui é uma área de swap, tá ok? 👉👉 ”\
Ele cria a estrutura de controle necessária.

Ativar a swap agora
```
sudo swapon /swapfile
```

Ligamos a swap imediatamente.\
A partir desse momento, o sistema pode começar a usar esse arquivo como memória auxiliar.\

Configurar para ativar automaticamente no boot
```
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

Alteramos o arquivo `/etc/fstab` (que diz o que montar no boot), pra garantir que **sempre que você reiniciar, a swap vai estar ativa.**

Por padrão, o Linux só usa a swap quando a RAM já está no limite. Você pode mudar isso pra ele ser um pouco mais "esperto" e já ir aliviando antes de travar tudo.

Rode:

```
cat /proc/sys/vm/swappiness
```
Para verificar o valor atual. Provavelmente `60`

Reduza o lag e evitar travamento total, coloca algo tipo `20`:

```
echo 'vm.swappiness=20' | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```
Isso faz o sistema usar a swap de forma mais equilibrada com a RAM. Se quiser testar o comportamento, você pode começar com `30` e ir ajustando depois.