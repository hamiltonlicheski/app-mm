# Estudo musical — Educatata

Este estudo explica de onde vieram as músicas e o som de clique do Educatata. Primeiro olhamos para trilhas de jogos que todo mundo lembra (Mario, Zelda, Banjo-Kazooie, Animal Crossing e outras) e para o que a pesquisa diz sobre crianças e música. Depois transformamos o que aprendemos em regras e compusemos, a partir delas, uma música para as telas do mascote e uma para cada minijogo.

As músicas não são arquivos de áudio: estão escritas dentro do `index.html` e o navegador as toca na hora (seção 9 da especificação técnica). Assim o app continua num arquivo só e funciona sem internet.

## 1. O que os jogos marcantes ensinam

### Super Mario Bros. (1985, Koji Kondo): a música segue o jogo

Kondo contou que fazia a música jogando: o ritmo dependia de como Mario e os inimigos se moviam, e a primeira versão da música da superfície foi descartada. Quando o jogo mudou, ele acelerou e refez o tema até combinar com os pulos. A melodia é sincopada (cai fora do tempo forte), com toque de ritmos latinos, o que dá a sensação de "quicar". O jogo também usa a música como aviso: quando o tempo está acabando, toca um alerta e a música acelera.

Outra lição vem da limitação do console: o NES tinha só cinco canais de som. Por isso cada música separa bem as funções: uma melodia, um baixo, um acompanhamento e uma percussão. Nada fica embolado.

**Para o Educatata:** o estilo e o andamento de cada música vêm do jogo (calmo para memorizar, animado para mexer no dinheiro). Cada música tem as quatro funções bem separadas.

### The Legend of Zelda (1986) e Ocarina of Time (1998): melodias curtas que viram lembrança

Kondo queria usar o "Bolero" de Ravel no primeiro Zelda. Descobriu, quase no fim do projeto, que a peça ainda tinha direitos autorais, e escreveu o tema novo numa noite. A série também ficou conhecida por *jingles*: frases de poucos segundos que avisam "você achou um segredo" ou "você ganhou um item". Em Ocarina of Time, o jogador toca as músicas numa ocarina que só tem **cinco notas** (ré, fá, lá, si e ré agudo). Músicas feitas com tão poucas notas são fáceis de lembrar e de cantar.

**Para o Educatata:** o mascote tem uma frase musical curta, com poucas notas, que aparece em todas as músicas.

### Super Mario World (1990) e Super Mario 64 (1996): a música ganha camadas

Em Super Mario World, quando Mario monta no Yoshi, entram bongôs por cima da mesma música, sem cortar nada. Em Super Mario 64, a música "Dire, Dire Docks" ganha instrumentos conforme o jogador nada para outras partes da fase: as cordas só tocam debaixo da água e a bateria só na caverna.

**Para o Educatata:** cada acerto acrescenta uma camada à música do jogo. A criança ouve que está avançando sem precisar ler nada.

### Banjo-Kazooie (1998, Grant Kirkhope): um tema, muitas roupas

No castelo que liga os mundos do jogo, a música é sempre o mesmo tema, mas a orquestração muda quando Banjo chega perto da entrada de cada mundo. Kirkhope explicou que o jogo aumenta e diminui o volume de canais diferentes quando o personagem cruza linhas invisíveis. Um estudo da UNIRIO descreve essas versões como reconfigurações do tema, que mantêm a melodia e a harmonia e trocam os instrumentos.

**Para o Educatata:** esta é a ideia central. Todas as músicas são variações do motivo do mascote, cada uma "vestida" com o estilo do seu jogo. Isso dá unidade ao app (tudo soa como o mundo do mascote) e variedade (cada jogo tem cara própria).

### Animal Crossing (2001 em diante, Kazumi Totaka e equipe): música para morar

A música da cidade muda a cada hora do dia real. Quando chove, a melodia continua a mesma e só os instrumentos mudam. São músicas calmas, com acordes de jazz, feitas para ouvir por muito tempo sem cansar.

