# Especificação técnica — Educatata Web

Nov 26, 2026 · @Hamilton

Este documento descreve o Educatata Web como ele está no arquivo `index.html` de 27/11/2026 (versão 39) e serve de referência para continuar o desenvolvimento com IA no modo de código. Quando o código e este documento discordarem, o código vale e o documento deve ser corrigido.

## 1. Visão geral

O Educatata é um aplicativo educacional para celular, em formato gamificado: o aluno cuida de um mascote (no estilo do Pou) e joga minijogos para ganhar moedas, XP e estrelas.

| Item | Definição |
| --- | --- |
| Público | Alunos do 2º e 3º ano do ensino fundamental |
| Origem da necessidade | Entrevista com uma professora do ensino fundamental: dificuldade de atenção dos alunos |
| Pedido da professora | Um aplicativo com foco em resolução de problemas |
| Habilidades trabalhadas | Lógica e foco |
| Matérias | Português, matemática, ciências, história, geografia, arte (artes visuais, música e teatro com literatura clássica) e tecnologia (uso de celular, tablet e computador, ligado à BNCC Computação) |
| Avaliação | Número de tentativas por partida: o aluno compara o próprio desempenho com ele mesmo |
| Plataforma | Web, publicada no GitHub Pages; pensada para celular (até 430 px de largura) |

**Contexto acadêmico.** Projeto em equipe da disciplina Experiência Criativa: Explorando Computação e Inteligência Artificial (PUCPR, Ciência da Computação, 2026/2). A primeira versão foi um protótipo em Java/Processing feito sem IA, só com o minijogo Mercadinho. Nesta fase, o objetivo é otimizar e ampliar o app com uso extensivo de IA, publicar o código no GitHub e o site no GitHub Pages.

**Entregáveis desta fase:**

- Código novo no GitHub.
- Página publicada no GitHub Pages (ou outro provedor web).
- Todos os prompts usados para pedir melhorias à IA, registrados em `PROMPTS.md` e entregues direto ao professor (o arquivo não fica no repositório).

## 2. Arquitetura e tecnologia

O app inteiro é **um único arquivo `index.html`** (cerca de 5.750 linhas) com HTML, CSS e JavaScript puro, sem bibliotecas, sem build e sem servidor. Ele funciona aberto direto no navegador, offline, e vai para o GitHub Pages como está.

| Item | Escolha |
| --- | --- |
| Linguagens | HTML5, CSS3, JavaScript (ES2020, `'use strict'`) |
| Dependências | Nenhuma |
| Imagens | Sprites PNG embutidos como data URL; mascote gerado em `<canvas>` |
| Som | Web Audio API (efeitos e música, sintetizados na hora, sem arquivos) e narração com voz gravada: uma voz de IA criada para o app, em pacotes `midia/voz/voz-<grupo>.js`; a síntese de voz do aparelho (Speech Synthesis, pt-BR) fica de reserva |
| Dados | `localStorage`, chave `appmm-web-v1` |
| Idioma do código | Português: variáveis, funções e comentários |
| Tamanho da tela | `#app` com no máximo 430 px de largura e altura `100dvh`; em telas maiores vira um "celular" centralizado com cantos arredondados |

### Ordem das partes dentro do `<script>`

1. **Utilidades:** `$`, `$$`, `sortear(a, b)`, `embaralhar(lista)`, `reais(centavos)`, `semAcento(s)`, `estrelasTxt(n)`.
2. **Estado salvo:** `CHAVE`, `estadoInicial()`, `carregar()`, `salvar()`, `estado`, `xpNecessario(nivel)`, `ECONOMIA` (todos os valores de moedas e XP).
3. **Som e narração:** `tocar(tipo)`, `falar(texto)`.
4. **Música de fundo:** `ARQUIVOS_MUSICA`, `INSTRUMENTOS`, `PERCUSSAO`, `MUSICAS`, a leitura das partituras (`lerNotas()`, `lerAcorde()`, `prepararMusica()`), a síntese (`tocarNota()`, `tocarEvento()`, `criarEco()`, `montarMusica()`), os efeitos (`cliqueMusical()`, `musicaDoTom()`, `tocarEfeito()`) e o tocador do app (`nivelDaCasa()`, `animarMusica()`, `abafarMusica()`, `pararMusica()`, `definirMusica(nome)`).
5. **Sprites:** `SPRITES`, `ESCALA`, `SOMBRA_MASCOTE`, `spr(nome, escala, alt)` e a pixel art dos jogos: `PALETA_PX`, `ARTE`, `arteURL()`, `arte()`, `grade()`, `retangulo()`, `elipsePx()`, `poligono()`, `colar()`, `FONTE_GRANDE`, `FONTE_PEQUENA`, `textoLinhas()`, `textoPx()` e `precoPx()` (seção 8).
6. **Sistema de vida:** `META_DIARIA`, `diaDe(ts)`, `vitoriasNoDia(dia)`, `vitoriasHoje()`, `coracaoDoDia(v)`.
7. **Componentes:** `SETA_VOLTAR`, `botaoVoltar(id, rotulo)`, `portaHTML()`, `sairPelaPorta(botao, acao)`, `cabecalho(titulo, extra)`, `ligarVoltar(main)`, `pillMoedas()`.
8. **Personalização e mascote:** `CORES`, `ITENS`, `desenharMascotePixel()`, `quadroMascote()`, `mascoteHTML(humor, visual, escala)`.
9. **Dados dos jogos:** `PRODUTOS`, `PRODUTOS_NA_PRATELEIRA`, `DINHEIRO`, `CONEXOES`, `CORES_GRUPO`, `EXPLICACAO_GRUPOS`, `FALA_GRUPOS`, `FIGURAS`, `PALAVRAS_CIENCIAS`.
10. **Minijogos:** objeto `JOGOS` e os geradores `gerarRodadasMercado()`, `gerarBalancas()`, `gerarGrade()`.
11. **Navegação:** `partida`, `aoRedimensionar`, `ir(tela, dados)`, `aviso(texto)`, `confirmar({...})`.
12. **Telas:** uma função `telaX(main, dados)` para cada tela, e o mapa `TELAS`.
13. **Início:** `ir(estado.onboarded ? 'inicio' : 'onboarding')`.

### Navegação

`ir(tela, dados)` é a única forma de trocar de tela. Ela:

1. Soma 1 em `partida`, o que cancela qualquer `setTimeout` pendente da tela anterior.
2. Limpa `aoRedimensionar`, cancela a fala e remove janelas, confetes, avisos e cópias de arraste abertos.
3. Escolhe a música: `'inicio'` para as telas do app, o id do jogo na tela `jogo`, e nenhuma na tela `resultado`.
4. Esvazia o `<main id="conteudo">` e chama `TELAS[tela](main, dados)`.

Telas registradas em `TELAS`: `onboarding`, `ajuda`, `inicio`, `jogos`, `jogo`, `resultado`, `visual`, `progresso`. As configurações não são uma tela: são uma janela aberta por `abrirConfig()`.

### Contrato dos minijogos

Cada jogo em `JOGOS` tem `nome`, `artigo` (`'o'` ou `'a'`, para o mascote dizer "no Mercadinho" e "na Balança"), `materia`, `icone` (nome do desenho em `ARTE`, ex.: `'icone-balanca'`), `fundo` (cor), `dificuldades` (as regras de `facil`, `medio` e `dificil`, seção 5) e `iniciar(area, ctl)`. A tela `jogo` monta o cabeçalho, o rodapé e passa ao jogo o objeto `ctl`:

| Campo de `ctl` | O que é |
| --- | --- |
| `config` | As regras da dificuldade escolhida, `JOGOS[id].dificuldades[estado.dificuldade]` (ex.: quantas palavras, rodadas ou pares). A dificuldade fica fixa até o fim da partida |

| Função de `ctl` | O que faz |
| --- | --- |
| `progresso(texto)` | Atualiza a pílula do topo (ex.: "2/3") |
| `instrucao(texto, falar = true)` | Mostra a instrução e, se pedido, lê em voz alta |
| `tentativa()` | Soma 1 tentativa e atualiza o rodapé |
| `msg(texto, tipo)` | Mensagem embaixo da área do jogo; `tipo` = `'ok'`, `'erro'` ou vazio |
| `acerto(texto)` | Som de acerto + mensagem verde + uma camada a mais na música (`animarMusica()`) |
| `erro(texto, elemento)` | Som de erro + mensagem vermelha + elemento treme |
| `depois(ms, fn)` | `setTimeout` que só roda se o aluno ainda estiver na mesma tela |
| `aoRedimensionar(fn)` | Função chamada quando a janela muda de tamanho |
| `fim(minimo)` | Encerra a partida após 800 ms e chama `terminarJogo(id, tentativas, minimo, dificuldade)` |

**Regra:** dentro de um jogo, todo atraso usa `ctl.depois`, nunca `setTimeout` direto. Assim nada roda depois que o aluno sai da tela.

## 3. Fluxo de telas

Na primeira vez, o aluno passa pela apresentação e pelo vídeo; depois, o app sempre abre na tela inicial, de onde sai todo o resto.

&#91;embedded content: fluxo de telas · 8 telas e 1 janela\]

Jogos, Visual e Progresso voltam à tela inicial pela seta do topo. A Vitória também tem "Voltar ao mascote". As Configurações abrem o Vídeo de ajuda e a Apresentação de novo, quando pedido.

## 4. Telas

### 4.1 Apresentação (`onboarding`, 3 telas)

Aparece só enquanto `estado.onboarded` for `false`.

| Tela | Título | Texto | Ilustração |
| --- | --- | --- | --- |
| 1 | Conheça seu mascote! | Ele vai acompanhar você nos jogos e comemorar cada conquista. | Mascote flutuando |
| 2 | Aprenda jogando | Resolva desafios de matemática, português e ciências. | Ícones dos 5 jogos + estrela |
| 3 | Ganhe moedas e encha o coração | Jogue todo dia para encher o coração e use as moedas no mascote. | Moeda (×8) e três corações (×4) |

- Botão **Pular** no canto superior direito e botão **Continuar** embaixo. **Pular** pede confirmação ("Pular a apresentação?", "Você pode ver de novo quando quiser, nas Configurações."), com **Continuar vendo** em destaque e **Pular** ao lado; tocar fora da janela só fecha.
- Quatro bolinhas de posição (a 4ª é a tela do vídeo).
- **Pular** (confirmado) e o fim da 3ª tela levam à tela de ajuda com `{ primeira: true }`.

### 4.2 Vídeo de ajuda (`ajuda`) — 4ª tela da apresentação

- Duas versões do vídeo de apresentação, em `VIDEOS_AJUDA`:
  - `semLibras`: `midia/apresentacao.mp4` (36,9 s, em pé, 1080 × 1920, com narração e legendas), capa `midia/apresentacao.jpg`;
  - `libras`: `midia/apresentacao-libras.mp4` (69 s, deitado, 1920 × 1080, com janela de Libras no canto), capa `midia/apresentacao-libras.jpg`.
- Ao abrir a tela, uma janela pergunta **"Vídeo com Libras?"** ("Você precisa do vídeo com tradução em Libras, a Língua Brasileira de Sinais?"), com **Não** e **Sim, com Libras**. Tocar fora da janela vale como "Não". A escolha não fica salva: a pergunta aparece toda vez.
- Depois da resposta, a caixa mostra a versão escolhida e o vídeo já começa a tocar. Um link embaixo ("Ver o vídeo com Libras" / "Ver o vídeo sem Libras") troca de versão.
- O vídeo vira `<video controls playsinline>`; a música para quando o vídeo toca e volta quando pausa. Quando o vídeo começa, a narração do app também para (`calar()`), para as duas vozes nunca tocarem juntas.
- A caixa acompanha a versão: em pé (9:16) para o vídeo sem Libras, com a altura acompanhando a tela (`clamp(200px, 100dvh − 398px, 540px)`), e deitada (16:9, largura toda) para o vídeo com Libras. O botão de tela cheia do player mostra o vídeo grande.
- Se o arquivo não carregar (por exemplo, só o `index.html` aberto sem a pasta `midia/`), a caixa mostra "Vídeo indisponível" e, ao tocar, "Abra o app pelo site para ver o vídeo.". Com `src` vazio, mostra "Vídeo em produção".
- Como os vídeos foram feitos (decisões de edição, roteiro e o texto adaptado para o avatar de Libras): `Estudo de edição — Educatata.md`.
- Título "Como usar" e três quadros com um desenho grande e uma palavra só: Tocar (dedo), Arrastar (mão) e Ouvir (alto-falante). Pensado para crianças que ainda leem pouco: quase sem texto.
- Na primeira vez, esta é a última tela da apresentação: não tem **Pular**, só o botão **Começar**.
- **Começar** ou confirmar o pulo marca `estado.onboarded = true` e vai para a tela inicial.
- Aberta pelas configurações (`primeira: false`), mostra a seta com **Voltar** no canto de cima.

### 4.3 Tela inicial (`inicio`)

Segue o wireframe de 20/11:

- **Topo:** à esquerda, selo "Nível N" com a estrela em pixel art e barra de XP; à direita, o coração do dia e a pílula de moedas. Tocar no coração mostra "Hoje: X de 3 vitórias" e a frase do nível do dia.
- **Aba de notícias:** faixa com o sprite do LED piscando e frases passando da direita para a esquerda, uma de cada vez, a 55 px/s, sem repetir a anterior. A lista (`noticiasDeHoje()`) junta `NOTICIAS` (28: novidades, uma dica para cada jogo, curiosidades e "desafios do dia", que levam o que a criança aprendeu para fora da tela, como "ache em casa três coisas com forma de círculo") com as datas do mês atual (`NOTICIAS_DO_MES`: Dia Mundial da Água, Dia Nacional do Livro Infantil, Dia dos Povos Indígenas, Meio Ambiente, festa junina, Folclore, Independência, Dia da Árvore, Dia das Crianças, Dia do Professor, Proclamação da República, Consciência Negra e o começo do verão). Com "reduzir movimento" ligado no aparelho (`prefers-reduced-motion`), cada frase aparece parada e troca a cada 7 s (`TEMPO_NOTICIA_PARADA`).
- **Centro:** mascote (×3) sobre a sombra em pixel art (`SOMBRA_MASCOTE`, 64 × 8, ×3) e, embaixo dele, o balão de fala em pixel art. Tocar no mascote: ele pula, toca um som, troca a frase do balão e lê a frase em voz alta (regras na seção 4.3.1). Na primeira vez, o balão diz "Oi! Eu sou seu mascote. Vamos jogar?"; depois, ao abrir a tela, ele cumprimenta pela hora ("Bom dia!", "Boa tarde!" ou "Boa noite!") e diz a frase do momento.
- **Canto inferior esquerdo:** Visual (sprite do guarda-roupa) e, abaixo, Jogos (sprite do controle, botão laranja em destaque).
- **Canto inferior direito:** Configurações (engrenagem) e, abaixo, Progresso (ampulheta).

#### 4.3.0 Cenário: o cantinho de estudo

Atrás do mascote fica um cenário em pixel art (ampliado 3x), com cantos arredondados: papel de parede listrado, janela com nuvem, quadrinho de estrela, estante com livros, cadernos e um globo, planta no vaso, chão de tábuas, um tapete lilás embaixo do mascote e, se couber, um baú e uma caixa de brinquedos. A sombra do mascote vira uma sombra escura e leve no tapete.

- `cenarioEstudo(largura, altura, pes)` desenha o cenário do tamanho do palco, com o chão na altura dos pés do mascote; os objetos ficam centralizados e presos à linha do chão.
- `ajustarCenario(palco)` mede o palco e os pés do mascote e põe o desenho como fundo. Um `ResizeObserver` redesenha quando o palco ou o balão de fala mudam de tamanho, então o mascote fica sempre em cima do tapete, em qualquer celular. Os desenhos ficam em cache.
- Antes de aplicar, foi criado um ponto de restauração em `Pontos de restauração/2026-11-27 — antes do cenário da tela inicial/` (esta pasta não vai para o GitHub).

#### 4.3.1 Frases do mascote

As frases seguem a pesquisa sobre **mentalidade de crescimento** (Carol Dweck). No estudo de Mueller e Dweck (1998), as crianças elogiadas pelo esforço escolheram mais desafios e explicaram as dificuldades pela falta de esforço, enquanto as elogiadas pela inteligência evitaram desafios e passaram a ver a inteligência como algo fixo. Por isso, o mascote:

- elogia o esforço, a atenção e o jeito de pensar, e **nunca** diz "você é inteligente" ou "você tem talento";
- trata o erro como parte de aprender ("Errar faz parte de aprender!", "Ainda não conseguiu? Ainda! Vamos treinar.");
- faz elogios específicos e verdadeiros, ligados ao que o aluno fez, em vez de elogios exagerados.

As frases ficam em `FRASES_MASCOTE`, em cinco grupos (82 frases). Todas saem de um "saco" sorteado sem repetir até usar todas:

| Grupo | Para quê | Exemplos |
| --- | --- | --- |
| `esforco` (22) | Esforço, atenção e erro como aprendizado | "Pensar com calma ajuda a acertar.", "Errou? Agora você sabe mais uma coisa!" |
| `curiosidades` (38) | Fatos simples e conferidos de todas as matérias dos jogos | "Você sabia? A capivara é o maior roedor do mundo.", "Você sabia? Capivara, pipoca e jacaré são palavras que vieram do tupi." |
| `cuidado` (10) | Cuidado com o corpo e segurança na internet | "Que tal beber um pouco de água?", "Senha é segredo: só conte para a sua família." |
| `adivinhas` (8) | Adivinhas do folclore ("O que é, o que é?"), já com a resposta | "O que é, o que é? Cai em pé e corre deitado. É a chuva!" |
| `travaLinguas` (4) | Trava-línguas do folclore, para treinar a fala e a leitura | "Trava-língua: o rato roeu a roupa do rei de Roma!" |

