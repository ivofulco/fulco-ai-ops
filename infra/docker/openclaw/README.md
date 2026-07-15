# OpenClaw Lab

Laboratório local para execução do OpenClaw utilizando Docker Compose, Ollama, PostgreSQL, Redis e Prometheus.

## Arquitetura

```text
┌─────────────────┐
│ OpenClaw CLI    │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ OpenClaw Gateway│
└────────┬────────┘
         │
 ┌───────┼────────┬──────────┐
 ▼       ▼        ▼          ▼
Ollama  Redis  PostgreSQL  Prometheus
```

---

# Pré-requisitos

* Docker Engine 28+
* Docker Compose Plugin
* Git
* Make

Verifique:

```bash
docker --version
docker compose version
git --version
make --version
```

---

# Estrutura do Projeto

```text
.
├── .env
├── docker-compose.yml
├── Makefile
├── README.md
├── openclaw-source/
├── data/
│   ├── openclaw/
│   ├── ollama/
│   ├── postgres/
│   ├── redis/
│   └── prometheus/
└── prometheus/
    └── prometheus.yml
```

---

# Primeira utilização

## 1. Clonar o repositório oficial do OpenClaw

```bash
make openclaw-clone
```

Será criada a pasta:

```text
openclaw-source/
```

---

## 2. Construir a imagem Docker local

```bash
make openclaw-build
```

Ao final, verifique:

```bash
docker images | grep openclaw
```

Resultado esperado:

```text
openclaw      local
```

---

## 3. Subir todo o laboratório

```bash
make compose-up
```

Verifique:

```bash
make compose-ps
```

Resultado esperado:

```text
openclaw-cli
openclaw-gateway
ollama
postgres
redis
prometheus
```

---

# Logs

Visualizar logs de todos os serviços:

```bash
make compose-logs
```

---

# Entrar no container do OpenClaw

```bash
docker exec -it openclaw-cli sh
```

---

# Executar o onboarding

Dentro do container:

```bash
openclaw onboard
```

---

# Baixar um modelo no Ollama

Entrar no container:

```bash
docker exec -it ollama bash
```

Baixar um modelo:

```bash
ollama pull qwen3:8b
```

ou

```bash
ollama pull llama3.3:8b
```

Listar modelos:

```bash
ollama list
```

---

# Atualizar o OpenClaw

Atualizar o código-fonte:

```bash
make openclaw-pull
```

Reconstruir a imagem:

```bash
make openclaw-rebuild
```

Reiniciar o laboratório:

```bash
make compose-restart
```

---

# Parar o laboratório

```bash
make compose-down
```

---

# Remover containers e volumes

```bash
make compose-clean
```

---

# Remover a imagem local do OpenClaw

```bash
make openclaw-clean
```

---

# Limpar completamente o Docker

```bash
make prune
```

⚠️ Este comando remove:

* imagens não utilizadas;
* cache de build;
* volumes não utilizados;
* containers parados.

---

# Targets disponíveis

## Docker Compose

| Comando                | Descrição                   |
| ---------------------- | --------------------------- |
| `make compose-up`      | Sobe o laboratório          |
| `make compose-down`    | Derruba o laboratório       |
| `make compose-restart` | Reinicia os containers      |
| `make compose-logs`    | Exibe os logs               |
| `make compose-ps`      | Lista os containers         |
| `make compose-build`   | Rebuild do compose          |
| `make compose-pull`    | Atualiza as imagens         |
| `make compose-clean`   | Remove containers e volumes |

---

## OpenClaw

| Comando                 | Descrição                     |
| ----------------------- | ----------------------------- |
| `make openclaw-clone`   | Clona o repositório oficial   |
| `make openclaw-pull`    | Atualiza o repositório        |
| `make openclaw-build`   | Constrói a imagem local       |
| `make openclaw-rebuild` | Reconstrói a imagem sem cache |
| `make openclaw-clean`   | Remove a imagem local         |
| `make openclaw-images`  | Lista as imagens              |
| `make openclaw-shell`   | Abre um shell na imagem       |

---

## Fluxo recomendado

### Primeira instalação

```bash
make openclaw-clone
make openclaw-build
make compose-up
```

### Atualização

```bash
make openclaw-pull
make openclaw-rebuild
make compose-restart
```

### Limpeza completa

```bash
make compose-clean
make openclaw-clean
make prune
```

---

# Troubleshooting

## Verificar status dos containers

```bash
make compose-ps
```

## Ver logs

```bash
make compose-logs
```

## Verificar espaço em disco

```bash
df -h
docker system df
```

## Limpar cache do Docker

```bash
make prune
```
