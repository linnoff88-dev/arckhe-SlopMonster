ï»¿# SlopMonster

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


**ê°Å¸â¡Â§ê°Å¸â¡Â· [PortuguêÂªs](README.md) êÂ· ê°Å¸â¡Âºê°Å¸â¡Â¸ [English](README.en.md) êÂ· ê°Å¸â¡Âªê°Å¸â¡Â¸ [EspaêÂ±ol](README.es.md)**

![Os cinco mascotes do SlopMonster enfileirados, um para cada regra que ele pontua](docs/img/hero.png)

**Transforme texto escrito por IA em texto que uma pessoa publicaria.**

## ê°Å¸ââ Guia de uso

Guia completo (landing + passo a passo): **https://inematds.github.io/SlopMonster/guia/**

Texto de IA tem cheiro. `delve`, `seamless`, `unlock`, `it's not just a tool, it's a
journey`. O leitor jêÂ¡ percebe, e uma pêÂ¡gina com esse cheiro êÂ© uma pêÂ¡gina em que ele
para de confiar.

A maioria das ferramentas que corrige isso nasce da mesma pesquisa pêÂºblica. Esta
acrescenta as duas coisas que as outras pulam.

**Ela dêÂ¡ uma nota de 0 a 5 ao seu texto e pode derrubar o seu build.** Sem opiniêÂ£o, sem
achismo, sêÂ³ padrêÂµes. Desenvolvedor chama isso de linter. Todo mundo pode chamar de um
verificador que nêÂ£o deixa vocêÂª publicar.

**Um modelo rival faz a limpeza.** Se o Claude escreveu o rascunho, o GPT limpa. Um modelo
nêÂ£o escuta o prêÂ³prio sotaque, do mesmo jeito que vocêÂª nêÂ£o escuta o seu.

Funciona em landing pages, READMEs, e-mails e roteiros. Qualquer coisa que uma pessoa vai
ler e julgar.

## O ciclo

```
1. PONTUAR   tools/deslop.py     dêÂ¡ nota de 0 a 5, sai em vermelho abaixo de 5. PadrêÂµes, sem opiniêÂ£o.
2. REESCREVER trêÂªs passadas       mata o vocabulêÂ¡rio ê¢â â mata as formas ê¢â â coloca uma pessoa de volta
3. LIMPAR    tools/cleanse.sh    uma famêÂ­lia de modelo diferente tira as marcas que a primeira deixou
4. REPONTUAR tools/deslop.py     sêÂ³ publica em 5/5
```

O pontuador tem a primeira e a êÂºltima palavra, porque o pontuador êÂ© honesto e o modelo êÂ©
persuasivo. "Quase limpo" êÂ© como uma pêÂ¡gina acaba soando igual a todas as outras pêÂ¡ginas
de IA da internet.

## Recibos, nêÂ£o promessas

Uma rodada real em sete frases literais da home do Jasper.ai (29 ago 2026):

| | nota |
|---|---|
| O texto deles, como foi baixado | **3/5**, com `unlock`, `empower` e quatro listas de trêÂªs |
| Depois de uma passada por este ciclo | **5/5**, sentido intacto, tamanho dentro de 10%, nada inventado |

Cada comando e sua saêÂ­da exata: [`examples/jasper-live-run.md`](examples/jasper-live-run.md).
O texto deles êÂ© citado para fins de crêÂ­tica e continua sendo deles. A licenêÂ§a MIT abaixo
cobre o cêÂ³digo e a prosa deste repositêÂ³rio.

## InêÂ­cio rêÂ¡pido

```bash
git clone https://github.com/inematds/SlopMonster && cd SlopMonster

# pontuar qualquer coisa
python3 tools/deslop.py --text "It's not just a tool, it's a game-changing journey."
# ê¢â â score 3/5, aponta as duas marcas, sai com cêÂ³digo 1

# pontuar uma pêÂ¡gina pronta (lêÂª sêÂ³ o que o visitante ENXERGA)
python3 tools/deslop.py index.html

# pontuar um arquivo markdown (pula trechos de cêÂ³digo, blocos cercados e texto riscado)
python3 tools/deslop.py README.md

# limpar um rascunho com um modelo rival e repontuar
tools/cleanse.sh rascunho.md > limpo.md
python3 tools/deslop.py --text "$(cat limpo.md)"

# seus nêÂºmeros sêÂ£o reais e vocêÂª consegue provar? impede a regra de prova de travar o build
python3 tools/deslop.py index.html --allow-proof

# mexeu numa regex? isto êÂ© o que pega um catêÂ¡logo que ficou meio cego em silêÂªncio
python3 tools/test_deslop.py
```

