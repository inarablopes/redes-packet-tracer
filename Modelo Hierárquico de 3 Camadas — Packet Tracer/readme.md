# Modelo Hierárquico de 3 Camadas — Packet Tracer

Prática de montagem de uma topologia hierárquica (Núcleo, Distribuição e Acesso), endereçamento IP, configuração via CLI e testes de conectividade ponta a ponta.

Concluí mais um desafio prático: a montagem de uma rede aplicando o Modelo Hierárquico de 3 Camadas (Núcleo, Distribuição e Acesso) no Cisco Packet Tracer.

O objetivo era sair de uma topologia "no papel" e chegar numa rede realmente funcional, usando o padrão de projeto que empresas de qualquer tamanho usam na prática.

Meu raciocínio foi separar a rede em 3 camadas: um roteador no Núcleo, responsável só por encaminhar tráfego rápido, sem filtragem pesada; um Multilayer Switch na Distribuição, cuidando de roteamento e políticas; e switches 2960 no Acesso, onde os PCs se conectam. Endereçei tudo em uma única rede (192.168.1.0/24), sem VLANs nessa etapa, e configurei o roteador via CLI (hostname, IP na interface, no shutdown).

O desafio apareceu nos testes: um dos links ficava vermelho mesmo com o cabo certo. Resolvi diagnosticando de baixo para cima — checando o IP dos PCs, o status físico dos links, a interface do roteador (show ip interface brief) e só depois a configuração lógica. Descobri que faltava adicionar um módulo de rede manualmente no equipamento, com ele desligado.

O resultado foi conectividade completa ponta a ponta: PCs no mesmo switch, PCs em switches de Acesso diferentes passando pela Distribuição, e todos os PCs até o roteador no Núcleo, todos respondendo ping com sucesso.

Esse exercício me ajudou a entender na prática por que a hierarquia de rede funciona como uma defesa em profundidade: um problema na camada de Acesso tende a ficar contido ali, sem afetar o restante da rede.
