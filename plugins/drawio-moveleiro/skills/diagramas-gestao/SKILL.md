---
name: diagramas-gestao
description: Desenha diagramas de gestão no draw.io (canvas ao vivo no navegador) para marcenarias, fábricas de móveis sob medida e consultorias — fluxo de processo em raias (AS-IS e TO-BE), mapa de gargalos da cadeia de valor, organograma com dependência do dono, jornada do cliente e arquitetura de sistemas e automações (CRM, Promob, ERP, WhatsApp). Use quando o usuário pedir para desenhar, mapear, diagramar ou visualizar um processo, fluxo, organograma, jornada, funil, gargalo ou arquitetura, ou mencionar draw.io / drawio.
---

# Diagramas de gestão no draw.io

Um diagrama de consultoria não é decoração: é a prova visual do diagnóstico. Cada caixa precisa responder a uma pergunta de negócio — onde o pedido trava, quem decide, quanto depende do dono, onde a margem vaza. Se o desenho não sustenta uma decisão, ele está errado, por mais bonito que seja.

As ferramentas vêm do servidor MCP `drawio` deste plugin (`@next-ai-drawio/mcp-server`). O diagrama é desenhado num canvas do draw.io aberto no navegador; o usuário pode editar à mão a qualquer momento e o Claude lê essas edições de volta.

## 1. Abrir a sessão

1. Chame `start_session`. Ele devolve a URL do preview e o guia de desenho.
2. **Sempre entregue a URL ao usuário** em uma linha. Em SSH, container ou nuvem o navegador não abre sozinho.
3. Para continuar um diagrama de outra conversa: `list_saved_diagrams` → `start_session` com o `session_id` encontrado.
4. Sem sessão, toda ferramenta falha com "No active session". O primeiro passo é sempre `start_session`.

## 2. Escolher o diagrama pela pergunta de negócio

| Pergunta do dono / do consultor | Diagrama | Guia (`get_drawing_guide` topic) |
|---|---|---|
| "Onde o pedido trava entre o fechamento e a instalação?" | Fluxo em raias horizontais (AS-IS) | `swimlane-horizontal` |
| "Como deveria funcionar sem o dono no meio?" | Fluxo em raias (TO-BE), na página ao lado do AS-IS | `swimlane-horizontal` |
| "Qual etapa da cadeia está derrubando prazo e margem?" | Mapa de gargalos da cadeia de valor | sem topic (fluxograma) |
| "Quem decide o quê? Quanto passa pelo dono?" | Organograma com camada de dependência | sem topic (árvore manual) |
| "Como o cliente vive a compra, do lead ao pós-obra?" | Jornada do cliente em fases | `swimlane` |
| "Como CRM, Promob, ERP e WhatsApp conversam?" | Arquitetura de sistemas / automações | sem topic; `sequence` para trocas passo a passo |

Quando usar um guia com topic, passe o mesmo valor em `type` no `create_new_diagram` (o guia diz quando). Sem guia, deixe `type` de fora.

## 3. Cadeia de valor de referência (móveis sob medida)

Use como esqueleto e ajuste à realidade da empresa. Nunca force uma etapa que a empresa não tem; marque a ausência quando ela for o próprio problema.

**Etapas:** Captação do lead → Briefing e medição inicial → Projeto 3D (Promob/similar) → Orçamento → Negociação e contrato → Medição fina → Engenharia e detalhamento (plano de corte, ferragens) → PCP → Compras e insumos → Corte → Fita de borda → Usinagem e furação → Pré-montagem → Embalagem e expedição → Instalação (montagem em obra) → Vistoria e entrega técnica → Pós-obra e assistência.

**Raias padrão:** Cliente · Comercial · Projetos/Engenharia · PCP/Compras · Fábrica · Implantação/Montagem · Dono/Diretoria.

A raia **Dono/Diretoria** é obrigatória no AS-IS: é nela que aparece, visualmente, a dependência do dono. No TO-BE, ela deve ficar quase vazia — só decisões estratégicas e indicadores.

## 4. Linguagem visual padrão

Mantenha a mesma semântica de cores em todos os diagramas do cliente; é isso que transforma desenhos soltos em método.

