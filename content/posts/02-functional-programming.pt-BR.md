---
title: "Programação Funcional: Diário Técnico - Parte 2 - Ilhas de Estacionaridade"
slug: "02-functional-programming-pt-2"
date: 2025-12-15
author: "Paulo Mendonça"
draft: true
weight: 1,
description: "Transparência referencial e as ilhas de estacionaridades"
tags: ["FP", "Programação Funcional", "Arquitetura de Software" ]
keywords: ["Imutabilidade", "Tipos Algébricos", "Efeitos Colaterais", "Modelagem Explícita"]
cover: "/blog/images/01-functional-programming.jpg"
---

Recomendo a leitura da [parte um]({{< relref "01-functional-programming.pt-BR.md" >}}).


Quero começar esse registro compartilhando o objetivo da minha tese, de que é possível atingir um **estado de estabilidade** impressionante ao se aproveitar conceitos de programação funcional numa linguagem multiparadigmática. E esse estado é alcançado quando componentes adquirem **transparência referencial (TR)**. 

Em poucas  palavras **TR** entende-se por uma propriedade observável de uma expressão ou componente. Essa propriedade é dada por uma condição estrtutural denominada **estado estacionário (EE)**. Um componente está em estado estacionário quando: _não possui **estado interno mutável**_; _não depende de **tempo**, **ordem de chamdas** ou **efeitos colaterais**; _pode ser modelada como um **função matemática**.



Ao isolar efeitos colaterais e tornar estados explícitos, o domínio passa a operar além de um limite onde o mundo externo deixa de influenciar diretamente o comportamento do código. A partir desse ponto, surgem regiões que não reagem ao tempo nem ao contexto — regiões que a teoria descreve como estacionárias e referencialmente transparentes.

## Transparência Referencial e Estado Estacionário

Ao final deste texto, pretendo mostrar como é possível, por meio de algumas decisões arquiteturais, alcançar aquilo que muitos consideram o **santo graal prático** do paradigma funcional mesmo utilizando linguagens multiparadigmáticas.

Antes de começarmos se faz necessário avisar o leitor que esse tema possui alguma complexidade. E por isso mereceu um texto dedicado, aqui também faço uma escolha deliberada por uma **abordagem de engenharia e não de ortodoxia acadêmica**. Por conta disso não tenho a mínima pretensão de tratar o tema com o rigor formal ou compromisso com definições canônicas, embora deixo claro que reconheço sua importância. Tomei essa decisão com o intuíto de explorar os conceitos a partir das implicações práticas.

Antes de mais nada, é importante ter em mente que sempre que mencionar **Transparência Referencial** estarei me referindo a uma propridade observável de uma expressão ou componente. E quando me referir ao **Estado Estacionário** falo da condição estrutural que torna essa propriedade possível. Em outras palavras, não existe transparência referencial sem estado estacionário, pois esse estado é o **pré-requisito material** para que a transparência referencial emerja.

Por hora vamos combinar que um componente está em estado estacionário quando obedece as seguintes condições:

- Não possui **estado interno mutável**;
- Não depende de **tempo, ordem de chamadas** ou **efeitos colaterais**;
- Pode ser modelado como uma **função matemática**;

Estado estacionário → Permite → Transparência referencial

Embora a parte um tenha abordado a importância da Imutabilidade, Modelagem explícita de estados, Redução consciente de efeitos colaterais, apenas deixei implícito sua importância. **E te preparei até aqui para revelar finalmente** como começar a colocar tudo em prática.

## Ilhas de Estado Estacionário

Ilhas de estado estacionário são o componentes do sistema onde o comportamento é inveriável ao longo do tempo, dependendo apenas de dados explícitos, e portanto passíveis de modelagem funcional e consequentimente transparência referencial. Fora dessas ilhas, tentar impor o rigor funcional geralmente aumenta a complexidade não valendo o esforço, importante frizar que sistemas reais possuiem efeitos colaterais, e não devemos lutar contra eles.

Essas "ilhas" costumam aparecer em locais clássicos como: lógica de domínio, validação de regras de negócio, cálculos de preço, transições de estado, política de decisões, enriquecimento e normalização de dados. **Onde houver regra, haverá potencial de estacionaridade"**.


## Hands on!

> [!WARNING] Tome o cuidado de não tentar alcançar a transparência referencial onde isso é inalcansável.
> - Escolha onde irá aplicar o paradigma funcional
> - Não caia na armadilha de tentar tornar a aplicação funcional

### Ilhas de Estacionaridade


