# 🌐 Modelo Hierárquico de 3 Camadas — Packet Tracer

> Da topologia "no papel" a uma rede funcional: montei uma rede com **Núcleo, Distribuição e Acesso** no Cisco Packet Tracer e validei a conectividade ponta a ponta.

![Cisco Packet Tracer](https://img.shields.io/badge/Ferramenta-Cisco%20Packet%20Tracer-1BA0D7?style=flat-square)
![Modelo](https://img.shields.io/badge/Modelo-Hier%C3%A1rquico%203%20Camadas-0A5F78?style=flat-square)
![Status](https://img.shields.io/badge/Status-Conclu%C3%ADdo-2E8B57?style=flat-square)

<p align="center">
  <img src="Gif%20do%20modelo%20H.gif" alt="Demonstração da topologia hierárquica no Packet Tracer" width="750">
</p>

---

## 📑 Índice

1. [Objetivo](#1-objetivo)
2. [Arquitetura da rede](#2-arquitetura-da-rede)
3. [Configuração](#3-configuração)
4. [O desafio: um link vermelho](#4-o-desafio-um-link-vermelho)
5. [Testes e resultados](#5-testes-e-resultados)
6. [O que aprendi](#6-o-que-aprendi)
7. [Arquivos do projeto](#7-arquivos-do-projeto)

---

## 1. Objetivo

Sair de uma topologia teórica e chegar a uma **rede realmente funcional**, usando o padrão de projeto que empresas de qualquer tamanho adotam na prática: o **Modelo Hierárquico de 3 Camadas**.

---

## 2. Arquitetura da rede

Separei a rede em três camadas, cada uma com uma função clara:

| Camada | Equipamento | Responsabilidade |
|:---:|---|---|
| 🔷 **Núcleo** | Roteador | Encaminhar tráfego rápido, **sem filtragem pesada** |
| 🔶 **Distribuição** | Multilayer Switch | Roteamento e **aplicação de políticas** |
| 🟩 **Acesso** | Switches 2960 | Ponto de conexão dos **PCs** |

**Endereçamento:** toda a rede usa um único bloco, `192.168.1.0/24`, **sem VLANs** nesta etapa.

---

## 3. Configuração

O roteador do Núcleo foi configurado via **CLI** com três passos básicos:

1. definir o **hostname**;
2. atribuir o **IP na interface**;
3. ativar a interface com **`no shutdown`**.

Modelo genérico dos comandos:

```
Router> enable
Router# configure terminal
Router(config)# hostname <nome>
<nome>(config)# interface <interface>
<nome>(config-if)# ip address <ip> <máscara>
<nome>(config-if)# no shutdown
```

---

## 4. O desafio: um link vermelho

Nos testes, **um dos links ficava vermelho** mesmo com o cabo correto.

### 🔍 Diagnóstico de baixo para cima

Segui a lógica de camadas, da física para a lógica:

1. **IP dos PCs:** conferi se o endereçamento estava correto.
2. **Status físico dos links:** verifiquei quais estavam ativos.
3. **Interface do roteador:** usei `show ip interface brief` para ver o estado da interface.
4. **Configuração lógica:** só depois revisei o restante.

### ✅ Causa raiz

Faltava **adicionar um módulo de rede manualmente** no equipamento, e isso precisa ser feito **com ele desligado**.

> 💡 Quando algo não funciona, começar pela camada física evita perder tempo revisando configurações que estavam certas.

---

## 5. Testes e resultados

| Teste | Resultado |
|---|:---:|
| PCs no **mesmo switch** | ✅ Ping com sucesso |
| PCs em **switches de Acesso diferentes** (passando pela Distribuição) | ✅ Ping com sucesso |
| **Todos os PCs** até o roteador do Núcleo | ✅ Ping com sucesso |

**Resultado final:** conectividade completa ponta a ponta.

---

## 6. O que aprendi

- **Cada camada tem uma função específica**, e separar responsabilidades deixa a rede mais organizada e fácil de manter.
- **Diagnosticar de baixo para cima** é um método eficiente de troubleshooting.
- A hierarquia funciona como uma forma de **defesa em profundidade**: um problema na camada de Acesso tende a ficar **contido ali**, sem afetar o restante da rede.

---

## 7. Arquivos do projeto

| Arquivo | Descrição |
|---|---|
| [`Topologia de rede em camadas.pkt`](Topologia%20de%20rede%20em%20camadas.pkt) | Projeto do Packet Tracer (abra no Cisco Packet Tracer) |
| [`Gif do modelo H.gif`](Gif%20do%20modelo%20H.gif) | Demonstração animada da topologia |
| `readme.md` | Documentação do projeto |

---

<sub>Projeto prático de estudos em redes. Parte do repositório <b>redes-packet-tracer</b>.</sub>
