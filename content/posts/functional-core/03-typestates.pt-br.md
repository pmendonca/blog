---
title: "Modelagem explícita de estados: do valor ao tempo"
slug: "03-modelagem-explicita-de-estados"
date: 2025-12-28
author: "Paulo Mendonça"
draft: false 
description: "Uma reflexão sobre como o domínio ganha semântica, proteção e expressividade quando estados deixam de ser implícitos."
weight: 3
tags:
  - Modelagem explícita de estados
  - Estados como tipo
keywords:
  - Primitivos
  - Value Objects
  - Marker Types
  - Phantom Types
  - Typestate
cover: "/blog/images/03-cover.jpeg"
---


# Introdução

Nas partes um e dois tentei fazer um relato. Em alguns momentos falhei e o texto acabou virando um misto do que aconteceu comigo e do que eu deveria fazer. Isso deu ao texto uma aparência meio deselegante, quase de tutorial. Vou tentar corrigir isso aqui, manter a narrativa mais limpa e aproveitar para agradecer a todos pelos feedbacks.

Enquanto me aventurava pelos caminhos ainda pouco explorados da modelagem explícita de estados, comecei a me perguntar por que esse tipo de abordagem é tão invisível em linguagens multiparadigmas como C#, Java, TypeScript e afins. A resposta, no fim das contas, era exatamente a que se poderia esperar. Esses ecossistemas cresceram sob uma mentalidade imperativa e orientada a dados mutáveis, onde o estado costuma ser representado por flags, enums e condicionais espalhadas. Tornar estados explícitos exige o oposto: antecipar estados possíveis, proibir combinações inválidas e modelar transições. Isso aumenta o esforço inicial e reduz a sensação de velocidade no curto prazo. Em ambientes pressionados por prazos, essa fricção costuma ser percebida como burocracia — mesmo quando reduz erros graves no médio e longo prazo.

Além disso, apesar de essas linguagens permitirem a modelagem explícita de estados, quase não há incentivos ergonômicos para isso. O sistema de tipos é poderoso, mas opcional e frequentemente contornado. É fácil cair em null, bool, string ou até um any e seguir em frente. Como os erros de estado aparecem tardiamente, o custo real fica invisível para quem escreve o código inicial. O resultado é um ciclo vicioso: estados implícitos parecem mais simples, viram padrão, e qualquer tentativa de explicitá-los soa como “complexa demais”. No fim, o que essa abordagem faz é apenas expor uma complexidade que sempre esteve lá — lacunas escondidas, decisões adiadas, coisas que deixamos para o eu do amanhã resolver.

## Blocos de construção

Acabei conhecendo esses atores de forma orgânica, alguns eu melhor compreendi ao usar `rust` e me aventurando pelo `elixir`. Apesar de alguns já serem bem conhecidos, acho que ainda são dignos de menção. E aqui é onde talvez o texto fique uma cara de tutorial. Eu aqui novamente deslizando na narrativa. Vou apresentar quem são, o que são mas ainda não é a hora de fala no "como" e sim na "forma de pensar" esse tema.

### Primitivos

O ponto zero. `int`, `string`, `Guid`, `decimal`… Eles não possuem semântica, são baratos, perigosos e inevitáveis nas bordas. Não pertencem ao domínio — mas, paradoxalmente, o domínio quase sempre começa com eles.

Os primitivos são perigosos porque carregam valor, mas não carregam significado. Um `int` não diz o que ele representa; uma `string` aceita qualquer coisa; um `Guid` é apenas um identificador sem identidade. O compilador não sabe distinguir um `UserId` de um `OrderId`, um `Age` de um `Quantity`, um `Price` de um `Balance`. Quando o domínio é expresso diretamente com primitivos, você transfere a responsabilidade semântica do **sistema de tipos** para a **memória humana**, comentários, convenções e disciplina. E isso não escala. O erro não aparece onde o código é escrito, aparece depois em integrações erradas, estados inválidos e regras violadas silenciosamente.

Eles são sedutores. São baratos, rápidos de escrever, universais, e por isso viram o caminho padrão, especialmente em linguagens multiparadigma. Mas esse baixo custo é uma ilusão, o que se economiza na modelagem é pago por validações espalhadas, `if`s defensivos, testes redundantes e bugs difíceis de rastrear. Por isso os primitivos são inevitáveis nas bordas (I/O, serialização, banco de dados e rede), mas tóxicos no núcleo. Eles não pertencem ao domínio porque o domínio é semântica, regra, intenção. E tudo isso começa justamente quando se abandona os **primitivos crus** e passa-se a nomear, restringir e proteger o significado do que o sistema realmente é.

### Value Objects

Os Value Objects carregam uma identidade semântica mínima. Aqui se encapsula validações, removemos ambiguidades básicas e cria-se igualdade por valor. **Aqui acontece o primeiro salto real de domínio**.

VOs representam o **primeiro salto real de domínio** porque são o momento em que o sistema deixa de manipular valores genéricos e passa a manipular significados explícitos. Um `Email` deixa de ser apenas uma string e passa a carregar regras de validade, formato e intenção. Um `Money` deixa de ser um `decimal` e passa a impor moeda, precisão, arredondamento e operações permitidas. Um `UserId` deixa de ser um identificador técnico e passa a ser uma identidade reconhecível pelo domínio. Aqui a semântica sai da cabeça do desenvolvedor e entra no código, tornando erros triviais em **impossibilidades de construção**.

