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
Você provavelmente viu um ou outro post de algum cara de tech falando "o Jev fez isso em 1 segundo", "ah, resolvi isso com Jev", "Jev não sei o quê", e ficou ali com aquela cara de quem não quer perguntar, pensando: quem diabos é o Jev?

Calma. Vamos com calma.

Se você não passou os últimos anos numa caverna, você conhece o ChatGPT, o Claude, o Gemini. Resumo de uma linha: o Jev não é igual a eles.

Ele foi criado pela TypeSafe AI, uma empresa de São Francisco que saiu do stealth em 15 de setembro de 2026 com [US$ 40 milhões em seed liderados pela DCVC](https://www.finsmes.com/2026/09/typesafe-ai-raises-40m-in-seed-funding.html). E o Jev, no fim das contas, é um classificador.

Sim. Um classificador. Aquela coisa que você aprendeu na segunda aula de machine learning e achou chatíssimo.

E eu sei exatamente o que você está pensando: "tá, Gabriel, e por que isso virou o novo wow dos gurus de tecnologia?". Vou te contar.

### O Jev não quer conversar com você

Ele não quer escrever seu e-mail, fazer uma redação sobre a Revolução Francesa ou criar uma legenda pro Instagram.

Ele quer **decidir coisas**.

E isso soa bem menos impressionante, até você parar pra contar a quantidade absurda de decisões que um software toma todo santo dia.

Imagina um atendimento ao cliente. Chega a mensagem: "fui cobrado duas vezes no meu cartão e quero meu dinheiro de volta".

Você poderia jogar isso num ChatGPT da vida e pedir: "analise essa mensagem, descubra a intenção do cliente, qual setor deve atendê-lo, a urgência do problema, e me devolva tudo em JSON". E ele provavelmente faria. Bem, inclusive.

Só que existe um detalhe. O ChatGPT é um modelo feito pra **gerar texto**. Mesmo quando você pede JSON, no fundo ele continua gerando token atrás de token, tentando montar a resposta que você pediu, e você fica ali torcendo pra chave não vir com o nome errado justo no dia do deploy.

O Jev parte de outra ideia. Você já diz, antes, quais são as decisões possíveis. Setor: financeiro, comercial ou suporte? Urgência: baixa, média ou alta? Precisa de humano: sim ou não?

E ele simplesmente decide. Sem escrever "após analisar cuidadosamente a mensagem do usuário, acredito que...". A resposta é basicamente `setor = financeiro (98%)`, `urgência = alta (91%)`, `precisa_humano = sim (74%)`.

E é aqui que começa a ficar interessante.

<img width="800" height="320" alt="Diagrama comparando uma arquitetura em que o LLM faz tudo com outra em que o LLM conversa e o Jev decide" src="/assets/jev-arquitetura.svg" />

Esse diagrama é meu, então se estiver feio a culpa não é da IBM dessa vez.

### ChatGPT gera. Jev decide.

Essa talvez seja a forma mais simples de entender a diferença.

ChatGPT, Claude, Gemini e os outros LLMs foram construídos em torno de linguagem. São absurdamente bons quando você precisa que uma máquina **fale**. O Jev foi construído pensando no que acontece quando você precisa que uma máquina **escolha**.

E escolher aparece em praticamente qualquer software. Qual lead tem mais chance de comprar? Essa transação parece fraude? Pra qual atendente eu mando esse cliente? Esse comentário é spam? Esse usuário está prestes a cancelar? Qual produto eu mostro primeiro? Essa ação pode ser executada sozinha ou precisa de revisão humana?

Tudo isso é decisão. E até agora muita gente vinha usando LLMs gigantes pra resolver esse tipo de coisa. É quase como contratar um escritor premiado pra responder prova de múltipla escolha. Funciona, mas talvez você não precise do escritor.

A TypeSafe chama essa categoria de **System One Models**, uma referência direta ao Sistema 1 que Daniel Kahneman descreve em _Thinking, Fast and Slow_: decisões rápidas, automáticas, intuitivas, em oposição ao Sistema 2, lento e deliberativo. A tese deles é que a indústria inteira passou os últimos anos construindo Sistema 2 e vendendo como se fosse resposta pra tudo.

### Na prática, ele só sabe responder três tipos de pergunta

A [documentação da TypeSafe](https://docs.typesafe.ai/concepts/system-one) é bem enxuta sobre isso, e sinceramente essa honestidade já é um ponto a favor.

O primeiro primitivo é o `Choice`: escolha uma opção de uma lista de até 255 itens. Você descreve cada opção ("financeiro" é cobrança, reembolso e nota fiscal, "técnico" é bug, erro e indisponibilidade) e ele escolhe uma.

O segundo é o `Score`: posicione alguma coisa num espectro de 2 a 10 níveis. Nível de frustração do cliente, por exemplo, indo de "calmo" até "muito irritado".

O terceiro é o `Noul`: uma probabilidade de 0 a 1 pra uma pergunta de sim ou não. "O cliente está pedindo reembolso explicitamente?"

É isso. Não tem "escreva a resposta pro cliente", não tem "explique seu raciocínio". Entrada é texto, saída é valor tipado com probabilidade. A própria empresa descreve o modelo como uma chamada de função com inteligência de fronteira: estado não estruturado entra, decisão tipada e probabilística sai.

### Tá, e por que ele é tão rápido?

Aqui está a parte que faz engenheiro levantar a sobrancelha.

Modelos como o ChatGPT geram resposta token por token. Uma palavra depende da anterior, que depende da anterior, até acabar. Isso se chama geração autorregressiva, e é ela que faz você ficar olhando o cursorzinho piscando enquanto pensa na vida.

O Jev abre mão disso. Como o espaço de respostas já foi definido antes da pergunta, ele não precisa escrever nada. Ele precisa apenas distribuir probabilidade entre opções que já existem.

<img width="800" height="293" alt="Comparação entre geração token a token e uma decisão estruturada devolvida de uma vez" src="/assets/jev-geracao-vs-decisao.svg" />

Esse também é meu. Tô ficando bom nisso.

Os números que a TypeSafe publica são estes: 70 a 500 milissegundos de ponta a ponta, e US$ 0,042 por milhão de tokens de entrada, com tokens de saída de graça. Faz sentido cobrar zero pela saída quando praticamente não existe saída. A empresa fala em ser [até 100 vezes mais rápida e mais barata](https://techstartups.com/2026/09/16/typesafe-ai-an-ai-startup-founded-by-chatgpt-co-inventor-emerges-from-stealth-with-40m-to-build-ai-thats-100x-faster-and-cheaper/) que modelos de fronteira em tarefas desse tipo.

Alguns exemplos que circularam na semana de lançamento dão a dimensão da coisa. Um site classificou 1.018 artigos científicos por US$ 0,08, contra US$ 3,99 que ele gastou só pra gerar os resumos com um LLM normal. Um agente de browser reservou voos em 7,1 segundos por US$ 0,0039. E em loop de _computer use_, cada decisão saiu por US$ 0,0002 contra US$ 0,032 com um modelo de fronteira.

Isso não é diferença de 20%. É diferença de ordem de grandeza, e é isso que muda o tipo de coisa que você se permite automatizar.

### A parte que ninguém posta: confiança calibrada

Essa, pra mim, é a característica mais subestimada do troço todo.

O Jev não diz só "é fraude". Ele diz que há 97% de confiança de que aquilo é fraude. E aí você consegue escrever uma regra de negócio de verdade: acima de 95% bloqueia automaticamente, entre 70% e 95% manda pra análise humana, abaixo de 70% deixa passar.

<img width="800" height="267" alt="Faixas de confiança convertidas em regras: deixar passar, revisão humana e bloqueio automático" src="/assets/jev-confianca.svg" />

Aí você me pergunta: "mas o ChatGPT também me dá um número de confiança se eu pedir".

Dá mesmo. O problema é que esse número costuma ser retórica, não estatística. A própria TypeSafe aponta isso: mesmo quando você pede uma estimativa de confiança, LLMs tendem a ser sistematicamente superconfiantes. Faz sentido, porque eles foram treinados com RLHF, ou seja, pra agradar humanos numa conversa, e não pra acertar uma probabilidade.

A TypeSafe diz ter treinado o Jev com uma variação que eles chamam de RLCD, _Reinforcement Learning for Calibrated Decisions_, otimizando justamente pra que o número queira dizer alguma coisa. Calibração significa que, no conjunto de todas as respostas em que o modelo disse 90%, aproximadamente 90% deveriam estar certas.

E tem um asterisco importante, que está na documentação deles mesmo: calibração é uma propriedade do conjunto, não uma garantia por resposta individual. Isso não é detalhe. É a diferença entre desenhar um threshold e acreditar em mágica.

### Agora a parte chata, que é onde eu acho que mora o valor

Se o artigo parasse aqui ele seria um post de LinkedIn, e eu já tenho perfil no LinkedIn pra isso.

Primeiro problema: o benchmark tem uma falha conceitual séria.

A TypeSafe reporta 67,8% de acurácia no benchmark interno de quatro workflows de produção, contra 66,8% do GPT-5.6 Luna. Custo de US$ 0,0004 por caso contra US$ 0,0033, e 0,4 segundo contra 12,9 segundos.

Olha a acurácia de novo. Um ponto percentual. Velocidade e custo são esmagadores, mas inteligência é empate técnico.

E tem algo mais delicado. [O gabarito do benchmark foi gerado por outros modelos](https://dev.to/gabrielanhaia/jev-beat-gpt-luna-by-1-point-gpt-6-and-claude-wrote-the-answer-key-314k), uma média das respostas do GPT-6 Astra e do Claude Fable 5.1 em modo de raciocínio alto. Ou seja, aquilo não mede acerto contra a realidade, mede concordância com dois modelos de fronteira. Onde os dois erram juntos, quem acerta é penalizado.

A TypeSafe reconhece esse viés na própria publicação, o que é honesto da parte deles. Mas continua sendo um número que você não deveria levar pra reunião como se fosse verdade revelada. Aliás, quando você olha workflow por workflow a média some: em atendimento ao cliente o Jev fica 2,3 pontos atrás, e em processamento de nota fiscal fica 17,3 pontos atrás. Média é uma péssima conselheira.

Segundo problema: ele lê literalmente. O Jev responde exatamente o que você escreveu, não o que você quis dizer. Instrução ambígua vira decisão ambígua, com 94% de confiança e cara de quem tem certeza.

Terceiro: ele tem buracos conhecidos. Não conta bem. Se atrapalha com datas. Só aceita texto, nada de imagem, áudio ou vídeo. E fica **pior** quando você enche o contexto de informação irrelevante, que é exatamente o contrário do reflexo "joga tudo no prompt e reza".

E o quarto, que é o mais incômodo de todos: boa parte do ganho pode não ser o modelo. Quando você decompõe uma tarefa em perguntas estruturadas bem definidas, todos os modelos melhoram. Talvez metade do efeito Jev seja, na verdade, o efeito de finalmente parar de pedir pra uma IA fazer sete coisas ao mesmo tempo num prompt de 900 palavras.

Isso não invalida o Jev. Só te lembra que o design do seu workflow provavelmente vale mais que a sua escolha de modelo. Lembra do que eu falei no artigo passado sobre 80% do trabalho ser engenharia de dados sem glamour? Pois é. Continua valendo.

### O Jev não veio matar o ChatGPT

São ferramentas pra problemas diferentes, sério.

Se eu quero "escreva um e-mail pedindo desculpas para esse cliente", eu quero um LLM. Se eu quero "esse cliente precisa receber um reembolso?", aí faz muito mais sentido usar algo como o Jev.

O padrão que está emergindo é justamente a combinação dos dois, e isso não é teoria minha. Já tem [integração no Pydantic AI](https://alphasignal.ai/news/pydantic-ai-adds-jev-to-cut-classification-latency-6x-without-generating-tokens) e [material de harness na LangChain](https://www.langchain.com/blog/building-a-harness-with-jev). O cliente escreve, o LLM conversa cuidando de empatia, contexto e redação, o Jev decide setor, urgência, risco e escalonamento, e o código executa a ação com regra e threshold.

O LLM fala. O Jev decide. O código executa. Cada um fazendo o que faz bem, que é uma ideia tão velha quanto separar responsabilidade em software.

### E o nome?

Não, "Jev" não é sigla futurista inventada por startup do Vale.

O nome vem de William Stanley Jevons, economista inglês associado ao Paradoxo de Jevons: quando uma tecnologia fica muito mais eficiente e barata, o consumo dela tende a aumentar, não a diminuir. Jevons observou isso com o carvão. Motores a vapor mais eficientes não reduziram o consumo de carvão na Inglaterra do século XIX, explodiram a demanda.

O fundador, Diogo Almeida, ex-pesquisador da OpenAI e um dos nomes por trás do RLHF que deu origem ao ChatGPT, diz basicamente isso: eles esperam que a inteligência de máquina siga um caminho parecido com o do carvão.

A aposta inteira da TypeSafe é essa, aplicada a IA. Se tomar uma decisão com inteligência artificial ficar 100 vezes mais rápido e mais barato, a gente vai começar a colocar pequenas decisões inteligentes em absolutamente tudo. Não porque é impressionante, mas porque fica barato demais pra não colocar.

E é provavelmente por isso que você está vendo tanta gente falar desse cara.

### O que eu faria no seu lugar

Se você é dev ou trabalha com automação, o teste de sanidade é simples e cabe numa tarde.

Pegue uma decisão que hoje você resolve chamando um LLM caro. Triagem de ticket, moderação, roteamento, scoring de lead, qualquer uma.

Monte um conjunto de 50 a 100 casos rotulados por gente, não por outro modelo. Esse é o passo que quase ninguém faz e o único que realmente importa.

Rode o Jev contra esses casos e olhe acurácia, latência e custo. Mas olhe principalmente uma coisa: a confiança serve como threshold? Se as respostas com 90% acertam perto de 90%, você acabou de ganhar uma alavanca de automação. Se não servir, você ganhou só um classificador rápido, que também não é pouco, mas é outra conversa.

Só então decida.

Benchmark de fornecedor nunca previu o seu resultado. Nem com o Jev, nem com o resto.

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