**Para o Educatata:** a música da tela inicial muda com o coração do dia (as vitórias de hoje), uma ligação com a rotina real da criança. As músicas das telas onde a criança fica mais tempo são calmas e com acordes suaves.

### WarioWare (2003): o tempo é contado em batidas

Nos microjogos, o tempo-limite é medido em batidas, e o "Speed Up!" acelera tudo junto. Funciona, mas cria pressa e tensão.

**Para o Educatata:** decidimos **não** acelerar a música. O foco do app é lógica e atenção (README), e pressa atrapalha quem está aprendendo. A recompensa vem das camadas, não da velocidade.

### Peggle 2 (2013): efeitos que fazem parte da música

Cada pino atingido toca uma nota mais aguda de uma escala, e essa escala muda para combinar com o acorde da música que está tocando. O jogador "compõe" sem perceber, e o som de acerto nunca briga com a trilha.

**Para o Educatata:** o clique é afinado no tom da música da tela, e toques rápidos seguidos sobem uma escadinha de notas.

### Minecraft (2011, C418): menos é mais

A música é esparsa, repetitiva de propósito, e toca em momentos aleatórios com silêncios entre as faixas. Jogadores reclamam dos silêncios longos, o que mostra que existe um limite.

**Para o Educatata:** as músicas são suaves e têm partes com pouca coisa tocando. A camada 0 já é completa, mas leve.

## 2. O que a pesquisa diz sobre crianças e música

- **As primeiras notas que as crianças cantam.** O método Kodály começa pelo intervalo sol-mi, porque ele aparece na maioria das canções infantis mais simples, e trabalha por muito tempo só com a escala pentatônica (dó, ré, mi, sol, lá). Os semitons (mi-fá e si-dó) são difíceis de afinar para crianças.
- **Extensão da voz.** Para 1º a 3º ano (6 a 8 anos), uma referência de educação musical indica a extensão de dó a dó (uma oitava, a partir do dó central). Para os menores, recomenda-se ainda menos (ré a lá).
- **Música enquanto lê.** Uma meta-análise de Vasilev, Kirkby e Angele (2018) achou um efeito negativo pequeno da música de fundo sobre a leitura, maior quando a música tem letra. Outros estudos confirmam que letra atrapalha mais do que música instrumental. Quase todos os estudos são com adolescentes e adultos, então tratamos isso como cautela.
- **Erro não é castigo.** A especificação do app já pede que os erros nunca sejam punitivos. Na música isso vale também: nada de sons de "fracasso" na trilha, e nenhuma camada é tirada quando a criança erra.

## 3. As dez regras das músicas do Educatata

1. **Uma identidade só.** Todas as músicas nascem do motivo do mascote (Banjo-Kazooie).
2. **A música combina com o jogo.** Andamento e estilo vêm do que a criança faz em cada jogo (Mario).
3. **A música reage.** Acertar acrescenta camadas; errar não tira nada (Yoshi, Dire Dire Docks).
4. **Recompensa sem pressa.** O andamento nunca acelera (contra-exemplo do WarioWare).
5. **Melodia cantável.** Motivo pentatônico, com o intervalo sol-mi e dentro de uma oitava (Kodály, ocarina de Zelda).
6. **Sem letra e abaixo da voz.** Só instrumentos, volume baixo, e a música abaixa quando a narração fala (pesquisa sobre leitura).
7. **Quatro funções claras.** Melodia, baixo, acompanhamento e percussão, sem embolar (limite do NES).
8. **Loops longos e macios.** De 33 a 40 segundos, com parte A e parte B, sem sons ásperos (Animal Crossing, Minecraft).
9. **Efeitos no tom da música.** O clique é afinado na tonalidade da tela (Peggle).
10. **Sotaque brasileiro.** Kondo usou ritmos latinos no Mario; aqui usamos baião, bossa nova e forró. É o que deixa a trilha única.

## 4. O motivo do mascote: "pula, pula, pulão!"

O motivo imita o mascote pulando. Uma nota grave fica quicando (o chão), enquanto as notas de cima sobem degrau por degrau, até o pulo grande:

```
dó   sol  ré   sol  mi   sol  SOL — | mi — dó — ré — — —
C5   G4   D5   G4   E5   G4   G5    | E5    C5    D5
pu - la,  pu - la,  pu - la,  pu-LÃO!  (pergunta)
```

- Usa só notas da escala pentatônica, então é fácil de cantar e combina com qualquer acompanhamento.
- O "sol" grave que volta sempre é o intervalo sol-mi do método Kodály, de cabeça para baixo.
- A primeira frase termina em ré, uma pergunta. A segunda termina em dó, a resposta ("pula, pula, pulão... e chega!").
- Na música da tela inicial, os pulos ficam cada vez mais altos: sol, depois lá, depois si e, por fim, o dó agudo. É o mascote tentando de novo e indo mais longe, a mesma mensagem das frases dele (mentalidade de crescimento, seção 4.3.1 da especificação).

O motivo foi composto para o Educatata e não copia melodia de nenhum jogo. Dos jogos estudados vieram só as técnicas.

## 5. As onze músicas

| Música | Onde toca | Estilo | Tom | Andamento | Loop |
| --- | --- | --- | --- | --- | --- |
| Pula, pula, pulão | Tela inicial e menus | Baião-pop | Dó maior | 104 bpm | 37 s |
| Jardim das descobertas | Caça-palavras | Exploração em modo dórico | Lá dórico | 96 bpm | 40 s |
| Bossa das palavras | Conexão de palavras | Bossa nova | Fá maior | 96 bpm | 40 s |
| Valsa da caixinha | Jogo da memória | Valsa de caixinha de música (3/4) | Ré maior | 116 bpm | 37 s |
| Forró da feirinha | Mercadinho | Forró pé-de-serra | Sol maior | 116 bpm | 33 s |
| Swing da gangorra | Balança | Jazz em swing | Si bemol maior | 100 bpm | 38 s |
| Samba dos 8 bits | Mundo digital | Samba com som de videogame (chip) | Mi maior | 104 bpm | 37 s |
| Ciranda do tempo | Linha do tempo | Ciranda | Dó menor | 96 bpm | 40 s |
| Marchinha do explorador | Caminho no mapa | Marchinha | Lá maior | 112 bpm | 34 s |
| Chorinho das cores | Ateliê de cores | Choro | Mi bemol maior | 100 bpm | 38 s |
| Toada do mamulengo | Teatro de histórias | Banda de pífano | Ré menor | 96 bpm | 40 s |

As camadas funcionam assim:

- **Nos jogos:** a camada 0 toca desde o começo; a 1 entra no 1º acerto; a 2 entra no 2º acerto. Uma camada nova sempre entra no começo de um compasso, para não atropelar a música. Trocar de jogo volta para a camada 0.
- **Na tela inicial:** a música segue o coração do dia. Coração vazio = camada 0; uma ou duas vitórias = camada 1; coração cheio = camada 2, a banda inteira.

### Pula, pula, pulão (tela inicial)

É o tema do mascote, a versão mais pura do motivo. O baixo faz o "tum . . tum . . tum" do baião (o mesmo desenho do tresillo latino), e o cavaquinho bate nos contratempos, o que dá o balanço que Kondo buscava no Mario.

- **Camada 0:** kalimba com o motivo, baixo e cavaquinho.
- **Camada 1:** bumbo, palmas no 2 e no 4 (convite para bater palma junto) e chocalho.
- **Camada 2:** a flauta canta junto na parte B, uma terça abaixo, como um amigo; um sininho brilha no ponto mais alto de cada pulo.

### Jardim das descobertas (Caça-palavras, ciências)

As palavras do jogo são do jardim e do céu: semente, folha, abelha, chuva, estrela. A música é um passeio de explorador. O modo dórico (lá menor com fá sustenido) soa curioso sem ficar triste, e a ocarina na parte B é uma homenagem às músicas de exploração de Zelda. A melodia é espaçada, porque a criança precisa ler a grade de letras.

- **Camada 0:** marimba com o motivo em lá, ocarina na parte B, baixo pizzicato e violão.
- **Camada 1:** chocalho de "chuvinha" e bloco de madeira.
- **Camada 2:** passarinhos e brilhos nos pulos do motivo.