Cuidados com o conteúdo: as curiosidades foram conferidas (nada de "fatos" duvidosos), as adivinhas e os trava-línguas são do folclore (domínio público) e as frases de cuidado falam de segurança na internet sem assustar.

**Frase do momento** (`fraseDoMomento()`): depende do que o aluno fez.

| Situação | Frase |
| --- | --- |
| A última partida de hoje usou menos tentativas que a anterior do mesmo jogo e dificuldade | "Você usou menos tentativas na Balança! Seu treino funcionou." |
| Usou mais tentativas | "A Balança ficou difícil, né? Treinar de novo ajuda!" |
| Ainda não venceu hoje | "Vamos encher o coração hoje?" |
| Faltam vitórias para a meta | "Faltam 2 vitórias para encher o coração!" (ou "Falta só 1 vitória...") |
| Meta cumprida | "Coração cheio! Você pode descansar ou treinar mais." |

**Ordem dos toques** (`proximaFrase()`): a cada 4 toques, o mascote diz a frase do momento. Nos outros toques, as frases dos três grupos saem embaralhadas e sem repetir, até acabarem todas (`sacoFrases`). Todas as frases são curtas e lidas em voz alta.

### 4.4 Jogos (`jogos`)

Inspirada na tela inicial de um console de videogame (como a do Nintendo Switch) e no rascunho da equipe:

- **Topo:** seta com a palavra **Voltar** (volta à tela inicial) e o título "Jogos".
- **Dificuldade e moedas:** a escolha da dificuldade (Fácil, Médio e Difícil, com 1, 2 ou 3 barrinhas em pixel art: `barrasDificuldade()`), que vale para todos os jogos e fica salva em `estado.dificuldade`, e embaixo o aviso de quantas moedas a mais ela vale.
- **Pontinhos:** um por jogo, o aceso é o jogo em destaque.
- **Carrossel:** a capa do jogo em destaque fica grande no meio (cor do jogo, ícone em pixel art ×10, moldura azul) e as capas vizinhas aparecem menores e apagadas nos lados. Botões redondos com setas em pixel art passam para o jogo anterior ou o próximo; arrastar o dedo para os lados e as setas do teclado fazem o mesmo. Ao passar, o nome do jogo é lido em voz alta. O carrossel é circular (depois da Balança vem o Caça-palavras).
- **Título:** nome do jogo, matéria, recorde (troféu + tentativas) e as melhores estrelas, sempre da dificuldade escolhida.
- **Jogar:** tocar na capa do meio ou no botão "Jogar" (com o triângulo de play) abre o jogo. Tocar numa capa do lado traz ela para o meio.
- O jogo em destaque fica guardado em `jogoEscolhido`: ao voltar de uma partida, o carrossel continua no mesmo jogo. Ordem: Caça-palavras, Conexão de palavras, Jogo da memória, Mercadinho, Balança, Mundo digital, Linha do tempo, Caminho no mapa, Maestro, Ateliê de cores e Teatro de histórias.

### 4.5 Tela de jogo (`jogo`)

- Topo: seta com a palavra **Sair**, ícone e nome do jogo, pílula de progresso e, no canto direito, o botão **?** (Como jogar, seção 4.5.1).
- Linha de instrução com o botão do alto-falante (sprite `som-ligado`) para ouvir de novo.
- Área do jogo, mensagem de acerto ou erro e, no rodapé, o contador "Tentativas: N" com a dificuldade (ex.: "Tentativas: 3 ·" barrinhas "Difícil").

#### 4.5.1 Como jogar (botão "?")

**Por quê.** A ideia do app é a criança descobrir sozinha, por curiosidade. Por isso nada abre sozinho, mas o apoio fica sempre à mão, num botão amarelo com "?" em fonte pixel no canto da tela do jogo. Abrir não conta tentativa nem tira estrelas.

- `abrirComoJogar(id)` abre uma janela com uma **demonstração animada** de uma versão pequena do jogo (`COMO_JOGAR[id]`). Uma mãozinha em pixel art (sprite `mao`, 12 × 12) mostra exatamente o que fazer: vai até o lugar, **toca** (a mão afunda e faz o clique), **segura** (uma barrinha verde enche, como no Mundo digital) ou **arrasta** (uma cópia da nota, da moeda ou do cabo vai junto com a mão).
- Cada passo tem uma frase curta, que aparece embaixo e é lida em voz alta, e as bolinhas mostram em que passo a demonstração está. Botões **Ver de novo** e **Entendi!**; tocar fora da janela também fecha.
- Cada passo é `{ texto, alvo, acao: 'tocar' | 'segurar' | 'arrastar', ate, leva, antes(c), depois(c) }`: `alvo` e `ate` são elementos da demonstração, e `antes`/`depois` mudam a cena (letras coloridas, linha ligando grupo e palavra, carta virando, total da mesa, tela do celular, objeto indo para a linha do tempo, mascote andando no mapa, sino acendendo e tocando, tinta misturando, mascote mudando de rosto).
- Os exemplos não usam a partida em andamento, para não entregar a resposta.

| Jogo | O que a demonstração mostra |
| --- | --- |
| Caça-palavras | Arrastar da primeira à última letra (SOL), tocar na primeira e na última (LUA) e a Dica |
| Conexão | Tocar no grupo e depois na palavra (Substantivo → Bola, Verbo → Correr) e o "O que é cada grupo?" |
| Jogo da memória | As cartas abertas para memorizar, virar uma carta e achar o par |
| Mercadinho | Ver o pedido, arrastar a nota de R$ 2 e a moeda de 50 centavos até a mesa, tocar em Pagar e tirar dinheiro da mesa |
| Balança | "8 = 5 + ?" e tocar no 3 |
| Mundo digital | Toque rápido (bloqueia), segurar até o menu, tocar em Desligar e arrastar o cabo até a entrada |
| Linha do tempo | Fogueira, vela e lâmpada, do mais antigo ao mais novo |
| Caminho no mapa | Andar uma casa por toque, desviar da árvore e chegar na escola |
| Maestro | Ouvir os sinos acenderem, tocar na mesma ordem e o Ouvir de novo |
| Ateliê de cores | Ver a cor pedida e misturar vermelho com amarelo |
| Teatro | Ler a cena e tocar no rosto da emoção |
- **Sair** pergunta antes: "Sair do jogo?", com o mascote triste e "Se sair agora, você perde o que já fez nesta partida.". **Continuar jogando** fica em destaque; **Sair** volta para a lista de jogos sem salvar a partida. Tocar fora da janela só fecha (`confirmar({ ..., foraFecha: true })`).

### 4.6 Vitória (`resultado`)

As informações aparecem uma de cada vez:

| Momento | O que aparece |
| --- | --- |
| 0 s | Mascote (×2) pulando, som de vitória e narração "Título. Você usou N tentativas. E ganhou M moedas!" (e "Nova conquista: ...!" para cada conquista nova). A janela de nível novo ("Nível N!") espera essa fala terminar |
| 0,15 s | Título: "Primeira vitória!", "Novo recorde!", "Perfeito!" (3 estrelas) ou "Muito bem!" |
| 0,65–1,15 s | As 3 estrelas, uma por vez |
| 1,5 s | Número grande de tentativas ("tentativas no Médio") e um selo do recorde daquela dificuldade: primeiro recorde, novo recorde ("Antes eram X") ou "Seu recorde: X tentativas" |
| 2,1 s | Moedas ganhas (contando de 0 até o valor), XP ganho e, embaixo, de onde vieram as moedas (`detalheGanhos(d)`, ex.: "15 pelas estrelas · +10 do Difícil · +5 do recorde"). Se a partida completou a meta do dia, aparece também o coração cheio em pixel art com "+20 moedas: meta do dia cumprida!" |
| 2,4 s | Coração do dia e "Hoje: X de 3 vitórias" |
| 2,8 s | Botões **Jogar de novo** e **Voltar ao mascote** |
| 3,3 s | Se subiu de nível: janela "Nível N!" com a estrela grande em pixel art, "Veja as novidades na tela Visual." e o botão "Oba!" |

Confete quando bate recorde ou faz 3 estrelas. Ele cai só dentro do `#app`, então no computador não sai do "celular". A tela de vitória fica sem música de fundo.

### 4.7 Visual (`visual`)

Cabeçalho com seta de voltar e moedas. Mascote (×3) no topo, barra de prova, e duas vitrines de 3 colunas: Cores e Acessórios. Regras na seção 7.

### 4.8 Progresso (`progresso`)

Cabeçalho com seta de voltar e título com a ampulheta. De cima para baixo:

1. **Nível:** mascote (×1), estrela e "Nível N", barra de XP e "Faltam X XP para o nível N+1".
2. **Dois números:** partidas jogadas e estrelas ganhas.
3. **Progresso diário:** coração do dia (×4), nome do nível do dia, barra vermelha e "X de 3 vitórias hoje".
4. **Gráfico:** barras de vitórias por dia nos últimos 7 dias (o último se chama "Hoje"), linha tracejada vermelha na meta, barras verdes nos dias em que a meta foi cumprida e azuis nos outros.
5. **Estatísticas:** botões de dificuldade que **só filtram** (as linhas mostram só as partidas daquela dificuldade). Eles começam na dificuldade em que a criança está jogando, mas trocar ali não muda a dificuldade dos jogos, que é escolhida só na tela Jogos (`difVista` em `telaProgresso`). Também há uma linha por jogo com recorde, número de partidas, melhores estrelas e um selo comparando a última partida com a anterior (seta verde para cima "Melhorou!", sinal de igual "Igual", seta circular "Tente de novo", "Primeira partida" ou "Ainda não jogou"; os desenhos são `seta-cima`, `igual` e `repetir`). Tocar na linha abre barras horizontais das últimas 6 partidas (barra menor = melhor; a última em laranja).

### 4.10 Conquistas (`conquistas`)

**Por quê.** Medalhas ajudam quando valorizam o caminho, e não só o resultado. Por isso, além de "vencer", várias medalhas premiam melhorar o próprio recorde, voltar em dias diferentes e revisar o Caderno, a mesma mentalidade de crescimento das frases do mascote (seção 4.3.1). As medalhas que faltam ficam visíveis, com o quanto falta, para a meta ser clara.

- Botão **Conquistas** (troféu) na tela inicial. A tela mostra "N de 43" com uma barra e uma grade de medalhas em pixel art: as ganhas ficam douradas; as que faltam ficam cinza, com cadeado e uma barrinha de progresso. Tocar numa medalha abre o que é preciso e a data em que foi ganha, e lê o texto em voz alta.
- `CONQUISTAS` (43, com Colecionador, seção 7): primeira vitória, 10 e 50 partidas, jogar todos os jogos, 3 estrelas uma vez e 10 vezes, bater o próprio recorde 3 vezes (`recordesBatidos()`), vencer no Difícil, cumprir a meta do dia uma vez e em 3 dias (`diasComMeta()`), conhecer 10 fases diferentes (Explorador) e todas as fases de um jogo (Viu tudo!), vencer 5, 20 e todas as fases (Aventureiro, Grande aventureiro, Mestre das fases), uma medalha por jogo por vencer todas as fases dele (`CONQUISTAS_DE_FASE`: Explorador da natureza, Dicionário vivo, Memória de matemático, Freguês de todas as lojas, Equilibrista, Técnico de aparelhos, Visitante de museus, Viajante, Compositor, Pintor de quadros, Contador de histórias), abrir 15 e 50 cartões no Caderno (Caderno cheio, Sabe-tudo), fazer 10 revisões (Memória de elefante), nível 5, comprar 1 e 6 itens, fazer 3 revisões e uma medalha por matéria (3 vitórias; 5 em Matemática, que tem três jogos).
- Tudo é calculado a partir do histórico; `estado.conquistas` só guarda quando cada uma foi ganha. `verificarConquistas()` roda ao fim de cada partida, depois de uma compra, depois de uma revisão e ao abrir a tela. Na vitória, as medalhas novas aparecem num quadro amarelo "Nova conquista!" (2,6 s); na compra e na revisão, num aviso.

### 4.11 Caderno (`caderno`) e Revisão (`revisao`)

**Por quê.** Lembrar é o que fortalece a memória: responder perguntas sobre o que se aprendeu fixa mais do que só reler (efeito de testagem), e revisar com intervalos cada vez maiores faz o conhecimento durar (prática espaçada). Para crianças, a revisão é curta, sem castigo e com a resposta explicada na hora.

- Botão **Caderno** (livro) na tela inicial. A lista mostra as 9 matérias (`MATERIAS`) com ícone, "N de M" conhecimentos e o selo vermelho "Revisar!" quando é hora de revisar.
- Cada matéria tem cartões de conhecimento (`CONHECIMENTOS`, 91 no total), de dois tipos:
  - **gerais** (41): o k-ésimo abre depois de k vitórias nos jogos daquela matéria; o fechado diz quantas vitórias faltam;
  - **de fase** (50, campo `fase: 'jogo:fase'`): um ou dois para cada uma das 48 fases, com o que ela ensina (ex.: Corpo humano: "O coração leva o sangue pelo corpo, e os pulmões puxam o ar."; Três pesos iguais: "Triplo é três vezes a mesma quantidade"). Abre quando o aluno **vence** aquela fase (`faseVencida(jogo, fase)`); o fechado diz qual fase vencer ("Vença a fase Grupos de animais no Caça-palavras para abrir.").
- `cartaoAberto(m, c)` e `comoAbrirCartao(m, c)` decidem; os abertos aparecem primeiro. As fases vencidas ficam salvas em `estado.fases[jogo].vencidas` (o histórico guarda as últimas 2000 partidas, `LIMITE_HISTORICO`). Tocar num cartão lê o texto em voz alta.
- **Revisar** (passa pela tela de carregando): até 5 perguntas sobre os cartões já abertos. Os cartões das fases vencidas depois da última revisão entram primeiro, para o aluno revisar logo o que acabou de aprender, e o selo "Revisar!" também aparece quando há um cartão novo assim (`cartaoNovoNaMateria`). Cada pergunta tem cada uma com o ícone e 3 respostas embaralhadas. Acertando ou errando, a resposta certa fica verde e a explicação aparece e é lida ("Isso! ..." ou "Quase! ..."). Não conta tentativa nem dá moedas: é para lembrar sem medo de errar.
- **Revisão espaçada:** `estado.revisoes[matéria] = { vezes, nivel, ultima, proxima }`. Revisão boa (75% ou mais) aumenta o intervalo até a próxima (`INTERVALOS_REVISAO` = 1, 3, 7, 14 e 30 dias); revisão fraca volta para 1 dia. Antes da data, dá para revisar do mesmo jeito.

### 4.12 Carregando (`carregando`)

**Por quê.** Quando uma ação leva perto de 1 segundo ou mais, a pessoa precisa ver que o app está trabalhando; uma barra de progresso deixa a espera mais curta para quem espera, e uma dica aproveita o tempo. Mas uma espera falsa irrita, por isso a tela só aparece quando o app prepara algo de verdade.

- Aparece ao abrir um jogo (pelo carrossel ou em "Jogar de novo", com `abrirJogo(id)`) e ao começar uma revisão. Mostra a capa do jogo, o nome, uma barra de progresso em pixel art, o mascote pulando e a dica do jogo (`JOGOS[id].dica`, ex.: "Para desligar de verdade, segure o botão!").
- **O trabalho de verdade:** desenhar e guardar as imagens em pixel art que ainda não foram desenhadas (`arteURL` de tudo em `ARTE`) e os rostos do mascote, um pouco por vez, para a animação não travar. A barra mostra o menor entre o trabalho feito e o tempo passado, e a tela fica no mínimo `TEMPO_CARREGANDO` (1,2 s), para não "piscar". Na primeira vez leva um pouco mais (cerca de 1,7 s), porque ainda não há nada guardado.

### 4.9 Configurações (janela)

Aberta pelo botão da engrenagem. Título com engrenagem e, nesta ordem:

- Interruptores **Música** (nota musical: `musica`, ou `musica-desligada`, com o X vermelho), **Sons** (alto-falante: `som-ligado`, ou `som`, com o X) e **Narração** (balão: `balao`, ou `balao-desligado`, com o X). O ícone com X aparece quando o interruptor está desligado ou o volume está em 0. Os dois sprites novos seguem o X do `som` da equipe.
- Embaixo de cada interruptor, o **volume** (`opcaoDeSom(tipo, nome, icone)`): botões "-" e "+" em fonte pixel e uma barra de 10 tracinhos que crescem (dá para tocar direto num tracinho). Fica salvo em `estado.volumes = { som, musica, voz }`, de 0 a 10. Nos Sons e na Música, 8 é o volume original (`fatorVolume = nível / 8`, então dá para subir até cerca de +2 dB); na Narração, 10 é o máximo da voz do aparelho (`fala.volume = nível / 10`). Os efeitos e os sinos do Maestro passam por `saidaSons()`, a música multiplica o seu volume pelo fator (também quando abaixa durante a fala), e a narração usa `fala.volume`. Mudar o volume já toca um exemplo: um clique nos Sons, a música na hora e "Assim está bom?" na Narração. Com o interruptor desligado, a linha de volume fica apagada.
- A janela rola se não couber na tela.
- **Vídeo de ajuda** (com a TV em pixel art, `video`): abre a tela de ajuda.
- **Ver a apresentação de novo**.
- **Apagar progresso**: o primeiro toque troca o texto para "Toque de novo para apagar tudo"; o segundo zera o estado, mas mantém `onboarded`, as escolhas de música, sons e narração (`musica`, `som`, `voz` e `volumes`) e a dificuldade.
- **Fechar** com o sprite da porta: a porta abre e fecha antes de a janela sumir.
- Rodapé "Educatata · Experiência Criativa · PUCPR".