**O catêÂ¡logo êÂ© sêÂ³ em inglêÂªs.** Texto em outra lêÂ­ngua, inclusive portuguêÂªs, recebe 5/5
porque o pontuador nêÂ£o consegue ler, nêÂ£o porque estêÂ¡ limpo. O passo de limpeza com modelo
rival funciona em qualquer lêÂ­ngua, mas a nota sêÂ³ vale para texto em inglêÂªs.

Sem dependêÂªncias. O pontuador êÂ© Python puro, biblioteca padrêÂ£o. O script de limpeza precisa
de uma CLI de IA (`codex` ou `claude`), ou de nenhuma, e nesse caso ele imprime o prompt
para vocêÂª colar. O arquivo `.github/workflows/slop.yml` êÂ© o portêÂ£o de build, pronto para
copiar no seu prêÂ³prio repositêÂ³rio.

### Instalar como skill de agente

**Claude Code:** copie esta pasta para `~/.claude/skills/slopmonster/` e diga `/slopmonster`,
"tira o slop disso" ou "de-slop this". **Codex e outros agentes:** aponte o agente para o
`SKILL.md`. SêÂ£o instruêÂ§êÂµes em markdown puro, nada especêÂ­fico do Claude.

## A limpeza sabe qual modelo escreveu

A regra: a limpeza roda numa **famêÂ­lia de modelo diferente** da que escreveu o rascunho.
FamêÂ­lias diferentes têÂªm sotaques diferentes, e um modelo êÂ© ruim em ouvir o prêÂ³prio.

| VocêÂª trabalha em | Sotaque do rascunho | O `cleanse.sh` faz |
|---|---|---|
| Claude Code | Anthropic | chama o **GPT-5.6** pela sua CLI `codex`, com tempo limite e sandbox sêÂ³ leitura |
| Codex / ChatGPT | OpenAI | com `DESLOP_WRITER=gpt` ele chama o **Claude** via `claude -p` |
| Gemini CLI | Google | a CLI rival que estiver instalada |
| nenhuma CLI rival | n/a | imprime o prompt completo para colar no chat da outra famêÂ­lia |

Ele se recusa a mandar um rascunho de volta para a prêÂ³pria famêÂ­lia. Modelo corrigindo a
prêÂ³pria prova êÂ© exatamente o que este passo existe para evitar.

Depois ele repontua, sempre: um modelo de ponta êÂ© muito bom em tirar marcas e bem capaz de
colocar outras novas enquanto faz isso.

## O que o pontuador caêÂ§a

Cinco grupos. Cai em um e perde um ponto. Abaixo de 5/5 o comando sai em vermelho, entêÂ£o
um build pode parar nele.

Quatro grupos tiram o sotaque de IA. O quinto pergunta se a linha vende alguma coisa.

Cada antes e depois abaixo êÂ© uma linha real do site da Ridgeline Roofing. Riscado êÂ© o que
o primeiro rascunho dizia. Negrito êÂ© o que foi publicado. O registro completo:
[`examples/ridgeline-roofing.md`](examples/ridgeline-roofing.md).

Cada exemplo nesta pêÂ¡gina estêÂ¡ em `formato de cêÂ³digo` ou riscado. Isso nêÂ£o êÂ© decoraêÂ§êÂ£o. Um
literal nêÂ£o êÂ© texto de venda, entêÂ£o o `deslop.py` pula os dois quando lêÂª um arquivo `.md`.

![Regra 1, vocabulêÂ¡rio de IA: delve, leverage, seamless, unlock](docs/img/rule-1-vocab.png)

**1. VocabulêÂ¡rio de IA.** Palavras que aparecem muito mais em texto de IA do que em texto
humano.

SêÂ£o duas listas, e elas funcionam de jeitos diferentes.

A primeira lista casa pela raiz. EntêÂ£o `elevate` tambêÂ©m pega `elevates`, `elevated` e
`elevating`. Isso importa mais do que parece. PêÂ¡gina de vendas êÂ© escrita na terceira pessoa.
`Acme elevates your workflow` êÂ© a forma mais comum da palavra, e a busca exata passava
direto por ela.

