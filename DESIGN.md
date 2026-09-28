# Design System — Apresentação Frila (Challenge 18 Final)

Sistema visual do deck em `index.html`. Um arquivo HTML autocontido, sem framework, com um palco fixo de 1920×1080 escalado para caber em qualquer tela. Este documento descreve as regras para quem for editar o arquivo à mão ou pedir alterações.

---

## 1. Conceito

**"Dado em destaque"**: fundo claro, grade suíça, números gigantes em laranja como protagonistas, tipografia grotesca pesada. A ideia é que cada slide caiba numa frase e num número — pouco texto, muita hierarquia.

Duas exceções à paleta clara:
- O slide de abertura da persona "Cauê" (vídeo do nascimento da ideia) usa fundo escuro (`.dark`), para simular o clima de tela de vídeo/observação.
- O restante do deck é sempre claro.

---

## 2. Cores

Definidas como variáveis CSS em `:root`, nunca hexadecimais soltos no meio do HTML.

| Token | Valor | Uso |
|---|---|---|
| `--papel` | `#f6f4ef` | Fundo do palco (bege claro, não branco puro) |
| `--tinta` | `#111111` | Texto principal, bordas, ícones |
| `--tinta2` | `#3a3a3a` | Texto secundário (parágrafos de apoio) |
| `--sutil` | `#6b6b6b` | Legendas, metadados, texto em mono |
| `--linha` | `#1111111a` | Linhas da grade de fundo (10% opacidade) |
| `--sinal` | `#ff4d17` | **Cor de destaque única.** Números, links visuais, sublinhados, ícone ativo |
| `--sinal-claro` | `#ffe3d8` | Fundo de badges/tags sobre a cor de destaque |
| `--cinza` | `#e7e4dc` | Fundos neutros (inputs de wireframe, barra de navegador) |
| `--cinza2` | `#d4d0c6` | Bordas suaves, divisores |

**Regra de ouro:** só existe **uma** cor de acento (`--sinal`, laranja). Nunca introduzir uma segunda cor de destaque (verde, azul, roxo) — a força do sistema vem de usar só uma cor de forma consistente para "isto é o dado/o ponto principal".

### Modo escuro (`.dark`)

