---
description: Mapeia um processo em raias no draw.io (AS-IS e TO-BE), com gargalos, dependências do dono, retrabalho e KPIs
argument-hint: <processo, ex.: "do contrato assinado à instalação">
---

Siga a skill `diagramas-gestao` deste plugin.

Processo a mapear: $ARGUMENTS

Se nenhum processo foi informado, use a cadeia completa "do fechamento do contrato à entrega técnica".

Entregue:

1. **Página "AS-IS"** — fluxo em raias horizontais (`get_drawing_guide` topic `swimlane-horizontal`) com a raia Dono/Diretoria. Marque com o estilo de gargalo cada etapa que acumula fila, espera ou retrabalho; marque em roxo cada etapa que hoje só anda com o dono; desenhe os laços de retrabalho em vermelho tracejado. Cada gargalo ganha uma nota KPI (causa raiz, impacto, indicador, responsável); sem dado, `[medir]`.
2. **Página "TO-BE"** — o mesmo processo redesenhado: padrão, responsável por etapa, pontos de controle e automações (em azul). A raia do dono fica só com decisões estratégicas e indicadores.
3. **Exportação** de cada página em `.drawio` e `.png` na pasta `diagramas/` da raiz do projeto, com caminho absoluto.
4. **Leitura executiva** em até 5 linhas: os 3 gargalos que mais custam e a primeira mudança a implantar.