## 5. Minijogos

Todos os jogos sempre terminam com o aluno acertando; o que muda é quantas tentativas ele usou. O **mínimo** é o menor número de tentativas possível e define as estrelas (seção 6). Valores e posições são sorteados a cada partida.

| Id | Jogo | Matéria | Ícone | Cor de fundo |
| --- | --- | --- | --- | --- |
| `caca` | Caça-palavras | Ciências | lupa | `#EBFBEE` |
| `conexao` | Conexão de palavras | Português | dois quadrados ligados | `#F3F0FF` |
| `memoria` | Jogo da memória | Matemática | duas cartas | `#FFF0F6` |
| `mercadinho` | Mercadinho | Matemática | carrinho | `#FFE8CC` |
| `balanca` | Balança | Matemática | balança | `#E7F5FF` |
| `aparelhos` | Mundo digital | Tecnologia | celular | `#E3FAFC` |
| `historia` | Linha do tempo | História | pergaminho | `#FFF4E6` |
| `geografia` | Caminho no mapa | Geografia | mapa | `#EBFBEE` |
| `maestro` | Maestro | Música | nota musical | `#F3F0FF` |
| `atelie` | Ateliê de cores | Arte | paleta | `#FFF0F6` |
| `teatro` | Teatro de histórias | Teatro | máscaras | `#FFE3E3` |

### 5.0 Dificuldades

O aluno escolhe a dificuldade na tela Jogos. **Médio** é o jogo como era antes das dificuldades. As regras ficam em `JOGOS[id].dificuldades` e chegam ao jogo por `ctl.config`. O mínimo de tentativas muda junto, então as estrelas continuam justas em cada dificuldade.

| Jogo | Fácil | Médio | Difícil |
| --- | --- | --- | --- |
| Caça-palavras | Grade 8 × 8, 3 palavras, só → e ↓ (mínimo 3) | Grade 8 × 8, 5 palavras, → ↓ ↘ ↗ (mínimo 5) | Grade 10 × 10, 6 palavras escondidas: a lista mostra **pistas** ("Inseto que faz mel.") no lugar das palavras; 8 direções, também de trás para frente (mínimo 6) |
| Conexão de palavras | 2 rodadas (mínimo 6) | 3 rodadas (mínimo 9) | 3 rodadas com **duas palavras por grupo** (6 palavras para 3 grupos; mínimo 18) |
| Jogo da memória | 4 pares em 4 colunas, 8 s para memorizar (mínimo 4) | 6 pares em 3 colunas, 6 s (mínimo 6) | 7 pares em 4 colunas, 6 s; triângulo, quadrado, pentágono e hexágono formam par com o **número de lados** ("5 lados"), e círculo, oval e estrela com o nome (mínimo 7) |
| Mercadinho | Pagar 2, pagar 2, pagar 3 produtos (mínimo 3) | Pagar 2, pagar 3, troco de 2 produtos (mínimo 3) | Pagar 3, **combinar** (achar 2 produtos que custam juntos o valor pedido), troco de 3 produtos (mínimo 3) |
| Linha do tempo | 3 temas com 2 objetos (mínimo 6) | 3 temas com 3 objetos (mínimo 9) | 4 temas com 4 objetos; a 2ª e a 4ª linha são **ao contrário**, do mais novo para o mais antigo (mínimo 16) |
| Caminho no mapa | Mapa 5 × 5, 3 obstáculos, setas | Mapa 6 × 6, 8 obstáculos, setas | Mapa 6 × 6, 9 obstáculos, pontos cardeais e uma **parada no caminho** ("Passe pela praça → casa"; mínimo = menor caminho passando pela parada) |
| Maestro | 3 sinos, sequências de 2, 3 e 3 notas (mínimo 8) | 4 sinos, 3, 4 e 4 notas (mínimo 11) | 4 sinos, sequências de 4, 5, 3 e 4 notas; as duas últimas são tocadas **ao contrário**, do fim para o começo (mínimo 16) |
| Ateliê de cores | 3 tintas primárias; laranja, verde e roxo (mínimo 3) | + branco; + rosa (mínimo 4) | + branco e preto; laranja, rosa, azul-claro, cinza e duas cores de **três tintas**: verde-claro (amarelo + azul + branco) e lilás (vermelho + azul + branco) (mínimo 6) |
| Teatro de histórias | 3 cenas; feliz, triste e bravo (mínimo 3) | 4 cenas; + com medo (mínimo 4) | 4 cenas com **dois personagens** que sentem coisas diferentes (o lobo e os porquinhos); 5 emoções (mínimo 8) |
| Mundo digital | Ligar, bloquear e carregar o celular (mínimo 3) | Desligar o celular, abaixar o som, carregar e desligar o computador (mínimo 8) | Desligar o tablet, deixar o som **no meio** (3 de 5), **carregar e depois ligar** o celular, abrir uma pasta e desligar o computador (mínimo 10) |
| Balança | 5 rodadas com 1 peso conhecido, prato esquerdo de 3 a 9 (mínimo 5) | 3 rodadas com 1 peso (5 a 12) e 2 com 2 pesos (10 a 18) (mínimo 5) | 1 rodada com 2 pesos (12 a 20), 1 com 3 pesos (15 a 24) e 3 rodadas de **dois pesos iguais** que faltam ("10 = ? + ?") (mínimo 5) |

**Como o Difícil fica mais difícil (versão 18).** Pensando em crianças de 7 a 9 anos, o Difícil não usa pressa (nenhum cronômetro novo) nem só "mais do mesmo". Ele pede **um passo de raciocínio a mais** em cada jogo, como nas "dificuldades desejáveis" (Bjork): pensar o que é a palavra antes de procurar, contar lados, comparar somas, inverter uma ordem, planejar o caminho em duas partes, ver a cena pelos dois lados. Cada um desses passos é ensinado na instrução da rodada, e o erro continua explicando o que fazer. O mínimo de tentativas acompanha, então as 3 estrelas continuam possíveis.

O Médio e o Difícil dão moedas e XP a mais (seção 6.1). Embaixo da escolha, a tela Jogos avisa: "No Difícil, cada vitória vale +10 moedas!".

### 5.0.1 Fases (temas de cada jogo)

**Por quê.** Para o aluno jogar muitas vezes sem cansar, cada jogo tem **fases**: temas com conteúdo próprio (plantas, bichos do Brasil, papelaria, fundo do mar...). A dificuldade (seção 5.0) continua mudando *como* se joga; a fase muda *o que* se aprende. O conteúdo de cada fase foi escolhido a partir das habilidades da BNCC do 2º e 3º ano de cada matéria, e conferido com estes cuidados:

- **Verdadeiro e simples:** só fatos conferidos, ditos com palavras do dia a dia; nada que dependa de opinião.
- **Uma resposta certa só:** palavras com uma classe só na Conexão, valores que não se repetem na Memória, ordem sem dúvida na Linha do tempo (ex.: relógio de pêndulo antes do de pulso) e uma pista da emoção no texto do Teatro ("bateu o pé", "tremeu").
- **Cultura brasileira:** bichos do Brasil, palavras vindas do tupi, peteca e bolinhas de gude, fogão a lenha, sítio, folclore (Saci, Curupira, Boitatá) e Monteiro Lobato (domínio público no Brasil desde 2019).
- **Adequado à idade:** nada de violência ou medo forte; as lendas escolhidas falam de proteger a natureza e de brincadeira.

**Como funciona.** `JOGOS[id].fases` é a lista de fases do jogo. A cada partida, `escolherFase(id)` tira a próxima fase de um "saco" embaralhado, sem repetir até o aluno ver todas; depois o saco enche de novo. O saco e as fases já vistas ficam em `estado.fases[id] = { saco, vistas }`. A fase chega ao jogo por `ctl.fase`, aparece embaixo do título ("Fase: Plantas") e fica no histórico da partida (`fase`). Na tela Jogos, cada jogo mostra quantas fases o aluno já conheceu ("3/9 fases"). `ctl.config.dif` traz o nome da dificuldade, para as fases que têm regras próprias por dificuldade.

| Jogo | Fases | O que cada fase traz |
| --- | --- | --- |
| Caça-palavras (`FASES_CACA`) | 9 | Plantas, Grupos de animais, Bichos do Brasil, Corpo humano, Céu e espaço, Água e tempo, Materiais, Cuidar do planeta, Luz e som: 9 a 13 palavras cada (97 no total), todas com pista |
| Conexão (`FASES_CONEXAO`) | 7 | Dia a dia, Na escola, Em casa, Bichos, Natureza, Brincadeiras, Hora de comer: 9 a 12 conjuntos de substantivo, verbo e adjetivo do mesmo tema (66 no total) |
| Jogo da memória (`FASES_MEMORIA`) | 6 | Figuras planas, Sólidos geométricos, Quantidades (quadro de dez), Contas, Números por extenso, Dobro e metade |
| Mercadinho (`FASES_MERCADO`) | 4 | Feira, Padaria e lanche, Papelaria, Loja de brinquedos (29 produtos) |
| Balança (`FASES_BALANCA`) | 3 | Peso que falta, Dois pesos iguais (dobro e metade), Três pesos iguais (triplo e terça parte) |
| Mundo digital (`FASES_DIGITAIS`) | 3 | Celular, Tablet, Computador e celular, cada uma com tarefas para as três dificuldades |
| Linha do tempo (`FASES_HISTORIA`) | 2 | Museu da casa e Museu das invenções, com 5 temas cada (10 no total) |
| Caminho no mapa (`FASES_MAPA`) | 3 | Bairro, Sítio e Cidade, cada um com 4 lugares e obstáculos próprios |
| Maestro (`FASES_MAESTRO`) | 4 | Notas misturadas, Escada que sobe, Escada que desce, Pergunta e resposta |
| Ateliê de cores (`QUADROS`) | 4 | Jardim, Fundo do mar, Fazenda, Praia |
| Teatro (`FASES_TEATRO`) | 3 | Fábulas, Contos de fadas, Folclore do Brasil (30 cenas e 15 cenas de dois personagens) |

Somando, são **48 fases**. Como cada fase ainda sorteia o conteúdo dentro dela (palavras, produtos, mapas, notas), duas partidas da mesma fase também saem diferentes.

### 5.1 Caça-palavras

- Sorteia as palavras da fase (`FASES_CACA`, seção 5.0.1): cada fase é um tema de Ciências com 9 a 13 palavras de no máximo 8 letras (para caber na grade), e cada palavra tem a sua pista (usada no Difícil). `PISTAS_CIENCIAS` e `PALAVRAS_CIENCIAS` juntam todas as fases. Exemplos: Bichos do Brasil (ARARA, TATU, ONÇA, JACARÉ, CAPIVARA, TUCANO, MICO, BOTO, TAMANDUÁ, PREGUIÇA, SUCURI); Corpo humano (OSSO, CORAÇÃO, PULMÃO, PELE, CÉREBRO, DENTE, OLHO, OUVIDO, NARIZ, SANGUE, MÚSCULO, ESTÔMAGO).
- Grade gerada por `gerarGrade(palavras, N, direcoes)`, com o tamanho, a quantidade de palavras e as direções da dificuldade (seção 5.0): palavras sem acento, colocadas da maior para a menor. Só no Difícil elas podem aparecer de trás para frente. A lista de cores tem 6 cores, uma por palavra. Podem se cruzar quando a letra coincide. O resto é preenchido com letras de `ABCDEFGHIJLMNOPRSTUVXZ`.
- Seleção por arrastar o dedo ou por dois toques (primeira e última letra). Selecionar de trás para frente também vale. Seleção fora de linha reta mostra "Escolha letras em linha reta." e não conta tentativa.
- Cada seleção em linha reta conta 1 tentativa. Acerto: as letras ganham uma cor (uma por palavra), a palavra é riscada na lista e lida em voz alta.
- As letras da grade usam a fonte pixel grande (`textoPx`); a imagem da letra não recebe toques (`pointer-events: none`), para o toque chegar na casa.
- Botão **Dica** (com a lâmpada em pixel art): faz piscar a primeira letra de uma palavra que falta e conta 1 tentativa.
- Progresso: "N/total de palavras" (ex.: "2/5").
- **Difícil (pistas):** no lugar da lista de palavras aparecem pistas numeradas de `PISTAS_CIENCIAS` (uma para cada uma das 29 palavras, ex.: "Inseto que faz mel."). Tocar numa pista lê em voz alta. Quando a palavra é achada, ela aparece no fim da pista. A Dica diz qual pista começa na letra que piscou.

### 5.2 Conexão de palavras

- **Antes do jogo:** tela explicando os três grupos, com exemplos e narração, e o botão "Entendi, vamos jogar!". Durante o jogo, o botão "O que é cada grupo?" (com o livro em pixel art) abre a explicação numa janela.
- Explicação: **Substantivo** é o nome das coisas, pessoas, animais e lugares (escola, cachorro, Maria). **Verbo** é uma ação, o que alguém faz (correr, falar, pular). **Adjetivo** diz como uma coisa ou pessoa é (bonito, grande, feliz).
- Cores fixas por grupo em `CORES_GRUPO`: Substantivo `#4C6EF5`, Verbo `#F76707`, Adjetivo `#2F9E44`. A mesma cor aparece na explicação, no texto do grupo e na linha da ligação.
- Sorteia os conjuntos da fase (`FASES_CONEXAO`, seção 5.0.1; `conjuntoConexao()` põe cada conjunto no formato do jogo). Dentro de uma fase, nenhuma palavra se repete, e todas têm uma só classe (nada de "limpo", "canto" ou "larga", que também são verbos). A fase Dia a dia é a lista original abaixo; as outras seguem o mesmo modelo, com palavras do tema (ex.: Brincadeiras: Pipa, Empinar, Colorida; Pião, Girar, Rápido; Peteca, Jogar, Macia):

| Conjunto | Substantivo | Verbo | Adjetivo |
| --- | --- | --- | --- |
| 1 | Escola | Falar | Bonito |
| 2 | Cachorro | Correr | Feliz |
| 3 | Árvore | Pular | Grande |
| 4 | Bola | Cantar | Rápido |
| 5 | Casa | Ler | Azul |
| 6 | Gato | Dormir | Pequeno |
| 7 | Livro | Escrever | Novo |
| 8 | Praia | Nadar | Quente |
| 9 | Mesa | Comer | Redonda |
| 10 | Lápis | Pintar | Colorido |
| 11 | Bicicleta | Pedalar | Vermelha |
| 12 | Jardim | Plantar | Cheiroso |

- **Difícil:** cada grupo tem duas palavras na rodada (6 palavras, de 2 conjuntos), e o grupo só fecha quando as duas estão ligadas. As palavras ficam em duas colunas (`.conexao.dois`). O mínimo é `rodadas × 3 × porGrupo`.
- Grupos à esquerda e palavras à direita, as duas colunas embaralhadas. O aluno toca no grupo (que é lido em voz alta) e depois na palavra. Cada tentativa de ligação conta 1. Acerto desenha uma linha em SVG entre os dois e diz "Escola é substantivo!". As linhas são redesenhadas quando a janela muda de tamanho.

### 5.3 Jogo da memória

- **Fases** (`FASES_MEMORIA`): cada fase gera os pares da partida (`gerar(n, config)` devolve `{ id, a, b }`, com cartas `{ html, fala }`), e os valores nunca se repetem numa partida:
  - **Figuras planas:** a carta do desenho e a do nome, entre as 10 de `FIGURAS` (Triângulo, Quadrado, Círculo, Retângulo, Losango, Pentágono, Hexágono, Trapézio, Oval e Estrela). Cada figura é desenhada em pixel art (20 × 20, ×3) por `desenhoFigura(f)`. No Difícil, o par é o número de lados.
  - **Sólidos geométricos** (`SOLIDOS`): cubo, bloco retangular, esfera, cilindro, cone e pirâmide, desenhados com um tom por face; no Difícil entra o círculo, para comparar o plano com o sólido.
  - **Quantidades:** bolinhas num quadro de dez (`desenhoPontinhos(n)`, como o material da escola) e o número. Fácil e Médio de 1 a 10; Difícil de 11 a 20, com dois quadros.
  - **Contas:** a conta e o resultado. Fácil: somas até 10; Médio: somas de 11 a 20; Difícil: contas de menos.
  - **Números por extenso:** o número e o nome escrito (`porExtenso(n)`). Fácil de 1 a 10, Médio de 11 a 20, Difícil de 21 a 99.
  - **Dobro e metade:** "Dobro de 3" e 6. No Difícil entram metade e triplo.
- O verso da carta tem um "?" em fonte pixel. A fonte pixel grande ganhou os sinais "-" e "×".
- **Fase de memorizar:** as cartas viram uma a uma (70 ms entre elas), ficam abertas pelo tempo da dificuldade (8 s no Fácil, 6 s no Médio e no Difícil) com uma barra de tempo, e viram de volta uma a uma. Toques nessa fase não contam. Depois aparece "Agora encontre os pares!".
- Cada carta virada tem o nome da figura lido em voz alta. Cada par aberto conta 1 tentativa. Par certo fica verde; par errado desvira depois de 1,1 s.
- Animação de virar: `rotateY` 3D de 0,6 s com `cubic-bezier(.45, .05, .25, 1)`.
- Progresso: "N/total de pares" (ex.: "2/6 pares").
- **Difícil (lados):** `FIGURAS` tem o número de lados de cada figura. Triângulo, quadrado, pentágono e hexágono formam par com uma carta "N lados" (número em fonte pixel), e a criança precisa contar os lados do desenho; círculo, oval e estrela formam par com o nome. `nomeCarta(c)` dá o texto lido em voz alta.

### 5.4 Mercadinho

