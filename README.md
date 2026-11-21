# SlopMonster

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


**ðŸ‡§ðŸ‡· [PortuguÃªs](README.md) Â· ðŸ‡ºðŸ‡¸ [English](README.en.md) Â· ðŸ‡ªðŸ‡¸ [EspaÃ±ol](README.es.md)**

![Os cinco mascotes do SlopMonster enfileirados, um para cada regra que ele pontua](docs/img/hero.png)

**Transforme texto escrito por IA em texto que uma pessoa publicaria.**

## ðŸ“– Guia de uso

Guia completo (landing + passo a passo): **https://inematds.github.io/SlopMonster/guia/**

Texto de IA tem cheiro. `delve`, `seamless`, `unlock`, `it's not just a tool, it's a
journey`. O leitor jÃ¡ percebe, e uma pÃ¡gina com esse cheiro Ã© uma pÃ¡gina em que ele
para de confiar.

A maioria das ferramentas que corrige isso nasce da mesma pesquisa pÃºblica. Esta
acrescenta as duas coisas que as outras pulam.

**Ela dÃ¡ uma nota de 0 a 5 ao seu texto e pode derrubar o seu build.** Sem opiniÃ£o, sem
achismo, sÃ³ padrÃµes. Desenvolvedor chama isso de linter. Todo mundo pode chamar de um
verificador que nÃ£o deixa vocÃª publicar.

**Um modelo rival faz a limpeza.** Se o Claude escreveu o rascunho, o GPT limpa. Um modelo
nÃ£o escuta o prÃ³prio sotaque, do mesmo jeito que vocÃª nÃ£o escuta o seu.

Funciona em landing pages, READMEs, e-mails e roteiros. Qualquer coisa que uma pessoa vai
ler e julgar.

## O ciclo

```
1. PONTUAR   tools/deslop.py     dÃ¡ nota de 0 a 5, sai em vermelho abaixo de 5. PadrÃµes, sem opiniÃ£o.
2. REESCREVER trÃªs passadas       mata o vocabulÃ¡rio â†’ mata as formas â†’ coloca uma pessoa de volta
3. LIMPAR    tools/cleanse.sh    uma famÃ­lia de modelo diferente tira as marcas que a primeira deixou
4. REPONTUAR tools/deslop.py     sÃ³ publica em 5/5
```

O pontuador tem a primeira e a Ãºltima palavra, porque o pontuador Ã© honesto e o modelo Ã©
persuasivo. "Quase limpo" Ã© como uma pÃ¡gina acaba soando igual a todas as outras pÃ¡ginas
de IA da internet.

## Recibos, nÃ£o promessas

Uma rodada real em sete frases literais da home do Jasper.ai (29 ago 2026):

| | nota |
|---|---|
| O texto deles, como foi baixado | **3/5**, com `unlock`, `empower` e quatro listas de trÃªs |
| Depois de uma passada por este ciclo | **5/5**, sentido intacto, tamanho dentro de 10%, nada inventado |

Cada comando e sua saÃ­da exata: [`examples/jasper-live-run.md`](examples/jasper-live-run.md).
O texto deles Ã© citado para fins de crÃ­tica e continua sendo deles. A licenÃ§a MIT abaixo
cobre o cÃ³digo e a prosa deste repositÃ³rio.

## InÃ­cio rÃ¡pido

```bash
git clone https://github.com/inematds/SlopMonster && cd SlopMonster

# pontuar qualquer coisa
python3 tools/deslop.py --text "It's not just a tool, it's a game-changing journey."
# â†’ score 3/5, aponta as duas marcas, sai com cÃ³digo 1

# pontuar uma pÃ¡gina pronta (lÃª sÃ³ o que o visitante ENXERGA)
python3 tools/deslop.py index.html

# pontuar um arquivo markdown (pula trechos de cÃ³digo, blocos cercados e texto riscado)
python3 tools/deslop.py README.md

# limpar um rascunho com um modelo rival e repontuar
tools/cleanse.sh rascunho.md > limpo.md
python3 tools/deslop.py --text "$(cat limpo.md)"

# seus nÃºmeros sÃ£o reais e vocÃª consegue provar? impede a regra de prova de travar o build
python3 tools/deslop.py index.html --allow-proof

# mexeu numa regex? isto Ã© o que pega um catÃ¡logo que ficou meio cego em silÃªncio
python3 tools/test_deslop.py
```

