# Liberação de flag — Atividades CIP

Ferramenta de página única que lê as exportações de atividades em CSV e monta o
e-mail de solicitação de liberação de flag já formatado, pronto para colar no Outlook.

## Como usar

1. Abrir a ferramenta (link do GitHub Pages ou o `index.html` baixado).
2. Arrastar os CSVs exportados — todos de uma vez, uma região por arquivo.
3. Conferir o nome da região em cada arquivo carregado e corrigir se precisar.
4. Clicar em **Copiar e-mail** e colar no corpo do Outlook com `Ctrl+V`.

O banner e a tabela vão formatados. Destinatário, saudação e assinatura ficam
salvos para os próximos dias.

## O que entra no e-mail

Uma atividade entra quando atende às duas condições:

- **Status** é `Iniciada` ou `Não Iniciada`
- **Contrato** está preenchido

Tudo que ficar de fora aparece na tela com o motivo, para conferência antes do envio.

## Trava de data

A ferramenta só aceita a exportação **do dia**. Ao carregar um arquivo, ela lê a coluna
`Data agendada` das linhas — não confia no nome do arquivo — e compara com a data de hoje.

- Arquivo com data diferente de hoje é **recusado** e não entra no e-mail.
- Arquivo com mais de uma data também é recusado, porque a exportação deveria ser de um dia só.
- O motivo aparece na tela, com a data encontrada e a data de hoje.

Se for mesmo necessário enviar outra data, existe a saída **usar assim mesmo** no próprio
arquivo recusado. É um clique deliberado, por arquivo, e o chip fica marcado em laranja
para deixar claro que aquele e-mail não é do dia.

As atividades são agrupadas por região e, dentro de cada região, as iniciadas vêm
antes das não iniciadas. A data `31/07/26` do CSV é convertida para `31/07/2026`.

## Colunas usadas

O CSV exportado tem 119 colunas; a ferramenta lê apenas estas:

| Coluna no CSV         | Uso                          |
| --------------------- | ---------------------------- |
| `Data agendada`       | 1ª coluna da tabela e assunto |
| `ID Atividade`        | 3ª coluna da tabela           |
| `Contrato`            | 4ª coluna e regra do filtro   |
| `Status da Atividade` | 5ª coluna e regra do filtro   |
| `Tipo de Atividade`   | 6ª coluna da tabela           |

A **região** não vem do CSV: é lida do nome do arquivo. Em
`Atividades-CIP_CAPITAL_31_07_26.csv`, o trecho `CAPITAL` é convertido para
`CURITIBA`. Outros nomes são usados como estão e podem ser editados na tela.

Para mudar essa conversão, editar `MAP_REGIAO` no `index.html`:

```js
const MAP_REGIAO = { CAPITAL:"CURITIBA" };
```

## Privacidade

Os CSVs **não são enviados para lugar nenhum**. A leitura acontece inteiramente no
navegador de quem está usando, com a API `FileReader`. Não há servidor, backend nem
chamada de rede — a página funciona até offline depois de aberta.

Por isso mesmo: **nunca faça commit de arquivos CSV neste repositório.** Eles contêm
nome, endereço, telefone e e-mail de cliente. O `.gitignore` já bloqueia `.csv`,
`.xlsx` e `.xls`, mas vale conferir com `git status` antes de cada commit.

## Estrutura

```
index.html    ferramenta inteira — HTML, CSS e JS em um arquivo só
.gitignore    bloqueia CSV e planilhas
README.md     este arquivo
```

Sem dependências e sem build. Para alterar qualquer coisa, basta editar o
`index.html` e recarregar a página.
