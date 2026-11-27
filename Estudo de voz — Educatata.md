# Estudo de voz — narração do Educatata

Este estudo explica como foi escolhida a voz que narra o aplicativo Educatata: a narração de apoio que lê as telas, os jogos e as janelas para quem ainda não lê ou lê devagar. Também descreve como a narração toca: uma fala por vez, que para na hora quando a criança troca de tela. A narração do vídeo de apresentação continua sendo a gravação da equipe.

## 1. O problema

- **No app**, a narração usava a voz do próprio aparelho (síntese de voz do navegador). Ela muda de um aparelho para outro e costuma soar robótica. A revisão do código também achou três jeitos de a voz continuar numa tela que já tinha sido fechada, ou de duas vozes tocarem juntas:
  - fechar as Configurações não parava a leitura delas;
  - fechar uma janela (Sair do jogo, comprar, medalha...) não parava a leitura da janela;
  - o vídeo da tela "Como usar" começava enquanto o app ainda lia a pergunta da Libras, e as duas narrações se sobrepunham.

  Em alguns sistemas (no Linux, por exemplo), a própria voz do aparelho também demora a obedecer quando o app manda parar.
- **Pedido da equipe:** uma voz de IA, a mais humana, fofa e acolhedora possível. Neste momento, não é possível usar a voz de uma pessoa real.

## 2. O que é uma voz "fofa e acolhedora" para crianças de 7 a 9 anos

O que as pesquisas dizem:

- **Fala dirigida a crianças.** É mais lenta, mais aguda e tem mais variação de entonação que a fala entre adultos (Shi et al., *Developmental Science*, 2022). Com crianças maiores, essas diferenças diminuem. Para 7 a 9 anos, então, o tom é de uma pessoa carinhosa, e não de "voz de bebê".
- **Velocidade.** Falar mais devagar ajuda a criança a entender e a lembrar. Lento demais, porém, atrapalha quem acompanha em tempo real (estudo sueco com crianças de 6 e 7 anos, 2014). Para comparar: crianças de 8 a 13 anos falam cerca de 2,9 sílabas por segundo, e adultos lendo em voz alta, de 5 a 6.
- **Sorriso na voz.** O sorriso se ouve, mesmo sem ver o rosto, porque muda a ressonância da boca. Uma voz levemente soprosa passa proximidade (solidariedade). Uma voz um pouco mais aguda é ouvida como mais amigável.

**Critérios usados na escolha:**

| Critério | Alvo |
| --- | --- |
| Quem fala | Mulher jovem adulta, doce e calma, "professora carinhosa"; nada de voz infantil ou de desenho |
| Altura da voz (F0 médio) | Cerca de 230 a 300 Hz |
| Entonação | Viva: variação de 10 semitons ou mais, com subida nas perguntas e no "Oi!" |
| Ritmo | Calmo, cerca de 4,5 a 5,5 sílabas por segundo, com pausa curta entre as frases |
| Pronúncia | Português do Brasil, com vogais nasais e "ão" corretos (o ponto fraco de vozes treinadas em inglês) |
| Ética e licença | Voz que não imita ninguém; roda no próprio PC (nenhum texto sai para a internet); licença que permite publicar |

## 3. Vozes avaliadas

| Opção | Por que sim | Por que não |
| --- | --- | --- |
| Voz do aparelho (atual) | Grátis e lê qualquer texto | Robótica, muda de aparelho para aparelho e, no Linux, não para quando deveria |
| ElevenLabs, Azure, Google (nuvem) | Muito naturais | Pagos, pedem conta e enviam o texto para fora |
| Kokoro (82M, Apache 2.0) | Rápido no processador | Treinado quase só em inglês: erra as vogais nasais e o "ão" do português (Santos et al., STIL 2026) |
| XTTS-v2 e F5-TTS | Bons para imitar uma voz | Licença não comercial e precisam de uma voz real para imitar |
| **Qwen3-TTS VoiceDesign** (Alibaba, Apache 2.0) | **Cria uma voz nova a partir de uma descrição em texto**, sem imitar ninguém | Muito lento no processador (cerca de 9 min para 13 s de fala) e não é afinado para o português do Brasil |
| **Chatterbox Multilingual V3 pt-BR** (Resemble AI, MIT) | **Afinado só para o português do Brasil**, com entonação expressiva | Precisa de uma voz de referência para imitar |