**O catÃ¡logo Ã© sÃ³ em inglÃªs.** Texto em outra lÃ­ngua, inclusive portuguÃªs, recebe 5/5
porque o pontuador nÃ£o consegue ler, nÃ£o porque estÃ¡ limpo. O passo de limpeza com modelo
rival funciona em qualquer lÃ­ngua, mas a nota sÃ³ vale para texto em inglÃªs.

Sem dependÃªncias. O pontuador Ã© Python puro, biblioteca padrÃ£o. O script de limpeza precisa
de uma CLI de IA (`codex` ou `claude`), ou de nenhuma, e nesse caso ele imprime o prompt
para vocÃª colar. O arquivo `.github/workflows/slop.yml` Ã© o portÃ£o de build, pronto para
copiar no seu prÃ³prio repositÃ³rio.

### Instalar como skill de agente

**Claude Code:** copie esta pasta para `~/.claude/skills/slopmonster/` e diga `/slopmonster`,
"tira o slop disso" ou "de-slop this". **Codex e outros agentes:** aponte o agente para o
`SKILL.md`. SÃ£o instruÃ§Ãµes em markdown puro, nada especÃ­fico do Claude.

## A limpeza sabe qual modelo escreveu

A regra: a limpeza roda numa **famÃ­lia de modelo diferente** da que escreveu o rascunho.
FamÃ­lias diferentes tÃªm sotaques diferentes, e um modelo Ã© ruim em ouvir o prÃ³prio.

| VocÃª trabalha em | Sotaque do rascunho | O `cleanse.sh` faz |
|---|---|---|
| Claude Code | Anthropic | chama o **GPT-5.6** pela sua CLI `codex`, com tempo limite e sandbox sÃ³ leitura |
| Codex / ChatGPT | OpenAI | com `DESLOP_WRITER=gpt` ele chama o **Claude** via `claude -p` |
| Gemini CLI | Google | a CLI rival que estiver instalada |
| nenhuma CLI rival | n/a | imprime o prompt completo para colar no chat da outra famÃ­lia |

Ele se recusa a mandar um rascunho de volta para a prÃ³pria famÃ­lia. Modelo corrigindo a
prÃ³pria prova Ã© exatamente o que este passo existe para evitar.

Depois ele repontua, sempre: um modelo de ponta Ã© muito bom em tirar marcas e bem capaz de
colocar outras novas enquanto faz isso.

## O que o pontuador caÃ§a

Cinco grupos. Cai em um e perde um ponto. Abaixo de 5/5 o comando sai em vermelho, entÃ£o
um build pode parar nele.

Quatro grupos tiram o sotaque de IA. O quinto pergunta se a linha vende alguma coisa.

Cada antes e depois abaixo Ã© uma linha real do site da Ridgeline Roofing. Riscado Ã© o que
o primeiro rascunho dizia. Negrito Ã© o que foi publicado. O registro completo:
[`examples/ridgeline-roofing.md`](examples/ridgeline-roofing.md).

Cada exemplo nesta pÃ¡gina estÃ¡ em `formato de cÃ³digo` ou riscado. Isso nÃ£o Ã© decoraÃ§Ã£o. Um
literal nÃ£o Ã© texto de venda, entÃ£o o `deslop.py` pula os dois quando lÃª um arquivo `.md`.

![Regra 1, vocabulÃ¡rio de IA: delve, leverage, seamless, unlock](docs/img/rule-1-vocab.png)

**1. VocabulÃ¡rio de IA.** Palavras que aparecem muito mais em texto de IA do que em texto
humano.

SÃ£o duas listas, e elas funcionam de jeitos diferentes.

A primeira lista casa pela raiz. EntÃ£o `elevate` tambÃ©m pega `elevates`, `elevated` e
`elevating`. Isso importa mais do que parece. PÃ¡gina de vendas Ã© escrita na terceira pessoa.
`Acme elevates your workflow` Ã© a forma mais comum da palavra, e a busca exata passava
direto por ela.

