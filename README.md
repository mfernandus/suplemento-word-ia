# Assistente IA — Suplemento de Word (Next Editorial)

Painel (task pane) que conecta o Word à API da OpenAI usando **a sua própria chave**.
Três modos:

- **Reescrever** — aplica a instrução ao trecho selecionado e **substitui** a seleção pelo resultado. Para revisar gramática, ajustar tom, condensar, traduzir, padronizar estilo.
- **Inserir** — usa a seleção como contexto (opcional) e **insere** o resultado como um novo parágrafo, sem apagar o original. Para gerar subtítulos, linhas de apoio, legendas, resumos de apoio.
- **Analisar** — analisa a seleção (ou o documento inteiro, se nada estiver selecionado) e devolve a resposta **no painel**, sem alterar o texto. Para apontar vieses, repetições, sugerir títulos.

A chave fica salva **apenas no navegador** (localStorage) e só é enviada à OpenAI.

> É o par do suplemento de Excel: mesma identidade, mesma chave/modelo salvos.
> A diferença é a lógica — aqui usa `Word.run()` e manipula texto (seleção, parágrafos, corpo do documento) em vez de células.

---

## Arquivos

- `taskpane.html` — interface + lógica (arquivo único, autocontido).
- `manifest.xml` — descreve o suplemento para o Word.

## Passo 1 — Hospedar o `taskpane.html` em HTTPS

Mesmo processo do Excel (GitHub Pages ou a hospedagem que você já usa). Use uma URL/repositório
**separado** do Excel, ou um subcaminho diferente, para não sobrescrever um arquivo pelo outro
(ex.: `.../suplemento-word-ia/taskpane.html`).

## Passo 2 — Editar o `manifest.xml`

Substitua **todas** as ocorrências de `https://SEU-DOMINIO` pela URL real do `taskpane.html` do Word.
O `<Id>` já vem com um GUID próprio, diferente do Excel — não reaproveite o mesmo GUID nos dois.

## Passo 3 — Carregar (sideload) no Word

**Word para Windows** — via Catálogo de Suplementos Confiáveis (pasta de rede compartilhada):
Arquivo → Opções → Centro de Confiabilidade → Configurações → Catálogos de Suplementos Confiáveis →
adicione o caminho da pasta com o `manifest.xml`. Reabra o Word → Página Inicial → Suplementos →
Suplementos Compartilhados → "Assistente IA — Word".

**Word na web** — Início → Suplementos → Carregar Meu Suplemento → selecione o `manifest.xml`.

**Word para Mac** — copie o `manifest.xml` para
`~/Library/Containers/com.microsoft.Word/Data/Documents/wef` (crie `wef` se não existir) e reabra o Word.

## Passo 4 — Usar

1. Guia **Página Inicial** → **Abrir Assistente**.
2. Abra **Chave da API**, cole `sk-...`, escolha o modelo, salve.
3. Selecione um trecho (ou só posicione o cursor, no modo Inserir), escolha o modo, escreva a
   instrução e clique no botão.

---

## Custo e segurança

Iguais ao do Excel: a API da OpenAI é cobrada por uso (tokens) e não vem no ChatGPT Plus.
A chave fica visível no cliente — ótimo para uso pessoal; para os ~37 usuários, use um proxy
no servidor para não expor a chave. Como os dois suplementos chamam o mesmo endpoint, **um único
proxy serve para Word e Excel**.
