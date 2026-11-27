# Educatata — Aprenda jogando

Aplicativo educacional gamificado para alunos do 2º e 3º ano do ensino fundamental. O aluno cuida de um mascote e joga minijogos de matemática, português, ciências, história, geografia, artes (música, artes visuais e teatro) e tecnologia para ganhar moedas, XP e estrelas. O foco é lógica e atenção: cada partida mostra quantas tentativas o aluno usou, e ele compara o resultado com as próprias partidas anteriores.

**Acesse:** [hamiltonlicheski.github.io/app-mm](https://hamiltonlicheski.github.io/app-mm/) (o endereço ainda usa o nome antigo do app, App MM)

Projeto da disciplina **Experiência Criativa: Explorando Computação e Inteligência Artificial**, Ciência da Computação, PUCPR, 2026/2.

## Minijogos

| Jogo | Matéria | O que o aluno faz |
| --- | --- | --- |
| Caça-palavras | Ciências | Encontra palavras de ciências numa grade de letras |
| Conexão de palavras | Português | Liga cada palavra ao seu grupo: substantivo, verbo ou adjetivo |
| Jogo da memória | Matemática | Memoriza as cartas e junta cada figura geométrica ao seu nome |
| Mercadinho | Matemática | Arrasta notas e moedas até a mesa para pagar compras e dar troco |
| Balança | Matemática | Escolhe o peso que falta para equilibrar a balança |
| Mundo digital | Tecnologia | Aprende a usar celular, tablet e computador: ligar, bloquear, desligar de verdade, som, carregar e abrir pastas |
| Linha do tempo | História | Põe objetos do dia a dia na ordem em que surgiram, do mais antigo ao mais novo |
| Caminho no mapa | Geografia | Leva o mascote pelo mapa do bairro, desviando de obstáculos; no Difícil, com os pontos cardeais |
| Maestro | Música | Ouve uma sequência de notas nos sinos coloridos e toca igual |
| Ateliê de cores | Arte | Mistura tintas (cores primárias e secundárias) para pintar um quadro |
| Teatro de histórias | Teatro | Escolhe a emoção dos personagens de contos clássicos, e o mascote interpreta no palco |

Todos os jogos têm três dificuldades (Fácil, Médio e Difícil), cada uma com o seu recorde. O Difícil não tem pressa: ele pede um passo a mais de raciocínio, como achar a palavra por uma pista, contar os lados das figuras, combinar preços, fazer a sequência ao contrário ou ver uma cena pelos olhos de dois personagens.

Cada jogo também tem **fases**, temas que mudam o conteúdo da partida: 48 no total. Alguns exemplos são bichos do Brasil e corpo humano no Caça-palavras, sólidos geométricos e quadro de dez na Memória, papelaria e loja de brinquedos no Mercadinho, sítio e cidade no mapa, fundo do mar no Ateliê e o folclore brasileiro no Teatro. As fases saem sem repetir até o aluno conhecer todas, então dá para jogar muitas vezes e encontrar coisas novas.

A ideia é o aluno descobrir cada jogo sozinho, por curiosidade. Se precisar, o botão **?** no canto da tela do jogo mostra como jogar: uma mãozinha em pixel art toca, segura e arrasta numa versão pequena do jogo, e cada passo é lido em voz alta. Nas **Configurações**, dá para ligar e desligar e também aumentar ou diminuir o volume dos sons, da música e da narração.

A **narração** começa desligada. Quem precisa liga pelo botão **Ler em voz alta**, já na primeira tela, ou nas Configurações. Ligada, ela lê cada tela ao abrir, as instruções, os acertos, os erros e as dicas dos jogos, as janelas e os avisos, para quem ainda não lê ou lê devagar. O botão do alto-falante ao lado da instrução lê a instrução mesmo com a narração desligada.

O aluno também tem um **Caderno**, com 91 cartões do que aprendeu em cada matéria (cada fase vencida abre o seu cartão) e revisões rápidas espaçadas que começam pelo que acabou de aprender, e uma tela de **Conquistas**, com 43 medalhas que valorizam o esforço, a melhora e as fases vencidas.

No **Visual**, além das cores e acessórios comuns, há **coleções especiais**: easter eggs inspirados nas pessoas que fizeram parte da criação do Educatata (amigos, família, professores e a turma de Ciência da Computação da PUCPR). São 20 coleções, com 16 cores e 98 acessórios em pixel art, mostradas como capas em que o mascote veste o visual de cada uma. O mascote pode até segurar coisas, como um contrabaixo, um microfone ou um dado de 20 lados, e até aparecer pensando numa frase.

Na tela inicial, o mascote fica num cantinho de estudo em pixel art, com janela, estante de livros, planta e um tapete.

O app pode ser usado com o dedo, com mouse ou com teclado: o que está selecionado ganha uma borda azul, como nos consoles, e uma setinha avisa quando dá para descer a tela.

Além dos jogos, o app tem um mascote que muda de cor e usa acessórios comprados com moedas, níveis, recordes, o coração do dia (meta de 3 vitórias), um gráfico da semana, narração em português e onze músicas próprias (uma para a tela inicial e uma para cada jogo, menos o Maestro), compostas no código a partir de um estudo de trilhas de jogos.

## Como usar

- **No celular ou no computador:** abra o endereço acima.
- **Sem internet:** baixe o `index.html` e abra no navegador. O app é um único arquivo, sem bibliotecas e sem servidor. Para ver o vídeo de apresentação sem internet, baixe também a pasta `midia/` e deixe-a ao lado do `index.html`. Ao abrir o vídeo, o app pergunta se o aluno precisa da versão com Libras.

O progresso fica salvo só no navegador do aparelho (`localStorage`). O app não pede nome, não cria conta e não envia dados para lugar nenhum.

## Arquivos

| Arquivo | O que é |
| --- | --- |
| `index.html` | O aplicativo inteiro: HTML, CSS e JavaScript |
| `midia/` | Vídeo de apresentação do app em duas versões, sem Libras (`apresentacao.mp4`, em pé) e com Libras (`apresentacao-libras.mp4`, deitado), e as capas, mostrados na tela "Como usar" |
| [`Especificação técnica — Educatata Web.md`](<Especificação técnica — Educatata Web.md>) | Como o app funciona, regras do código e pendências |
| [`Estudo musical — Educatata.md`](<Estudo musical — Educatata.md>) | Estudo de trilhas de jogos e o porquê de cada música e de cada efeito sonoro |
| [`Estudo de edição — Educatata.md`](<Estudo de edição — Educatata.md>) | Como o vídeo de apresentação foi editado, o roteiro e a versão com Libras |
| `visuais/` | Sprites em pixel art (PNG) desenhados pela equipe. Em `Gerados no código/` ficam os dois feitos pela IA |
| `pixelart/` | Arquivos originais dos sprites no Pixilart (`.pixil`) |

## Equipe

- Hamilton Licheski Filho
- Emmanuel Junior Cabrera dos Santos
- Arthur Machado Santos
- Enzo Sampietro Rinaldi
- João Luiz Gelas Olea

## Declaração de uso de IA

Durante o desenvolvimento da versão web do aplicativo e da sua documentação, o(s) autor(es) usaram Claude Opus 5.5 e Claude Fable 5.2 para gerar e otimizar o código a partir das especificações, wireframes e pedidos de alteração da equipe, gerar em pixel art o mascote, o balão, a sombra do mascote, o alto-falante sem o X e os desenhos dos jogos (produtos, dinheiro, balança, figuras, ícones e a fonte de letras e números), compor as músicas e os efeitos sonoros e redigir o estudo musical que os embasa, testar o aplicativo e redigir a especificação técnica e este README. Os sprites em pixel art (moeda, corações, ícones e porta) foram desenhados pela equipe. Após usar essas ferramentas, o(s) autor(es) revisaram e editaram o conteúdo conforme necessário e assumem total responsabilidade pelo conteúdo.