A segunda lista tem palavras que tambÃ©m tÃªm um sentido honesto no dia a dia. `crafted`,
`harness`, `landscape`, `journey`. Essas sÃ£o casadas palavra por palavra. Assim "we craft
furniture by hand" continua limpo, e sÃ³ o uso de marketing Ã© pego.

A correÃ§Ã£o Ã© uma palavra mais simples. NÃ£o um sinÃ´nimo mais chique para a mesma ideia.

> ~~We leverage industry-leading materials to deliver unparalleled protection.~~
> **We source materials from manufacturers who test for wind, hail and sun.**
> `leverage`, `deliver`, `unparalleled`. Nenhuma delas diz nada, e cada uma custa uma linha.

![Regra 2, construÃ§Ãµes de IA: "not just a tool, it's a journey"](docs/img/rule-2-phrases.png)

**2. ConstruÃ§Ãµes de IA.** Este grupo pega formas de frase, nÃ£o palavras isoladas.

Uma forma Ã© um molde que vocÃª preenche com qualquer coisa. `not just X, but Y` Ã© o mais
barulhento do inglÃªs hoje. Depois que vocÃª vÃª, nÃ£o consegue mais deixar de ver.

HÃ¡ 17 formas na lista. Coisas como `that's where X comes in`, `say goodbye to` e
`whether you're X or Y`, pilhas de ressalva como `could potentially`, e perguntas que o
autor responde sozinho.

As formas curta e longa sÃ£o checadas, "it's" e "it is". Registro formal nÃ£o Ã© disfarce
esperto. Ã‰ o padrÃ£o do que um modelo escreve.

Formas valem mais que palavras, porque uma pÃ¡gina pode passar num teste de vocabulÃ¡rio e
ainda parecer escrita por mÃ¡quina.

> ~~Not just a roof, but peace of mind.~~
> **A written scope and a fixed number before anyone climbs a ladder.**
> A forma promete uma revelaÃ§Ã£o e entrega uma abstraÃ§Ã£o.

![Regra 3, cadÃªncia de pontuaÃ§Ã£o: dois travessÃµes numa frase](docs/img/rule-3-punctuation.png)

**3. CadÃªncia de pontuaÃ§Ã£o.** Dois travessÃµes dentro de uma mesma frase.

Um travessÃ£o num parÃ¡grafo Ã© pontuaÃ§Ã£o. TrÃªs Ã© tique. Modelos usam travessÃ£o numa taxa de
trÃªs a cinco vezes a de um humano.

A checagem sÃ³ olha dentro de uma janela de 220 caracteres, e esse limite faz trabalho de
verdade. Texto de interface nÃ£o tem ponto final. Itens de menu, botÃµes e rÃ³tulos se
emendam, entÃ£o uma quebra de frase ingÃªnua trata a pÃ¡gina inteira como uma frase sÃ³ e a
regra dispara em tudo. Um pontuador que grita lobo Ã© desligado, entÃ£o a janela fica.

Ponto e vÃ­rgula tambÃ©m conta, mas sÃ³ acima de um piso de trÃªs, proporcional ao tamanho da
pÃ¡gina. Dois pontos e vÃ­rgula num documento tÃ©cnico longo Ã© estilo, nÃ£o marca.

> ~~Our team â€” trained, certified and local â€” is ready to help.~~
> **Thirty-eight on the crew, factory-trained for every material we install.**
> Os travessÃµes escondiam que a frase nÃ£o tinha informaÃ§Ã£o nenhuma.

![Regra 4, ritmo de trÃªs: "faster, smarter, and better"](docs/img/rule-4-rhythm.png)

**4. Ritmo de trÃªs.** TrÃªs itens em sequÃªncia. `faster, smarter, and better`.

TrÃªs adjetivos Ã© ritmo, nÃ£o argumento. Um tricolon Ã© retÃ³rica. TrÃªs deles numa pÃ¡gina Ã©
mÃ¡quina. Tricolon Ã© sÃ³ o nome chique de uma lista de trÃªs itens.