- Prateleira com 6 produtos (`PRODUTOS_NA_PRATELEIRA`) sorteados a cada partida entre os da loja da fase (`FASES_MERCADO`; cada produto tem `loja`): **Feira** (Maçã R$ 1,00, Banana R$ 1,50, Uva R$ 3,00, Cenoura R$ 1,50, Melancia R$ 5,00, Laranja R$ 1,00, Milho R$ 1,50), **Padaria e lanche** (Pão R$ 2,50, Suco R$ 3,00, Leite R$ 4,00, Biscoito R$ 2,00, Queijo R$ 3,50, Sorvete R$ 3,50, Ovo R$ 0,50, Pipoca R$ 2,00), **Papelaria** (Lápis R$ 1,00, Borracha R$ 0,50, Caderno R$ 5,00, Régua R$ 2,00, Cola R$ 3,00, Tesoura R$ 4,00, Giz de cera R$ 3,50) e **Loja de brinquedos** (Bola R$ 5,00, Pipa R$ 2,50, Pião R$ 3,00, Ioiô R$ 2,00, Carrinho R$ 4,00, Peteca R$ 1,50, Bolinhas de gude R$ 0,50). Os preços são múltiplos de 50 centavos. Cada produto aparece como desenho em pixel art (16 × 16, ×3), com o nome pequeno e o preço na fonte pixel. Os pedidos usam só os produtos da prateleira, e os do pedido ficam destacados com o "certo" verde em pixel art.
- O pedido é mostrado com desenhos, quase sem palavras: "Compre" + os produtos; no troco, os produtos = total e "Pagou" + a nota, "Troco?".
- Rodadas de `gerarRodadasMercado()`:

| Tipo de rodada | Pedido | Botão |
| --- | --- | --- |
| Pagar | 2 ou 3 produtos da prateleira; o aluno põe na mesa o valor da compra | Pagar |
| Troco | 2 ou 3 produtos; o cliente paga com R$ 10,00 (ou R$ 20,00 se o total for R$ 10,00 ou mais, o que só acontece com 3 produtos, no Difícil); mostra o total | Dar troco |

| Combinar (Difícil) | Um valor (ex.: "R$ 5,50"); a criança toca em 2 produtos da prateleira que custam juntos esse valor (`renderCombinar`) | Comprar |

A sequência de rodadas de cada dificuldade está na seção 5.0 e é montada por `gerarRodadasMercado(prateleira, plano)`.

- Dinheiro de `DINHEIRO`, em centavos: notas R$ 20, R$ 10, R$ 5, R$ 2; moedas R$ 1, 50¢, 25¢, 10¢. Notas (26 × 13) e moedas (13 × 13) são desenhadas em pixel art por `desenhoDinheiro(d)`, com cores parecidas com as de verdade (`CORES_DINHEIRO`) e o valor na fonte pequena. A mesa mostra o total na fonte pixel.
- **Arrastar** (não tocar): o aluno segura uma nota ou moeda e arrasta até a mesa. Uma cópia segue o dedo; a mesa fica verde quando o dedo está em cima. Soltou fora, a cópia volta ao lugar. Um toque sem arrastar mostra "Segure e arraste o dinheiro.".
- Para tirar da mesa, arrasta-se o dinheiro para fora dela. Botão **Limpar mesa** zera. A mesa aceita no máximo 20 itens e mostra o total.
- Cada toque em Pagar/Dar troco conta 1 tentativa. Respostas: "Valor certo!"/"Troco certo!", "Tem dinheiro demais na mesa." ou "Ainda falta dinheiro na mesa.".
- A instrução do Pagar lista os produtos ("Arraste até a mesa o valor da compra: sorvete e biscoito."). A narração diz um produto por frase ("Arraste até a mesa o valor da compra. Sorvete. E biscoito."), porque cada frase tem o seu áudio gravado (`ctl.instrucao(texto, falarAgora, fala)`). No Combinar, o acerto mostra os produtos e a narração diz "Isso! Juntos, custam R$ 5,50!".
- Todos os valores são inteiros em centavos, para evitar erro de arredondamento.

### 5.5 Balança

- 5 rodadas de `gerarBalancas(plano)`, com os pesos da dificuldade (seção 5.0) ou da fase (`FASES_BALANCA`): **Peso que falta** usa as rodadas da dificuldade; **Dois pesos iguais** (`'dobro'`) e **Três pesos iguais** (`'triplo'`) têm rodadas próprias para cada dificuldade, em que menor e maior são o valor de cada peso que falta (ex.: "8 = ? + ?" é a metade de 8; "12 = ? + ? + ?" é a terça parte). Cada peso conhecido vai de 1 a 9, e sempre falta um peso de pelo menos 1. Com 4 blocos no prato direito, os blocos ficam menores e o prato fica mais largo.
- Três opções: a resposta e dois valores próximos (±3), todos positivos e diferentes.
- **Difícil (dois pesos iguais):** nas rodadas `'dobro'` faltam dois blocos iguais (`repete: 2`, cada um de 2 a 6). A equação fica "10 = 4 + ? + ?", a criança escolhe um valor e ele entra duas vezes no prato. As opções ficam a ±2 da resposta, e o acerto diz "Equilibrou! Cada peso vale 3.".
- Desenho em pixel art dentro de um SVG (`<image>` ampliado ×3, com `image-rendering: pixelated`): suporte, viga, pino, pratos com cordas e blocos de peso com o número na fonte pequena. O peso que falta é amarelo com "?"; o escolhido entra no prato e fica verde quando equilibra. A viga gira `(direita − esquerda) × 3` graus, limitado a ±16°, e os pratos sobem e descem junto, com transição elástica de 0,7 s.
- Mostra a equação em fonte pixel (ex.: "12 = 5 + ?") e as três opções também em fonte pixel. Cada escolha conta 1 tentativa. Acerto deixa o peso verde e diz "Equilibrou!"; erro diz "Ficou pesado demais!" ou "Ainda está leve!" e a balança volta depois de 1,1 s.

### 5.6 Mundo digital (tecnologia)

**Por que existe.** Na entrevista, a professora contou que, quando pediu para os alunos desligarem o tablet, alguns só apertaram o botão e bloquearam a tela: eles não sabiam a diferença entre bloquear e desligar. As pesquisas indicam o mesmo padrão: aos 5 a 7 anos a maioria já toca, arrasta e abre apps sozinha, mas tarefas de várias etapas (desligar, cuidar da bateria, usar o computador) ainda dependem de um adulto que ensine. A BNCC Computação (complemento de 2022) traz esses conhecimentos nos eixos Mundo Digital e Cultura Digital desde os anos iniciais.

**Como funciona.** Cada rodada mostra um aparelho em pixel art (celular, tablet ou computador, desenhados com `grade()` em `APARELHOS`) com botões de verdade nos lugares certos, e uma tarefa com ícone e uma palavra ("Desligar", "Som", "Carregar"...). A instrução é curta e lida em voz alta. A criança faz a tarefa como faria no aparelho real:

- **Apertar** (toque rápido) e **segurar** (1,2 s, `SEGURAR_MS`) o botão de ligar são ações diferentes, como num celular. Enquanto a criança segura, uma barra verde enche embaixo do aparelho.
- O cabo do carregador é **arrastado** até a entrada embaixo do celular.
- No computador, a pasta abre com **dois toques rápidos** (até 0,5 s), como o clique duplo do mouse.

Cada ação nos botões conta 1 tentativa. Um erro nunca trava o jogo: o aparelho reage como o de verdade e o app explica o que aquele botão faz (ex.: "Isso só bloqueou a tela! O celular continua ligado. Para desligar, segure o botão."). O botão **Dica** (lâmpada) faz piscar o botão certo e fala como usar, e vale 1 tentativa.

**Tarefas (`TAREFAS_DIGITAIS`):**

| Tarefa | Aparelho | O que fazer | Mínimo | O que o erro ensina |
| --- | --- | --- | --- | --- |
| `ligar` | Celular desligado | Segurar o botão do lado | 1 | Toque rápido não liga; os botões de som não ligam |
| `bloquear` | Celular ligado | Apertar rápido o botão | 1 | Segurar abre o menu de desligar |
| `desligar` | Celular ligado | Segurar o botão e tocar em "Desligar" | 2 | **Apertar rápido só bloqueia a tela** (o caso da professora); "Reiniciar" desliga e liga de novo |
| `desligar-tablet` | Tablet ligado | O mesmo, mas o botão fica em cima | 2 | O botão muda de lugar de um aparelho para outro |
| `volume-baixar` | Celular | Botão de baixo até o som acabar (3 toques) | 3 | Qual botão aumenta e qual diminui |
| `volume-aumentar` | Tablet | Botão de cima até o máximo (4 toques) | 4 | O mesmo, do outro lado do aparelho |
| `carregar` | Celular sem bateria | Arrastar o cabo até a entrada | 1 | Sem bateria os botões não funcionam |
| `desligar-computador` | Computador | Tocar no menu e em "Desligar" | 2 | **O botão do monitor só apaga a tela**; o computador continua ligado |
| `abrir-pasta` | Computador | Dois toques rápidos na pasta | 2 | Um toque só escolhe a pasta |
| `ligar-tablet` | Tablet desligado | Segurar o botão de cima | 1 | No tablet, o botão fica em outro lugar |
| `bloquear-tablet` | Tablet ligado | Apertar rápido o botão de cima | 1 | Segurar abre o menu de desligar |
| `reiniciar` | Celular travado | Segurar o botão e tocar em "Reiniciar" | 2 | Reiniciar desliga e liga sozinho: ajuda quando o aparelho trava |
| `reiniciar-tablet` | Tablet travado | O mesmo, no tablet | 2 | O mesmo, com o botão em cima |

**Fases** (`FASES_DIGITAIS`): Celular, Tablet e Computador e celular. Cada fase tem a sua lista de tarefas para cada dificuldade (ex.: Tablet no Médio: desligar, aumentar o som, bloquear e ligar). A fase Computador e celular usa as listas originais de `dificuldades`.
| `volume-meio` (Difícil) | Tablet com o som no máximo | Baixar até 3 dos 5 risquinhos | 2 | Controlar o som, não só ligar e desligar |
| `carregar-e-ligar` (Difícil) | Celular sem bateria | Arrastar o cabo e depois segurar o botão | 2 | A ordem certa: primeiro a bateria, depois o botão |

As telas do aparelho (`desenharTela()`) também são pixel art: desligada (preta), logo (estrela), bloqueada (cadeado e a hora certa em fonte pixel), início (os ícones dos jogos do Educatata como apps), menu de energia, barra de volume, bateria fraca e carregando, área de trabalho do computador com pasta e menu, e pasta aberta.

### 5.7 Por que estes cinco jogos (história, geografia e artes)

A equipe pediu um jogo para cada área de conhecimento, inspirado nas escolas que apostam no lado criativo do aluno. O estudo encontrou três ideias que viraram regras para os jogos:

- **Artes no centro, não como extra.** Escolas Waldorf tratam artes e música como parte do currículo, e não como atividade a mais; cada turma monta uma peça de teatro com o tema do ano. Os primeiros anos giram em torno de contos de fadas, fábulas e lendas. Escolas inspiradas em Reggio Emilia têm um ateliê de artes no centro da escola.
- **Efeito comprovado.** Um estudo com sorteio de escolas em Houston (EUA, 2019), com 10.548 alunos do 3º ao 8º ano, encontrou nos alunos que tiveram mais aulas de artes melhora na escrita, menos problemas de disciplina e mais empatia. Não houve efeito em matemática nem em leitura, então as artes entram aqui pelo que fazem bem: criatividade, expressão e empatia.
- **BNCC.** História trabalha passado e presente e as mudanças nos objetos do dia a dia; Geografia, a localização e os referenciais de lugar; Arte, as linguagens das artes visuais, da música e do teatro.

| Jogo | Área | Ideia vinda do estudo |
| --- | --- | --- |
| Linha do tempo | História | Antes e depois com objetos do cotidiano, terminando com um fato do Brasil (Santos Dumont, rádio, Serra da Capivara) |
| Caminho no mapa | Geografia | Ler um mapa, desviar de obstáculos e, no Difícil, usar os pontos cardeais com a rosa dos ventos |
| Maestro | Música | Percepção de altura e memória musical com sinos coloridos (dó, ré, mi, sol), como nas aulas com instrumentos de cores |
| Ateliê de cores | Arte | Cores primárias e secundárias, misturando tintas para pintar um quadro de colorir |
| Teatro de histórias | Teatro e literatura | Contos e fábulas clássicos (domínio público) e empatia: escolher a emoção do personagem e ver o mascote interpretar |

### 5.8 Linha do tempo (história)

- Cada rodada é um tema de `LINHAS_DO_TEMPO` do museu da fase (`FASES_HISTORIA`), com 4 objetos do mais antigo ao mais novo, desenhados em pixel art (16 × 16). **Museu da casa:** Luz, Ouvir música, Cozinhar (fogueira, fogão a lenha, fogão a gás, micro-ondas), Ver as horas (relógio de sol, de pêndulo, de pulso, digital) e Fazer contas (pedrinhas, ábaco, calculadora, celular). **Museu das invenções:** Falar de longe, Viajar, Registrar ideias, Brincar (pião, bicicleta, videogame, tablet) e Guardar retratos (pintura, câmera antiga, câmera, celular). A ordem de cada tema foi escolhida para não ter dúvida.
- A criança toca nos objetos do mais antigo para o mais novo, e cada um vai para a próxima vaga da linha ("Antes" → "Hoje"). Tocar num mais novo do que outro que falta conta tentativa e mostra "Esse veio depois! Procure o mais antigo.".
- Com 2 objetos, ficam o primeiro e o último (a maior diferença); com 3, o primeiro, um do meio e o último.
- **Difícil (ao contrário):** na 2ª e na 4ª linha (`inverter`), "Hoje" fica à esquerda e "Antes" à direita, aparece o aviso "ao contrário!" e a criança toca do mais novo para o mais antigo.
- Ao completar a linha, o app lê um fato curto (ex.: "O brasileiro Santos Dumont voou no avião 14-bis em 1906."). Na última rodada, a vitória espera o fato terminar.

### 5.9 Caminho no mapa (geografia)

- `gerarMapa(N, obstaculos, comParada, fase)` sorteia um mapa de N × N casas com o mascote, o destino e obstáculos, conforme a fase (`FASES_MAPA`): **Bairro** (escola, praça, mercado e casa; lago e árvore), **Sítio** (celeiro, horta, galinheiro e poço; vaca, lago e cerca) e **Cidade** (hospital, biblioteca, quartel dos bombeiros e padaria; prédio, carro estacionado e árvore, num chão de calçada). Cada lugar tem um fato curto sobre o que se faz lá, lido quando o mascote chega (ex.: "Os bombeiros apagam incêndios e salvam pessoas. O telefone deles é 193."). Uma busca em largura garante que existe caminho e calcula o menor número de passos, que é o mínimo da rodada.
- Os botões de seta movem o mascote uma casa. Cada passo conta 1 tentativa; bater num obstáculo ou na borda conta, não anda e explica ("Tem um lago aí! Desvie.").
- **Difícil:** os botões viram N, S, L e O, a rosa dos ventos aparece ao lado do botão N (fora do mapa, para não cobrir nenhuma casa) e a instrução lembra que o Norte fica para cima. A cada passo, o nome do ponto cardeal aparece na mensagem.
- **Difícil (parada):** `gerarMapa(N, obstaculos, true)` sorteia também um lugar onde o mascote precisa passar antes do destino. O título mostra "Passe pela praça → casa". O destino só vale depois da parada, e o mínimo é o menor caminho até a parada mais o menor caminho dela até o destino (busca em largura `menorCaminho`).

### 5.10 Maestro (música)

- Sinos em pixel art com as cores usadas em aulas de música (dó vermelho, ré laranja, mi amarelo, sol azul-esverdeado) e o nome da nota embaixo. Só notas da escala pentatônica, como o motivo do mascote. Os sinos ficam em ordem do grave para o agudo.
- **Fases** (`FASES_MAESTRO`): Notas misturadas (sorteadas), Escada que sobe (do grave para o agudo), Escada que desce (do agudo para o grave) e Pergunta e resposta (a segunda metade repete a primeira, como um eco). A instrução de cada fase explica a ideia, e o acerto repete o que aconteceu ("Bravo! As notas subiram: do grave para o agudo!"). Trabalha a altura do som, um dos elementos da música na BNCC de Arte.
- O maestro (o mascote) toca uma sequência: cada sino acende e toca a nota na kalimba (`tocarSino`, que usa `tocarNota` e `INSTRUMENTOS.kalimba` do motor de música). Depois aparece "Sua vez!" e a criança repete. Cada toque conta 1 tentativa; uma nota errada mostra "Essa nota era outra. Ouça de novo!" e o maestro toca de novo.
- "Ouvir de novo" repete a sequência e conta 1 tentativa (como a dica dos outros jogos).
- **Difícil (ao contrário):** as duas últimas sequências (`contrario`) são ouvidas normalmente e tocadas do fim para o começo. Aparece "Sua vez: do fim para o começo!". Treina a memória de trabalho, como repetir números ao contrário.
- Este jogo tem `semMusica: true`: fica sem música de fundo para a criança ouvir as notas. Se os Sons estiverem desligados, um aviso pede para ligar; os sinos continuam acendendo.

### 5.11 Ateliê de cores (artes visuais)

