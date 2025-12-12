---
title: "Programação Funcional: Diário Técnico"
slug: "01-functional-programming"
date: 2024-01-01
draft: false
weight: 1
description: "Reflexões práticas sobre como conceitos de Programação Funcional mudam a forma de trabalhar em sistemas reais."
---

> [!NOTE]
> O que acontece quando um desenvolvedor se depara com FP pela primeira vez

Caro leitor, este não é um blog didático. Não pretende ser tutorial, manifesto ou guia definitivo. É um **diário técnico**, escrito para mim, sobre o que acontece quando conceitos de Programação Funcional começam a infiltrar um código real — com prazos, legado, times e sistemas distribuídos.

Essa jornada começou movida por curiosidade, mas os resultados rapidamente superaram qualquer expectativa. Medos comuns desapareceram: o receio constante de alterar código e a forte dependência de contexto histórico. O que segue é o diário de como eu gostaria que esse tema tivesse me sido apresentado no início.

## **Imutabilidade** como tema central
Minha primeira porta de entrada prática para esse tema foi a imutabilidade. Não como um conceito abstrato, mas como uma tentativa consciente de reduzir o número de coisas que poderiam mudar sem eu perceber. Algumas decisões de linguagem que hoje reconheço apenas como ferramentas surgiram naturalmente desse incômodo e merecem um texto próprio. Aqui, o que importa é o efeito **menos estados implícitos**, **menos surpresas** e mais **confiança ao ler e modificar** código. 
### **Copiar** é melhor que modificar
A primeira implicação de tornar algo imutável, se reflete na necessidade de criar um novo estado para o objeto sempre que houver a necessidade de mudança. E isso não é um desperdício e sim estratégia. 

Quando paramos de compartilhar objetos mutáveis entre partes do sistema, paramos de depender de “disciplina” e começamos a depender de uma regra física _cada etapa recebe seu próprio valor_. Isso reduz o espaço de estados possíveis porque elimina aliasing (duas referências apontando para a mesma coisa) e, com isso, corta uma classe inteira de bugs causadas por mutações fora do campo de visão, **efeitos em cadeia**, **condições de corrida** e aquele tipo de “funciona até não funcionar”.

O contraintuitivo é que essa abordagem **pode ser mais performática**, porque o custo real em sistemas grandes raramente é **copiar bytes** e sim **coordenação**. Travar, sincronizar, disputar recursos, invalidar cache, esperar I/O, serializar acessos, debugar estado compartilhado… **isso é caro**. Muitas vezes, criar um novo valor (ou uma nova instância) é mais barato do que pagar o pedágio de garantir que o valor compartilhado não será corrompido. E mesmo quando existe custo de cópia, ele é previsível, local e fácil de medir; já o custo do estado compartilhado costuma ser intermitente, emergente e difícil de atribuir.

No fim, **“cópias”** aqui não são um capricho funcional e sim uma forma de trocar **complexidade global** (coordenação) por **trabalho local** (derivação de valores). E isso, repetido centenas de vezes num código real, vira segurança e frequentemente vira performance.
### **Modelagem** explícita de estados
Outro ponto em que senti uma mudança profunda foi quando passei a **modelar estados de forma explícita**, em vez de deixá-los implícitos no comportamento dos objetos. Em muitos sistemas OO tradicionais, o estado real de algo não está em um lugar só, ele emerge da combinação de flags, propriedades mutáveis e da ordem em que métodos foram chamados. Para entender “em que estado estamos”, é preciso reconstruir uma linha do tempo mental.

Quando o estado passa a ser explícito, essa reconstrução deixa de ser necessária.

Modelar estados explicitamente significa aceitar que um sistema **não está em qualquer estado a qualquer momento**. Ele está em **um de alguns estados possíveis**, bem definidos, conhecidos e nomeados. E mais importante: certas transições simplesmente **não existem**. Elas não são proibidas por convenção, documentação ou testes, **elas não são representáveis no código.**

O efeito disso é imediato. Perguntas como _“isso pode acontecer?”_ deixam de ser respondidas com _“acho que sim”_ ou _“depende do fluxo”_. A resposta passa a ser estrutural, ou o estado permite aquela operação, ou ela não compila, não encaixa, não faz sentido.

Esse tipo de modelagem muda o papel do código. Ele deixa de ser apenas uma sequência de instruções e passa a ser um **mapa do que é possível**. Estados inválidos não precisam ser tratados, eles não existem. Transições improváveis não precisam ser defendidas, elas não são modeladas.

O ganho aqui não é só correção. É cognitivo. Ler o código passa a ser entender **onde estamos**, não deduzir **como chegamos até aqui**. E isso reduz drasticamente a carga mental, especialmente em sistemas que evoluem ao longo do tempo.

Hoje percebo que muitos bugs que eu atribuía a “complexidade do domínio” eram, na verdade, consequência de **estados implícitos demais**. Ao tornar esses estados explícitos, o domínio não ficou mais simples mas ficou honesto. E sistemas honestos são mais fáceis de manter.

### Redução consciente de efeitos colaterais
Com o tempo, percebi que imutabilidade e cópias não resolvem tudo sozinhas. Elas ajudam muito, mas o ganho real só aparece quando começo a tratar **efeitos colaterais como algo que precisa ser explicitamente decidido**, e não como um detalhe inevitável do código.

Durante muito tempo, efeitos colaterais eram apenas “parte do trabalho”: escrever no banco, chamar uma API, logar algo, publicar um evento. O problema é que, quando essas coisas estão misturadas com a lógica de domínio, o código deixa de explicar _o que ele faz_ e passa a explicar _o que acontece quando ele roda_. A diferença parece sutil, mas muda completamente a forma de raciocinar.

Reduzir efeitos colaterais não significa eliminá-los. Sistemas reais precisam conversar com o mundo externo. O que mudou para mim foi a consciência de **onde eles acontecem** e **quando acontecem**. Quando a maior parte do código descreve transformações de valores e não interações o comportamento do sistema passa a ser previsível por inspeção, não por simulação mental.

O efeito imediato disso é que erros deixam de se espalhar. Um efeito colateral mal comportado passa a ser um problema localizado, e não algo que contamina toda a base de código. Testar deixa de exigir encenação complexa, mocks profundos ou estados artificiais. E, talvez o mais importante, ler o código volta a ser um exercício de entendimento, não de desconfiança.

Hoje percebo que muitos bugs difíceis que enfrentei não eram causados por regras erradas de negócio, mas por **efeitos colaterais demais acontecendo cedo demais**. Ao empurrá-los para as bordas do sistema e torná-los explícitos, o núcleo do código ficou mais calmo. E código calmo é mais fácil de manter, de explicar e de evoluir.