Usado em um único slide (o do vídeo). Quando `.dark` está na `<section>`:
- fundo vira `var(--tinta)` (#111), texto vira `#f4f1ea`
- `.chrome` (cabeçalho) fica `#9a968d`
- `.lead` (parágrafo de apoio) fica `#cfcac0`
- a grade de fundo (`.grid`) fica com `opacity:.12` e `filter:invert(1)`

---

## 3. Tipografia

Duas famílias, do Google Fonts, carregadas uma vez no `<head>`:

```html
<link href="https://fonts.googleapis.com/css2?family=Bricolage+Grotesque:opsz,wght@12..96,400;12..96,500;12..96,700;12..96,800&family=IBM+Plex+Mono:wght@400;500&display=swap" rel="stylesheet">
```

| Papel | Fonte | Variável | Onde |
|---|---|---|---|
| **Interface / conteúdo** | Bricolage Grotesque | `var(--sans)` | Títulos, parágrafos, números, cards — tudo que não é metadado |
| **Metadado / fonte / técnico** | IBM Plex Mono | `var(--mono)` | Cabeçalho de cada slide, legendas de fonte, rótulos de seção, timestamps do chat |

Essa separação (grotesca para conteúdo, mono para metadado) é a mesma "regra das vozes tipográficas" usada no resto do ecossistema Frila/Bancada: a fonte declara se o texto é ideia (sans) ou proveniência/dado técnico (mono).

### Escala de tamanhos

| Classe | `font-size` | Peso | Uso |
|---|---|---|---|
| `.h2` | 108px | 800 | Título de destaque (raro; capa e slides "big number") |
| `.h3` | 76px | 800 | Título padrão de slide de conteúdo |
| `.h4` | 60px | 800 | Título curto (ex.: "Os números.") |
| `.lead` | 38px (varia por slide) | 400 | Parágrafo de apoio, uma ou duas linhas |
| `.card h4` | 38px | 800 | Título de card |
| `.card p` | 27px | 400 | Corpo de card |
| `.card .big` | 92px | 800 | Número gigante dentro de um card |
| `.num` | varia (140–300px conforme o slide) | 800 | Estatística isolada, sempre em `--sinal` |
| `.nl .a` | 34px | 800 | Item de lista numerada (título) |
| `.nl .a span` | 23px | 400 | Item de lista numerada (descrição) |
| `.chrome` | 22px | 500 (mono) | Cabeçalho fixo de cada slide |
| `.fonte` | 19px | 400 (mono) | Rodapé de citação de fonte |

**Todos os títulos** (`.h`) usam `letter-spacing:-.035em` e `line-height:.95` — grotesca pesada, apertada, sem respiro entre linhas. Isso é proposital: cria densidade visual sem precisar de mais peso de fonte.

---

## 4. Grade e layout

### Palco fixo

O deck **não é responsivo no sentido tradicional**. Existe um "palco" de exatamente `1920×1080px`, com todo o conteúdo posicionado em `position:absolute` dentro dele. Um script em JS escala esse palco (`transform: scale()`) para caber na tela do espectador, mantendo a proporção 16:9 sempre.

```css
#stage{position:absolute;left:50%;top:50%;width:1920px;height:1080px;transform-origin:center}
```

Consequência prática: **toda margem e posição no CSS é em pixels absolutos do palco de 1920×1080**, nunca em `%` ou `vw/vh`. Ao editar um slide, pense em coordenadas de um cartaz de 1920×1080, não em CSS responsivo.

### Margens padrão

- Margem lateral do conteúdo: **120px** da esquerda e da direita (`left:120px; right:120px`)
- Cabeçalho (`.chrome`): `top:60px`
- Rodapé de fonte (`.fonte`): `bottom:52px`
- Barra de sinal lateral (`.bar`): faixa laranja de 24px de largura, encostada na borda esquerda de todo slide

### Grade de fundo

Um `.grid` decorativo (linhas de 1px a cada 120px, opacidade 55%) fica atrás do conteúdo em quase todo slide, reforçando a estética "papel milimetrado / dado".

---

## 5. Cabeçalho de cada slide (`.chrome`)

Todo slide (exceto a capa e o encerramento) tem um cabeçalho fixo:

```html
<div class="chrome r"><span><b>01 POR QUÊ?</b></span><span>Cauê Carneiro</span></div>
```

- **Esquerda**, em negrito laranja: o bloco atual (`01 POR QUÊ?`, `02 COMO?`, `03 O QUÊ?`)
- **Direita**, em cinza: quem apresenta aquele trecho (nome ou "Nome 1 e 2" / "Nome 3" / "Nome 4", conforme o pedido do time) — pode ficar vazio quando não há rótulo

Fonte sempre mono, 22px, `letter-spacing:.04em`.

---

## 6. Componentes

### Cards (`.card`)
Retângulo branco, borda preta de 2px, `border-radius:12px`, padding 36px. Usado para estatística + legenda, ou citação. Dentro dele:
- `.big` → número gigante em laranja
- `.q` → citação, com aspas curvas (`“ ”`) coloridas em laranja via `::before`/`::after`
- `.src` → fonte da citação, mono, cinza

### Lista numerada (`.nl`)
Lista sem marcador, cada item com borda superior de 2px (e o último item ganha também borda inferior, fechando a lista como uma tabela). Número do item em mono laranja à esquerda; título em negrito; descrição em cinza abaixo do título.

### Tag (`.tag`)
Selo retangular com borda laranja de 2px, texto mono maiúsculo — usado para avisar "isto é uma persona ilustrativa" ou similar.

### Imagens (prints, telas, sites)
Quatro variantes, todas com sombra suave e cantos arredondados sobre o fundo bege:
- `.shot` — print solto (diagramas)
- `.clip` — recorte de manchete de jornal, com padding branco (parece um recorte colado)
- `.phoneimg` — moldura de celular (borda grossa preta arredondada) em volta de uma captura de tela do protótipo
- `.browser` — janela de navegador falsa (barra cinza com 3 bolinhas) em volta de um print de site

### Fluxo (`.flow` / `.step`)
Etapas lado a lado, cada uma com uma borda superior grossa (6px). A etapa `.hot` recebe a borda na cor de destaque em vez de preta, para marcar os passos mais importantes do ciclo.

### Preço (`.price`)
Bloco vertical com rótulo mono, valor gigante e descrição — usado no slide de modelo de negócio.

---

## 7. Animação

Cada elemento visível de um slide leva a classe `.r` (reveal) mais um modificador de atraso `.d1` a `.d7`:

```css
.slide .r{opacity:0;transform:translateY(28px)}
.slide.active .r{animation:in .7s cubic-bezier(.2,.7,.2,1) forwards}
.slide.active .d1{animation-delay:.12s}
/* .d2 = .26s · .d3 = .40s · .d4 = .54s · .d5 = .68s · .d6 = .82s · .d7 = .96s */
```

- Efeito: cada bloco de conteúdo sobe 28px e ganha opacidade, em cascata, com ~140ms de defasagem entre um e outro.
- Elementos que precisam de uma rotação sutil (recortes de jornal) usam a variável `--rot` (ex.: `--rot:rotate(-2deg)`), que a animação também anima via `transform:var(--rot,none)`.
- **Acessibilidade:** tudo isso é desligado com `prefers-reduced-motion: reduce` — os elementos aparecem direto, sem transição.
- Transição entre slides: crossfade de opacidade em 450ms (`.slide{transition:opacity .45s ease}`), controlado por JS adicionando/removendo a classe `.active`.

---

## 8. Interação (o "motor" do deck)

Tudo em ~40 linhas de JS puro no fim do arquivo, sem biblioteca:

| Ação | Efeito |
|---|---|
| `→` / `Espaço` / clique no terço direito da tela | Avança slide |
| `←` / clique no terço esquerdo | Volta slide |
| `Home` / `End` | Vai para o primeiro / último slide |
| `N` | Abre/fecha o painel de notas do apresentador (lê o conteúdo de `<aside class="notes">` do slide atual) |
| `F` | Tela cheia |
| `E` | Entra em modo de edição (todo texto vira `contenteditable`) |
| `Esc` | Sai do modo de edição |
| `Cmd/Ctrl+S` | Baixa uma cópia do HTML atual (com as edições feitas na hora) |
| Swipe (celular) | Avança/volta slide |

A barra de progresso (`#prog`) e o contador (`#count`) no rodapé da janela (fora do palco) refletem a posição atual. O hash da URL (`#7`) guarda o slide atual, então dá para recarregar a página no mesmo ponto.

---

## 9. Notas do apresentador

Cada `<section class="slide">` pode conter um `<aside class="notes">…</aside>` invisível no palco (`display:none`), mas lido pelo JS e mostrado no painel preto flutuante ao apertar `N`. É onde ficam as instruções de fala ("pausar aqui", "citar tal frase", link do protótipo clicável, etc.) — nunca aparecem projetadas.

---

## 10. Convenções para editar

1. **Nunca** usar hexadecimal direto fora das variáveis de `:root` — se precisar de uma cor nova, pergunte antes (mas via de regra não deveria precisar: o sistema é deliberadamente monocromático + 1 acento).
2. **Nunca** usar `%`/`vw`/`vh` para posicionar conteúdo dentro de um slide — sempre pixels absolutos do palco 1920×1080.
3. Título de slide sempre em `.h.h3` (76px) salvo exceção deliberada (números grandes usam `.h2`, 108px).
4. Toda citação/dado precisa de uma linha `.fonte` no rodapé do slide, com a fonte e a data — nenhum número aparece sem proveniência.
5. Ao adicionar um novo elemento visível, dar a ele a classe `.r` + um `.d1`–`.d7` para entrar na cascata de animação; sem isso, o elemento aparece estático, quebrando o ritmo do slide.
6. Imagens: usar a variante certa (`.shot`, `.clip`, `.phoneimg`, `.browser`) em vez de um `<img>` solto, para manter a moldura consistente.
7. Manter as margens de 120px — não colar conteúdo na borda do palco.

---

## Referência rápida (copiar e colar)

```css
:root{
  --papel:#f6f4ef; --tinta:#111; --tinta2:#3a3a3a; --sutil:#6b6b6b; --linha:#1111111a;
  --sinal:#ff4d17; --sinal-claro:#ffe3d8; --cinza:#e7e4dc; --cinza2:#d4d0c6;
  --sans:"Bricolage Grotesque",sans-serif; --mono:"IBM Plex Mono",monospace;
}
```

**Fontes:** [Bricolage Grotesque](https://fonts.google.com/specimen/Bricolage+Grotesque) (conteúdo) · [IBM Plex Mono](https://fonts.google.com/specimen/IBM+Plex+Mono) (metadado/fonte)

**Palco:** 1920×1080px, escalado uniformemente, nunca reflui para telas menores.

**Cor de acento:** `#ff4d17` — uma só, em todo o deck.