- Um quadro de colorir em pixel art (52 × 36, ×5), um por fase (`QUADROS`): **Jardim** (céu, sol, nuvem, grama, flor e borboleta), **Fundo do mar** (água, peixe, algas, polvo, estrela-do-mar e pedras), **Fazenda** (céu, nuvem, grama, abóboras, porquinho e flores, com o celeiro e a cerca já pintados) e **Praia** (céu, sol, folhas do coqueiro, guarda-sol, estrela-do-mar e pedras, com o mar e a areia já pintados). Cada quadro tem uma parte para cada cor de mistura e partes fixas (`fixas`). Cada pixel sabe de que parte é (`mapaDoQuadro(q)`) e a borda entre duas partes vira contorno, como num livro de colorir. A parte da vez fica quadriculada.
- A criança toca em duas tintas; a mistura aparece (`misturarTintas`, tabela `MISTURAS`). Se for a cor pedida, a parte é pintada e o app fala a mistura ("Amarelo e azul fazem verde!"; as tintas são ditas sempre na mesma ordem, vermelho, amarelo, azul, branco e preto, não na ordem dos toques); se não for, explica o que saiu ("Vermelho e azul fazem roxo. Para rosa, tente outra mistura."). Cada mistura conta 1 tentativa.
- **Difícil (três tintas):** verde-claro e lilás pedem três tintas (`MISTURAS_3`, `TINTAS_DA_COR`), e o ateliê mostra tantas vagas de mistura quantas a cor pede. A criança precisa pensar em duas mudanças ao mesmo tempo: a cor (verde = amarelo + azul) e o claro (+ branco).
- No fim, com o quadro pronto, o app lê: "Vermelho, amarelo e azul são as cores primárias. Misturando, a gente faz as outras!".

### 5.12 Teatro de histórias (literatura e teatro)

- Cenas em domínio público (`CENAS_TEATRO`, 30), em três fases (`FASES_TEATRO`, campo `livro`): **Fábulas** (A Lebre e a Tartaruga, O Leão e o Rato, A Cigarra e a Formiga, A Raposa e as Uvas, O Corvo e a Raposa, A Formiga e a Pomba, A Galinha Ruiva), **Contos de fadas** (Os Três Porquinhos, O Patinho Feio, Chapeuzinho Vermelho, João e Maria, Cachinhos Dourados, João e o Pé de Feijão, Cinderela, Os Músicos de Bremen, O Anão Saltador) e **Folclore do Brasil** (O Saci, O Curupira, O Boitatá e Reinações de Narizinho, de Monteiro Lobato). Cada fase tem cenas das cinco emoções e pelo menos quatro cenas de dois personagens (`CENAS_DUPLAS`, 15).
- Cada cena tem a pergunta com o nome do personagem (`pergunta`, ex.: "Como a raposa se sentiu?") e uma pista da emoção no texto ("bateu o pé no chão", "tremia"), para a resposta não depender de adivinhar. O erro diz "Hum... leia de novo o que aconteceu na cena. Tente outra!".
- Cada rodada mostra o nome da história, a fala da cena e um palco em pixel art (cortinas vermelhas e chão de madeira) com o mascote. A criança escolhe a emoção do personagem entre rostos do mascote; acertando, o mascote muda o rosto e pula, a narração diz "Isso! Feliz!" e, depois (na fila, sem cortar), o mascote "interpreta" a cena com o áudio gravado daquela emoção (`falar(texto, { emocao, fila: true })`). No Difícil, o segundo personagem só ganha o "Isso!".
- **Difícil (dois personagens):** `CENAS_DUPLAS` tem 6 cenas em que dois personagens sentem coisas diferentes (ex.: na casa de tijolos, o lobo fica bravo e os porquinhos ficam felizes). Uma linha pergunta por um personagem de cada vez ("Como o lobo se sentiu?"). Trabalha a empatia: a mesma situação vista pelos dois lados.
- Uma cena de cada emoção aparece primeiro, para todas serem trabalhadas. Errar mostra "Hum... pense em como ele se sentiu. Tente outra!".
- Rostos novos do mascote (`desenharMascotePixel`): `triste` (sobrancelhas caídas, pálpebra caída, boca para baixo, lágrima), `bravo` (sobrancelhas em V, dentes trincados, bochechas vermelhas), `assustado` (sobrancelhas no alto, pupila pequena, boca aberta, gotas de suor) e `surpreso` (sobrancelhas no alto, pupila pequena, boca em "O"). Os traços têm 2 pixels para dar para ler o rosto no tamanho pequeno dos botões.

## 6. Progressão

Tudo é calculado em `terminarJogo(id, tentativas, minimo)`, chamada quando uma partida termina.

### 6.1 Estrelas, moedas e XP

| Regra | Fórmula |
| --- | --- |
| 3 estrelas | `tentativas <= minimo` |
| 2 estrelas | `tentativas <= minimo + ceil(minimo / 2)` |
| 1 estrela | acima disso |
| Moedas | estrelas (1 estrela = 5, 2 = 10, 3 = 15) + dificuldade (Fácil 0, Médio +5, Difícil +10) + 5 se bater o recorde + 20 na vitória que completa a meta do dia (uma vez por dia) |
| XP | `10 + estrelas × 5` + dificuldade (Fácil 0, Médio +5, Difícil +10): 15 a 35 por partida |
| XP para subir do nível N | `30 + (N − 1) × 15` (nível 1 → 30, nível 2 → 45, nível 3 → 60…) |
| Subir de nível | `while (xp >= xpNecessario(nivel))`: desconta o XP e soma 1 nível; o XP que sobra continua |

**Ritmo da economia.** A loja inteira custa 2.140 moedas (cores 1.010 e acessórios 1.130). Jogando 3 partidas por dia com 3 estrelas, o aluno ganha cerca de 65 moedas por dia no Fácil, 80 no Médio e 95 no Difícil, contando o bônus da meta. Assim, comprar tudo leva de 3 a 5 semanas, e o primeiro item (Óculos, 60) sai no primeiro dia. Antes da versão 9, a loja custava 605 moedas e acabava em menos de uma semana. Para mudar o ritmo, ajuste `ECONOMIA` e os preços em `CORES` e `ITENS`.

### 6.2 Recordes e histórico

- Recorde = menor número de tentativas em cada jogo **e dificuldade**, guardado em `estado.recordes[chaveRecorde(id, dificuldade)]`: a chave é só `id` no Médio (a mesma de antes, então os recordes antigos continuam valendo) e `id:facil` ou `id:dificil` nas outras. A primeira partida de um jogo em cada dificuldade sempre vira recorde.
- Cada partida terminada vai para `estado.historico` como `{ jogo, dificuldade, tentativas, estrelas, data }` (`data` em milissegundos). Partidas antigas, sem `dificuldade`, contam como Médio (`difDe(partida)`). O histórico guarda as últimas 2000 partidas (`LIMITE_HISTORICO`; eram 200, mas aí o Progresso parava de contar e cartões do Caderno fechavam de novo depois da 201ª vitória).
- Sair de um jogo (com a confirmação) não grava nada.

### 6.3 Sistema de vida (coração do dia)

Funciona como uma nota diária: o coração enche conforme o aluno vence partidas no dia e volta a ficar vazio no dia seguinte. As vitórias do dia são contadas a partir do histórico, pela data local (`diaDe(ts)` → `AAAA-MM-DD`). Hoje, toda partida terminada conta como vitória.

| Vitórias hoje (meta = 3) | Fração da meta | Sprite | Nome | Frase |
| --- | --- | --- | --- | --- |
| 0 | 0 | `coracao-vazio` | Ainda não jogou hoje | Jogue para encher o coração. |
| 1 | > 0 | `coracao-quarto` | Abaixo da média | Bom começo! Continue jogando. |
| 2 | ≥ 1/2 | `coracao-metade` | Média | Na média. Falta pouco! |
| 3 ou mais | ≥ 1 | `coracao-cheio` | Alto | Coração cheio! Meta do dia cumprida. |

A meta fica em `META_DIARIA` (3). Mudá-la ajusta sozinho o coração, a barra diária e a linha do gráfico. O coração aparece no topo da tela inicial, na tela de vitória e no Progresso.

## 7. Personalização

O aluno gasta moedas na tela Visual para mudar a cor do mascote e colocar acessórios. Cada acessório ocupa um lugar (*slot*) do corpo; dois itens do mesmo lugar não podem ser usados ao mesmo tempo.

### Cores (`CORES`)

| Id | Nome | Cor | Preço | Nível mínimo |
| --- | --- | --- | --- | --- |
| `laranja` | Laranja | `#FFB547` | grátis (já vem) | 1 |
| `azul` | Azul | `#74C0FC` | 80 | 1 |
| `verde` | Verde | `#8CE99A` | 120 | 2 |
| `rosa` | Rosa | `#FFA8D2` | 160 | 3 |
| `roxo` | Roxo | `#B197FC` | 250 | 5 |
| `dourado` | Dourado | `#FFD43B` | 400 | 8 |

### Acessórios (`ITENS`)

| Id | Nome | Lugar | Preço | Nível mínimo |
| --- | --- | --- | --- | --- |
| `chapeu` | Chapéu | `cabeca` | 100 | 1 |
| `oculos` | Óculos | `rosto` | 60 | 1 |
| `cachecol` | Cachecol | `pescoco` | 120 | 1 |
| `laco` | Laço | `cabeca` | 100 | 2 |
| `coroa` | Coroa | `cabeca` | 300 | 5 |
| `capa` | Capa de herói | `costas` | 450 | 8 |

Nas vitrines, cada acessório tem um ícone em pixel art (`item-chapeu`, `item-oculos`, `item-cachecol`, `item-laco`, `item-coroa`, `item-capa`); no mascote eles são desenhados em pixel art.

### Coleções especiais (`COLECOES`)

**Por quê.** Easter eggs da equipe: visuais inspirados em pessoas que fizeram parte da criação do Educatata (a equipe, amigos, família, professores e a turma de Ciência da Computação da PUCPR, 2026/2). O **título** de cada coleção representa a pessoa sem dizer o nome (ex.: "Punk que quer ser árvore", "O Pavão das curiosidades"); o nome só fica quando não há outra forma clara de representar, como nos itens da Turma do Código.

**Cuidados.**
- Tudo foi desenhado do zero em pixel art; personagens e marcas de outros aparecem só como inspiração, com nomes genéricos (ex.: "Boina de pudim", "Capuz de orelhinhas", "Carrão preto de estrada", "Caixa de rosquinhas").
- Temas adequados para crianças: das séries favoritas entram só elementos sem violência (o carrão, as rosquinhas). Nada tem nome religioso: as referências são discretas, para quem conhece entender (turbante branco, colar de contas).
- Os textos das coleções são curtos e carinhosos e não mostram informação pessoal.

| Coleção | Cores | Acessórios |
| --- | --- | --- |
| O artista que programa | Roxo paranormal | Chapéu de dançarino, Óculos de persiana, Cabelo espetado, Luva brilhante, Lápis de ilustrador |
| Pudim, palco e cobertinha | Pudim | Nana, a cobertinha, Nana na cabeça, Nana enroladinha, Turbante branco, Colar de contas, Pudinzinho, Boina de pudim, Máscaras de teatro, Bastão de luz de show, Estetoscópio |
| Punk que quer ser árvore | Verde floresta, Preto punk | Copa de árvore, Moicano verde, Óculos redondos de rock, Jaqueta de spikes, Contrabaixo, Colar da Nana, Pensamento da Hachi, Chapéu de cogumelo, Bandeira quadriculada |
| Doçura que fala com as mãos | Rosa meigo | Capuz de orelhinhas, Laço de gatinha, Mãozinha de Libras, Colar de coração |
| Três gatos e uma maratona | Noite de série | Três gatinhos, Orelhinhas de gato, Touca de pijama, Óculos 3D, Milk-shake, Caixa de rosquinhas, Carrão preto de estrada |
| Malas, perfumes e novelas | Coral de novela | Frasco de perfume, Mala de viagem, Revista de caça-palavras, Colar de ouro |
| Headset e cara de bloquinho | — | Headset, Cara de bloquinho |
| Futuro professor nota 10 | Verde lousa | Capelo de formatura, Medalha nota 10, Lousinha com um H |
| Mestre do RPG | — | Dado de 20 lados, Chapéu de mago, Capa de aventureiro, Flor cheirosa |
| Estilo e código | Bege da moda | Cabelo curtinho ruivo, Óculos de grife, Camiseta de código, Pilha de livros |
| Estilo cor-de-rosa | Rosa-chiclete | Cabelo descolorido, Óculos de coração, Bolsinha rosa |
| Traço de mangá | Azul aquarela | Laço listrado, Boina de mangaká, Prancheta de desenho |
| Rock em Amsterdã | — | Gravata de advogado, Violão nas costas, Tulipa de Amsterdã, Óculos de estudante |
| Criadora de jogos | Azul caverna | Capa de cavaleirinha, Crachá de desenvolvedora, Caderno de ideias de jogos |
| O Pavão das curiosidades | Azul pavão | Cauda de pavão, Penacho de pavão, Faixa preta, Lâmpada da curiosidade |
| Filosofia apaixonante | — | Coroa de louros, Toga de filósofo, Barba de filósofo, Pergaminho de ideias |
| Mestre dos circuitos | Verde de placa | Mochila de circuitos, Antena de robô, Óculos de proteção, Microchip |
| Calma e algoritmos | Azul sereno | Xícara de chá, Chave de criptografia, Escudo de segurança |
| Voz de trovão | Azul trovão | Camisa social de botão, Aura farmada, Microfone, Cata-vento |
| Turma do Código | — | Teclado do Arthur, Chip do Cristian, Boné de engrenagem do Enzo, Gabinete do Gabriel, Placa de vídeo do Guilherme, Controle do Gustavo, Joystick do João, Café do José, Notebook do Leonardo, Lupa caça-bugs do Léo, Pendrive do Pedro B., Óculos pixelados do Pedro G., Robozinho do Rafael, Visor do Vitor |

São 20 coleções. com 15 cores e 92 acessórios (10.430 moedas no total). As cores custam 150 e os acessórios de 60 a 160; quase tudo abre no nível 1, e as capas e guitarras no nível 2.

**Misturar peças.** O aluno pode usar várias peças ao mesmo tempo, uma por **lugar**, então nunca dá para usar dois cabelos ou dois colares:
- `cabeca`: cabelos, chapéus, coroas;
- `rosto`: óculos, barba, headset;
- `pescoco`: colares, medalhas, gravatas, cachecol;
- `roupa`: jaqueta, camisetas, camisa social, toga, faixa;
- `costas`: capas, mochila, violão, aura;
- `lado`: o que o mascote segura;
- `pensamento`: o balão.

**Peça de dois lugares.** Uma peça pode ocupar dois lugares (campo `tambem`). A "Nana enroladinha" é `roupa` e também `cabeca`, porque cobre a cabeça, o corpo e as pernas (fica uma bola redondinha, só com o rosto de fora); com ela não dá para usar chapéu ou cabelo. `vestir(usando, x)` põe a peça, libera o outro lugar dela e tira a peça de dois lugares que estiver ocupando o lugar novo. Vale para usar, comprar, provar e montar as capas. Todos os `id` de cores e itens são únicos (o teste confere): comprar uma cor nunca marca um item como comprado.

Ordem de desenho, de trás para a frente: costas, corpo, roupa, pescoço, rosto, cabeça, lado e, por fim, as partes marcadas como `topo` (ex.: as conchas do headset ficam por cima do cabelo e do chapéu; campo `topo` do item, em `DESENHO_TOPO`).

**Teste de combinações.** Um teste automático desenhou o mascote com cada par de peças de lugares diferentes (3.676 pares) e mediu quanto de cada peça a outra esconde. O que sobrou acima de 15% é o natural de camadas (um objeto na frente da capa, o cachecol por cima da camisa, o crachá por cima da faixa). O teste fez mudar:
- a barba ficou mais curta, para não esconder colares e gravatas;
- a flor foi para a lapela, mais abaixo;
- a faixa preta desceu para a cintura, abaixo dos colares e do cachecol;
- a aura ficou maior, para aparecer em volta de cabelos compridos;
- as conchas do headset passaram para a camada `topo`.

Depois, as combinações foram conferidas também na tela, com o mascote ampliado.

**Como aplicar no mascote (o que foi revisado).** O mascote é uma bola sem pescoço, então:
- os **colares** descem em "U" no peito, abaixo da boca, e vão de uma borda do corpo à outra, como se dessem a volta por trás; o pingente fica no fundo do "U" e as contas diminuem perto das bordas, dando profundidade (`LINHA_COLAR`, `COLAR`, `CONTAS_EM_VOLTA`);
- as **roupas** (jaqueta, camisetas, toga, faixa, gravata) ficam em camadas: saem 1 pixel para fora do corpo (`VESTE`), têm contorno e têm luz e sombra (`TOM_ROUPA`), em vez de uma cor lisa; as capas têm dobras. As camisetas cobrem o corpo da gola para baixo, com a estampa longe da boca;
- a **jaqueta de spikes** é aberta na frente, e os spikes saem dos ombros sempre para fora, na direção da curva do corpo;
- os **cabelos e toucas** acompanham a curva da cabeça (`TOPO_CABECA`); o cabelo comprido (`CABELO_LONGO`) cai por trás da cabeça e aparece em volta dela, com a raiz mais escura;
- o **violão nas costas** é desenhado inteiro, na diagonal, atrás do corpo (o braço sai pelo ombro e o corpo aparece embaixo, à direita), e só a alça passa na frente, na barriga (campo `frente`). No ícone da loja ele aparece inteiro, sem a alça;
- a **toga** é de um ombro só, com a borda dourada na diagonal;
- o **chapéu de cogumelo** (lembrança das corridas no fliperama) fica justo na cabeça: uma cúpula em cima, unida a uma parte que acompanha a curva da cabeça e passa só 2 pixels dos lados. Tem luz e sombra, a parte de baixo clarinha aparecendo na aba e as pintas menores nas laterais, para dar profundidade;
- tudo que a pessoa segura vai no lugar **`lado`**, na frente do corpo (o escudo de segurança também fica assim, como a cobertinha).
- Cada visual foi conferido no mascote ampliado 3 vezes, peça por peça e nas capas.

