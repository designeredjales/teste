---
description: Desenha o mapa de gargalos da cadeia de valor (captação ao pós-obra) com status por etapa e KPIs, a partir de um diagnóstico
argument-hint: <empresa ou diagnóstico, ou caminho de um arquivo com os dados>
---

Siga a skill `diagramas-gestao` deste plugin.

Base do diagnóstico: $ARGUMENTS

Se for um caminho de arquivo, leia-o primeiro. Se nada foi informado, peça em uma pergunta só: as etapas que mais atrasam, onde há mais retrabalho e o que hoje depende do dono.

Entregue:

1. **Uma página "Mapa de gargalos"** com a cadeia de valor de referência da skill, da esquerda para a direita, em uma linha (ou duas, se não couber). Cada etapa recebe um status visível: estilo normal (sob controle), amarelo `#FFF8E1`/`#F9A825` (atenção) ou estilo de gargalo (crítico). Etapas que passam pelo dono recebem a borda roxa.
2. **Notas KPI** sobre as etapas críticas: causa raiz, impacto em prazo/margem/retrabalho, indicador e responsável. Valores desconhecidos como `[medir]` — nunca invente.
3. **Faixa inferior "Prioridades"** com no máximo 3 frentes de correção, numeradas, cada uma ligada às etapas que ela resolve.
4. **Exportação** em `.drawio` e `.png` na pasta `diagramas/` da raiz do projeto, com caminho absoluto.
5. **Leitura executiva** em até 5 linhas: onde está o gargalo principal, o custo de continuar igual e o primeiro movimento.