Esta checagem Ã© estreita de propÃ³sito, e sÃ³ duas formas a disparam. Com a vÃ­rgula de Oxford,
precisa de trÃªs palavras soltas. Sem ela, o terceiro item tem que ser uma frase curta que
fecha a oraÃ§Ã£o.

A estreiteza Ã© o ponto. "Inspection, repair and replacement for homes and commercial
buildings" sÃ£o trÃªs coisas reais que um telhadista faz, e passa limpo. Marcar isso seria
gritar lobo, e a prÃ³xima pessoa desligaria o pontuador.

> ~~Trusted, reliable and built to last.~~
> **Six nails per shingle, every shingle.**
> Uma especificaÃ§Ã£o ganha de trÃªs adjetivos toda vez. NinguÃ©m inventa uma linha dessas,
> porque texto inventado nÃ£o sabe disso.

![Regra 5, vendas e marketing: a reescrita baseada em Krug, Priestley e Hormozi](docs/img/rule-5-conversion.png)

**5. Vendas e marketing.** As quatro primeiras regras tiram o robÃ´. Esta faz a pergunta
mais difÃ­cil. A linha vende alguma coisa?

Texto limpo que nÃ£o diz nada continua sendo pÃ¡gina morta. Duas coisas rodam aqui.

**A regra dura: nunca inventar prova.** Nada de contagem de clientes, depoimentos ou
avaliaÃ§Ãµes que o negÃ³cio nÃ£o conquistou. O pontuador marca qualquer nÃºmero ao lado de um
substantivo de gente, como `10,000+ happy users`. Ele dispara fÃ¡cil de propÃ³sito. Um alarme
falso custa dez segundos. Um erro deixa no seu site uma afirmaÃ§Ã£o que vocÃª nÃ£o sustenta. Se
o nÃºmero Ã© real e vocÃª consegue provar, `--allow-proof` rebaixa para aviso e ainda imprime
os acertos.

Prova falsa Ã© uma falha de vendas antes de ser uma falha de escrita. NinguÃ©m compra de uma
pÃ¡gina que pegou mentindo.

> ~~Loved by 10,000+ happy homeowners.~~
> **Project names and photography are placeholders. Swap in your own jobs before this goes live.**
> Diga que o espaÃ§o estÃ¡ vazio. Soa como confianÃ§a, nÃ£o como fraqueza.

> ~~The area's most trusted roofing experts.~~
> **Roofing, and only roofing, since 2001.**
> "Mais confiÃ¡vel" nÃ£o dÃ¡ para checar, entÃ£o o leitor desconta. Uma data nÃ£o tem como discutir.

**De onde as linhas vÃªm.** Quatro fontes. Se uma frase nÃ£o consegue dizer sua fonte, ela
nÃ£o entra na pÃ¡gina.

1. **O que o ofÃ­cio faz de verdade.** A fonte mais forte, de longe. "Seis pregos por telha"
   Ã© uma especificaÃ§Ã£o real com um modo de falha real por trÃ¡s.
2. **O que o cliente jÃ¡ teme.** Que o preÃ§o vai mudar. Que o quintal vai ficar destruÃ­do.
   Que estÃ£o vendendo um telhado inteiro para um problema de rufo.
3. **O que a concorrÃªncia nÃ£o vai dizer.** Recusa viaja mais longe que promessa. "NÃ£o fazemos
   sobreposiÃ§Ã£o" posiciona vocÃª e desqualifica o cliente errado numa linha sÃ³.
4. **As linhas que jÃ¡ eram boas.** "From first call to final nail" chegou escrita no
   wireframe e ganhou de toda reescrita. Ficou.

**O trabalho nomeado por trÃ¡s da reescrita:**

| Quem | O que isso pede |
|---|---|
| **Steve Krug**, *Don't Make Me Think* (2000) | cada linha que o leitor precisa decifrar Ã© uma linha que ele pula |
| **Daniel Priestley**, ordem do pitch | abre no problema e no insight, nunca no produto |
| **Alex Hormozi**, o lado da oferta | dor nomeada, especificidade checÃ¡vel, prova que vocÃª tem de fato |

Esses trÃªs mais o benchmark da categoria viram cinco princÃ­pios de trabalho, cada um com um
antes e depois real: [`references/principles.md`](references/principles.md).

