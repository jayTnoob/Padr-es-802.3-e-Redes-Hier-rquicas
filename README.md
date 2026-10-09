# Atividade 4 — Padrões 802.3 e Redes Hierárquicas

## 📋 Sobre a Atividade

Esta atividade tem como objetivo colocar em prática o conhecimento adquirido sobre os padrões 802.3, utilizando o simulador **Cisco Packet Tracer** para montar uma rede completa com os elementos necessários para uma rede hierárquica funcional, contemplando **escalabilidade, hierarquia, redundância e disponibilidade**.

Trata-se da simulação de um ambiente hierárquico de rede local, representando a etapa de planejamento de implementação de uma rede em um contexto real.

- **Assunto:** Padrões 802.3 e redes hierárquicas
- **Tempo previsto:** 4 horas
- **Prazo de entrega:** 27/08/2026

---

## 🎯 Tarefas / Requisitos da Atividade

### Etapa 1 — Ligações físicas

Nesta etapa é exigida **apenas a configuração das ligações físicas** entre os dispositivos, respeitando os seguintes requisitos:

1. Construir uma rede que tenha, de forma visível, a estrutura hierárquica, respeitando as características de cada camada (**núcleo, distribuição e borda/acesso**).
2. Incluir um **roteador com duas interfaces FastEthernet**, ligando-se a dois switches de núcleo diferentes.
3. Os dois switches de núcleo devem ter **ligação física entre si**, preparada para ativação futura de uma agregação de link (*link aggregation*) de **4 Gbps**.
4. Os dois switches de núcleo devem ser conectados **individualmente** a outros dois switches de distribuição, por meio de **interfaces de fibra óptica**, preparadas para ativação futura de uma agregação de link de **2 Gbps**.
5. Disponibilizar **quatro switches de acesso**, sem qualquer recurso de redundância.
6. Disponibilizar **quatro computadores desktop, quatro notebooks e um servidor**, todos conectados com fio à rede.

**Extra (opcional):** configurar os endereços IP em cada dispositivo final de forma que eles se comuniquem entre si (teste via `ping`) — vale 10% de bônus na pontuação da atividade.

### Etapa 2 — Configurações básicas dos switches (Avaliação 04, 5 pontos)

Com base no cenário desenvolvido na Avaliação 04 (prática de simulação de ambiente hierárquico), realizar as configurações básicas de **todos os switches**:

- hostname
- autenticação para acesso console
- autenticação para acesso privilegiado
- criptografia de senhas
- banner

---

## 🗺️ Topologia Implementada

```
                                  [R-BORDA-WAN]
                                  (Cisco 1841)
                             (2x interfaces FastEthernet)
                                   /              \
                            Fa0/0 /                \ Fa0/1
                                 /                  \
                   +------------------+      +------------------+
 [Server1] --------|  SW-NUCLEO-01    |======|  SW-NUCLEO-02    |
                   |  (Núcleo)        | 4Gbps|  (Núcleo)        |
                   +------------------+      +------------------+
                            |                         |
                       fibra|2Gbps               fibra|2Gbps
                            |                         |
                   +------------------+      +------------------+
                   |  SW-DIST-01      |      |  SW-DIST-02      |
                   |  (Distribuição)  |      |  (Distribuição)  |
                   +------------------+      +------------------+
                      /            \            /            \
                     /              \          /              \
          +--------------+  +--------------+  +--------------+  +--------------+
          | SW-ACESSO-01 |  | SW-ACESSO-02 |  | SW-ACESSO-03 |  | SW-ACESSO-04 |
          |   (Acesso)   |  |   (Acesso)   |  |   (Acesso)   |  |   (Acesso)   |
          +--------------+  +--------------+  +--------------+  +--------------+
              /     \           /     \           /     \           /     \
            PC1     PC2       PC3     PC4     Laptop1 Laptop2   Laptop3 Laptop4
```

### Dispositivos utilizados

| Papel | Nome no Packet Tracer | Hostname configurado | Modelo |
|---|---|---|---|
| Roteador | R-BORDA-WAN | — | Cisco 1841 |
| Núcleo | SW-NUCLEO-02(1) | `SW-NUCLEO-01` | Switch-PT |
| Núcleo | SW-NUCLEO-02 | `SW-NUCLEO-02` | Switch-PT |
| Distribuição | Switch8 | `SW-DIST-01` | Switch-PT |
| Distribuição | Switch7 | `SW-DIST-02` | Switch-PT |
| Acesso | Switch9 | `SW-ACESSO-01` | 2960-24TT |
| Acesso | Switch10 | `SW-ACESSO-02` | 2960-24TT |
| Acesso | Switch11 | `SW-ACESSO-03` | 2960-24TT |
| Acesso | Switch12 | `SW-ACESSO-04` | 2960-24TT |
| Desktops | PC1, PC2, PC3, PC4 | — | PC-PT |
| Notebooks | Laptop1, Laptop2, Laptop3, Laptop4 | — | Laptop-PT |
| Servidor | Server1 | — | Server-PT |