A segunda lista tem palavras que tambêÂ©m têÂªm um sentido honesto no dia a dia. `crafted`,
`harness`, `landscape`, `journey`. Essas sêÂ£o casadas palavra por palavra. Assim "we craft
furniture by hand" continua limpo, e sêÂ³ o uso de marketing êÂ© pego.

A correêÂ§êÂ£o êÂ© uma palavra mais simples. NêÂ£o um sinêÂ´nimo mais chique para a mesma ideia.

> ~~We leverage industry-leading materials to deliver unparalleled protection.~~
> **We source materials from manufacturers who test for wind, hail and sun.**
> `leverage`, `deliver`, `unparalleled`. Nenhuma delas diz nada, e cada uma custa uma linha.

![Regra 2, construêÂ§êÂµes de IA: "not just a tool, it's a journey"](docs/img/rule-2-phrases.png)

**2. ConstruêÂ§êÂµes de IA.** Este grupo pega formas de frase, nêÂ£o palavras isoladas.

Uma forma êÂ© um molde que vocêÂª preenche com qualquer coisa. `not just X, but Y` êÂ© o mais
barulhento do inglêÂªs hoje. Depois que vocêÂª vêÂª, nêÂ£o consegue mais deixar de ver.

HêÂ¡ 17 formas na lista. Coisas como `that's where X comes in`, `say goodbye to` e
`whether you're X or Y`, pilhas de ressalva como `could potentially`, e perguntas que o
autor responde sozinho.

As formas curta e longa sêÂ£o checadas, "it's" e "it is". Registro formal nêÂ£o êÂ© disfarce
esperto. êâ° o padrêÂ£o do que um modelo escreve.

Formas valem mais que palavras, porque uma pêÂ¡gina pode passar num teste de vocabulêÂ¡rio e
ainda parecer escrita por mêÂ¡quina.

> ~~Not just a roof, but peace of mind.~~
> **A written scope and a fixed number before anyone climbs a ladder.**
> A forma promete uma revelaêÂ§êÂ£o e entrega uma abstraêÂ§êÂ£o.

![Regra 3, cadêÂªncia de pontuaêÂ§êÂ£o: dois travessêÂµes numa frase](docs/img/rule-3-punctuation.png)

**3. CadêÂªncia de pontuaêÂ§êÂ£o.** Dois travessêÂµes dentro de uma mesma frase.

Um travessêÂ£o num parêÂ¡grafo êÂ© pontuaêÂ§êÂ£o. TrêÂªs êÂ© tique. Modelos usam travessêÂ£o numa taxa de
trêÂªs a cinco vezes a de um humano.

A checagem sêÂ³ olha dentro de uma janela de 220 caracteres, e esse limite faz trabalho de
verdade. Texto de interface nêÂ£o tem ponto final. Itens de menu, botêÂµes e rêÂ³tulos se
emendam, entêÂ£o uma quebra de frase ingêÂªnua trata a pêÂ¡gina inteira como uma frase sêÂ³ e a
regra dispara em tudo. Um pontuador que grita lobo êÂ© desligado, entêÂ£o a janela fica.

Ponto e vêÂ­rgula tambêÂ©m conta, mas sêÂ³ acima de um piso de trêÂªs, proporcional ao tamanho da
pêÂ¡gina. Dois pontos e vêÂ­rgula num documento têÂ©cnico longo êÂ© estilo, nêÂ£o marca.

> ~~Our team ê¢â¬â trained, certified and local ê¢â¬â is ready to help.~~
> **Thirty-eight on the crew, factory-trained for every material we install.**
> Os travessêÂµes escondiam que a frase nêÂ£o tinha informaêÂ§êÂ£o nenhuma.

![Regra 4, ritmo de trêÂªs: "faster, smarter, and better"](docs/img/rule-4-rhythm.png)

**4. Ritmo de trêÂªs.** TrêÂªs itens em sequêÂªncia. `faster, smarter, and better`.

TrêÂªs adjetivos êÂ© ritmo, nêÂ£o argumento. Um tricolon êÂ© retêÂ³rica. TrêÂªs deles numa pêÂ¡gina êÂ©
mêÂ¡quina. Tricolon êÂ© sêÂ³ o nome chique de uma lista de trêÂªs itens.

