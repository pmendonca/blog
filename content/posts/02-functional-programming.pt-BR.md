---
title: "Programação Funcional: Diário Técnico - Parte 2"
slug: "02-functional-programming-pt-2"
date: 2025-12-13
draft: true
weight: 1,
description: "Reflexões práticas sobre como conceitos de Programação Funcional mudam a forma de trabalhar em sistemas reais."
tags: ["FP", "Programação Funcional", "Arquitetura de Software" ]
keywords: ["Imutabilidade", "Tipos Algébricos", "Efeitos Colaterais", "Modelagem Explícita"]
---

> [!NOTE]
> Recomendo a leitura da [parte um]({{< relref "01-functional-programming.pt-BR.md" >}})

## Transparência Referencial e Estado Estacionário

Ao final deste texto, pretendo mostrar como é possível, por meio de algumas decisões arquiteturais, alcançar aquilo que muitos consideram o **santo graal prático** do paradigma funcional mesmo utilizando linguagens multiparadigmáticas.

Esse tema é complexo. E por isso mereceu um texto dedicado à ele, além da necessidade uma **abordagem de engenharia e não de ortodoxia acadêmica** por parte de quem vos escreve. Apesar de não ter a mínima pretensão de tratar o tema com o rigor formal ou compromisso com definições canônicas se faz necessário deixar claro que reconheço sua importância. Tomei essa decisão com o intuíto de explorar os conceitos a partir das implicações práticas no design de software.

Antes de mais nada, é importante ter em mente que sempre que mencionar **Transparência Referencial** estarei me referindo a uma propridade observável de uma expressão ou componente. E quando me referir ao **Estado Estacionário** falo da condição estrutural que torna essa propriedade possível. Em outras palavras, não existe transparência referencial sem estado estacionário, pois esse estado é o **pré-requisito material** para que a transparência referencial emerja. PS: O autor pretende não utilizar esse linguajar onde não for extritamente necessário.

Ou seja: um componente está em estado estacionário quando obedece as seguintes condições:

- Não possui **estado interno mutável**;
- Não depende de **tempo, ordem de chamadas** ou **efeitos colaterais**;
- Pode ser modelado como uma **função matemática**;

Observância das condições -> Implicação: **referencialmente transparente**;

A parte nos preparou abordando a importância dos seguintes tópicos:

- Imutabilidade;
- Modelagem explícita de estados;
- Redução consciente de efeitos colaterais;

## Hands on!

> [!WARNING] Tome o cuidado de não tentar alcançar a transparência referencial onde isso é inalcansável.
> - Escolha onde irá aplicar o paradigma funcional
> - Não caia na armadilha de tentar tornar a aplicação funcional

### Ilhas de Estacionaridade




