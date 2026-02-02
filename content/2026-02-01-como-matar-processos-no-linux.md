---
title: "Como matar processos no linux"
slug: como-matar-processos-no-linux
date: 2026-02-01
tags: tutorial, linux,
author: fabricio
---

É um tutorial de algo besta, mas eu sempre acabo esquecendo como faz, então vou salvar aqui

Lista todos os processo que estão rodando

```bash
sudo netstats -tlpn
```
Lista todos os processos que estão rodando na porta <número>
```bash
sudo netstats -tlpn | grep <número>
```
Mata processo que está rodando na porta 8000
```bash
sudo kill -9 `sudo lsof -t -i:<número>`
sudo kill -9 $(lsof -t -i:<número>)
```

obs.: <número> deve ser todo incluindo o <> ser torcado pelo número