É também nos VOs que se estabelece uma mudança fundamental de comportamento: igualdade por valor, imutabilidade e encapsulamento de invariantes. Isso reduz drasticamente estados inválidos, elimina validações duplicadas e torna o código mais legível, porque o tipo passa a comunicar intenção. Ainda assim é um salto mínimo.

VOs não model processos nem transições, mas criam um alicerce onde o domínio pode existir sem ambiguidades. Sem eles, qualquer tentativa de modelagem avançada nascerão sob areia movediça.

### Marker Types

Servem para realizar **qualificação conceitual**. Eles não mudam dados, mudam o significado criando categorias semânticas. Aqui o domínio vai começar a **dizer coisas sobre si mesmo**.

Os **Markers Types** realizam uma **qualificação** porque introduzem uma camada de significado sem adicionar dados nem comportamento. Eles existem apenas para classificar, rotular e diferenciar semanticamente algo que, em termos estruturais, poderia ser idêntico. Um objeto `User` continua tendo os mesmos campos, mas um `User<Authenticaded>` não é conceitualmente o mesmo que um `User<Anonymous>`.  O Marker não transforma o valor, ele transforma o que o valor representa dentro do domínio. Essa distinção, feita no sistema de tipos, permite que certas operações se tornem ilegais por definição, sem `if`, sem `bool`, sem validações defensivas.

Quando digo que a partir daí, o domínio começa a **dizer coisas sobre si mesmo**, estou falando de auto-descrição semântica. O código deixa de depender de comentários, convenções ou documentação externa para explicar em que condições algo se encontra. O próprio tipo comunica _isso já foi validado_, _isso já foi autenticado_. O domínio passa a carregar afiramções verificáveis em tempo de compilação sobre o seu estado. Não é mais o programador dizendo "confia em mim, isso aí é válido!" mas sim, o compilador dizendo.

Nesse ponto o domínio deixa de ser apenas um conjunto de dados com regras implícitas e começa a se comportar como uma linguagem que descreve a si própria.

### Phantom Types

São invariantes relacionais, aqui codifico pré-condições no tipo, eliminamos sequências inválidas, impedimos usos errados. Em suma: O domínio passa a **se defender sozinho**.

Os Phantom Types fazem o domínio se defender sozinho porque deslocam a verificação de pré-condições do **runtime** para o sistema de tipos. Eles não exsistem em tempo de execução, não carregam dados, mas carregam restrições relacionais como: _este valor só pode ser usado depois que certa condição for satisfeita_, _só faz sentido em combinação com outro tipo específico_, _só pode ppassar por determinadas operações_. O tipo deixa de representar apenas "algo" e passa a representar algo que pode ser utilizado em determinadas condições. Sequências inválidas simplesmente não são expressáveis e não há como "pular etapas", porque o compilado não permitirá.

É aqui que o domínio começa a se **defender sozinho** porque ele não confia mais na disciplina do desenvolvedor nem em validações tardias. Erros deixam de ser possibilidades e passam a ser impossibilidades estruturais. Um recurso não validado não chega a funções que exigem validação e um identificador de contexto não pode ser passado para outro. O domínio vira um **sistema de restrições vivas**, onde usos errados não são tolerados. Nesse ponto, o código deixa de "esperar que façam certo" e passa a forçar o **certo por construção**.



### Typestate

São responsáveis por regras temporais explícitas. Aqui o domínio expressa **ordem**, transições válidas e fecha o cerco contra estados ilegais. Isso é o pronto mais alto de expressividade que consegui ver em linguagens multiparadigma.

Os Typestates tornam explícitas as regras temporais do domínio ao modelar não apenas quais estados existem, mas em que ordem eles podem ocorrer. Cada estado relevante passa a ser representado como um tipo distinto, e cada operação válida é, na prática, uma transcrição tipada. Ela consome um estado e produz outro. O tempo normalmente implícito em comentários, fluxos mentais ou documentação externa, passa a ser codificado na própria assinatura das funções. Não é mais possível chamar algo "fora de hora", porque o compilador exige que o objeto esteja no estado correto antes da operação existir como possibilidade.

É por isso que aqui está o ponto mais alto expressividade acessível em linguagens multiparadigma que consegui encontrar. O domínio não apenas carrega significado (VOs), nem apenas se qualifica conceitualmente (Markers), nem apenas se protege de usos indevidos (Phantom Types). Com Typestates, ele passa a expressar comportamento ao longo do tempo, transformando fluxo em tipo e ordem em estrutura. O código deixa de ser uma sequência de instruções defensivas e passa a ser uma descrição formal do que pode e do que nunca poderá acontecer.

## Conclusão

Talvez o verdadeiro motivo pelo qual a modelagem explícita de estados seja tão rara não seja técnico, mas cultural. Ela nos força a encarar algo desconfortável: que a complexidade não surge porque escrevemos “código ruim”, mas porque o domínio é complexo. E, quando tornamos isso explícito, perdemos o conforto das abstrações vagas e das decisões adiadas. Primitivos, flags e ifs não são escolhas neutras, são formas de empurrar responsabilidade para o futuro. Modelar estados de forma explícita é fazer o oposto, é assumir agora o custo cognitivo que normalmente deixamos para o sistema pagar depois. Talvez por isso essa abordagem incomode tanto. Ela não promete velocidade imediata; promete apenas algo mais difícil de vender: menos surpresas quando já é tarde demais.