**Como funciona.**
- Cada coleção (`{ id, curto, nome, texto, fundo, capa, cores, itens }`) entra em `CORES` e `ITENS` com o campo `colecao`. A compra, a prova e o uso são iguais aos da loja comum.
- Cada acessório tem o seu desenho em `DESENHO_ACESSORIO[id]` (e, se precisar, a parte da frente em `DESENHO_FRENTE[id]`): uma função que pinta no quadro de 64 × 64 do mascote com formas simples (`AE` elipse, `AR` retângulo, `AP` polígono, `AL` linha, `AA` anel, `AU` união), com cores em "#hex".
- O ícone de cada acessório na loja é o próprio desenho dele sozinho, recortado e ampliado por inteiro (`iconeAcessorio(x)`).
- **Tela Visual** (como no desenho da equipe):
  - em cima, o mascote, com a prova do item;
  - embaixo, o título **Coleções** e uma grade de capas, 3 por linha;
  - a **capa** é o mascote vestido com o visual da coleção (`visualDaCapa(c)`: as peças de `capa`, ou a 1ª cor e uma peça por lugar), sobre a cor de fundo dela, com o título e quantos itens o aluno já tem;
  - a primeira capa, **Clássicos**, tem as cores e os acessórios de sempre;
  - tocar numa capa lê o título e abre a página da coleção: o botão do canto vira "< Coleções" (no lugar de "Voltar"), e aparecem o cartão com o texto e as vitrines de cores e acessórios;
  - ao voltar para as capas, a tela fica no mesmo lugar em que estava (`posicaoCapas`), e provar ou comprar não muda a rolagem;
  - **Pensamento da Hachi** (coleção "Punk que quer ser árvore"): um item comprável no lugar novo **`pensamento`**, com a frase do anime favorito da pessoa (campo `frase`). Quem usa aparece pensando nela: uma nuvem de pensamento em pixel art fica em cima do mascote, com bolinhas descendo até a cabeça (`pensamentoHTML(visual)`), na tela Visual (também na prova) e na tela inicial. O ícone na loja é o sprite `nuvem-pensamento`.
- Conquista **Colecionador**: completar uma coleção especial.

### Provar e comprar

1. **Item que o aluno ainda não tem:** tocar faz o mascote **provar** o item, sem gastar nada. O cartão fica laranja com "Provando". Tocar de novo tira a prova.
2. Com um item em prova, aparece a **barra de prova** abaixo do mascote: nome, preço, botão **Tirar** e o botão de compra. O botão de compra mostra:
   - cadeado em pixel art + "Nível N" (desativado) se o nível do aluno for menor que o do item;
   - "Faltam N moedas" (desativado) se faltar dinheiro;
   - **Comprar**, nos outros casos.
3. **Comprar** abre a confirmação "Comprar X? Vai custar N moedas. Você tem M." com **Cancelar** e **Comprar**. Ao confirmar: desconta as moedas, guarda o item em `comprados`, já coloca no mascote e mostra o aviso "X comprado!".
4. **Item que o aluno já tem:** tocar coloca ou tira na hora. Cores só trocam (sempre há uma cor em uso).
5. Itens bloqueados por nível também podem ser provados.

O rótulo de cada cartão, em ordem de prioridade: "Provando", "Usando", "Usar" (já é do aluno), cadeado + "Nível N" ou o preço com a moeda.

## 8. Pixel art

Os elementos de interface estão migrando para pixel art desenhada pela equipe no Pixilart. A regra principal: **todo sprite é ampliado por um número inteiro**, para os pixels não borrarem. O padrão é ×3 (`ESCALA = 3`); ×2 e ×1 só em lugares pequenos.

### Regras para novos sprites

- PNG no tamanho original (não ampliado), fundo transparente, sem antisserrilhado (nenhum pixel semitransparente).
- Contorno escuro `#2B2A33` de 1 pixel, igual ao do mascote.
- Animações: quadros do mesmo tamanho, com o mesmo ponto de apoio em todos.
- No código, sprites são embutidos como data URL no objeto `SPRITES` e mostrados com `spr(nome, escala, alt)`, que gera `<img class="px">` com `image-rendering: pixelated`. Para usar arquivos separados no GitHub, basta trocar o data URL pelo caminho (ex.: `'sprites/moeda.png'`).

| Tamanho recomendado | Para quê | Tamanho na tela (×3) |
| --- | --- | --- |
| 12 × 12 | Moeda, corações e ícones de botão | 36 px |
| 16 × 16 | Produtos do Mercadinho e itens da loja (ainda não feitos) | 48 px |
| 64 × 64 | Mascote | 192 px |
| 24 × 24 (9-slice) + 8 × 6 | Balão de fala e sua ponta | estica conforme o texto |
| 120 × 214 | Fundo de tela (ainda não feito) | tela inteira |

### Sprites no app (`SPRITES`)

| Nome | Tamanho | Onde aparece |
| --- | --- | --- |
| `moeda` | 12 × 12 | Pílula de moedas (×2), preços da loja (×1), vitória (×2), apresentação (×8) |
| `coracao-vazio`, `coracao-quarto`, `coracao-metade`, `coracao-cheio` | 12 × 12 | Topo da tela inicial (×3), vitória (×2), Progresso (×4), apresentação (×4). O de ¼ é provisório, montado a partir do cheio e do vazio |
| `controle` | 12 × 12 | Botão Jogos |
| `visual` | 12 × 12 | Botão Visual |
| `engrenagem` | 12 × 12 | Botão e título das Configurações |
| `ampulheta` | 12 × 12 | Botão e título do Progresso |
| `estrela` | 12 × 12 | Selo de nível (tela inicial e Progresso) |
| `led` | 12 × 12 | Aba de notícias (pisca a cada 1,2 s) |
| `musica`, `som`, `balao` | 12 × 12 | Opções Música, Sons (desligados) e Narração das Configurações |
| `som-ligado` | 12 × 12 | Opção Sons ligada e botão de ouvir a instrução nos jogos. Feito pela IA a partir do `som`: sem o X e com duas ondas |
| `SOMBRA_MASCOTE` (fora de `SPRITES`) | 64 × 8 | Sombra embaixo do mascote na tela inicial (×3). Feita pela IA |
| `porta-fechada`, `porta-aberta` | 12 × 12 | Botão Fechar das Configurações |

**Porta.** Fica só no botão **Fechar** das Configurações; nas outras telas o botão de voltar é a seta com a palavra (`botaoVoltar`). `portaHTML()` mostra a porta fechada; `sairPelaPorta(botao, acao)` troca para aberta, volta para fechada aos 320 ms e executa a ação aos 520 ms. Toques repetidos são ignorados.

### Pixel art dos jogos (desenhada no código)

Como o mascote, os desenhos dos jogos são feitos no próprio código, para o app continuar num arquivo só e a equipe poder editar sem programa de desenho.

- **`PALETA_PX`:** as cores, cada uma com uma letra (ex.: `K` contorno `#2B2A33`, `R` vermelho, `Y` amarelo). `.` é transparente.
- **`ARTE`:** desenhos feitos à mão, como listas de linhas de letras: os 15 produtos (16 × 16) e os ícones de 12 × 12 (`icone-caca`, `icone-conexao`, `icone-memoria`, `icone-mercadinho`, `icone-balanca`, `icone-dica`, `icone-livro`, `icone-tocar`, `icone-arrastar`), os ícones dos acessórios (`item-...`) e os de prêmios e botões: `trofeu`, `cadeado`, `seta-cima`, `igual`, `repetir`, `certo` (produto no pedido), `play`, `video`, `seta-esquerda` e `seta-direita` (carrossel). Para mudar um desenho, troque as letras.
- **Estrelas:** `estrelasPx(n)` mostra 3 estrelas em pixel art, com as não ganhas apagadas (cinza). O ícone da aba do navegador também é a estrela em pixel art.
- **Desenhos feitos com formas:** `grade(w, h, formas)` pinta formas (`retangulo`, `elipsePx`, `poligono`) e põe o contorno escuro sozinho; `colar()` junta um desenho em cima de outro. Assim são feitas as figuras da memória, as notas e moedas, a balança e as barrinhas da dificuldade.
- **Fonte pixel:** `FONTE_GRANDE` (5 × 7, letras e números) e `FONTE_PEQUENA` (3 × 5, números, R, $ e vírgula). `textoPx(texto, opções)` gera a imagem do texto e `precoPx(centavos)` escreve preços como "R$2,50". O texto alternativo da imagem é o próprio texto, para leitores de tela.
- **`arte(desenho, escala, alt)`** gera o `<img class="px">` sempre ampliado por número inteiro; `arteURL()` transforma as linhas em imagem uma vez só (com cache).

### Mascote

O mascote não é um PNG: é desenhado pixel a pixel por `desenharMascotePixel(visual, humor, olhosFechados)` numa grade de 64 × 64, transformado em imagem por `quadroMascote()` (com cache por combinação) e mostrado por `mascoteHTML(humor, visual, escala)`. Assim, as cores e os acessórios mudam sem precisar de um sprite para cada combinação.

- **Forma:** corpo oval (centro 32,38; raios 22 × 20), dois pés, barriga mais clara, bochechas rosadas, olhos brancos com pupila e brilho, boca aberta com língua (`'feliz'`) ou sorriso fechado (`'normal'`).
- **Cores derivadas da cor escolhida:** sombra (embaixo à direita, misturada 30% com marrom), brilho (em cima à esquerda, 50% branco) e barriga (55% branco).
- **Acessórios desenhados:** chapéu azul com faixa amarela, óculos redondos, cachecol listrado vermelho, laço rosa (em cima da cabeça, um pouco à direita: centro em x = 40, y = 21,5, apoiado no corpo), coroa dourada com pedra vermelha e capa vermelha atrás do corpo.
- **Escalas:** ×3 (192 px) na tela inicial, na apresentação e na Visual; ×2 (128 px) na vitória; ×1 (64 px) no Progresso.
- **Animações:** balança 3 px para cima e para baixo a cada 0,8 s (sem esticar); pisca por 150 ms a cada 3,8 s em todas as telas; pula quando tocado.
- `sprites/mascote.png` (128 × 64) tem os quadros de olhos abertos e piscando, para referência ou redesenho.

### Balão de fala

Usa `border-image` (9-slice) com `balao-base.png` (24 × 24, cantos de 8 × 8, contorno de 1 pixel e sombra clara embaixo) ampliado ×3, e `balao-ponta.png` (8 × 6) apontando para cima, sobre a borda do balão. Os cantos ficam fixos e o meio estica conforme a frase. Largura máxima: 300 px.

## 9. Áudio

Há três camadas de som, cada uma com seu interruptor nas Configurações e salva no estado: `som`, `musica` e `voz`. Navegadores só liberam áudio depois do primeiro toque, por isso um ouvinte de `pointerdown` marca `interagiu = true` e só então a música começa.

### Efeitos sonoros — `tocar(tipo)`

Gerados na hora pela Web Audio API, sem arquivos. Todos tocam no tom da música da tela (`musicaDoTom()`); na tela de vitória, que não tem música, usam o tom do jogo que acabou (`ultimaMusica`). O clique fica em `cliqueMusical()`; os outros, em `tocarEfeito(ctx, saida, tipo, t)`, que monta frases curtas com os instrumentos de `INSTRUMENTOS`. As notas abaixo estão em dó maior (tela inicial).

| Tipo | Quando toca | Como soa | Pico |
| --- | --- | --- | --- |
| `clique` | Botões e navegação | "Bolhinha de madeira" na 5ª do tom (abaixo) | 0,13 |
| `acerto` | Resposta certa (`ctl.acerto`) | Kalimba subindo (sol → dó) com um sininho; a cada acerto da partida (`acertosNaPartida`), o par sobe um degrau da pentatônica, até 4 degraus acima | 0,16 |
| `erro` | Resposta errada (`ctl.erro`) | "Toc-toc" de madeira abafada (`madeira`) descendo um degrau até a tônica (ré → dó) | 0,08 |
| `moeda` | Dinheiro na mesa, compra e fim da contagem de moedas | Três sininhos rápidos subindo (sol, dó, mi) | 0,15 |
| `mascote` | Tocar no mascote | O pulo do motivo na kalimba (sol grave, dó, SOL agudo) | 0,16 |
| `vitoria` | Tela de vitória e subida de nível | Fanfarra de 1,4 s: o motivo rápido, o pulo e um acorde dedilhado com baixo e sininhos; sempre termina em maior | 0,21 a 0,29 |

`acertosNaPartida` volta a zero quando a música da tela muda (`definirMusica` com outro nome), ou seja, a cada partida nova, mesmo com a música desligada.

**Clique (`cliqueMusical`).** Uma nota que sobe rápido de 3/4 da altura até a nota certa ("blup"), mais uma parcial 4× mais aguda e curtíssima (a batida da baqueta), com menos de 0,1 s e volume 0,12. Toca a 5ª nota do tom da música da tela (`tom` em `MUSICAS`; se a música estiver desligada, usa a música que tocaria ali). Toques com menos de 0,45 s entre eles sobem uma escadinha pentatônica de 5 notas (`[0, 2, 5, 7, 9]` no modo maior, `[0, 3, 5, 8, 10]` no menor) e recomeçam; depois de uma pausa, volta à primeira nota.

### Música de fundo — `definirMusica(nome)`

As músicas são compostas no código, a partir do estudo em `Estudo musical — Educatata.md`. Todas são variações do **motivo do mascote** (dó-sol-ré-sol-mi-sol-SOL, "pula, pula, pulão"), cada uma com o estilo do seu jogo. Trocar de tela com a mesma música não a reinicia.

| Nome | Onde toca | Música | Estilo e tom | Andamento | Loop |
| --- | --- | --- | --- | --- | --- |
| `inicio` | Todas as telas fora dos jogos (exceto a vitória) | Pula, pula, pulão | Baião-pop, dó maior | 104 bpm | 16 compassos, 37 s |
| `caca` | Caça-palavras | Jardim das descobertas | Lá dórico | 96 bpm | 16 compassos, 40 s |
| `conexao` | Conexão de palavras | Bossa das palavras | Bossa nova, fá maior | 96 bpm | 16 compassos, 40 s |
| `memoria` | Jogo da memória | Valsa da caixinha | Valsa (3/4), ré maior | 116 bpm | 24 compassos, 37 s |
| `mercadinho` | Mercadinho | Forró da feirinha | Forró, sol maior | 116 bpm | 16 compassos, 33 s |
| `balanca` | Balança | Swing da gangorra | Jazz em swing, si bemol maior | 100 bpm | 16 compassos, 38 s |
| `aparelhos` | Mundo digital | Samba dos 8 bits | Samba com som de videogame antigo, mi maior | 104 bpm | 16 compassos, 37 s |
| `historia` | Linha do tempo | Ciranda do tempo | Ciranda pernambucana, dó menor | 96 bpm | 16 compassos, 40 s |
| `geografia` | Caminho no mapa | Marchinha do explorador | Marchinha, lá maior | 112 bpm | 16 compassos, 34 s |
| `atelie` | Ateliê de cores | Chorinho das cores | Choro, mi bemol maior | 100 bpm | 16 compassos, 38 s |
| `teatro` | Teatro de histórias | Toada do mamulengo | Banda de pífano (teatro de bonecos), ré menor | 96 bpm | 16 compassos, 40 s |
| `maestro` | Maestro | sem música | Silêncio de propósito, para ouvir as notas dos sinos (`semMusica`) | — | — |

**Como uma música é escrita (`MUSICAS`).** Cada música tem `bpm`, `compasso` (tempos por compasso), `tom` (MIDI, 60 = dó central), `modo` (`'maior'` ou `'menor'`), `balanco` (swing), `volume`, `acordes` (um grupo por compasso, separados por `|`) e `faixas`. Cada faixa tem `inst` (nome em `INSTRUMENTOS` ou `PERCUSSAO`), `camada`, `vol`, `pan`, `eco`, `passos` (2 = colcheias, 4 = semicolcheias), `centro` e `notas`, em texto: `C5` = nota, `-` = continua a anterior, `.` = pausa, `_` = compasso em silêncio, `1 3 5 7 8` e `2 4 6` = notas do acorde do momento, `A` = acorde inteiro, `x X` = batida fraca e forte. A legenda completa está no comentário acima de `MUSICAS`.

**Instrumentos.** Cada instrumento em `INSTRUMENTOS` é uma soma de ondas (`parciais`), com ataque, decaimento, filtro, vibrato e sopro (kalimba, marimba, caixinha, sino, vibrafone, piano, flauta, ocarina, pífano, pássaro, clarinete, sanfona, violão, cavaquinho, harpa, baixo, pizzicato, cordas e chip, a onda quadrada de videogame antigo usada no Mundo digital). A percussão (`PERCUSSAO`) usa chiado filtrado ou ondas que caem de tom: bumbo, zabumba, bacalhau, aro, palma, chocalho, caixa, prato, blocos de madeira e triângulo (fechado e aberto). Tudo passa por um compressor leve e uma reverberação de sala (`criarEco()`).

**Camadas.** Cada faixa tem `camada` 0, 1 ou 2, e só toca se `camada <= nivelMusica`. Uma camada nova só entra no começo de um compasso.

- Nos jogos, a música começa na camada 0; `ctl.acerto()` chama `animarMusica()`, e a camada 1 entra no 1º acerto e a 2 no 2º. Errar não tira camadas.
- Na tela inicial, `nivelDaCasa()` segue o coração do dia: 0 vitórias = camada 0, 1 ou 2 = camada 1, `META_DIARIA` ou mais = camada 2.