### Bossa das palavras (Conexão de palavras, português)

No jogo, a criança liga cada palavra ao seu grupo. Na música, dois instrumentos conversam: o vibrafone pergunta (o motivo em fá) e a flauta responde no compasso seguinte, sempre ligando a frase de um à do outro. A bossa nova é calma como a música de menu do Mii Channel, que muita gente lembra, e os acordes com sétima dão o ar de Animal Crossing. O acorde de si bemol menor na parte B é o "tempero" da bossa.

- **Camada 0:** vibrafone, flauta, violão com a batida da bossa e baixo.
- **Camada 1:** aro de caixa com a clave da bossa e chocalho.
- **Camada 2:** cordas suaves por baixo.

### Valsa da caixinha (Jogo da memória, matemática)

Memorizar pede calma. A música é uma valsa de caixinha de música: previsível, repetitiva no bom sentido e sem surpresas que roubem a atenção. Em três tempos, o motivo fica naturalmente "tique-tique-tique, pulo". Na parte B, a melodia desce em degraus iguais (si-lá-sol, fá#-mi-ré, sol-fá#-mi), um padrão fácil de prever.

- **Camada 0:** caixinha de música, harpa no "pa-pá" da valsa e baixo no 1.
- **Camada 1:** cordas suaves.
- **Camada 2:** um sininho a cada dois compassos.

As camadas mudam pouco a música, de propósito: no jogo da memória, menos é mais.

### Forró da feirinha (Mercadinho, matemática)

O Mercadinho é uma feira, e feira no Brasil tem forró. É o trio clássico: sanfona, zabumba e triângulo. A sanfona toca o motivo em sol e usa o fá natural (a sétima menor), o tempero nordestino das músicas de Luiz Gonzaga. A parte B imita o pregão do feirante, com notas repetidas ("olha a laranja!"). Aqui a criança arrasta dinheiro e lê pouco, então a música pode ser a mais animada do app, sem passar de 116 bpm.

- **Camada 0:** sanfona, baixo e zabumba.
- **Camada 1:** triângulo (fechado e aberto, o "tchi-ki-tchin") e o bacalhau da zabumba.
- **Camada 2:** segunda sanfona em terças, como nos trios de forró, e o pífano chamando para a próxima parte.

### Swing da gangorra (Balança, matemática)

Para pensar no peso que falta, um jazz em swing, sem pressa. Dois blocos de madeira fazem tique-taque, um na esquerda e outro na direita, como os pratos de uma balança. No motivo, as notas sobem na primeira frase e descem na segunda, como uma gangorra. O baixo anda nota por nota (o "walking bass" do jazz), e o si natural do acorde de sol com sétima é uma surpresa, como os cromatismos do Mario.

- **Camada 0:** piano elétrico com o motivo, baixo andando e tique-taque.
- **Camada 1:** vassourinha na caixa e prato de condução no balanço do swing.
- **Camada 2:** clarinete com as notas-guia (a terça e a sétima de cada acorde).


### Samba dos 8 bits (Mundo digital, tecnologia)

O jogo ensina a usar celular, tablet e computador, então o motivo do mascote veste o som mais "tecnológico" da história dos games: a onda quadrada dos videogames de 8 bits (o limite do NES, seção 1). Para manter o sotaque brasileiro (regra 10), por baixo do chip toca um samba: o surdo marca o 2 e o 4, o tamborim (aro) faz a síncope e o chocalho mantém o balanço. Em mi maior e a 104 bpm, os pulos sobem (si, dó#, lá) como na música da tela inicial, e o arpejo rápido em semicolcheias é o acompanhamento típico dos jogos antigos.

- **Camada 0:** chip com o motivo, baixo e arpejo de chip bem baixinho.
- **Camada 1:** surdo, tamborim e chocalho.
- **Camada 2:** sininho no ponto mais alto de cada pulo.
- **Instrumento novo:** `chip`, duas ondas quadradas (a segunda uma oitava acima e bem fraca) com filtro em 2.400 Hz, para o chiado não ficar áspero (regra 8).
- **Mixagem:** medida com `OfflineAudioContext`, como as outras: −31,7 dB na camada 0 e −31,1 dB na camada 2, com picos de 0,11 a 0,17 (volume final 0,125).

### Ciranda do tempo (Linha do tempo, história)

A ciranda é a dança de roda de Pernambuco: todos de mãos dadas, girando devagar. A roda que gira é o tempo que passa, o tema do jogo. Em dó menor e a 96 bpm, a flauta canta o motivo do mascote em modo menor (dó, sol, ré, sol, mi bemol...), e o zabumba pesa no primeiro tempo, como o passo da ciranda.

- **Camada 0:** flauta com o motivo, baixo e violão.
- **Camada 1:** zabumba e ganzá (chocalho).
- **Camada 2:** sininho no alto de cada pulo.

### Marchinha do explorador (Caminho no mapa, geografia)

Para sair andando pelo bairro, uma marchinha, o ritmo das bandas de rua e do carnaval. Lá maior, a 112 bpm: o baixo faz o "pum-pá" da marcha, o cavaquinho bate nos contratempos e a caixa marca o passo. O clarinete leva o motivo.

- **Camada 0:** clarinete, baixo e cavaquinho.
- **Camada 1:** bumbo e caixa.
- **Camada 2:** sininho.

### Chorinho das cores (Ateliê de cores, arte)

O choro é a música de quem cria com capricho, cheia de detalhes, como uma pintura. Mi bemol maior, a 100 bpm: a flauta sobe e desce como um pincel, o cavaquinho faz a "levada" e o baixo anda pelas notas do acorde, lembrando a baixaria do violão de sete cordas. O pandeiro aparece como aro e chocalho.

- **Camada 0:** flauta, baixo e cavaquinho.
- **Camada 1:** pandeiro (aro e chocalho).
- **Camada 2:** sininho.

### Toada do mamulengo (Teatro de histórias, teatro)

O mamulengo é o teatro de bonecos de Pernambuco, e as bandas de pífano tocam nas ruas do Nordeste. Ré menor, a 96 bpm, para contar histórias: o pífano leva o motivo, o zabumba faz o baião lento e o triângulo e a caixa entram com os acertos.

- **Camada 0:** pífano, baixo e zabumba.
- **Camada 1:** triângulo e caixa.
- **Camada 2:** sininho.

### Maestro (música): o silêncio também é escolha

O Maestro é o único jogo sem música de fundo (`semMusica`). A criança precisa ouvir a altura de cada nota dos sinos, e uma música por baixo atrapalharia. As notas são tocadas na kalimba, o instrumento do mascote, e só usam a pentatônica (dó, ré, mi, sol), como o motivo.

**Mixagem das novas:** medidas com `OfflineAudioContext`, como as outras: entre −30,5 e −31,3 dB, com picos de 0,09 a 0,15, e loops de 34 a 40 s.
## 6. O clique e os outros efeitos

### O clique

O clique é o som mais repetido do app (26 lugares no código), então precisa ser o mais discreto e o menos cansativo. Ele é uma "bolhinha de madeira":

- **Corpo:** uma nota que sobe rapidinho de 3/4 da altura até a nota certa ("blup"). Subir soa positivo; descer soaria como "ops".
- **Brilho:** uma parcial quatro vezes mais aguda e curtíssima, como uma baqueta batendo em madeira.
- **Duração:** menos de um décimo de segundo, sem ataque áspero.
- **Afinação:** toca a quinta nota do tom da música da tela (o sol, em dó maior), que combina com quase todo acorde. Se a música estiver desligada, usa o tom da música que tocaria ali.
- **Escadinha:** toques rápidos seguidos (menos de 0,45 s entre eles) sobem as cinco notas da pentatônica (sol, lá, dó, ré, mi) e recomeçam. Depois de uma pausa, volta para a primeira nota. É a ideia do Peggle em versão de bolso: dois cliques rápidos, como virar duas cartas na memória, soam como uma pergunta e uma resposta.
- **Volume:** pico de 0,13, abaixo do som de acerto (0,16), porque toca muito mais vezes.

### Acerto, erro, moeda, mascote e vitória

Os outros efeitos seguem as mesmas regras do clique. São frases curtinhas, tocadas com os instrumentos das músicas e no tom da música da tela. Na tela de vitória, que não tem música, eles usam o tom do jogo que acabou. Assim nenhum efeito "briga" com a trilha (Peggle), e cada tela soa como um lugar só. Os exemplos abaixo estão em dó maior, o tom da tela inicial.

| Efeito | Quando toca | Como soa | Por quê |
| --- | --- | --- | --- |
| Acerto | Resposta certa | Kalimba subindo, sol → dó, com um sininho por cima. A cada acerto da partida, o par sobe um degrau da pentatônica (lá → ré, dó → mi, ré → sol, mi → lá) | Subir soa como conquista. A escada mostra que a criança está avançando, como os pinos do Peggle, e recomeça a cada partida |
| Erro | Resposta errada | "Toc-toc" de madeira abafada, descendo um degrau até a tônica (ré → dó). Nos tons menores, desce o intervalo sol-mi | Antes era uma onda quadrada grave, quase uma buzina. Agora é um "quase!" baixinho que termina na nota de casa, sem som de castigo (mentalidade de crescimento) |
| Moeda | Dinheiro na mesa, compra e fim da contagem de moedas | Três sininhos rápidos subindo (sol, dó, mi) | O antigo (si → mi) lembrava muito a moeda do Mario. O "tlim" de três notas é do Educatata e continua soando como recompensa |
| Mascote | Tocar no mascote | O pulo do motivo na kalimba: sol grave, dó, SOL agudo | Antes o mascote usava o som de moeda. Agora ele "fala" com o próprio tema enquanto pula, como os personagens que têm um motivo musical |
| Vitória | Tela de vitória e subida de nível | Fanfarra de 1,4 s: o motivo inteiro bem rápido, o pulo e um acorde dedilhado com baixo e sininhos | É o "pula, pula, pulão" completo, a recompensa final. Termina sempre em acorde maior: depois do Caça-palavras, em lá dórico, a vitória "clareia" para lá maior |

Volumes (pico): erro 0,08, clique 0,13, moeda 0,15, acerto e mascote 0,16, vitória entre 0,21 e 0,29. A vitória é a mais forte porque toca sozinha, sem música por baixo. O erro é o mais baixo de todos.

## 7. Mixagem

- **Volume:** medimos cada música com o `OfflineAudioContext` do navegador (renderizando a música sem tocar) e ajustamos o volume final para ficar perto de −31 dB RMS (entre −30 e −33 dB, conforme o trecho), com picos entre 0,10 e 0,17, no nível do som de acerto (0,16). Os jogos de leitura e de memória ficam um pouco mais baixos.
- **Equilíbrio:** a melodia fica na frente; o baixo, 4 a 5 dB abaixo; acordes e percussão, de 12 a 24 dB abaixo.
- **Narração:** enquanto o app fala, a música desce para 35% e depois volta. Se o navegador não avisar o fim da fala, ela volta sozinha depois de um tempo proporcional ao tamanho da frase.
- **Espaço:** cada instrumento tem um lugar no estéreo (`pan`) e um pouco de eco de sala (`eco`), o que deixa a mistura mais clara em fones.

## 8. Como mexer nas músicas

Tudo fica em `MUSICAS`, no `index.html`, numa notação de texto (a legenda completa está no comentário acima de `MUSICAS`):

```
{ inst: 'kalimba', vol: .5, eco: .25, notas: `C5 G4 D5 G4 E5 G4 G5 - | E5 - C5 - D5 - - - | ...` }
```

- Cada símbolo é um passo (uma colcheia, se `passos` for 2): `C5` é uma nota, `-` estica a nota anterior, `.` é pausa e `_` é um compasso inteiro de silêncio.
- Em faixas de acompanhamento, `1 3 5 7 8` são notas do acorde do momento e `A` é o acorde inteiro. Assim, trocar os `acordes` da música rearmoniza o baixo e o violão sozinhos.
- `camada` diz quando a faixa entra (0, 1 ou 2).
- Para testar o volume depois de mudar algo, renderize a música com `OfflineAudioContext` usando `montarMusica(ctx, nome)` e `tocarTique()`, como está descrito na seção 9 da especificação.

## 9. Limites e próximos passos

- Os instrumentos são sintetizados, sem gravações. Soam bem em fones e caixas, mas não como instrumentos de verdade. Se a equipe gravar versões reais, basta preencher `ARQUIVOS_MUSICA` (perde-se o efeito das camadas).
- A mixagem foi medida no computador. Vale ouvir num celular de verdade, onde graves abaixo de uns 150 Hz quase não aparecem.

## Fontes

- Super Mario Bros.: [Super Mario Bros. theme (Wikipedia)](https://en.wikipedia.org/wiki/Super_Mario_Bros._theme); [entrevista de Koji Kondo à NHK, traduzida pela Nintendo Everything](https://nintendoeverything.com/super-mario-bros-composer-on-making-of-ground-theme/); [Slate, sobre a trilha de Kondo](https://www.slate.com/articles/arts/culturebox/2015/06/koji_kondo_s_super_mario_bros_video_game_score_and_the_story_of_his_game.html).
- Zelda e o Bolero: [Nintendo Life](https://www.nintendolife.com/news/2016/11/an_intriguing_tale_of_how_the_legend_of_zeldas_iconic_opening_song_almost_never_happened).
- Ocarina of Time: [Zelda Dungeon Wiki, músicas da ocarina](https://zeldadungeon.net/wiki/Ocarina_of_Time_Songs).
- Super Mario World (bongôs do Yoshi): [Super Mario Wiki](https://www.mariowiki.com/Ground_BGM_(Super_Mario_World)).
- Super Mario 64: [Super Mario Wiki, "Dire, Dire Docks"](https://www.mariowiki.com/Dire,_Dire_Docks_(theme)).
- Banjo-Kazooie: [Kotaku](https://kotaku.com/i-love-how-banjo-kazooie-s-music-changes-as-i-play-1845544436); [artigo da Revista Debates (UNIRIO)](https://seer.unirio.br/revistadebates/article/download/14212/13642/82977).
- Animal Crossing: [Dazed Digital](https://www.dazeddigital.com/music/article/48988/1/going-deep-on-the-blissful-brilliance-animal-crossings-soundtrack-new-horizons).
- WarioWare: [Super Mario Wiki, "Speed Up!"](https://mariowiki.com/Speed_Up!).
- Peggle: [Audiogang, Peggle 2 e a música adaptativa](https://www.audiogang.org/peggle-2-storytelling-through-adaptive-music/); [Audiokinetic, Peggle Blast](https://blog.audiokinetic.com/en/peggle-blast-peg-hits-and-the-music-system).
- Minecraft: [Vice, entrevista com C418](https://www.vice.com/en/article/minecrafts-composer-explains-why-the-music-is-so-weird); [Kill Screen](https://www.killscreen.com/quiet-importance-minecraft-soundtrack/).
- Método Kodály: [Dynamic Music Room](https://dynamicmusicroom.com/kodaly-method-first-interval); [sequência Kodály da Wichita State University (PDF)](https://wichita.edu/academics/fine_arts/music/kodaly/documents/G_2009_Wichita_A_KODALY_SEQUENCE_FINAL.pdf).
- Voz infantil: [Music and the Child, cap. 9 (Lumen Learning)](https://courses.lumenlearning.com/suny-music-and-the-child/chapter/chapter-9-music-and-the-older-child-3/); [ChoralNet](https://choralnet.org/archives/578765).
- Música e leitura: Vasilev, M. R.; Kirkby, J. A.; Angele, B. (2018). *Auditory distraction during reading: a Bayesian meta-analysis of a continuing controversy*. Perspectives on Psychological Science; [Should we turn off the music? Music with lyrics interferes with cognitive tasks (PMC)](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC10162369/).