| Elemento | Estilo draw.io |
|---|---|
| Etapa normal | `rounded=1;whiteSpace=wrap;html=1;fillColor=#F5F5F5;strokeColor=#424242;fontSize=12;` |
| Decisão | `rhombus;whiteSpace=wrap;html=1;fillColor=#FFF8E1;strokeColor=#F9A825;` |
| **Gargalo** | `rounded=1;whiteSpace=wrap;html=1;fillColor=#FDECEA;strokeColor=#C62828;strokeWidth=2;fontStyle=1;` |
| **Passa pelo dono** | `rounded=1;whiteSpace=wrap;html=1;fillColor=#EDE7F6;strokeColor=#5E35B1;strokeWidth=2;` |
| Sistema / automação | `rounded=1;whiteSpace=wrap;html=1;fillColor=#E3F2FD;strokeColor=#1565C0;` |
| Retrabalho (laço de volta) | aresta `dashed=1;strokeColor=#C62828;` com rótulo "retrabalho" |
| KPI / evidência | `shape=note;whiteSpace=wrap;html=1;fillColor=#FFFDE7;strokeColor=#9E9D24;fontSize=11;` |
| Título | texto 16 px, negrito: **Empresa · Processo · AS-IS ou TO-BE · data** |

Toda página leva uma **legenda** no canto inferior direito com as cores usadas.

## 5. Regras de conteúdo (rigor de consultoria)

- **Todo gargalo tem quatro coisas**, numa nota KPI ligada a ele: causa raiz, impacto (prazo, margem ou retrabalho), indicador e responsável pelo indicador.
- **Não invente números.** Sem dado do cliente, escreva `[medir]` no lugar do valor. Um diagrama com número falso destrói a autoridade do diagnóstico inteiro.
- **Use a linguagem da fábrica:** "plano de corte", "fita de borda", "lead time de produção", "OS de montagem", "assistência pós-obra" — não jargão genérico de processos.
- **Uma página, uma ideia.** Acima de ~25 formas, divida em páginas (`add_page`), por exemplo "Comercial", "Fábrica", "Implantação".
- **AS-IS e TO-BE lado a lado:** página "AS-IS" e página "TO-BE" no mesmo arquivo. O contraste vende a transformação melhor que qualquer slide.

## 6. Fluxo de trabalho

1. **Coletar insumos.** Leia o que o usuário forneceu (descrição, arquivos do projeto, diagnósticos, planilhas). Se faltar algo que muda a estrutura (por exemplo, se a montagem é própria ou terceirizada), pergunte uma vez, de forma objetiva; o resto, assuma e declare.
2. **Abrir a sessão** (`start_session`) e passar a URL.
3. **Carregar o guia** do tipo escolhido (`get_drawing_guide` com `topic`).
4. **Desenhar** com `create_new_diagram`. Peça `screenshot: true` quando o preview estiver aberto e em primeiro plano.
5. **Autoconferência** (`screenshot_diagram` se não pediu screenshot): textos cortados, formas sobrepostas, setas atravessando caixas, legenda presente, todo gargalo com nota KPI. Corrija antes de mostrar.
6. **Iterar com `edit_diagram`**, nunca recriando do zero: assim as edições manuais do usuário são preservadas. Se o usuário pode ter mexido à mão, chame `get_diagram` antes de editar. Quando ele disser "esses", "estas caixas", use `get_selection`.
7. **Desfazer:** `restore_version` (últimas 20 versões; o usuário também tem Ctrl+Z no canvas).
8. **Exportar** com `export_diagram`, sempre com **caminho absoluto** — caminho relativo é resolvido na pasta do servidor MCP, não do projeto, e o arquivo "some". Padrão:
   - `<raiz do projeto>/diagramas/<slug>.drawio` — fonte editável;
   - `<raiz do projeto>/diagramas/<slug>.png` — para proposta, apresentação e WhatsApp;
   - `.drawio.svg` quando o diagrama for para documentação ou site.
   Exporte cada página relevante (`page_name`), não só a primeira.
9. **Fechar** com 3 a 5 linhas: o que o diagrama revela, os gargalos prioritários e o próximo passo recomendado.

## 7. Armadilhas conhecidas

- **Screenshot ou exportação PNG/SVG com timeout:** a aba do preview está em segundo plano. Peça ao usuário para trazê-la para frente e tente de novo.
- **Preview em branco** dentro do navegador do editor (VS Code/Cursor): abrir a URL num navegador comum.
- **Organograma automático** não existe na cópia embarcada do draw.io: monte a árvore manualmente, com espaçamento regular (ex.: 180 px entre irmãos, 120 px entre níveis).
- **Regras próprias de estilo** podem ser fixadas em `~/.next-ai-drawio/instructions.md`; elas entram no guia de desenho a cada chamada.
