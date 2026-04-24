# 🖥️ Monitor de PDVs — Grupo Formosa

> Painel web de monitoramento em tempo real dos terminais de ponto de venda (PDV) distribuídos pelas unidades do Grupo Formosa.

![PHP](https://img.shields.io/badge/PHP-777BB4?style=flat-square&logo=php&logoColor=white)
![JavaScript](https://img.shields.io/badge/Vanilla_JS-F7DF1E?style=flat-square&logo=javascript&logoColor=black)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=flat-square&logo=css3&logoColor=white)
![Status](https://img.shields.io/badge/Status-Em_Produção-00ff88?style=flat-square)

---

## Visão Geral

O Monitor de PDVs é uma ferramenta interna desenvolvida para a equipe de TI do Grupo Formosa. Permite visualizar em tempo real o estado de todos os terminais PDV da rede, identificando rapidamente quais estão operacionais, quais respondem apenas ao ping e quais estão completamente offline.

---

## Funcionalidades

- ✅ Monitoramento em tempo real via **ping ICMP + teste de porta TCP**
- ✅ Três estados de status: `ONLINE` · `PINGONLY` · `OFFLINE`
- ✅ Organização por unidade: **Super · Mix · Restaurante · Farmácia**
- ✅ Resumo geral e por loja com contadores dinâmicos
- ✅ Barra de progresso de disponibilidade geral
- ✅ Atualização automática a cada **60 segundos**
- ✅ Botão de **verificação manual imediata**
- ✅ Seletor de porta: HTTP (80), HTTPS (443), SSH (22), Custom (8080)
- ✅ Cards clicáveis para PDVs online — abre o terminal diretamente no navegador
- ✅ Tema dark com layout responsivo

---

## Estrutura dos PDVs monitorados

| Unidade | PDVs | Faixa de IP |
|---|---|---|
| Super | PDV 1–41, 57, 60 | `10.12.8.1` – `10.12.8.60` |
| Mix | PDV 49, 50, 61–74 | `10.12.8.49` – `10.12.8.74` |
| Restaurante | PDV 1–5 | `10.12.8.201` – `10.12.8.205` |
| Farmácia | PDV 1–5 | `10.12.8.101` – `10.12.8.105` |

---

## Stack Técnica

| Camada | Tecnologia |
|---|---|
| Backend | PHP (ping + teste de porta) |
| Frontend | HTML5 + CSS3 + Vanilla JavaScript |
| Comunicação | `fetch()` assíncrono por PDV |
| Dependências externas | **Nenhuma** |

---

## Como funciona

```
[ Navegador ]
     │
     │ fetch() por PDV a cada 60s
     ▼
[ status.php ]
     │
     ├── ping ICMP  ──► responde? ──► SIM → testa porta TCP
     │                                        │
     │                               porta aberta? ──► SIM → ONLINE
     │                                        │
     │                               porta fechada? ──► PINGONLY
     │
     └── não responde ao ping ──► OFFLINE
```

---

## Como usar

### Pré-requisitos
- Servidor web com **PHP** (Apache ou Nginx)
- Permissão para executar `ping` via PHP (`exec()`)
- Acesso à rede dos PDVs

### Instalação

```bash
# 1. Clone o repositório
git clone https://github.com/joseramos-ti/monitor-pdv.git

# 2. Copie para o diretório do servidor web
cp -r monitor-pdv/ /var/www/html/

# 3. Ajuste a permissão de ping (Linux)
sudo chmod +s /bin/ping

# 4. Acesse no navegador
# http://IP-DO-SERVIDOR/monitor-pdv/
```

### Personalização dos IPs

Edite o array `$lojas` no topo do `index.php`:

```php
$lojas = [
    "Super" => [
        "range" => array_merge(range(1, 41), [57, 60]),
        "base"  => 0
    ],
    // Adicione ou ajuste as demais lojas aqui
];
```

---

## Estrutura do Projeto

```
monitor-pdv/
├── index.php     ← Interface principal + lógica de renderização
└── status.php    ← Endpoint de ping e teste de porta (retorna JSON)
```

---

## Autor

**José Ramos** — Analista de TI · Grupo Formosa · Belém, PA

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/joseramos-ti/)
[![E-mail](https://img.shields.io/badge/E--mail-D14836?style=flat-square&logo=gmail&logoColor=white)](mailto:ti.joseramos@gmail.com)
