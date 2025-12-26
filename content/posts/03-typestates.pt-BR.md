---
title: "Typestates" 
slug: "03-typestates"
date: 2025-12-24
author: "Paulo Mendonça"
draft: true
description: "Typestates"
weight: 3
tags:
  - Design Patterns
  - Typestate
  - Phantom types
  - Marker types
  - Constraints como regras
keywords:
  - Domínios
  - Typestate
  - Design Patterns
# cover: "/blog/images/02-functional-programming.jpeg"
---

# Typestate Analysis [^3]
 


Escrevi recentemente sobre como modelar estados explíticos pode te ajudar a criar uma camada de resiliência que até então você poderia acreditar não ser possível. Como tratei o tema de uma forma extritamente teórica achei necessário trazer uma implementação concreta do que estava falando. Vou te apresentar como iniciar os primeiros testes a partir do design pattern **Typestate**[^1] muito bem documentado.

> Em C#, **Typestate** (ou Pattern de Estado) é um padrão de projeto comportamental que permite um objeto mudar seu comportamento quando seu estado interno muda, como se ele estivesse trocado de classe, sem usar muitos `if/else` ou `switch` e movendo erros de estado para o tempo de compilação, tornando o código mais limpo e seguro. Ele encapsula os comportamentos relacionados a cada estado em classes separadas, delegando a tarefa ao objeto de contexto.[^2]

O objetivo dessa técnica é encapsular uma variedade de comportamentos para um mesmo objeto, baseado no seu estado interno. Isto pode tornar limpa a forma como um objeto muda seu comportamento em tempo de 

## O Exemplo

Para esse exemplo 

{{< gist pmendonca 4ab77c10d202e94add95dfae6ceaae0b program.cs >}}

https://gist.github.com/pmendonca/4ab77c10d202e94add95dfae6ceaae0b

## Referências

[^1]: [Design Pattern](https://refactoring.guru/design-patterns/state)
[^2]: [C# - Apresentando o padrão State](https://www.youtube.com/watch?v=w2PtQWTytW8)
[^3]: [Typestate Analysis](https://en.wikipedia.org/wiki/Typestate_analysi)
