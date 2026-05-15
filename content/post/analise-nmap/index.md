---
title: "Writeup Rápido: Análise de Reconhecimento de Rede com Nmap"
description: "Como identificar portas abertas e serviços vulneráveis num host."
date: 2026-05-15
categories:
    - Pentesting
    - Recon
tags:
    - Nmap
    - Networks
    - Lab
draft: false
---

## Objetivo do Laboratório
O objetivo deste exercício foi realizar um scan de reconhecimento (*recon*) num host de testes para mapear a superfície de ataque e identificar potenciais vetores de entrada.

---

## 1. Execução do Comando
Utilizei o Kali Linux para correr um scan agressivo, extraindo as versões dos serviços e o sistema operativo do alvo:

```bash
nmap -sV -sC -O -F 192.168.1.50
```

---

## 2. Resultados
O scan reportou as seguintes portas abertas:

| Porta | Estado | Serviço | Versão Detetada | Impacto de Segurança |
| :--- | :--- | :--- | :--- | :--- |
| **22/TCP** | Aberto | SSH | OpenSSH 8.2p1 | Seguro (se usar chaves em vez de password) |
| **80/TCP** | Aberto | HTTP | Apache httpd 2.4.41 | Potencial vetor se houver exploits públicos |
| **445/TCP**| Aberto | SMB | Samba 4.X | **Crítico:** Alvo comum para movimentação lateral |

---

## 3. Conclusão e Mitigação
A porta 445 (SMB) exposta representa o maior risco neste cenário. Como recomendação de mitigação imediata:

1. Isolar a porta 445 através de regras de Firewall;
2. Garantir que o protocolo SMBv1 está totalmente desativado, forçando o uso de SMBv2/v3 cifrado.