Esta checagem êÂ© estreita de propêÂ³sito, e sêÂ³ duas formas a disparam. Com a vêÂ­rgula de Oxford,
precisa de trêÂªs palavras soltas. Sem ela, o terceiro item tem que ser uma frase curta que
fecha a oraêÂ§êÂ£o.

A estreiteza êÂ© o ponto. "Inspection, repair and replacement for homes and commercial
buildings" sêÂ£o trêÂªs coisas reais que um telhadista faz, e passa limpo. Marcar isso seria
gritar lobo, e a prêÂ³xima pessoa desligaria o pontuador.

> ~~Trusted, reliable and built to last.~~
> **Six nails per shingle, every shingle.**
> Uma especificaêÂ§êÂ£o ganha de trêÂªs adjetivos toda vez. NinguêÂ©m inventa uma linha dessas,
> porque texto inventado nêÂ£o sabe disso.

![Regra 5, vendas e marketing: a reescrita baseada em Krug, Priestley e Hormozi](docs/img/rule-5-conversion.png)

**5. Vendas e marketing.** As quatro primeiras regras tiram o robêÂ´. Esta faz a pergunta
mais difêÂ­cil. A linha vende alguma coisa?

Texto limpo que nêÂ£o diz nada continua sendo pêÂ¡gina morta. Duas coisas rodam aqui.

**A regra dura: nunca inventar prova.** Nada de contagem de clientes, depoimentos ou
avaliaêÂ§êÂµes que o negêÂ³cio nêÂ£o conquistou. O pontuador marca qualquer nêÂºmero ao lado de um
substantivo de gente, como `10,000+ happy users`. Ele dispara fêÂ¡cil de propêÂ³sito. Um alarme
falso custa dez segundos. Um erro deixa no seu site uma afirmaêÂ§êÂ£o que vocêÂª nêÂ£o sustenta. Se
o nêÂºmero êÂ© real e vocêÂª consegue provar, `--allow-proof` rebaixa para aviso e ainda imprime
os acertos.

Prova falsa êÂ© uma falha de vendas antes de ser uma falha de escrita. NinguêÂ©m compra de uma
pêÂ¡gina que pegou mentindo.

> ~~Loved by 10,000+ happy homeowners.~~
> **Project names and photography are placeholders. Swap in your own jobs before this goes live.**
> Diga que o espaêÂ§o estêÂ¡ vazio. Soa como confianêÂ§a, nêÂ£o como fraqueza.

> ~~The area's most trusted roofing experts.~~
> **Roofing, and only roofing, since 2001.**
> "Mais confiêÂ¡vel" nêÂ£o dêÂ¡ para checar, entêÂ£o o leitor desconta. Uma data nêÂ£o tem como discutir.

**De onde as linhas vêÂªm.** Quatro fontes. Se uma frase nêÂ£o consegue dizer sua fonte, ela
nêÂ£o entra na pêÂ¡gina.

1. **O que o ofêÂ­cio faz de verdade.** A fonte mais forte, de longe. "Seis pregos por telha"
   êÂ© uma especificaêÂ§êÂ£o real com um modo de falha real por trêÂ¡s.
2. **O que o cliente jêÂ¡ teme.** Que o preêÂ§o vai mudar. Que o quintal vai ficar destruêÂ­do.
   Que estêÂ£o vendendo um telhado inteiro para um problema de rufo.
3. **O que a concorrêÂªncia nêÂ£o vai dizer.** Recusa viaja mais longe que promessa. "NêÂ£o fazemos
   sobreposiêÂ§êÂ£o" posiciona vocêÂª e desqualifica o cliente errado numa linha sêÂ³.
4. **As linhas que jêÂ¡ eram boas.** "From first call to final nail" chegou escrita no
   wireframe e ganhou de toda reescrita. Ficou.

**O trabalho nomeado por trêÂ¡s da reescrita:**

| Quem | O que isso pede |
|---|---|
| **Steve Krug**, *Don't Make Me Think* (2000) | cada linha que o leitor precisa decifrar êÂ© uma linha que ele pula |
| **Daniel Priestley**, ordem do pitch | abre no problema e no insight, nunca no produto |
| **Alex Hormozi**, o lado da oferta | dor nomeada, especificidade checêÂ¡vel, prova que vocêÂª tem de fato |

Esses trêÂªs mais o benchmark da categoria viram cinco princêÂ­pios de trabalho, cada um com um
antes e depois real: [`references/principles.md`](references/principles.md).