**Tocador.** `montarMusica(ctx, nome)` monta os nós num contexto de áudio e devolve `tocarTique(i, t, nivel)`; um tique é 1/4 de tempo. `definirMusica()` agenda os tiques 0,4 s à frente, a cada 100 ms. Se a aba ficou em segundo plano, o tocador pula o atraso em vez de tocar tudo de uma vez.

**Volume.** O volume final de cada música (`volume`) foi medido com `OfflineAudioContext` para ficar perto de −31 dB RMS, com picos entre 0,10 e 0,17 (o som de acerto tem pico 0,16). Enquanto a narração fala, `abafarMusica()` baixa a música para 35% (`ABAFADO`).

**Arquivos (opcional).** Para usar uma gravação no lugar da música gerada, coloque o caminho em `ARQUIVOS_MUSICA` (ex.: `inicio: 'musica/inicio.mp3'`). O app toca o arquivo em loop, volume 0,35 (`VOLUME_ARQUIVO`), também abaixando durante a narração, mas sem camadas.

### Narração — `falar(texto)`

Usa uma **voz gravada**: uma voz de IA criada para o Educatata, que não imita ninguém. O porquê da escolha e como foi feita estão em `Estudo de voz — Educatata.md`, e os scripts, em `Voz/`. **A narração começa desligada** (`estado.voz = false`): quem usa decide ligar, pelo botão "Ler em voz alta" no alto da apresentação ou pelo interruptor Narração das Configurações (ao ligar, diz "Narração ligada.").

- **Frases e áudios.**
  - `falar()` divide o texto em frases (`dividirFrases`: depois de `.`, `!` ou `?` seguidos de espaço; reticências não dividem). Cada frase tem um código (`chaveVoz`, FNV-1a de 32 bits do texto) e um áudio MP3.
  - Os áudios ficam em pacotes, um por tela ou jogo (`midia/voz/voz-<grupo>.js`, MP3 em base64). Cada pacote chama `VOZ_PACOTE(grupo, chaves, audios)` ao carregar.
  - `CODIGOS_VOZ` diz em que pacote está cada código. Essa lista é escrita no `index.html` pelo script `Voz/finalizar.py`, entre as marcas `VOZ_GRAVADA`.
  - `ir()` já começa a carregar os pacotes da tela (`prepararVoz`). O pacote de um jogo carrega durante a tela de "Carregando".
- **Texto novo precisa de áudio.** Se mudar ou criar uma frase falada, gere os áudios de novo com os scripts de `Voz/` (`inventario.py`, `producao.py`, `qa.py`, `finalizar.py`). Uma frase sem áudio, ou sem a pasta `midia/`, é lida pela melhor voz pt-BR do aparelho (`lerComVozDoAparelho`).
- **Um canal só:**
  - uma fala nova corta a anterior; com `{ fila: true }`, espera a atual terminar (nas instruções dos jogos, nos avisos e na interpretação do Teatro);
  - a mesma fala não é lida duas vezes seguidas enquanto ainda está tocando;
  - `calar()` para tudo na hora (o áudio, a voz do aparelho e a fila) e devolve a música.
- **Quando a fala para:**
  - ao trocar de tela (`ir()`);
  - ao fechar qualquer janela (`confirmar` só cala a leitura dela, com `calarSe`);
  - ao fechar as Configurações e o "Como jogar";
  - ao desligar a narração;
  - quando o vídeo começa.
- **Esperar a fala terminar.** A vitória espera a última fala do jogo terminar, com no máximo 9 s (`depoisDaFala` em `ctl.fim`). O mesmo vale para a próxima cena do Teatro e para cada passo do "Como jogar".
- **Fala diferente do texto mostrado.** `ctl.instrucao(texto, falarAgora, fala)`, `ctl.msg/acerto/erro(..., fala)`, `aviso(texto, fala)` e `confirmar({ fala })` aceitam o que a narração deve dizer, em texto ou numa lista de frases.
- **Teatro.** `{ emocao }` usa o áudio gravado com a emoção (pacote `teatro`, código `<frase>-<emoção>`).
- `{ forcar: true }` fala mesmo com a narração desligada. Só o botão do alto-falante ao lado da instrução usa isso, porque é um pedido direto da criança.
- **Primeiro toque.** Antes do primeiro toque na tela, o navegador não deixa tocar som. A fala fica guardada (`narracao.pendente`) e toca no primeiro toque, se a tela for a mesma. Esse toque também "libera" o elemento de áudio, para o iPhone deixar a narração tocar depois.
- **Música e volume.** Enquanto fala, a música abaixa (`abafarMusica(true)`) e volta no fim. O volume da Narração (0 a 10) vale para os áudios e para a voz do aparelho.
- **Números.** "Você tem N moedas" (até 999, acima diz "mais de mil"), "Você usou N tentativas", "Nível N" e os outros números falados têm uma frase gravada para cada valor.

**O que é lido (pensado para quem ainda não lê ou lê devagar):**

- **Cada tela, ao abrir** (`FALA_DA_TELA`, em `ir()`): a fala do mascote na tela inicial, "Jogos" com o jogo em destaque, Visual com as moedas, Progresso com o nível e as moedas, Conquistas com quantas foram ganhas, Caderno, "Como usar" e "Abrindo (jogo)". As Configurações também dizem o que se faz nelas. As telas de jogo, de vitória e de revisão leem o próprio conteúdo.
- **A apresentação:** o título e o texto de cada tela.
- **Nos jogos:** a instrução de cada rodada e **toda mensagem** de acerto, erro e dica (`ctl.msg`, `ctl.acerto`, `ctl.erro`), além das falas próprias de cada jogo (grupos de palavras, carta virada, fatos dos museus e do mapa, a interpretação do Teatro).
- **Janelas** (`confirmar`): o título, o texto e os botões ("Toque em Sair ou em Continuar jogando."). Avisos (`aviso`), como "Nova conquista" ou "Progresso apagado.", também são lidos.
- **Escolhas:** a dificuldade tocada, o jogo do carrossel, a coleção aberta e o item do Visual (com o preço, se ainda não foi comprado), a medalha tocada, o cartão do Caderno, as perguntas e respostas da revisão, os passos do "Como jogar", a frase do mascote ao tocar nele e o resultado da partida, com as tentativas e as moedas ganhas.

## 10. Dados salvos

O progresso fica só no navegador do aparelho, em `localStorage`, com a chave **`appmm-web-v1`**, como um JSON do objeto `estado`. A chave guarda o nome antigo do app (App MM) de propósito: trocá-la apagaria o progresso de quem já joga. Não há servidor nem conta: trocar de aparelho ou limpar os dados do navegador apaga o progresso.

| Campo | Tipo | Padrão | Significado |
| --- | --- | --- | --- |
| `onboarded` | boolean | `false` | Já passou pela apresentação e pelo vídeo |
| `moedas` | number | `0` | Moedas disponíveis |
| `xp` | number | `0` | XP dentro do nível atual |
| `nivel` | number | `1` | Nível atual |
| `som` | boolean | `true` | Efeitos sonoros ligados |
| `voz` | boolean | `false` | Narração ligada (começa desligada; quem usa liga) |
| `musica` | boolean | `true` | Música de fundo ligada |
| `cor` | string | `'laranja'` | Id da cor em uso |
| `comprados` | string\[\] | `['laranja']` | Ids de cores e acessórios já comprados |
| `usando` | object | `{ cabeca: null, rosto: null, pescoco: null, roupa: null, costas: null, lado: null, pensamento: null }` | Acessório em uso em cada lugar (`lado`: o que o mascote segura; `pensamento`: o balão de pensamento) |
| `dificuldade` | string | `'medio'` | Dificuldade escolhida: `'facil'`, `'medio'` ou `'dificil'` |
| `recordes` | object | `{}` | Menor número de tentativas por jogo e dificuldade (chaves `caca`, `caca:facil`, `caca:dificil`...) |
| `historico` | object\[\] | `[]` | Partidas: `{ jogo, dificuldade, fase, tentativas, estrelas, data }`, no máximo 2000 (`LIMITE_HISTORICO`) |
| `conquistas` | object | `{}` | Id da conquista → data em que foi ganha |
| `revisoes` | object | `{}` | Matéria → `{ vezes, nivel, ultima, proxima }` da revisão espaçada do Caderno |
| `volumes` | object | `{ som: 8, musica: 8, voz: 10 }` | Volume de 0 a 10 de cada tipo de som (Configurações) |
| `fases` | object | `{}` | Jogo → `{ saco, vistas, vencidas }`: fases vencidas abrem cartões no Caderno; fases que ainda vão sair (sem repetir) e fases que o aluno já conheceu |

- `carregar()` junta o que estiver salvo com `estadoInicial()`, então campos novos ganham o valor padrão sem quebrar dados antigos. Se o JSON estiver corrompido ou o armazenamento estiver bloqueado, o app começa do zero.
- `salvar()` grava o estado inteiro; erros de gravação são ignorados.
- **Não renomeie a chave nem campos existentes** sem criar uma migração: isso apagaria o progresso dos alunos.

## 11. Visual e acessibilidade

### Cores (variáveis CSS em `:root`)

| Variável | Valor | Uso |
| --- | --- | --- |
| `--fundo` | `#FFF7EC` | Fundo do app (creme) |
| `--card` | `#fff` | Cartões e botões |
| `--texto` | `#2B2A33` | Texto e contorno da pixel art |
| `--suave` | `#6B6A75` | Texto secundário |
| `--borda` | `#EADFCF` | Bordas |
| `--prim` / `--prim-esc` | `#FF8A3D` / `#E8701F` | Cor principal (laranja) e sua sombra |
| `--azul` | `#4C6EF5` | Selo de nível, seleção, gráficos |
| `--verde` / `--verde-claro` | `#2F9E44` / `#D3F9D8` | Acerto, "Usando", meta cumprida |
| `--vermelho` / `--verm-claro` | `#E03131` / `#FFE3E3` | Erro, coração, meta |
| `--amarelo` | `#FFC933` | Estrelas e dica |
| `--raio` | `18px` | Cantos arredondados |
| `--sombra` | `0 4px 0 rgba(0,0,0,.08)` | Sombra sólida dos botões |

### Tipografia e componentes

- Fonte: `"Nunito", "Trebuchet MS", system-ui` (nenhuma fonte é baixada da internet). Pesos 700 a 900, textos grandes para crianças.
- **Botão** (`.btn`): laranja, largura total, sombra sólida que "afunda" ao tocar; `.btn.sec` é branco com borda.
- **Pílula** (`.pill`), **cartão** (`.card`), **cabeçalho com seta** (`cabecalho()`), **aviso temporário** (`aviso()`, 2,2 s, um por vez) e **janela de confirmação** (`confirmar({ titulo, texto, html, sim, nao, aoSim, aoNao })`).
- Feedback visual: elemento treme no erro (`.treme`), mascote pula (`.pulo`), confete na vitória.

### Acessibilidade e responsividade

- **Mãozinha no mascote:** na tela inicial, como nos jogos de clicar, um cursor de luva branca com contorno (sprite `cursor-mao`, 16 × 16, ×4), inclinado, fica "clicando" na barriga do mascote, e o mascote pulsa de leve, para mostrar que dá para tocar nele. Ela some quando o aluno toca no mascote e volta depois de 12 s sem nenhum toque na tela (`OCIOSO_MS`). O mascote não ganha a borda azul de seleção.
- **Seleção azul (como nos consoles):** o que está selecionado ganha uma borda azul (`#4C6EF5`): com o teclado (`:focus-visible`), com o mouse passando por cima (só em aparelhos com mouse) e com o dedo encostado ou arrastando pela tela (classe `mirado`, que sai quando o dedo levanta). Enquanto o aluno arrasta dinheiro no Mercadinho, a mesa é que mostra onde vai cair.
- **Tem mais embaixo:** quando a tela pode rolar, aparece no pé dela uma setinha em pixel art que pula de leve (`prepararIndicativos()`); ela some quando chega ao fim, e tocar nela desce a tela. As janelas que rolam (ex.: Configurações em telas pequenas) mostram uma sombra em cima e embaixo enquanto há mais conteúdo.

- Tela pensada para celular em pé, de 360 a 430 px de largura; acima de 500 px vira um "celular" centralizado.
- Botões grandes (mínimo de 42 px), textos com contraste alto e narração para alunos que ainda leem devagar.
- Botões só com ícone têm `aria-label`; mensagens dos jogos ficam em `aria-live="polite"`; o mascote e os gráficos têm texto alternativo.
- Com `prefers-reduced-motion: reduce`, as animações e transições ficam quase instantâneas, e as notícias da tela inicial ficam paradas (trocam a cada 7 s).
- Arrastar (Mercadinho e caça-palavras) usa Pointer Events, então funciona com dedo, mouse e caneta; as áreas arrastáveis têm `touch-action: none` para não rolar a tela.

## 12. Regras para a IA ao alterar o código

### Convenções

1. **Mantenha o arquivo único** `index.html`, sem bibliotecas, frameworks ou etapa de build, a menos que a equipe peça outra coisa. Ele tem que abrir direto no navegador e funcionar offline. Os vídeos e os pacotes de voz ficam em `midia/`; sem essa pasta o app continua funcionando (o vídeo mostra um aviso e a narração usa a voz do aparelho).
2. **Escreva em português**: nomes de variáveis e funções, comentários e todos os textos da interface (com acentos corretos).
3. **Comente o que não for óbvio**, no mesmo estilo dos comentários atuais. O código será explicado pela equipe na apresentação.
4. **Troque de tela só com `ir(tela, dados)`.** Nova tela = nova função `telaX(main, dados)` registrada em `TELAS`. Telas abertas a partir da inicial usam `cabecalho()` e `ligarVoltar(main)`.
5. **Novo minijogo** = nova entrada em `JOGOS` com `nome`, `artigo`, `materia`, `icone`, `fundo`, `dificuldades` (`facil`, `medio` e `dificil`, lidas em `ctl.config`) e `iniciar(area, ctl)`. Use só as funções de `ctl` (seção 2), chame `ctl.tentativa()` a cada tentativa e `ctl.fim(minimo)` no final. Adicione também a música em `MUSICAS` e `ARQUIVOS_MUSICA` com o mesmo id: uma variação do motivo do mascote no estilo do jogo, com camadas 0, 1 e 2 (veja o estudo musical).
6. **Atrasos dentro de jogos** sempre com `ctl.depois(ms, fn)`; fora deles, guarde `partida` e confira antes de agir.
7. **Não quebre os dados salvos**: campos novos entram em `estadoInicial()`; nunca renomeie `CHAVE` nem campos existentes sem migração.
8. **Pixel art**: novos sprites entram em `SPRITES` e são mostrados com `spr()` em escala inteira (padrão ×3). Nunca amplie por valores quebrados nem use CSS que borre (`image-rendering: pixelated` é obrigatório).
9. **Dinheiro sempre em centavos inteiros.**
10. **Sons:** use `tocar()`, `falar()` e `definirMusica()`; respeite os interruptores `estado.som`, `estado.voz` e `estado.musica`. Toda frase nova falada precisa do seu áudio gravado (seção 9, scripts em `Voz/`); sem ele, a frase sai com a voz do aparelho.
11. **Textos para crianças de 7 a 9 anos**: frases curtas, positivas, sem termos técnicos. Erros nunca são punitivos ("Tente de novo!").
12. **Sem emojis no app:** todo ícone é pixel art (em `SPRITES` ou `ARTE`). Para um ícone novo, desenhe em `ARTE`.
13. **Não adicione fotos ou vídeos** até a equipe mandar os arquivos; use os espaços reservados (`VIDEO_AJUDA`, `ARQUIVOS_MUSICA`).
14. **Registre cada pedido feito à IA** em `PROMPTS.md`, porque os prompts também são entregáveis.

### Como testar

- Abra o `index.html` no navegador com a largura de um celular (ex.: 390 × 844 nas ferramentas de desenvolvedor) e confira que o console não mostra erros.
- Jogue os cinco jogos até o fim, conferindo tentativas, estrelas, moedas, XP e recorde na tela de vitória.
- Confira o coração: 0, 1, 2 e 3 vitórias no dia devem mostrar vazio, ¼, metade e cheio.
- Na Visual: provar, tirar, comprar com confirmação, item bloqueado e falta de moedas.
- No Mercadinho, teste o arraste com mouse e com toque, inclusive tirar dinheiro da mesa.
- Recarregue a página e confirme que o progresso continua.
- Música: toque na tela para liberar o som e confira a música de cada tela, a camada nova a cada acerto (1º e 2º), a música abaixando durante a narração e o clique subindo a escadinha com toques rápidos. Depois de mudar `MUSICAS`, confira que cada faixa fecha compassos inteiros e meça o volume com `OfflineAudioContext` (`montarMusica(ctx, nome)` + `tocarTique()`).
- Para testar a apresentação de novo, use "Apagar progresso" ou rode `localStorage.removeItem('appmm-web-v1')` no console.
- Testes automatizados foram feitos com Playwright (Chromium, 390 × 844), jogando os cinco jogos pelos seletores dos botões; vale manter esse tipo de teste a cada mudança grande.

### Publicação

No GitHub, o `index.html` fica na raiz do repositório. Em Settings → Pages, escolha a branch principal e a pasta raiz. Arquivos de apoio no repositório: `README.md`, `visuais/` (PNGs originais), `pixelart/` (arquivos do Pixilart), `midia/` (vídeos e os pacotes de voz em `midia/voz/`, poucos arquivos, que cabem num envio pelo site do GitHub), `Voz/` (scripts e voz de referência da narração) e, quando existir, `musica/`. O `PROMPTS.md` **não** vai para o repositório: ele é entregue direto ao professor.

## 13. Pendências e decisões em aberto

### Arte e mídia