## O que entra no lugar das marcas

Limpo nÃ£o Ã© o mesmo que bom. Cinco princÃ­pios decidem o que a linha diz no lugar: o
*Don't Make Me Think* do Krug, a ordem de pitch do Priestley que abre no problema, e o
argumento do Hormozi, pelo lado da oferta, de que especificidade vence superlativo. Cada
um com um antes e depois real: [`references/principles.md`](references/principles.md).

## Um exemplo completo

O site da Ridgeline Roofing: de um wireframe em Lorem ipsum a um site publicado, com o
antes â†’ depois de cada tÃ­tulo, as seis marcas pegas nos primeiros rascunhos e a passada de
verificar-ou-marcar em cada nÃºmero. Este Ã© o arquivo que ensina:
[`examples/ridgeline-roofing.md`](examples/ridgeline-roofing.md).

> ~~The area's most trusted roofing experts.~~
> **Roofing, and only roofing, since 2001.**
> "Mais confiÃ¡vel" nÃ£o Ã© falseÃ¡vel, entÃ£o o leitor desconta inteiro. Uma data nÃ£o tem como discutir.

## A Ãºnica regra dura

**Nunca inventar prova.** Nada de contagem de usuÃ¡rios, depoimentos ou avaliaÃ§Ãµes que vocÃª
nÃ£o conquistou. Se uma afirmaÃ§Ã£o precisa de um nÃºmero que vocÃª nÃ£o tem, escreva
`[needs number]` e siga em frente. O ganho de um nÃºmero inventado Ã© menor que o ganho de
uma especificidade real, e Ã© o Ãºnico erro sem volta.

E esta skill nunca vai prometer "enganar detectores de IA". Detector Ã© ruÃ­do. O alvo Ã© o
instinto de um leitor humano.

Sobre este arquivo: `python3 tools/deslop.py README.md` dÃ¡ **5/5**, mas com uma ressalva
honesta. Os exemplos em inglÃªs estÃ£o marcados como literais, em `cÃ³digo` ou riscados, e o
pontuador pula os dois. A prosa em volta estÃ¡ em portuguÃªs, e o catÃ¡logo nÃ£o lÃª portuguÃªs.
A versÃ£o original em inglÃªs deste README passava no prÃ³prio pontuador sem nada suavizado.

## Em que isto se apoia

Todas as fontes estÃ£o em [`references/sources.md`](references/sources.md):
o *Signs of AI writing* da Wikipedia (WikiProject AI Cleanup) como catÃ¡logo canÃ´nico,
mais quatro humanizadores open source sob MIT: `blader/humanizer`,
`harshaneel/humanize`, `lguz/humanize-writing-skill`, `haidrrrry/humanize-ai-writing`.
Os princÃ­pios de reescrita vÃªm de Krug, Priestley e Hormozi. RepositÃ³rios de burlar
detector ficam de fora de propÃ³sito.

## Mapa do repositÃ³rio

```
SKILL.md                            a skill de agente, o ciclo inteiro como instruÃ§Ãµes
tools/deslop.py                     o pontuador. biblioteca padrÃ£o, sem deps, sai em vermelho abaixo de 5/5
tools/test_deslop.py                suÃ­te de regressÃ£o. rode depois de mexer em qualquer regex
tools/cleanse.sh                    limpeza com modelo rival, roteada sozinha, com tempo limite
.github/workflows/slop.yml          o portÃ£o de build, pronto para copiar
prompts/cleanse.txt                 a instruÃ§Ã£o exata que o modelo de limpeza recebe
references/signs-of-ai-writing.md   o catÃ¡logo completo: 2 nÃ­veis de vocabulÃ¡rio, 8 formas, cadÃªncia, ritmo, prova
references/principles.md            os cinco princÃ­pios de reescrita, cada um com um par real
references/sources.md               cada fonte em que isto se apoia
examples/ridgeline-roofing.md       site completo, cada linha antes â†’ depois
examples/jasper-live-run.md         rodada real sem ediÃ§Ã£o: 3/5 â†’ 5/5 numa pÃ¡gina de verdade
```

MIT. A mesma dos humanizadores em que se apoia.

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>