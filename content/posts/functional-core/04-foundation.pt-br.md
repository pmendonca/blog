---
title: "Fundação do arco: estados, tipos e tempo"
slug: "fundacao-arco-estados-como-tipo"
date: 2025-12-31
author: "Paulo Mendonça"
draft: false
description: "O texto que organiza e dá linguagem ao arco sobre modelagem explícita de estados: por que estados implícitos falham, como tipos carregam semântica e onde a previsibilidade realmente importa."
weight: -1000

tags:
  - Programação Funcional
  - Modelagem de Domínio
  - Modelagem Explícita de Estados
  - Estados como Tipo
  - Arquitetura de Software

keywords:
  - estados implícitos
  - estados explícitos
  - estados como tipo
  - modelagem explícita de estados
  - system of types
  - semântica no tipo
  - typestate
  - phantom types
  - marker types
  - value objects
  - previsibilidade
  - transparência referencial
cover: "/blog/images/00-cover.jpeg"  
---


# Este arco nunca foi sobre Programação Funcional

A maioria dos bugs mais graves que vi ao longo de mais de vinte anos trabalhando com software não veio de algoritmos errados, mas da incompreensão de conceitos que eu ainda não sabia nomear, ideias que, por muito tempo, me pareceram abstratas demais, quase esotéricas, e portanto muito fáceis de ignorar.

O curioso é que os problemas quase sempre tinham os mesmos culpados aparentes. Falávamos de **entropia de software**, de **más decisões de design**, de **legado mal resolvido**. E essas explicações faziam sentido, mas só depois. O fato é que no começo, ninguém decide criar um sistema complexo. Ninguém acorda querendo tomar más decisões. Esses rótulos na verdade só surgem quando o estrago já está feito.

O que eu demorei a perceber é que todos esses “culpados” eram, na verdade, efeitos colaterais de algo mais fundamental: estados demais sendo carregados sem linguagem, sem estrutura e sem proteção. O problema não era que as decisões eram ruins desde o início é que não sabíamos o que estávamos decidindo.

## O problema real: estados invisíveis

Em muitos sistemas, o estado não vive em um lugar só. Ele emerge da combinação de flags, propriedades mutáveis, valores opcionais e da ordem em que métodos foram chamados. Para entender “em que estado estamos”, é preciso reconstruir uma linha do tempo mental.

Esse modelo funciona enquanto o sistema é pequeno, o time é o mesmo e o contexto ainda está fresco. Com o tempo, ele começa a falhar silenciosamente. O conhecimento deixa de estar no código e passa a morar na cabeça das pessoas. O domínio continua existindo, mas sua semântica se dissolve em condicionais defensivos, validações espalhadas e convenções implícitas.

Flags, enums e ifs são sedutores porque dão velocidade no curto prazo. Mas cobram juros altos depois. Eles empurram a responsabilidade para o futuro, para o próximo desenvolvedor, para o próximo refactor quase sempre para quando já é tarde demais.

**Por que isso é tão comum em linguagens multiparadigma?**

Linguagens como C#, Java ou TypeScript não impedem a modelagem explícita de estados. Elas permitem. O problema é que não incentivam.

O sistema de tipos é poderoso, mas opcional. É sempre possível contornar uma restrição com um null, um bool, uma string, um any. Primitivos são baratos, universais e rápidos de usar e exatamente por isso viram padrão. O custo real só aparece mais tarde, quando o domínio começa a exigir garantias que nunca foram formalizadas.

Isso cria um ciclo vicioso: estados implícitos parecem mais simples, viram norma, e qualquer tentativa de explicitá-los soa como complexidade desnecessária. No fim, não estamos criando complexidade nova. Estamos apenas expondo uma complexidade que sempre esteve lá, mas que preferimos ignorar.

## A virada de chave: semântica no sistema de tipos

Em algum momento ficou claro para mim que o problema não era controle de fluxo. Era semântica perdida.

Quando o estado não está no tipo, ele está espalhado pelo código. Quando a validade não está no tipo, ela depende de disciplina. Quando a ordem não está no tipo, ela depende de memória. Tudo aquilo que o compilador poderia verificar passa a ser responsabilidade humana e isso não escala.

O sistema de tipos deixa de ser apenas um detalhe técnico e passa a funcionar como memória externa do domínio. Ele não carrega apenas dados; carrega significado, restrições e contexto. Ele diz o que algo é, em que condições pode existir e o que pode acontecer a seguir.

É aqui que este arco se organiza.

## A escada conceitual deste arco

Ao longo dos textos, fui explorando uma escada de expressividade que não surgiu como técnica isolada, mas como forma de pensar:

**Primitivos** → **Value Objects** → **Marker Types** → **Phantom Types** → **Typestate**

Essa sequência não representa “níveis de sofisticação”, nem um caminho obrigatório. Ela representa formas progressivas de tornar estados explícitos, de mover garantias do runtime para o sistema de tipos, e de reduzir o espaço de estados possíveis.

Cada degrau responde a uma pergunta diferente:

- O que isso significa?
- Em que condição isso está?
- O que pode ou não pode ser feito agora?
- Em que ordem as coisas podem acontecer?

Não como técnica. Como linguagem do domínio.

## Onde a Programação Funcional entra — e onde não entra

A Programação Funcional aparece neste arco como ferramenta, não como dogma. Conceitos como imutabilidade, transparência referencial e redução consciente de efeitos colaterais ajudam a criar regiões do código mais previsíveis, mais estáveis e mais fáceis de raciocinar.

Em linguagens multiparadigma, isso não acontece de forma total. Acontece localmente. Em ilhas. Principalmente no domínio, onde regras, invariantes e transições importam mais do que integração com banco, rede ou UI.

Não se trata de “virar funcional”. Trata-se de escolher onde pagar o da previsibilidade e onde aceitar a imperfeição. O objetivo nunca foi pureza. Foi confiança ao ler, modificar e evoluir.

## Pagar agora ou pagar depois

Modelar estados de forma explícita é desconfortável porque antecipa decisões. Ela nos obriga a assumir agora um custo cognitivo que normalmente empurramos para o futuro. Primitivos, flags e ifs não são escolhas neutras; são formas de adiar responsabilidade.

Este arco é uma tentativa de fazer o oposto: aceitar que o domínio é complexo e tratá-lo com honestidade. Não para torná-lo simples, mas para torná-lo compreensível. Porque sistemas compreensíveis falham menos — e, quando falham, falham de forma menos surpreendente.

Se este texto servir para alguma coisa, que seja como ponto de entrada. Não para Programação Funcional, mas para uma pergunta mais fundamental:

> _"Onde o estado do meu sistema está realmente sendo decidido — e quem está pagando por isso depois?"_

## Conclusão

Este arco não é o resultado de alguns dias de pesquisa ou de um entusiasmo passageiro com uma técnica nova. Ele é o apanhado de um ano inteiro **2025** dedicado a observar, testar, errar, refatorar e reconsiderar escolhas em sistemas reais.

Nada do que está aqui surgiu no vácuo. Cada ideia foi sustentada por mais de duas décadas trabalhando como programador em ambientes de produção, lidando com prazos, legados, times, pressões e consequências reais. Se há alguma convicção nestes textos, ela não vem de teoria isolada, mas de repetição, contraste e tempo suficiente para que padrões se tornassem visíveis.

