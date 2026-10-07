---
title: "Tasks — Decomposição"
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
Erros
Situação	Status	Body
Placa ausente ou inválida,	422,	{"erro": "placa_invalida"}
entrada fora de ISO-8601,	422,	{"erro": "entrada_invalida"}
data fora de AAAA-MM-DD,	422,	{"erro": "data_invalida"}
Bilhete inexistente	,404,	{"erro": "bilhete_nao_encontrado"}
Encerrar bilhete já encerrado,	409,	{"erro": "bilhete_ja_encerrado"}
Cancelar bilhete não aberto,	409,	{"erro": "bilhete_nao_aberto"}
Abrir bilhete com placa já ocupada,	409	,{"erro": "bilhete_em_aberto"}