> **Observação:** o switch de núcleo da esquerda aparece no desenho como `SW-NUCLEO-02(1)` (o `(1)` é um sufixo automático do Packet Tracer para nomes repetidos). O hostname real, configurado no CLI, é `SW-NUCLEO-01`.

---

## 🔌 Tabela de Conexões

### Núcleo, Distribuição e Servidor

| Origem | Porta | Cabo | Porta | Destino |
|---|---|---|---|---|
| SW-NUCLEO-02 | Fa7/1 | Cobre | Fa0/1 | R-BORDA-WAN (Router 1841) |
| SW-NUCLEO-01 | Fa0/1 | Cobre | Fa0/0 | R-BORDA-WAN (Router 1841) |
| SW-NUCLEO-01 | Fa6/1 | Cobre | Fa0/1 | SW-NUCLEO-02 |
| SW-NUCLEO-01 | Fa7/1 | Cobre | Fa8/1 | SW-NUCLEO-02 |
| SW-NUCLEO-01 | Fa8/1 | Cobre | Fa9/1 | SW-NUCLEO-02 |
| SW-NUCLEO-01 | Fa9/1 | Cobre | Fa6/1 | SW-NUCLEO-02 |
| SW-NUCLEO-02 | Fa2/1 | Fibra | Fa4/1 | SW-DIST-02 |
| SW-NUCLEO-01 | Fa2/1 | Fibra | Fa5/1 | SW-DIST-01 |
| SW-DIST-02 | Fa5/1 | Fibra | Fa1/1 | SW-NUCLEO-02 |
| SW-DIST-01 | Fa4/1 | Fibra | Fa1/1 | SW-NUCLEO-01 |
| Server1 | Fa0 | Cobre | Fa5/1 | SW-NUCLEO-01 |

### Acesso (switches de acesso para PCs/Laptops)

| Origem | Porta | Cabo | Porta | Destino |
|---|---|---|---|---|
| SW-DIST-01 | Fa0/1 | Cobre | Fa0/1 | SW-ACESSO-01 |
| SW-DIST-01 | Fa1/1 | Cobre | Fa0/3 | SW-ACESSO-02 |
| SW-DIST-02 | Fa0/1 | Cobre | Fa0/3 | SW-ACESSO-03 |
| SW-DIST-02 | Fa1/1 | Cobre | Fa0/3 | SW-ACESSO-04 |
| PC1 | Fa0 | Cobre | Fa0/2 | SW-ACESSO-01 |
| PC2 | Fa0 | Cobre | Fa0/3 | SW-ACESSO-01 |
| PC3 | Fa0 | Cobre | Fa0/1 | SW-ACESSO-02 |
| PC4 | Fa0 | Cobre | Fa0/2 | SW-ACESSO-02 |
| Laptop1 | Fa0 | Cobre | Fa0/1 | SW-ACESSO-03 |
| Laptop2 | Fa0 | Cobre | Fa0/2 | SW-ACESSO-03 |
| Laptop3 | Fa0 | Cobre | Fa0/1 | SW-ACESSO-04 |
| Laptop4 | Fa0 | Cobre | Fa0/2 | SW-ACESSO-04 |

---

## 🌐 Endereçamento IP (Extra)

Rede: **192.168.10.0/24**
Máscara de sub-rede: **255.255.255.0**

| Dispositivo | Endereço IP | Máscara | DNS |
|---|---|---|---|
| PC1 | 192.168.10.2 | 255.255.255.0 | — |
| PC2 | 192.168.10.3 | 255.255.255.0 | — |
| PC3 | 192.168.10.4 | 255.255.255.0 | — |
| PC4 | 192.168.10.5 | 255.255.255.0 | — |
| Laptop1 | 192.168.10.6 | 255.255.255.0 | — |
| Laptop2 | 192.168.10.7 | 255.255.255.0 | — |
| Laptop3 | 192.168.10.8 | 255.255.255.0 | — |
| Laptop4 | 192.168.10.9 | 255.255.255.0 | — |
| Server1 | 192.168.10.100 | 255.255.255.0 | 8.8.8.8 |

---

## ⚙️ Configurações Básicas dos Switches (Etapa 2)