- [ ] Redesenhar o coração de ¼ (`coracao-quarto`), hoje provisório.
- [x] Versão do alto-falante sem o X, para mostrar o X só quando os sons estiverem desligados (`som-ligado`, feita pela IA a partir do sprite da equipe; pode ser redesenhada).
- [x] Produtos do Mercadinho, dinheiro, balança, figuras da memória, ícones dos jogos e letras em pixel art (feitos pela IA no código; a equipe pode redesenhar trocando as letras em `ARTE`).
- [x] Ícones dos acessórios, prêmios e selos em pixel art: o app não tem mais emojis.
- [x] Sombra do mascote em pixel art (64 × 8), feita pela IA (`SOMBRA_MASCOTE`). Os PNGs das duas estão em `visuais/Gerados no código/`.
- [ ] Mascote e cenário dentro do Mercadinho.
- [ ] Redesenho opcional do mascote à mão (64 × 64 por quadro, acessórios em camadas separadas).
- [ ] Fundos de tela em pixel art (120 × 214).
- [x] Músicas compostas a partir do estudo musical (`MUSICAS`, `Estudo musical — Educatata.md`). Gravações continuam opcionais em `ARQUIVOS_MUSICA`.
- [ ] Ouvir as músicas num celular de verdade (graves abaixo de ~150 Hz quase somem no alto-falante).
- [x] Efeitos `acerto`, `erro`, `moeda` e `vitoria` refeitos no tom da música, `erro` mais gentil e som próprio para tocar no mascote (`mascote`).
- [x] Vídeo de ajuda (`VIDEOS_AJUDA`): vídeo de apresentação em duas versões, sem Libras (`midia/apresentacao.mp4`) e com Libras (`midia/apresentacao-libras.mp4`) feitas a partir da narração da equipe (scripts em `Video/trabalho/`, roteiro em `roteiro.py`). A narração dos vídeos continua sendo a gravação da equipe.
- [x] Voz da narração do app: voz de IA criada para o Educatata (`Estudo de voz — Educatata.md`, scripts em `Voz/`).
- [ ] Ouvir e aprovar a voz nova do app antes de publicar. Se um dia alguém da equipe gravar as frases, basta trocar os pacotes de `midia/voz/`.
- [ ] Libras do vídeo: a janela usa o avatar do VLibras (tradução automática). Antes de publicar, uma pessoa que saiba Libras deve assistir e aprovar.

### Decisões

- [ ] Onde o vídeo de ajuda fica depois de pulado (`LOCAL_VIDEO`; hoje está nas Configurações).
- [ ] Valor final da meta diária (`META_DIARIA`, hoje 3).
- [ ] Se toda partida conta como vitória ou só as com 2 estrelas ou mais.
- [x] Conteúdo da aba de notícias: dicas de cada jogo, curiosidades, desafios do dia e datas do mês (`NOTICIAS`, `NOTICIAS_DO_MES`).
- [x] Mais conteúdo por jogo: 29 palavras, 12 conjuntos, 10 figuras e 15 produtos.
- [x] Níveis de dificuldade: Fácil, Médio e Difícil (seção 5.0).
- [x] Recompensa por dificuldade: Médio +5 e Difícil +10 moedas e XP (`ECONOMIA`).

### Entrega

- [x] Renomear o app para `index.html` e criar o `README.md` e o `PROMPTS.md`.
- [x] Criar o repositório no GitHub, subir os arquivos e ativar o GitHub Pages: repositório `hamiltonlicheski/app-mm`, site em https://hamiltonlicheski.github.io/app-mm/. O endereço ainda usa o nome antigo (App MM); para mudar, renomeie o repositório no GitHub (o link antigo deixa de funcionar).
- [ ] Completar o `PROMPTS.md` com os prompts das versões Web 1 a 5. Os da fase de finalização já estão registrados.
- [ ] Incluir a declaração de uso de IA nos outros materiais entregues (no `README.md` ela já está).

## 14. Histórico de versões

| Data | Versão | O que mudou |
| --- | --- | --- |
| 27/11/2026 | Web 39 | Narração refeita com uma voz de IA mais humana, doce e acolhedora, criada para o app (estudo em `Estudo de voz — Educatata.md`): todas as frases faladas gravadas em pacotes (`midia/voz/`), com a voz do aparelho de reserva; um canal só para a narração, que para ao trocar de tela, ao fechar janelas e as Configurações e quando o vídeo começa; o "Isso!" do Teatro e a última fala de cada jogo não são mais cortados; números, moedas e tentativas falados por inteiro; correções no texto falado ("Coragem!.", "Pedro B..", "1 tentativas", lista do Mercadinho). A narração do vídeo de apresentação não mudou |
| 27/11/2026 | Web 38 | Vídeo de apresentação refeito com as telas novas e edição mais humana (estudo em `Estudo de edição — Educatata.md`), em duas versões: em pé sem Libras e deitada com janela de Libras (avatar do VLibras); a tela "Como usar" pergunta se o aluno precisa de Libras e tem um link para trocar de versão; ponto de restauração antes da mudança |
| 27/11/2026 | Web 37 | Revisão e testes de todos os jogos e fases antes da publicação (robôs jogando até a vitória e milhares de sorteios conferidos). Corrigido: rosa dos ventos desenhada em cima da casa do canto do mapa (agora fica ao lado do botão N); SOM podia nascer dentro de SOMBRA no Caça-palavras; prédios repetidos no mapa; pesos para fora do prato na Balança (Dois pesos iguais, Difícil); botão "?" e mapa do Fácil fora da tela em celular de 320 px; bolinhas do Maestro que não apagavam no "Ouvir de novo"; sinos do Maestro e dinheiro do Mercadinho agora funcionam pelo teclado; foco preso na setinha "tem mais embaixo"; borda azul de seleção no botão Jogos da tela inicial (mouse e teclado), que faltava; histórico de 200 para 2000 partidas (o Progresso parava de contar e cartões do Caderno fechavam de novo depois da 201ª vitória); janelas pelo teclado (Esc fecha como tocar fora, o foco entra na janela e volta ao botão que a abriu, o Tab não sai dela, e nas Configurações o foco fica no mesmo botão depois de cada toque); leitor de tela: "Volume dos sons" e o liga/desliga diz se está ligado (`aria-pressed`); a dificuldade da tela Progresso só filtra as estatísticas, sem mudar a dos jogos; `.gitignore` |
| 27/11/2026 | Web 36 | Narração começa desligada, com o botão "Ler em voz alta" na apresentação; revisão de acessibilidade: passam a ser lidas as telas ao abrir, as mensagens de acerto, erro e dica de todos os jogos, as janelas, os avisos, a dificuldade e os itens do Visual; o botão de ouvir a instrução funciona mesmo com a narração desligada |
| 27/11/2026 | Web 35 | Cenário Cantinho de estudo na tela inicial, que se ajusta ao tamanho de cada tela, com o chão nos pés do mascote; ponto de restauração antes da mudança |
| 27/11/2026 | Web 34 | Kart de corrida retirado (e o lugar `veiculo`); chapéu de cogumelo mais justo na cabeça e com profundidade |
| 27/11/2026 | Web 33 | Kart como no desenho da equipe (rodas nas pontas, barra e estrutura até o corpo, sem volante); chapéu de cogumelo encaixado na cabeça; kart, bandeira e cogumelo passaram para a coleção Punk que quer ser árvore; a Dupla do fliperama, o capuz e o ovo de dinossauro e o Verde dino saíram |
| 27/11/2026 | Web 32 | Kart de corrida refeito: o mascote fica sentado em cima (o kart atrás e embaixo do corpo, os pés de fora e um volante na frente), num lugar novo, `veiculo` |
| 27/11/2026 | Web 31 | Coleção Dupla do fliperama (kart de corrida, chapéu de cogumelo, capuz de dinossauro verde, ovo de dinossauro, bandeira quadriculada e Verde dino); o botão Jogos da tela inicial sem a borda amarela, para não confundir com a seleção azul |
| 27/11/2026 | Web 30 | Nana enroladinha cobrindo também as pernas, bem redondinha |
| 27/11/2026 | Web 29 | Pensamento da Hachi; Nana na cabeça e Nana enroladinha (como um gatinho na coberta, ocupando cabeça e roupa, com `vestir()`); mãozinha de luva branca clicando no mascote, como nos jogos de clicar; Pudinzinho de volta e `id` repetido corrigido |
| 27/11/2026 | Web 28 | Lugar novo `roupa` (jaqueta e colar juntos, sem dois do mesmo tipo); teste automático das 3.676 combinações de peças e ajustes (barba, flor, faixa, aura e headset por cima); colar de ouro no lugar das pérolas; Pensamento da Nana colado no mascote; mãozinha que toca no mascote na tela inicial, no lugar da borda azul |
| 27/11/2026 | Web 27 | Pensamento da Nana como item (balão de pensamento ligado ao mascote, também na tela inicial); escudo na frente; Voz de trovão com cores combinando (azul, dourado e branco), camisa social de botão com bolso de gráfico e aura dourada; seleção com borda azul (teclado, mouse e dedo) e setinha de "tem mais embaixo" no app todo |
| 27/11/2026 | Web 26 | Coleções revisadas: sem nomes religiosos, contrabaixo vermelho, jaqueta de spikes aberta, Colar da Nana e a frase da Nana numa nuvem de pensamento, milk-shake, cabelo descolorido refeito, violão inteiro, escudo à vista, Aura farmada; colares que dão a volta no corpo e roupas em camadas; botão Coleções no lugar de Voltar e a grade de capas voltando no mesmo lugar |
| 27/11/2026 | Web 25 | Visual refeito como no desenho da equipe (mascote em cima e grade de capas das coleções, com o mascote vestido em cada capa); títulos das coleções sem nomes; coleção nova (Estilo cor-de-rosa) e itens novos (ojá, Nana, máscaras de teatro, copa de árvore, contrabaixo, três gatinhos, headset, perfume...); colares em "U" no peito, cabelos que acompanham a cabeça, guitarra com alça na frente e toga de um ombro, conferidos um a um |
| 26/11/2026 | Web 24 | Coleções especiais no Visual: 19 coleções de easter eggs da equipe (amigos, família, professores e a turma), com 13 cores e 81 acessórios em pixel art; lugar novo `lado` no mascote; ícones da loja gerados do próprio desenho; conquista Colecionador |
| 26/11/2026 | Web 23 | Seta com a palavra Voltar ou Sair em todas as telas (a porta ficou só no Fechar); confirmação ao sair de um jogo e ao pular a apresentação; a tela do vídeo da apresentação sem Pular; 35 cartões novos no Caderno, que abrem ao vencer cada fase, e revisão com até 5 perguntas começando pelo que o aluno acabou de aprender; 17 conquistas novas (42 no total) |
| 26/11/2026 | Web 22 | Botão "?" em todos os jogos com uma demonstração animada de como jogar (mãozinha que toca, segura e arrasta, com narração); volume de 0 a 10 para Sons, Música e Narração nas Configurações; sprites `musica-desligada` e `balao-desligado` com X |
| 26/11/2026 | Web 21 | Fases em todos os jogos (48 no total, seção 5.0.1), sorteadas sem repetir: temas de ciências, palavras, matemática (sólidos, quadro de dez, contas, extenso, dobro), lojas, museus, sítio e cidade, melodias que sobem e descem, quatro quadros e cenas do folclore; 43 sprites novos; revisão do conteúdo e das frases; 82 frases do mascote (com adivinhas e trava-línguas), notícias com dicas, desafios e datas do mês; 18 cartões novos no Caderno e 2 medalhas de fases |
| 26/11/2026 | Web 20 | Vídeo de apresentação na tela de ajuda (`midia/apresentacao.mp4`, em pé, com capa), caixa de vídeo vertical e aviso quando o arquivo não carrega |
| 26/11/2026 | Web 19 | Novo nome: App MM passou a se chamar Educatata (título da aba, descrição, rodapé das Configurações e documentos). A chave do `localStorage` continua `appmm-web-v1`, para não apagar o progresso |
| 26/11/2026 | Web 18 | Difícil mais desafiador em todos os jogos, com um passo de raciocínio a mais e sem pressa: pistas no Caça-palavras, duas palavras por grupo na Conexão, número de lados na Memória, combinar produtos no Mercadinho, dois pesos iguais na Balança, linhas e sequências ao contrário, parada no mapa, cores de três tintas, cenas com dois personagens e tarefas de duas etapas no Mundo digital |
| 26/11/2026 | Web 17 | Conquistas (23 medalhas, com aviso de conquista nova), Caderno com os conhecimentos de cada matéria e revisão espaçada, e tela de carregando ao abrir jogos e revisões; tela inicial com seis botões |
| 26/11/2026 | Web 16 | Cinco jogos novos, a partir de um estudo das escolas que apostam no lado criativo: Linha do tempo (história), Caminho no mapa (geografia), Maestro (música), Ateliê de cores (arte) e Teatro de histórias (teatro e literatura clássica); rostos de emoção do mascote; narração com voz de emoção; quatro músicas novas |
| 26/11/2026 | Web 15 | Novo jogo Mundo digital (tecnologia): ligar, bloquear e desligar de verdade, som, carregar a bateria, desligar o computador e abrir uma pasta, em celular, tablet e computador de pixel art; música "Samba dos 8 bits" |
| 26/11/2026 | Web 14 | Efeitos sonoros refeitos no tom da música: acerto que sobe um degrau a cada acerto da partida, erro gentil de madeira, moeda de três sininhos, fanfarra de vitória com o motivo do mascote e som próprio ao tocar no mascote |
| 26/11/2026 | Web 13 | Músicas compostas a partir de um estudo de trilhas de jogos: motivo do mascote em seis estilos (baião-pop, dórico, bossa, valsa, forró e swing), novos instrumentos e percussão sintetizados, camadas que entram com os acertos e com o coração do dia, música abaixando durante a narração e clique afinado no tom da música |
| 25/11/2026 | Web 12 | Frases do mascote baseadas em pesquisa (mentalidade de crescimento): 27 frases de esforço, curiosidades e cuidado, frase do momento ligada ao coração e à última partida, e cumprimento pela hora do dia |
| 25/11/2026 | Web 11 | Tela Jogos no estilo de console (carrossel com capa grande, setas, arrastar e título); nenhum emoji no app (acessórios, troféu, cadeado, selos, estrelas, vídeo e ícone da aba em pixel art); sem a dica de texto nas estatísticas; laço do mascote apoiado na cabeça |
| 25/11/2026 | Web 10 | Pixel art em todos os jogos: ícones dos jogos, letras e números em fonte pixel, figuras da memória, produtos, notas e moedas do Mercadinho e a balança inteira; dificuldades com barrinhas em vez de emojis; tela do vídeo com desenhos grandes e uma palavra |
| 25/11/2026 | Web 9 | Nova economia: menos moedas por partida, bônus do Médio e do Difícil, bônus da meta do dia, preços da loja cerca de 3,5 vezes maiores e a vitória mostrando de onde vieram as moedas; tudo em `ECONOMIA` |
| 25/11/2026 | Web 8 | Dificuldades Fácil, Médio e Difícil em todos os jogos, escolhidas na tela Jogos; recorde e estatísticas separados por dificuldade (os antigos contam como Médio) |
| 25/11/2026 | Web 7 | Mais conteúdo: 29 palavras no Caça-palavras, 12 conjuntos na Conexão, 10 figuras no Jogo da memória (6 sorteadas por partida) e 15 produtos no Mercadinho (6 sorteados por partida); alto-falante sem X quando os sons estão ligados; sombra do mascote em pixel art |
| 25/11/2026 | Web 6 | Arquivo renomeado para `index.html`; `README.md` e `PROMPTS.md`; CSS sem uso removido; confete só dentro do app; "Apagar progresso" mantém as escolhas de música, sons e narração; notícias paradas com "reduzir movimento" |
| 25/11/2026 | Web 5 | Mascote refeito em pixel art (64 × 64, gerado no código, com cores e acessórios); balão de fala em pixel art (9-slice); novo ícone de música |
| 23/11/2026 | Web 4 | Novos ícones em pixel art: engrenagem, ampulheta, estrela, LED, nota musical, alto-falante, balão e porta; animação da porta ao sair do jogo e ao fechar as configurações |
| 20/11/2026 | Web 3 | Nova tela inicial (wireframe de 20/11) sem barra de abas; telas com seta de voltar; sistema de vida diário com coração (vazio, ¼, ½, cheio); aba de notícias; Progresso com progresso diário, gráfico de 7 dias e estatísticas; primeiros sprites (moeda, corações, controle, Visual) |
| 18/11/2026 | Web 2 | Explicação antes da Conexão; fase de memorizar no Jogo da memória; arrastar no Mercadinho; provar antes de comprar; Progresso simplificado; vitória animada em etapas; música de fundo; tela do vídeo de ajuda com confirmação ao pular |
| 17/11/2026 | Web 1 | Primeira versão web: os cinco minijogos, mascote em SVG, loja, progresso salvo, sons e narração |
| 13/11/2026 | Processing | Protótipo em Java/Processing feito sem IA, só com o Mercadinho (entregue) |

## Declaração de uso de IA

Durante a preparação deste documento de especificação e da versão web do aplicativo, o(s) autor(es) usaram Claude Opus 5.5 e Claude Fable 5.2 para gerar e otimizar o código a partir das especificações, wireframes e pedidos de alteração da equipe, gerar em pixel art o mascote, o balão, a sombra do mascote, o alto-falante sem o X e os desenhos dos jogos, ampliar o conteúdo dos jogos, testar o aplicativo e redigir esta especificação com base no código. Os sprites em pixel art (moeda, corações, ícones e porta) foram desenhados pela equipe. Após usar essas ferramentas, o(s) autor(es) revisaram e editaram o conteúdo conforme necessário e assumem total responsabilidade pelo conteúdo.
