# drawio-moveleiro

Plugin do Claude Code para desenhar **diagramas de gestão no draw.io**, com canvas ao vivo no navegador, voltado a marcenarias, fábricas de móveis sob medida e consultorias do setor moveleiro.

O Claude desenha, você ajusta à mão no mesmo canvas, e ele lê suas edições antes de mexer de novo. Cada mudança fica versionada (últimas 20), com Ctrl+Z e exportação para `.drawio`, `.png`, `.svg` e `.drawio.svg`.

## O que vem no plugin

| Componente | O que faz |
|---|---|
| Servidor MCP `drawio` | [`@next-ai-drawio/mcp-server`](https://github.com/DayuanJiang/next-ai-draw-io/tree/main/packages/mcp-server) 0.4.15, com o editor em português (`DRAWIO_LANG=pt-br`). 16 ferramentas: sessão, desenho, edição, seleção, screenshot, páginas, versões e exportação. |
| Skill `diagramas-gestao` | Método de diagramação: qual diagrama responde a qual pergunta de negócio, a cadeia de valor de referência do móvel sob medida, a linguagem visual padrão (gargalo, dependência do dono, automação, KPI) e o fluxo de trabalho com as ferramentas. Ativa sozinha quando você pede para desenhar ou mapear algo. |
| `/drawio-moveleiro:fluxo` | Processo em raias, páginas AS-IS e TO-BE, com gargalos, retrabalho e KPIs. |
| `/drawio-moveleiro:gargalos` | Mapa de gargalos da cadeia de valor, do lead ao pós-obra, com status por etapa e as 3 prioridades. |
| `/drawio-moveleiro:organograma` | Organograma atual com a camada de dependência do dono, e a estrutura-alvo autogerenciável. |

## Instalação

Requisitos: Claude Code e Node.js 18+ (o servidor roda via `npx`).

No Claude Code:

```text
/plugin marketplace add designeredjales/teste
```

```text
/plugin install drawio-moveleiro@designeredjales-plugins
```

Reinicie o Claude Code para as ferramentas aparecerem.

**Se você já adicionou o servidor à mão** com `claude mcp add --scope user drawio ...`, remova-o para não ter dois servidores iguais disputando a porta do preview e duplicando as ferramentas:

```bash
claude mcp remove --scope user drawio
```

## Uso

```text
/drawio-moveleiro:fluxo do contrato assinado à instalação
```

```text
/drawio-moveleiro:gargalos diagnostico/marcenaria-xyz.md
```

```text
/drawio-moveleiro:organograma dono + 1 projetista + 1 vendedora + 6 na fábrica + 2 equipes de montagem
```

Ou simplesmente peça: *"mapeia o fluxo de assistência pós-obra dessa empresa"*. A skill entra sozinha.

O Claude devolve a URL do preview (por padrão `http://localhost:6002/...`). Abra num navegador comum. Os arquivos exportados vão para `diagramas/` na raiz do projeto.

## Configuração

Variáveis do servidor ficam em `.mcp.json`, no bloco `env`:

| Variável | Uso |
|---|---|
| `DRAWIO_LANG` | Idioma do editor. O plugin define `pt-br`. |
| `DRAWIO_UI` | Tema do draw.io: `kennedy`, `atlas`, `dark`, `min`, `sketch`, `simple`. |
| `DRAWIO_AUTO_SCREENSHOT` | `true` anexa screenshot a cada criação/edição (2 a 10 s por chamada). |
| `PORT` | Porta do preview (padrão 6002, sobe até 6020 se ocupada). |
| `DRAWIO_DATA_DIR` | Onde os diagramas são salvos (padrão `~/.next-ai-drawio`); `off` desliga. |

Regras de estilo próprias (paleta da sua marca, fonte, "sempre minimalista") podem ir em `~/.next-ai-drawio/instructions.md`; entram no guia de desenho a cada chamada, sem reiniciar.

### Versão do servidor

O servidor está fixado em `0.4.15` para que todo mundo que instalar o plugin rode exatamente o código que foi testado, e uma publicação nova no npm não entre sem revisão. Para atualizar, troque a versão em `.mcp.json`, teste, e suba a `version` em `.claude-plugin/plugin.json`. Para sempre pegar a última, use `@next-ai-drawio/mcp-server@latest`.

## Créditos

O servidor MCP é o [Next AI Draw.io](https://github.com/DayuanJiang/next-ai-draw-io), de DayuanJiang, sob licença Apache-2.0. Este plugin apenas o empacota com o método e os comandos de diagramação de gestão.