## O que entra no lugar das marcas

Limpo nêÂ£o êÂ© o mesmo que bom. Cinco princêÂ­pios decidem o que a linha diz no lugar: o
*Don't Make Me Think* do Krug, a ordem de pitch do Priestley que abre no problema, e o
argumento do Hormozi, pelo lado da oferta, de que especificidade vence superlativo. Cada
um com um antes e depois real: [`references/principles.md`](references/principles.md).

## Um exemplo completo

O site da Ridgeline Roofing: de um wireframe em Lorem ipsum a um site publicado, com o
antes ê¢â â depois de cada têÂ­tulo, as seis marcas pegas nos primeiros rascunhos e a passada de
verificar-ou-marcar em cada nêÂºmero. Este êÂ© o arquivo que ensina:
[`examples/ridgeline-roofing.md`](examples/ridgeline-roofing.md).

> ~~The area's most trusted roofing experts.~~
> **Roofing, and only roofing, since 2001.**
> "Mais confiêÂ¡vel" nêÂ£o êÂ© falseêÂ¡vel, entêÂ£o o leitor desconta inteiro. Uma data nêÂ£o tem como discutir.

## A êÂºnica regra dura

**Nunca inventar prova.** Nada de contagem de usuêÂ¡rios, depoimentos ou avaliaêÂ§êÂµes que vocêÂª
nêÂ£o conquistou. Se uma afirmaêÂ§êÂ£o precisa de um nêÂºmero que vocêÂª nêÂ£o tem, escreva
`[needs number]` e siga em frente. O ganho de um nêÂºmero inventado êÂ© menor que o ganho de
uma especificidade real, e êÂ© o êÂºnico erro sem volta.

E esta skill nunca vai prometer "enganar detectores de IA". Detector êÂ© ruêÂ­do. O alvo êÂ© o
instinto de um leitor humano.

Sobre este arquivo: `python3 tools/deslop.py README.md` dêÂ¡ **5/5**, mas com uma ressalva
honesta. Os exemplos em inglêÂªs estêÂ£o marcados como literais, em `cêÂ³digo` ou riscados, e o
pontuador pula os dois. A prosa em volta estêÂ¡ em portuguêÂªs, e o catêÂ¡logo nêÂ£o lêÂª portuguêÂªs.
A versêÂ£o original em inglêÂªs deste README passava no prêÂ³prio pontuador sem nada suavizado.

## Em que isto se apoia

Todas as fontes estêÂ£o em [`references/sources.md`](references/sources.md):
o *Signs of AI writing* da Wikipedia (WikiProject AI Cleanup) como catêÂ¡logo canêÂ´nico,
mais quatro humanizadores open source sob MIT: `blader/humanizer`,
`harshaneel/humanize`, `lguz/humanize-writing-skill`, `haidrrrry/humanize-ai-writing`.
Os princêÂ­pios de reescrita vêÂªm de Krug, Priestley e Hormozi. RepositêÂ³rios de burlar
detector ficam de fora de propêÂ³sito.

## Mapa do repositêÂ³rio

```
SKILL.md                            a skill de agente, o ciclo inteiro como instruêÂ§êÂµes
tools/deslop.py                     o pontuador. biblioteca padrêÂ£o, sem deps, sai em vermelho abaixo de 5/5
tools/test_deslop.py                suêÂ­te de regressêÂ£o. rode depois de mexer em qualquer regex
tools/cleanse.sh                    limpeza com modelo rival, roteada sozinha, com tempo limite
.github/workflows/slop.yml          o portêÂ£o de build, pronto para copiar
prompts/cleanse.txt                 a instruêÂ§êÂ£o exata que o modelo de limpeza recebe
references/signs-of-ai-writing.md   o catêÂ¡logo completo: 2 nêÂ­veis de vocabulêÂ¡rio, 8 formas, cadêÂªncia, ritmo, prova
references/principles.md            os cinco princêÂ­pios de reescrita, cada um com um par real
references/sources.md               cada fonte em que isto se apoia
examples/ridgeline-roofing.md       site completo, cada linha antes ê¢â â depois
examples/jasper-live-run.md         rodada real sem ediêÂ§êÂ£o: 3/5 ê¢â â 5/5 numa pêÂ¡gina de verdade
```

MIT. A mesma dos humanizadores em que se apoia.

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>