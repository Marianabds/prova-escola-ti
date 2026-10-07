---
title: "Spec — API de Operadora de estacionamento rotativo"
type: knowledge
status: done
area: resources
resource: talks
tags:
  - kind/knowledge
  - area/resources
  - resource/talks
  - status/done
---
# Spec — API de Operadora de estacionamento rotativo

**UC1 — Abrir bilhete**
- Entrada: {"id": 1, "placa": "ABC1D23", "entrada": "<ISO-8601 com fuso -03:00>", "status": "aberto"}
- Critérios de aceite:
  - O body aceita entrada opcional (ISO-8601 com fuso): quando presente, o bilhete abre naquele instante em vez de "agora".
 
**UC2 - Encerrar bilhete**
- POST /bilhetes/{id}/encerramento → 200
- Regras de valor:
-Cobra-se por fração de 15 minutos, arredondando para cima (fração exata cobra 1 fração; 1 minuto a mais já cobra a fração seguinte);
- hora cheia = 450;
- valor da fração = tarifa ÷ (60 ÷ 15);
aplica-se o teto diário: valor_centavos nunca supera 6000;
valor sempre em centavos, inteiro — a API nunca retorna ponto flutuante.1

**UC3 - Listar ativos**
-GET /bilhetes/ativos → 200 com array dos bilhetes abertos, mais recentes primeiro.

**UC4 - UC4 — Relatório diário**
- GET /relatorios/diario?data=AAAA-MM-DD → 200
- Filtro por dia de encerramento, total, faturamento e média.
-{"data": "2026-10-05", "total_bilhetes": 12,
 "faturamento_centavos": 8400, "tempo_medio_minutos": 47}
- tempo_medio_minutos considera apenas bilhetes encerrados no dia, arredondando 0,5 para cima.

**UC5 - Cancelar bilhete**
- POST /bilhetes/{id}/cancelamento → 200 com status: "cancelado". Só bilhetes abertos podem ser cancelados — sem cobrança (não gera saida nem valor_centavos).

**UC6 - Histórico por placa**
- GET /bilhetes?placa=ABC1D23 → 200 com array de todos os bilhetes da placa (qualquer status), mais recentes primeiro. Placa que nunca estacionou → array vazio.

**UC7 - Tolerância gratuita**
Os primeiros 15 de um bilhete são grátis: duração ≤ tolerância → valor_centavos: 0. Passou da tolerância (mesmo por 1 minuto) → cobra integral desde o primeiro minuto — a tolerância não é descontada.

**UC8 - Uma vaga por placa**
- POST /bilhetes para placa que já tem bilhete aberto → 409 {"erro": "bilhete_em_aberto"}. Após encerrar ou cancelar, a placa volta a poder abrir.
