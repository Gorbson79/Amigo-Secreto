# Amigo Secreto

Aplicativo web de amigo secreto: realiza o sorteio e gera um link individual por participante, para envio via WhatsApp, que revela o nome do sorteado.

## Participantes

Cris, Gabriel, Henrique, Felipe, Vera, Eliane, Celso, Marcelo, Katia, Laura.

## Regras do sorteio

- Ninguém pode tirar a si mesmo.
- **Laura não pode tirar Katia nem Marcelo.**
- As restrições ficam em uma lista de configuração no topo de `app.js`, fácil de editar.
- Algoritmo: embaralhar a lista (Fisher-Yates usando `crypto.getRandomValues`) e formar um **ciclo único** (cada pessoa tira a seguinte). Isso evita pares fechados (A↔B). Se alguma restrição for violada, embaralhar de novo (com 10 pessoas, a solução sai instantaneamente).

## Arquitetura

- **Site 100% estático**: HTML, CSS e JavaScript puros, sem build, sem backend e sem dependências.
- **Hospedagem**: GitHub Pages deste repositório, no endereço padrão `https://gorbson79.github.io/Amigo-Secreto/` (sem domínio próprio, sem DNS).
- **Sem banco de dados**: o resultado vai codificado no próprio link de cada participante.

## Páginas

| Página | URL | Quem usa | Função |
|---|---|---|---|
| Organizador | `gorbson79.github.io/Amigo-Secreto/` | Organizador | Faz o sorteio e lista "Link do X → [Enviar no WhatsApp]" |
| Revelação | `gorbson79.github.io/Amigo-Secreto/#<código>` | Participante | Mostra "Olá, X! Seu amigo secreto é…" e revela o nome ao tocar no botão |

## Sigilo

- O sorteio roda no navegador do organizador, que **não vê os nomes sorteados**: a tela só mostra quem recebe cada link.
- O código no link é o par {participante, sorteado} ofuscado (XOR com uma chave + Base64 URL-safe). Isso **não é criptografia**: basta para impedir a leitura casual, não para resistir a alguém técnico.
- O código fica no fragmento (`#`), que nunca é enviado ao servidor e por isso não aparece em logs.
- O resultado fica salvo no `localStorage` do organizador, para permitir reenviar os links. O botão "Refazer sorteio" pede confirmação.

## WhatsApp

- Cada participante tem um botão "Copiar mensagem" com o texto pronto, colado manualmente no WhatsApp:
  > Oi Gabriel! 🎁 Clique para ver quem você tirou no amigo secreto: https://gorbson79.github.io/Amigo-Secreto/#...

## Estrutura de arquivos

```
index.html   # tudo: config, sorteio, codificação, tela do organizador e de revelação
```

## Deploy

1. GitHub → Settings → Pages → Source: branch `main`, pasta raiz.
2. Aguardar 1–2 minutos e acessar `https://gorbson79.github.io/Amigo-Secreto/`.

## Decisões tomadas

- Endereço padrão do GitHub Pages (`gorbson79.github.io/Amigo-Secreto/`) em vez de domínio próprio, para não precisar configurar DNS.
- Ciclo único (sem pares A↔B).
- Sem telefones: o organizador copia a mensagem de cada pessoa e envia manualmente pelo WhatsApp.
- Implementação simplificada: tudo em um único `index.html` (sem `#` = tela do organizador; com `#código` = tela de revelação).

## Convenções

- Interface e textos em português (pt-BR).
- Layout mobile-first, porque os links são abertos no celular pelo WhatsApp.
- Não adicionar frameworks, build nem backend sem necessidade.
