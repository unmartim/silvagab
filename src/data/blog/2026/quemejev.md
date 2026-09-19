---
title: Quem é o Jev e por que todo mundo tá falando desse cara
author: Gabriel da Silva
pubDatetime: 2026-09-19T14:30:00Z
featured: false
draft: false
tags:
  - AI
  - Automation
description: "O Jev não quer conversar com você. Ele quer decidir coisas. E isso é bem mais interessante do que parece, inclusive nas partes que os posts de LinkedIn não contam."
---
Você provavelmente viu um ou outro post de algum cara de tech falando "o Jev fez isso em 1 segundo", "ah, resolvi isso com Jev", "Jev não sei o quê", e ficou ali com aquela cara de quem não quer perguntar, pensando: quem diabos é o Jev? Calma, vamos com calma. Se você não passou os últimos anos numa caverna, você conhece o ChatGPT, o Claude, o Gemini, e o resumo de uma linha é que o Jev não é igual a eles.

Ele foi criado pela TypeSafe AI, uma empresa de São Francisco que saiu do stealth em 15 de setembro de 2026 com [US$ 40 milhões em seed liderados pela DCVC](https://www.finsmes.com/2026/09/typesafe-ai-raises-40m-in-seed-funding.html). E o Jev, no fim das contas, é um classificador. Sim, um classificador, aquela coisa que você aprendeu na segunda aula de machine learning e achou chatíssimo. E eu sei exatamente o que você está pensando: "tá, Gabriel, e por que isso virou o novo wow dos gurus de tecnologia?". Vou te contar.

O anúncio saiu da boca do próprio criador, Diogo Almeida, num post que passou de 36 milhões de visualizações em poucos dias. Vale assistir antes de continuar, porque é a fonte primária de tudo que vem daqui pra baixo.

<blockquote class="twitter-tweet" data-dnt="true" data-theme="dark" data-align="center"><p lang="en" dir="ltr">After co-inventing ChatGPT, I kept asking myself: why have superhuman chat models not led to AGI?</p>&mdash; Diogo Almeida (@CompleteSkeptic) <a href="https://twitter.com/CompleteSkeptic/status/2099925682726002904">September 15, 2026</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Post e vídeo são de [Diogo Almeida](https://x.com/CompleteSkeptic), fundador da TypeSafe AI.

### O Jev não quer conversar com você

Ele não quer escrever seu e-mail, fazer uma redação sobre a Revolução Francesa ou criar uma legenda pro Instagram. Ele quer **decidir coisas**, o que soa bem menos impressionante até você parar pra contar a quantidade absurda de decisões que um software toma todo santo dia.

Imagina um atendimento ao cliente e a mensagem "fui cobrado duas vezes no meu cartão e quero meu dinheiro de volta". Você poderia jogar isso num ChatGPT da vida e pedir "analise essa mensagem, descubra a intenção do cliente, qual setor deve atendê-lo, a urgência do problema, e me devolva tudo em JSON", e ele provavelmente faria, bem inclusive. Só que existe um detalhe: o ChatGPT é um modelo feito pra **gerar texto**, e mesmo quando você pede JSON ele continua gerando token atrás de token, tentando montar a resposta que você pediu, enquanto você fica ali torcendo pra chave não vir com o nome errado justo no dia do deploy.

O Jev parte de outra ideia. Você já diz, antes, quais são as decisões possíveis: setor é financeiro, comercial ou suporte? Urgência é baixa, média ou alta? Precisa de humano, sim ou não? E ele simplesmente decide, sem escrever "após analisar cuidadosamente a mensagem do usuário, acredito que...". A resposta é basicamente `setor = financeiro (98%)`, `urgência = alta (91%)`, `precisa_humano = sim (74%)`, e é aqui que começa a ficar interessante.

<img width="800" height="450" alt="Diagrama comparando um fluxo concentrado no LLM com um fluxo em que o LLM cuida da conversa e o Jev decide" src="/assets/jev-arquitetura.svg" />

### ChatGPT gera. Jev decide.

Essa talvez seja a forma mais simples de entender a diferença. ChatGPT, Claude, Gemini e os outros LLMs foram construídos em torno de linguagem e são absurdamente bons quando você precisa que uma máquina **fale**. O Jev foi construído pensando no que acontece quando você precisa que uma máquina **escolha**.

E escolher aparece em praticamente qualquer software. Qual lead tem mais chance de comprar? Essa transação parece fraude? Pra qual atendente eu mando esse cliente? Esse comentário é spam? Esse usuário está prestes a cancelar? Essa ação pode ser executada sozinha ou precisa de revisão humana? Tudo isso é decisão, e até agora muita gente vinha usando LLMs gigantes pra resolver esse tipo de coisa, o que é quase como contratar um escritor premiado pra responder prova de múltipla escolha. Funciona, mas talvez você não precise do escritor.

A TypeSafe chama essa categoria de **System One Models**, uma referência direta ao Sistema 1 que Daniel Kahneman descreve em _Thinking, Fast and Slow_: decisões rápidas, automáticas, intuitivas, em oposição ao Sistema 2, lento e deliberativo. A tese deles é que a indústria inteira passou os últimos anos construindo Sistema 2 e vendendo como se fosse resposta pra tudo.

Se você quiser a versão de 45 segundos disso tudo, o Matija Sosic fez um resumo que explica a ideia central melhor que o vídeo oficial, e ele mesmo admite isso no post.

<blockquote class="twitter-tweet" data-dnt="true" data-theme="dark" data-align="center"><p lang="en" dir="ltr">Here&#39;s a 45-second TL;DR on Jev.</p>&mdash; Matija Sosic (@MatijaSosic) <a href="https://twitter.com/MatijaSosic/status/2100190746389135772">September 16, 2026</a></blockquote>

Post e vídeo são de [Matija Sosic](https://x.com/MatijaSosic).

### Na prática, ele só sabe responder três tipos de pergunta

A [documentação da TypeSafe](https://docs.typesafe.ai/concepts/system-one) é bem enxuta sobre isso, e sinceramente essa honestidade já é um ponto a favor. O primeiro primitivo é o `Choice`, que escolhe uma opção de uma lista de até 255 itens: você descreve cada opção, tipo "financeiro" sendo cobrança, reembolso e nota fiscal, e "técnico" sendo bug, erro e indisponibilidade, e ele escolhe uma. O segundo é o `Score`, que posiciona alguma coisa num espectro de 2 a 10 níveis, como o nível de frustração do cliente indo de "calmo" até "muito irritado". O terceiro é o `Noul`, que devolve uma probabilidade de 0 a 1 pra uma pergunta de sim ou não, do tipo "o cliente está pedindo reembolso explicitamente?".

É isso. Não tem "escreva a resposta pro cliente", não tem "explique seu raciocínio". Entrada é texto, saída é valor tipado com probabilidade, e a própria empresa descreve o modelo como uma chamada de função com inteligência de fronteira: estado não estruturado entra, decisão tipada e probabilística sai.

### Tá, e por que ele é tão rápido?

Aqui está a parte que faz engenheiro levantar a sobrancelha. Modelos como o ChatGPT geram resposta token por token, onde uma palavra depende da anterior, que depende da anterior, até acabar. Isso se chama geração autorregressiva, e é ela que faz você ficar olhando o cursorzinho piscando enquanto pensa na vida. O Jev abre mão disso: como o espaço de respostas já foi definido antes da pergunta, ele não precisa escrever nada, precisa apenas distribuir probabilidade entre opções que já existem.

<img width="800" height="450" alt="Comparação entre geração de tokens em sequência e saídas tipadas devolvidas em paralelo" src="/assets/jev-geracao-vs-decisao.svg" />

Os números que a TypeSafe publica são estes: 70 a 500 milissegundos de ponta a ponta, e US$ 0,042 por milhão de tokens de entrada, com tokens de saída de graça, o que faz sentido quando praticamente não existe saída. A empresa fala em ser [até 100 vezes mais rápida e mais barata](https://techstartups.com/2026/09/16/typesafe-ai-an-ai-startup-founded-by-chatgpt-co-inventor-emerges-from-stealth-with-40m-to-build-ai-thats-100x-faster-and-cheaper/) que modelos de fronteira em tarefas desse tipo, e alguns exemplos da semana de lançamento dão a dimensão da coisa. Um site classificou 1.018 artigos científicos por US$ 0,08, contra US$ 3,99 que ele gastou só pra gerar os resumos com um LLM normal. Um agente de browser reservou voos em 7,1 segundos por US$ 0,0039. Em loop de _computer use_, cada decisão saiu por US$ 0,0002 contra US$ 0,032 com um modelo de fronteira.

Isso não é diferença de 20%. É diferença de ordem de grandeza, e é isso que muda o tipo de coisa que você se permite automatizar.

### A parte que ninguém posta: confiança calibrada

Essa, pra mim, é a característica mais subestimada do troço todo. O Jev não diz só "é fraude", ele diz que há 97% de confiança de que aquilo é fraude, e aí você consegue escrever uma regra de negócio de verdade: acima de 95% bloqueia automaticamente, entre 70% e 95% manda pra análise humana, abaixo de 70% deixa passar.

<img width="800" height="450" alt="Fluxo em que a confiança devolvida pelo Jev define se o sistema executa a ação ou manda para revisão" src="/assets/jev-confianca.svg" />

Aí você me pergunta "mas o ChatGPT também me dá um número de confiança se eu pedir", e dá mesmo. O problema é que esse número costuma ser retórica, não estatística. A própria TypeSafe aponta que, mesmo quando você pede uma estimativa de confiança, LLMs tendem a ser sistematicamente superconfiantes, o que faz todo sentido, porque eles foram treinados com RLHF, ou seja, pra agradar humanos numa conversa, e não pra acertar uma probabilidade.

A TypeSafe diz ter treinado o Jev com uma variação que eles chamam de RLCD, _Reinforcement Learning for Calibrated Decisions_, otimizando justamente pra que o número queira dizer alguma coisa. Calibração significa que, no conjunto de todas as respostas em que o modelo disse 90%, aproximadamente 90% deveriam estar certas. E tem um asterisco importante, que está na documentação deles mesmo: calibração é uma propriedade do conjunto, não uma garantia por resposta individual. Isso não é detalhe, é a diferença entre desenhar um threshold e acreditar em mágica.

### Agora a parte chata, que é onde eu acho que mora o valor

Se o artigo parasse aqui ele seria um post de LinkedIn, e eu já tenho perfil no LinkedIn pra isso. Então vamos ao primeiro problema, que é o benchmark.

A TypeSafe reporta 67,8% de acurácia no benchmark interno de quatro workflows de produção, contra 66,8% do GPT-5.6 Luna, com custo de US$ 0,0004 por caso contra US$ 0,0033, e 0,4 segundo contra 12,9 segundos. Olha a acurácia de novo: um ponto percentual. Velocidade e custo são esmagadores, mas inteligência é empate técnico. E tem algo mais delicado, porque [o gabarito do benchmark foi gerado por outros modelos](https://dev.to/gabrielanhaia/jev-beat-gpt-luna-by-1-point-gpt-6-and-claude-wrote-the-answer-key-314k), uma média das respostas do GPT-6 Astra e do Claude Fable 5.1 em modo de raciocínio alto. Ou seja, aquilo não mede acerto contra a realidade, mede concordância com dois modelos de fronteira, e onde os dois erram juntos quem acerta é penalizado.

A TypeSafe reconhece esse viés na própria publicação, o que é honesto da parte deles, mas continua sendo um número que você não deveria levar pra reunião como se fosse verdade revelada. Aliás, quando você olha workflow por workflow a média some: em atendimento ao cliente o Jev fica 2,3 pontos atrás, e em processamento de nota fiscal fica 17,3 pontos atrás. Média é uma péssima conselheira.

O segundo problema é que ele lê literalmente, respondendo exatamente o que você escreveu e não o que você quis dizer, então instrução ambígua vira decisão ambígua, com 94% de confiança e cara de quem tem certeza. O terceiro é que ele tem buracos conhecidos: não conta bem, se atrapalha com datas, só aceita texto, nada de imagem, áudio ou vídeo, e fica **pior** quando você enche o contexto de informação irrelevante, que é exatamente o contrário do reflexo "joga tudo no prompt e reza".

E o quarto, que é o mais incômodo de todos, é que boa parte do ganho pode não ser o modelo. Quando você decompõe uma tarefa em perguntas estruturadas bem definidas, todos os modelos melhoram, então talvez metade do efeito Jev seja na verdade o efeito de finalmente parar de pedir pra uma IA fazer sete coisas ao mesmo tempo num prompt de 900 palavras. Isso não invalida o Jev, só te lembra que o design do seu workflow provavelmente vale mais que a sua escolha de modelo. Lembra do que eu falei no artigo passado sobre 80% do trabalho ser engenharia de dados sem glamour? Pois é, continua valendo.

### O Jev não veio matar o ChatGPT

São ferramentas pra problemas diferentes, sério. Se eu quero "escreva um e-mail pedindo desculpas para esse cliente", eu quero um LLM. Se eu quero "esse cliente precisa receber um reembolso?", aí faz muito mais sentido usar algo como o Jev.

O padrão que está emergindo é justamente a combinação dos dois, e isso não é teoria minha, já tem [integração no Pydantic AI](https://alphasignal.ai/news/pydantic-ai-adds-jev-to-cut-classification-latency-6x-without-generating-tokens) e [material de harness na LangChain](https://www.langchain.com/blog/building-a-harness-with-jev). O cliente escreve, o LLM conversa cuidando de empatia, contexto e redação, o Jev decide setor, urgência, risco e escalonamento, e o código executa a ação com regra e threshold. O LLM fala, o Jev decide, o código executa, cada um fazendo o que faz bem, que é uma ideia tão velha quanto separar responsabilidade em software.

### E o nome?

Não, "Jev" não é sigla futurista inventada por startup do Vale. O nome vem de William Stanley Jevons, economista inglês associado ao Paradoxo de Jevons: quando uma tecnologia fica muito mais eficiente e barata, o consumo dela tende a aumentar, não a diminuir. Jevons observou isso com o carvão, porque motores a vapor mais eficientes não reduziram o consumo de carvão na Inglaterra do século XIX, explodiram a demanda. O fundador, Diogo Almeida, ex-pesquisador da OpenAI e um dos nomes por trás do RLHF que deu origem ao ChatGPT, diz basicamente isso: eles esperam que a inteligência de máquina siga um caminho parecido com o do carvão.

A aposta inteira da TypeSafe é essa, aplicada a IA. Se tomar uma decisão com inteligência artificial ficar 100 vezes mais rápido e mais barato, a gente vai começar a colocar pequenas decisões inteligentes em absolutamente tudo, não porque é impressionante, mas porque fica barato demais pra não colocar. E é provavelmente por isso que você está vendo tanta gente falar desse cara.

### O que eu faria no seu lugar

Se você é dev ou trabalha com automação, o teste de sanidade é simples e cabe numa tarde. Pegue uma decisão que hoje você resolve chamando um LLM caro, triagem de ticket, moderação, roteamento, scoring de lead, qualquer uma. Monte um conjunto de 50 a 100 casos rotulados por gente, não por outro modelo, que é o passo que quase ninguém faz e o único que realmente importa. Rode o Jev contra esses casos e olhe acurácia, latência e custo.

Mas olhe principalmente uma coisa: a confiança serve como threshold? Se as respostas com 90% acertam perto de 90%, você acabou de ganhar uma alavanca de automação. Se não servir, você ganhou só um classificador rápido, que também não é pouco, mas é outra conversa. Só então decida, porque benchmark de fornecedor nunca previu o seu resultado, nem com o Jev, nem com o resto.

P.S.: eu deixei de fora um monte de coisa. Arquitetura não autorregressiva de verdade, como o RLCD funciona por dentro, e o debate bem mais nerd sobre se "System One Model" é uma categoria nova ou um classificador muito bom com marketing muito bom. Se render conversa, eu escrevo a parte 2.

### Fontes

[Introducing System One Models & Jev](https://typesafe.ai/blog/introducing-system-one-models-and-jev), no blog da TypeSafe AI

[System One](https://docs.typesafe.ai/concepts/system-one), na documentação da TypeSafe

[TypeSafe AI Raises $40M in Seed Funding](https://www.finsmes.com/2026/09/typesafe-ai-raises-40m-in-seed-funding.html), FinSMEs

[TypeSafe AI Emerges From Stealth With $40M](https://www.hpcwire.com/aiwire/2026/09/16/typesafe-ai-emerges-from-stealth-with-40m-in-funding-with-new-model-for-composable-ai/), AIwire

[ChatGPT co-inventor emerges from stealth with $40M](https://techstartups.com/2026/09/16/typesafe-ai-an-ai-startup-founded-by-chatgpt-co-inventor-emerges-from-stealth-with-40m-to-build-ai-thats-100x-faster-and-cheaper/), Tech Startups

[Jev Beat GPT Luna by 1 Point. GPT-6 and Claude Wrote the Answer Key](https://dev.to/gabrielanhaia/jev-beat-gpt-luna-by-1-point-gpt-6-and-claude-wrote-the-answer-key-314k), DEV

[How to Use Jev: A practical guide to TypeSafe's System One model](https://dev.to/valyuai/how-to-use-jev-a-practical-guide-to-typesafes-system-one-model-g5e), DEV

[Jev: TypeSafe's System One Model That Never Hallucinates](https://www.datacamp.com/blog/system-one-models-jev), DataCamp

[Building a harness with Jev](https://www.langchain.com/blog/building-a-harness-with-jev), LangChain

[Pydantic AI Adds Jev to Cut Classification Latency 6x](https://alphasignal.ai/news/pydantic-ai-adds-jev-to-cut-classification-latency-6x-without-generating-tokens), AlphaSignal

[TypeSafe AI's Jev Is Not an LLM, And That May Be the Point](https://forkast.news/typesafe-ais-jev-is-not-an-llm-and-that-may-be-the-point/), Forkast

[jev-benchmark](https://github.com/themsquared/jev-benchmark), um benchmark independente e reprodutível no GitHub

Daniel Kahneman, _Thinking, Fast and Slow_ (2011), de onde vem a ideia de Sistema 1 e Sistema 2