**A solução junta os dois:**

1. O Qwen3-TTS **criou uma voz original** a partir de uma descrição: uma mulher brasileira de uns 30 anos, de voz quente, suave e doce, de tom médio (adulta, não infantil), que fala com calma e clareza, sorrindo, como uma professora gentil lendo para os alunos.
2. O Chatterbox pt-BR **fala todas as frases** com essa voz, com a pronúncia e a entonação do português do Brasil.

## 4. Como as vozes foram comparadas

A IA não escuta. Por isso, cada amostra foi medida com ferramentas que rodam no próprio PC:

- **Transcrição automática** (Whisper large-v3-turbo). O texto ouvido é comparado com o esperado, o que mostra palavras trocadas, cortadas ou mal pronunciadas.
- **Medidas da voz:** altura média (F0), variação da entonação (em semitons), sílabas por segundo e pausas.
- **Espectrograma com a curva de entonação.** Uma curva lisa e reta é sinal de voz robótica; uma curva que sobe e desce é sinal de voz viva.
- **Semelhança de timbre** com a voz criada, pelo codificador de voz do próprio Chatterbox.

Vozes criadas no Qwen3-TTS, todas lendo o mesmo texto:

| Descrição | F0 médio | Variação | Ritmo | Transcrição | Resultado |
| --- | --- | --- | --- | --- | --- |
| Professora de educação infantil | 367 Hz | 10,4 st | 5,4 síl/s | 1 erro ("Ui") | Aguda demais, parece criança |
| Apresentadora de programa infantil | 437 Hz | 8,9 st | 5,0 síl/s | 2 erros | Voz de desenho animado |
| Voz aconchegante, soprosa | 447 Hz | 9,0 st | 5,2 síl/s | 1 erro | Aguda demais |
| Professora (descrição em português) | 349 Hz | 9,9 st | 5,8 síl/s | sem erro | Ainda aguda |
| Mulher adulta, descrição em português | 327 Hz | 8,4 st | 4,8 síl/s | sem erro | Pouca expressão |
| **Mulher adulta, doce, tom médio** | **251 Hz** | **11,4 st** | 5,9 síl/s | **sem erro** | **Escolhida** |

Para comparar, a gravação da equipe no vídeo tem 182 Hz, 20,7 st e 6,2 síl/s.

O Chatterbox pt-BR falando com a voz escolhida, em frases do app:

| Frase | Transcrição | F0 | Ritmo |
| --- | --- | --- | --- |
| "Oi! Que bom que você chegou. Vamos jogar?" | perfeita | 277 Hz | 3,9 síl/s |
| "Errar faz parte de aprender!" | perfeita (o "rr" sai certo) | 320 Hz | 5,2 síl/s |
| "Toque em um grupo e depois na palavra que combina com ele." | perfeita | 280 Hz | 5,8 síl/s |

A voz padrão do Chatterbox foi descartada: era grave (150 a 200 Hz), rápida (6,4 a 6,9 síl/s) e trocou "Errar" por "É raro".

## 5. Ajustes de produção

- **Velocidade no processador.** O PC não tem placa de vídeo, então tudo roda no processador. Três mudanças deixaram a geração cerca de 4 vezes mais rápida, sem mudar o som:
  - várias frases são geradas juntas ("em lote");
  - o decodificador usa só os primeiros 6 s da voz de referência (a semelhança de timbre ficou igual: 0,891 contra 0,878 com 10 s);
  - o decodificador faz 6 passos, em vez de 10, com int8 só nas camadas dele (o espectrograma fica igual).

  Um atalho foi testado e **descartado**: comprimir o modelo principal para int8. Ele trocou palavras ("os sons e a narração" virou "ou são só que eles estão aqui") e mudou o timbre.
