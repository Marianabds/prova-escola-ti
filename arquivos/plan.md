---
title: "Plan — Arquitetura e Decisões"
type: knowledge
status: done
area: resources
resource: talks
tags:
  - kind/knowledge
  - area/resources
  - resource/talks
  - status/done
created: 2026-10-07
updated: 2026-10-07
---
# Plan — Arquitetura e decisões

## Stack

- **Python 3.11 + FastAPI + uvicorn** (justificativa: sintaxe simples e de fácil compreenção para agentes de IA).
- Persistência direcionada em memória
- 

## Estrutura de arquivos a gerar

```
main.py        # crir app FastAPI, definir as rotas http: POST /bilhetes, POST /bilhetes/{id}/encerramento, GET /bilhetes/ativos, POST /bilhetes/{id}/cancelamento, GET /bilhetes, GET /relatorios/diario
models.py      # entidade ticket que representa um bilhete de estacionamento contendo id unico, placa com 7 caracteres alfanuméricos/maiúsculos, data e hora de entrada, data e hora de saida,  duração em minutos e valor cobrado em centavos. Exemplo: {"id": 1, "placa": "ABC1D23", "entrada": "<ISO-8601 com fuso -03:00>", "status": "aberto"}
service.py     # seguir as regras aplicadas no arquivo spec.md (UC1, UC2,...,UC8)
store.py       # Guardar, pesquisar e atualizar bilhetes em memória.
test_app.py    # testes endpoints
test_service.py	# Testar cálculo monetário, tolerância, estados e relatórios.
```

## Decisões

1. Bilhete deve ter apenas os estados aberto, encerrado e cancelado.
2. IDs sequenciais simples (int), gerados no repositório.
3. O body aceita entrada opcional (ISO-8601 com fuso): quando presente, o bilhete abre naquele instante em vez de "agora".
