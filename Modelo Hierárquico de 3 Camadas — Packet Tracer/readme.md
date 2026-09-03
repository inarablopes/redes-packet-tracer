# Modelo Hierárquico de 3 Camadas — Packet Tracer

Prática de montagem de uma topologia hierárquica (Núcleo, Distribuição e Acesso), endereçamento IP, configuração via CLI e testes de conectividade ponta a ponta.

## 🎯 Objetivo

Sair de uma topologia "desenhada" para uma rede realmente funcional, aplicando o **Modelo Hierárquico de 3 Camadas**, um padrão de projeto usado em redes corporativas de qualquer tamanho.

## 📚 Conceitos — as 3 camadas

| Camada | Dispositivo | Função |
|---|---|---|
| **Núcleo** | Roteador | Encaminhar tráfego o mais rápido possível entre a rede interna e o mundo externo (ou entre grandes blocos internos). Sem filtragem pesada ou lógica complexa. |
| **Distribuição** | Multilayer Switch | Faz a ponte entre Núcleo e Acesso. Roteia (Camada 3) além de comutar (Camada 2). É onde ficam VLANs, políticas de segurança e decisões de encaminhamento. |
| **Acesso** | Switches 2960 | Onde os dispositivos finais (PCs, impressoras, telefones IP) se conectam à rede. |

**Analogia do shopping:**
- Núcleo = avenida principal que liga o shopping à cidade (rápida, sem semáforos)
- Distribuição = estacionamento e corredores centrais, direcionando o fluxo
- Acesso = as portas de cada loja, onde o cliente realmente entra

**Por que importa:** essa separação em camadas funciona como defesa em profundidade. Um incidente na Camada de Acesso (ex: SYN Flood, tempestade de broadcast) tende a ficar contido ali, sem afetar o Núcleo — a hierarquia limita o "raio de explosão" de qualquer problema.

##  Conexões físicas

- Cabo automático: opção "Automatically Choose Connection Type" habilitada, para o Packet Tracer escolher entre reto/crossover.
- Status do link pela cor da bolinha: verde = ativo, laranja piscando = negociando, vermelho/ausente = problema (interface desligada, cabo incompatível ou porta errada).
- Link vermelho persistente: verificar aba **Physical** do dispositivo — módulos de rede às vezes precisam ser adicionados manualmente (com o equipamento desligado).

## Plano de endereçamento IP

Rede única (192.168.1.0/24), sem VLANs nessa etapa.

| Dispositivo | Interface | Endereço IP | Máscara | Gateway |
|---|---|---|---|---|
| Router (Núcleo) | GigabitEthernet0/0 | 192.168.1.1 | 255.255.255.0 | — |
| PC — Secretaria | — | 192.168.1.10 | 255.255.255.0 | 192.168.1.1 |
| PC — Biblioteca | — | 192.168.1.11 | 255.255.255.0 | 192.168.1.1 |
| PC — Sala de Reuniões | — | 192.168.1.12 | 255.255.255.0 | 192.168.1.1 |
| PC — LAB1 | — | 192.168.1.13 | 255.255.255.0 | 192.168.1.1 |
| PC — LAB2 | — | 192.168.1.14 | 255.255.255.0 | 192.168.1.1 |

> Todos os dispositivos precisam estar na mesma rede porque switches de Acesso e Distribuição, sozinhos, apenas encaminham quadros — não separam redes. Quem separaria redes diferentes seria o roteamento entre VLANs (etapa mais avançada).

## ⚙️ Configuração do roteador (CLI)

```
Router> enable
Router# configure terminal
Router(config)# hostname R-Core
R-Core(config)# interface GigabitEthernet0/0
R-Core(config-if)# ip address 192.168.1.1 255.255.255.0
R-Core(config-if)# no shutdown
R-Core(config-if)# exit
R-Core(config)# exit
R-Core# show ip interface brief
```

**Checagem esperada:** a interface configurada deve aparecer com `Status = up` e `Protocol = up`.

- `administratively down` → falta o `no shutdown`
- `down` (sem "administratively") → problema físico (cabo/link) ou configuração incorreta

##  Configuração dos PCs

Em cada PC: aba **Desktop → IP Configuration → Static**, preenchendo IP, máscara e gateway conforme a tabela acima. Repetir para os 5 PCs.

## Testes de conectividade

Do mais simples ao mais complexo, para isolar onde está o problema (se houver):

1. **PC → PC no mesmo switch de Acesso** (ex: Secretaria → Biblioteca, ambos no Switch1) — passa só pela Camada de Acesso.
   ```
   ping 192.168.1.11
   ```
2. **PC → PC em switches de Acesso diferentes** (ex: Secretaria → LAB1) — passa obrigatoriamente pela Camada de Distribuição.
   ```
   ping 192.168.1.13
   ```
3. **PC → interface do Roteador** — confirma que toda a rede enxerga o Núcleo.
   ```
   ping 192.168.1.1
   ```


## 🔧 Diagnóstico (se algum ping falhar)

Checar de baixo para cima:

1. O PC tem o IP certo? → `ipconfig` no PC
2. O link até o switch de Acesso está verde? → conferir cabo
3. O link até a Distribuição está verde? → conferir Multilayer Switch
4. A interface do Roteador está `up/up`? → `show ip interface brief`
5. Os dispositivos estão na mesma rede/máscara? → revisar tabela de endereçamento

