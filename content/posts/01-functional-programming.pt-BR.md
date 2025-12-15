---
title: "Programação Funcional: Diário Técnico - Parte 1"
slug: "01-functional-programming"
date: 2025-12-13
draft: false
weight: 1,
description: "Reflexões práticas sobre como conceitos de Programação Funcional mudam a forma de trabalhar em sistemas reais."
tags: ["FP", "Programação Funcional", "Arquitetura de Software" ]
keywords: ["Imutabilidade", "Tipos Algébricos", "Efeitos Colaterais", "Modelagem Explícita"]
cover: "/blog/images/01-functional-programming.jpg"
---

[Read in english](/blog/pt-BR/posts/01-functional-programming/)

# Introdução

Caro leitor, este não é um blog didático. Não pretende ser tutorial, manifesto ou guia definitivo. É um **diário técnico**, escrito para mim, sobre o que aconteceu quando conceitos de **Programação Funcional** se misturaram a um código real, num projeto real com prazos, legados, times e sistemas de verdade.

Essa jornada começou como qualquer outra, movida pela curiosidade, vou contar como rapidamente os resultados superaram qualquer expectativa. Medos comuns desapareceram: o receio constante de alterar código e a forte dependência de contexto histórico. O que segue é um diário que agora compatilho com você.

## **Imutabilidade** como tema central

Minha primeira porta de entrada foi entender o conceito por trás da imutabilidade. Não como um conceito abstrato, mas como uma tentativa consciente de reduzir o número de coisas que poderiam mudar sem eu perceber. Algumas decisões de linguagem que hoje reconheço apenas como ferramentas, surgiram naturalmente desse incômodo e merecem um texto próprio. Aqui, o que importa é o efeito **menos estados implícitos**, **menos surpresas** e mais **confiança ao ler e modificar** foram mitigado ou se tornou irrelevante.

### **Copiar** é melhor que modificar

A primeira implicação de tornar algo imutável se reflete na necessidade de criar um novo estado para um objeto sempre que houver a necessidade de mudança. E embora isso possa parecer um um desperdício na verdade é uma estratégia. Pois quando paramos de compartilhar objetos mutáveis entre partes do sistema, paramos de depender de “disciplina” e começamos a depender de uma regra rígida onde _cada etapa recebe seu próprio valor_. Isso reduz o espaço de estados possíveis eliminando aliasing (quando duas referências apontando para a mesma coisa) e, com isso, corta-se uma classe inteira de bugs causadas por mutações fora do campo de visão, **efeitos em cadeia**, **condições de corrida** e aquele tipo de “funciona até não funcionar”.

O contraintuitivo é que essa abordagem **pode ser mais performática**, porque o custo real em sistemas grandes raramente é **copiar bytes** e sim **coordenação**. Travar, sincronizar, disputar recursos, invalidar cache, esperar I/O, serializar acessos, debugar estado compartilhad etc, **isso é caro**. Na maioria das vezes, criar um novo valor (ou uma nova instância) é mais barato do que pagar o pedágio que garante que o valor compartilhado não será corrompido. E mesmo quando existe custo de cópia, ele é previsível, local e fácil de medir; já o custo do estado compartilhado costuma ser intermitente, emergente e difícil de atribuir.

No fim, **“cópias”** aqui não são um capricho funcional e sim uma forma de trocar **complexidade global** (coordenação) por **trabalho local** (derivação de valores). E isso, repetido centenas de vezes num código real, vira segurança e frequentemente vira performance.

### **Modelagem** explícita de estados

Outro ponto em que senti uma mudança profunda foi quando passei a **modelar estados de forma explícita**, em vez de deixá-los implícitos no comportamento dos objetos. Em muitos sistemas tradicionais, o estado real de algo não está em um lugar só, ele surge da combinação de flags, propriedades mutáveis e da ordem em que métodos foram chamados. Para entender “em que estado estamos”, é preciso reconstruir uma linha do tempo mental.

Quando o estado passa a ser explícito, essa reconstrução deixa de ser necessária.

Modelar estados explicitamente significa aceitar que um sistema **não está em qualquer estado a qualquer momento**. Ele está em **um de alguns estados permitidos**, bem definidos, conhecidos e nomeados. E mais importante, certas transições simplesmente **não existem**. Elas não são proibidas por convenção, documentação ou testes, **elas simplesmente não podem ser representáveis no código.**

O efeito disso é imediato. Perguntas como _“isso pode acontecer?”_ deixam de ser respondidas com _“acho que sim”_ ou _“depende do fluxo”_. A resposta passa a ser estrutural, ou o estado permite aquela operação, ou ela não compila, não encaixa, não faz sentido.

Esse tipo de modelagem muda o papel do código. Ele deixa de ser apenas uma sequência de instruções e passa a ser um **mapa do que é possível**. Estados inválidos não precisam ser tratados, eles não existem. Transições improváveis não precisam ser defendidas, elas não são modeladas.

O ganho aqui não é só algoritmico. É cognitivo. Ler o código passa a ser entender **onde estamos**, não deduzir **como chegamos até aqui**. E isso reduz drasticamente a carga mental, especialmente em sistemas que evoluem ao longo do tempo.

Hoje percebo que muitos bugs que eu atribuía por exemplo a “complexidade do domínio” eram, na verdade, consequência de **estados implícitos demais**. Ao torná-los explícitos, o domínio não ficou mais simples mas ficou honesto. E sistemas honestos são mais fáceis de manter.

### Redução consciente de efeitos colaterais

Com o tempo, percebi que imutabilidade e cópias não resolvem tudo sozinhas. Elas ajudam muito, mas o ganho real só aparece quando se começa a tratar **efeitos colaterais** como algo que precisa ser explicitamente decidido, e não como um detalhe inevitável do código.

Durante muito tempo, efeitos colaterais eram apenas “parte do trabalho”: escrever no banco, chamar uma API, logar algo, publicar um evento. O problema é que, quando essas coisas estão misturadas com a lógica de domínio, o código deixa de explicar _o que ele faz_ e passa a explicar _o que acontece quando ele roda_. A diferença parece sutil, mas muda completamente a forma de raciocinar.

Reduzir efeitos colaterais não significa eliminá-los. Sistemas reais precisam conversar com o mundo externo. O que mudou para mim foi a consciência de **onde eles acontecem** e **quando acontecem**. Quando a maior parte do código descreve transformações de valores e não interações, o comportamento do sistema passa a ser previsível por inspeção, não por simulação mental.

O efeito imediato disso é que erros deixam de se espalhar. Um efeito colateral mal comportado passa a ser um problema localizado, e não algo que contamina toda a base de código. Testar deixa de exigir encenação complexa, mocks complexos ou estados "artificiais". E, talvez o mais importante, ler o código volta a ser um exercício de entendimento, não de desconfiança.

