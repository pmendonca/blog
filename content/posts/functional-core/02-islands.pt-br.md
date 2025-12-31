---
title: "Onde a previsibilidade importa: ilhas de estabilidade em sistemas reais"
slug: "02-functional-programming"
date: 2025-12-21
author: "Paulo Mendonça"
draft: false
weight: 2
description: "Transparência referencial, estado estacionário e pragmatismo em linguagens multiparadigma."
tags:
  - Programação Funcional
  - Arquitetura de Software
  - Diário Técnico
keywords:
  - Transparência Referencial
  - Estado Estacionário
  - Imutabilidade
  - Efeitos Colaterais
  - Modelagem de Domínio
cover: "/blog/images/02-functional-programming.jpeg"
---



Recomendo a leitura da [parte um]({{< relref "01-functional-programming.pt-BR.md" >}}).

# A Tese

O caminho natural para a parte dois especialmente quando lidamos com linguagens multiparadigmáticas é sair das definições e chegar **e executar a tese** de que é possível construir um **núcleo estacionário**. Minha tese é menos sobre _virar funcional_ e mais sobre colher ganhos concretos ao adotar práticas que a gente costuma associar a linguagens funcionais como imutabilidade, modelagem explícita de estados e redução consciente de efeitos colaterais. Em linguagens multiparadigmáticas isso não vira dogma, e sim recurso. E proponho usar esses recursos para comprar previsibilidade onde ela vale mais e melhor ainda, onde os outros paradigmas falham em entregar.

É possível empurrar um sistema para um núcleo mais "matemático", mais substituível, mais estável? Sim. Mas eu não trato isso como destino obrigatório. O esforço cognitivo existe e nem sempre paga o preço. O que me interessa é o ponto em que essas práticas começam a criar áreas naturalmente mais fáceis de raciocinar, testar e evoluir, e, na minha experiência, o lugar mais fértil para isso acontecer é o domínio, principalmente quando ele é rico o suficiente para justificar regras, transições e invariantes explícitos.

Os conceitos abordados na parte um servem como pavimento. Nesta parte, eu quero mapear onde esse pavimento vira estrada firme para as regiões do sistema em que dá para explorar o modo funcional com menos atrito, e isso não por purismo, mas porque ali a estabilidade se torna material.

> O maior erro que percebo hoje não é ignorar Programação Funcional, e sim acreditar que adotá-la é uma decisão binária.

## Multiparadigma

No texto anterior eu falei de Programação Funcional como se eu estivesse escrevendo de dentro de uma linguagem funcional. Não é o caso. Minha referência prática vem de anos escrevendo sistemas em linguagens **multiparadigmáticas** como C#, Java e JavaScript — lugares onde a gente vive entre o imperativo e a orientação a objetos, e onde as ideias funcionais aparecem mais como **recursos** do que como **contrato**.

E esse detalhe muda tudo. Em linguagens que não te obrigam a ser “puro”, é fácil escrever código com cara de funcional e, ainda assim, carregar dependências implícitas por baixo: um `now`, um singleton, um cache global, um repositório chamado no meio da regra, uma mutação silenciosa. O resultado é que a promessa de previsibilidade não vem “de graça”; ela precisa ser comprada com decisões conscientes e, sim, com algum custo cognitivo.

Por isso, eu não estou defendendo que dá (ou que vale) transformar uma aplicação inteira em funcional. Minha aposta é mais pragmática: **há ganhos reais** quando a gente adota práticas associadas ao mundo funcional — imutabilidade, estados explícitos, efeitos bem localizados — e escolhe onde isso rende mais. E, na minha experiência, o lugar mais fértil para isso é o **domínio**, principalmente quando o domínio é rico o suficiente para merecer regras, invariantes e transições bem definidas.

## Transparência referencial (TR) e estado estacionário (EE)

Eu vou usar dois termos aqui como ferramentas de raciocínio, não como selo de pureza acadêmica. O primeiro é a **transparência referencial (TR)**, que é uma propriedade observável. Uma expressão/componente é TR quando eu consigo substituí-la pelo valor que ela produz sem mudar nada de relevante no comportamento do sistema.

Um jeito bem prático de sentir isso é perguntar: *se eu chamar isso duas vezes com a mesma entrada, eu juro que o resultado é o mesmo e que nada “aconteceu” no mundo por causa disso?* Se a resposta for “depende”, quase sempre há alguma dependência implícita escorrendo.

O ponto é que, em linguagens multiparadigmáticas, TR raramente aparece como um “sim/não” perfeito. Ela aparece como regiões do código onde TR vira uma boa aproximação, boa o suficiente para reduzir medo, reduzir simulação mental e aumentar confiança para refatorar.

O segundo termo, **estado estacionário (EE)**, é a condição estrutural que permite essa propriedade emergir. Eu considero que um trecho de código entra em EE quando ele não carrega **estado interno mutável** que afete o resultado sem aparecer na assinatura, quando ele não depende de **tempo**, **ordem de chamadas** ou **efeitos colaterais** para “dar certo”, e quando ele pode ser entendido como transformação: **entrada explícita → saída explícita**.

> **TR** é o que eu observo. **EE** é o que eu construo.

Em sistemas reais, a quebra quase nunca é “porque o código é imperativo”, mas sim porque ele tem dependências invisíveis: tempo (`now`), aleatoriedade, IDs gerados “do nada”, leitura/escrita em banco, cache, fila, filesystem, rede, estado global (singletons, `static`, config mutável), mutação compartilhada (um objeto que “vai sendo preenchido”) e ordem (um método que só funciona se outro foi chamado antes).

Nada disso é pecado. O problema é quando isso aparece misturado com regra de domínio, porque aí a regra deixa de ser regra e vira uma performance do ambiente.

Isso importa especialmente no domínio, porque lá é onde moram regras, invariantes e transições. Em domínios ricos, a complexidade não está no “como chamar o banco”, mas no “o que pode ou não pode acontecer”. E é exatamente aí que EE costuma pagar o preço: ao tornar entradas e estados explícitos, a gente troca incerteza difusa por um espaço de possibilidades mais estreito.

## Ilhas

No seu projeto vão surgir “ilhas”, independentemente da sua arquitetura. Não é uma promessa e nem uma obrigação: são trechos onde dá para tratar regras e decisões como transformação de dados, sem depender o tempo todo de banco, rede, tela ou log. Nem sempre esses espaços têm limites perfeitamente claros, mas mesmo uma fronteira “boa o suficiente” já muda o jeito de pensar.

Quando você identificar uma ilha, tente protegê-la: deixe as entradas explícitas, devolva saídas claras, e empurre as ações no mundo (buscar/salvar, chamar API, logar) para a borda. Não é sobre pureza; é sobre dar condições para que um **estado de EE** emerja e, com o tempo, colher os frutos: menos surpresa, menos medo de mexer, testes mais diretos e evolução mais constante.

## Conclusão

Este texto é um relato; tudo o que descrevi aqui foi fruto de muito tempo testando várias possibilidades. Convido você a começar a testar: vá com calma, tente explorar cada tema individualmente e vá prosseguindo sempre que se sentir confortável, e apenas quando começar a enxergar valor. Lembre-se: se a forma não tiver função, ela não terá valor.

Com o tempo você começará a experimentar menos surpresas, menos medo de mexer, testes mais diretos e um ritmo melhor de evolução. **Hoje, quando olho para um sistema, não pergunto se ele é funcional ou orientado a objetos. Pergunto onde a previsibilidade importa mais, e quanto estou disposto a pagar por ela.**

Até mais, e obrigado pelos peixes!