- **Texto para a voz.** O texto mostrado na tela não muda. Só o texto enviado à voz é preparado:
  - números por extenso, com o gênero certo ("uma moeda", "duzentas moedas");
  - reais e centavos ("sete reais e cinquenta centavos");
  - contas ("sete mais cinco é igual a doze", "cinco mais quanto?");
  - palavras em maiúsculas viram minúsculas, para a voz não soletrar (ADUBO);
  - telefones de emergência dígito por dígito ("um, nove, três");
  - letras soltas pelo nome ("Pedro bê", "um agá") e siglas ("érre pê gê").
- **Cada frase é gerada uma vez.** O app divide cada fala em frases, e cada frase tem o seu áudio. Frases que se repetem usam o mesmo áudio.
- **Números.** "Você tem N moedas" (de 0 a 999), "Você usou N tentativas", "Nível N" e outros têm uma frase pronta para cada número. Acima de 999 moedas, a narração diz "mais de mil".
- **Revisão automática.** Cada áudio é transcrito e comparado com o texto. Os que erram são gerados de novo com outra semente. Também são conferidos a duração e os silêncios no meio da frase.
- **Marca d'água.** O Chatterbox põe em todo áudio uma marca d'água inaudível (PerTh), que identifica a fala como gerada por IA. Isso é bom para a transparência.

## 6. Como a narração toca no app

- **Um canal só.** Uma fala nova corta a anterior. Com `fila`, ela espera a fala atual terminar, como na instrução que vem depois do "Isso!". `calar()` para tudo na hora: ao trocar de tela, ao fechar uma janela ou as Configurações e quando o vídeo começa. Duas vozes nunca tocam juntas.
- **Fim de jogo e Teatro.** A tela de vitória espera a última fala do jogo terminar (por exemplo, o fato da Linha do tempo). No Teatro, primeiro vem o "Isso! Feliz!", depois o mascote interpreta a cena, sem um cortar o outro.
- **Pacotes de voz.** Os áudios (MP3 a 40 kbps) ficam em poucos arquivos, `midia/voz/voz-<grupo>.js`, um por tela ou jogo. Assim:
  - dá para enviar tudo pelo site do GitHub, que aceita até 100 arquivos por envio;
  - o app abre direto do computador, sem servidor;
  - o pacote de um jogo já começa a carregar na tela de "Carregando".
- **Reserva.** Se faltar o áudio de uma frase, por exemplo quando só o `index.html` foi baixado, essa frase é lida pela melhor voz em português do Brasil do aparelho.
- **Antes do primeiro toque**, o navegador não deixa tocar som. A fala da tela fica guardada e toca no primeiro toque, se a criança continuar na mesma tela.

## 7. Limites

- A voz é de IA. Ela soa natural, mas não é uma pessoa. Se no futuro alguém da equipe puder gravar, os scripts da pasta `Voz/` mostram que frases gravar, e a troca é só substituir os pacotes.
- No Teatro, a emoção vem do texto da cena e de uma intensidade maior ou menor na geração. Não é uma atuação de verdade.
- Esta escolha foi feita por medidas, não de ouvido. **A equipe precisa ouvir** as amostras e aprovar a voz antes de publicar.

## Referências

- Shi, J. et al. (2022). *Prosodic modulations in child-directed language and their impact on word learning*. Developmental Science.
- Santos, L. G. et al. (2026). *Clonagem de Voz por IA para o Português Brasileiro em Contexto Assistivo: Desafios Comparativos em Cinco Ferramentas de Síntese de Fala*. STIL 2026, p. 348–360.
- Resemble AI (2026). *Chatterbox Multilingual V3* e *Chatterbox-Multilingual-pt-br* (licença MIT).
- Qwen Team (2026). *Qwen3-TTS: voice design, clone and generation* (licença Apache 2.0).