Foram configurados os **8 switches** (2 de núcleo, 2 de distribuição e 4 de acesso), usando a mesma sequência de comandos e alterando apenas o `hostname`.

### Parâmetros utilizados

| Item | Valor |
|---|---|
| Senha do modo privilegiado (`enable secret`) | `1234` |
| Senha do console | `1234` |
| Criptografia de senhas | `service password-encryption` |
| Banner (MOTD) | `Acesso Restrito! SOMENTE PESSOAL AUTORIZADO.` |

### Hostname de cada switch

| Switch no Packet Tracer | Comando |
|---|---|
| SW-NUCLEO-02(1) | `hostname SW-NUCLEO-01` |
| SW-NUCLEO-02 | `hostname SW-NUCLEO-02` |
| Switch8 | `hostname SW-DIST-01` |
| Switch7 | `hostname SW-DIST-02` |
| Switch9 | `hostname SW-ACESSO-01` |
| Switch10 | `hostname SW-ACESSO-02` |
| Switch11 | `hostname SW-ACESSO-03` |
| Switch12 | `hostname SW-ACESSO-04` |

### Sequência de comandos

| Nº | Comando | Função |
|---|---|---|
| 1 | `enable` | Entra no modo privilegiado |
| 2 | `configure terminal` | Entra no modo de configuração global |
| 3 | `hostname NOME-DO-SWITCH` | **Hostname** |
| 4 | `service password-encryption` | **Criptografia de senhas** |
| 5 | `enable secret 1234` | **Autenticação para acesso privilegiado** |
| 6 | `line console 0` | Entra na configuração da linha de console |
| 7 | `password 1234` | **Autenticação para acesso console** (senha) |
| 8 | `login` | Exige a senha no console |
| 9 | `exit` | Volta ao modo de configuração global |
| 10 | `banner motd #` | **Banner** (início, `#` é o delimitador) |
| 11 | `Acesso Restrito! SOMENTE PESSOAL AUTORIZADO.` | Texto do banner |
| 12 | `#` | Encerra o banner |
| 13 | `end` | Volta ao modo privilegiado |
| 14 | `copy running-config startup-config` | Salva a configuração |

### Bloco completo (copiar e colar)

```
enable
configure terminal
hostname NOME-DO-SWITCH
service password-encryption
enable secret 1234
line console 0
 password 1234
 login
 exit
banner motd #
Acesso Restrito! SOMENTE PESSOAL AUTORIZADO.
#
end
copy running-config startup-config
```

> **Atenção:** o comando `password` deve vir **antes** do `login` na linha de console. Caso contrário, o switch exibe `% Login disabled on line 0, until 'password' is set`.

### Verificação

Em cada switch foi executado:

```
show running-config | include hostname|enable|service
```

Resultado esperado (confirmado nos 8 switches):

```
service password-encryption
hostname SW-...
enable secret 5 $1$...
```

No final do `show running-config` também foram conferidos o `banner motd` e o `line con 0` com `password 7 ...` e `login`.

---

## ✅ Checklist de Requisitos Atendidos

### Etapa 1 — Ligações físicas

- [x] Estrutura hierárquica visível (núcleo, distribuição, acesso)
- [x] Roteador com duas interfaces FastEthernet ligadas a dois switches de núcleo
- [x] Ligação física entre os switches de núcleo (preparada para 4 Gbps)
- [x] Ligação individual dos switches de núcleo aos switches de distribuição via fibra óptica (preparada para 2 Gbps)
- [x] Quatro switches de acesso sem redundância
- [x] Quatro desktops, quatro notebooks e um servidor conectados com fio
- [x] Endereçamento IP configurado nos dispositivos finais (extra)

### Etapa 2 — Configurações básicas dos switches

| Switch | Hostname | Console | Privilegiado | Criptografia | Banner |
|---|---|---|---|---|---|
| SW-NUCLEO-01 | ✅ | ✅ | ✅ | ✅ | ✅ |
| SW-NUCLEO-02 | ✅ | ✅ | ✅ | ✅ | ✅ |
| SW-DIST-01 | ✅ | ✅ | ✅ | ✅ | ✅ |
| SW-DIST-02 | ✅ | ✅ | ✅ | ✅ | ✅ |
| SW-ACESSO-01 | ✅ | ✅ | ✅ | ✅ | ✅ |
| SW-ACESSO-02 | ✅ | ✅ | ✅ | ✅ | ✅ |
| SW-ACESSO-03 | ✅ | ✅ | ✅ | ✅ | ✅ |
| SW-ACESSO-04 | ✅ | ✅ | ✅ | ✅ | ✅ |

---

## 🛠️ Ferramentas

- Cisco Packet Tracer
