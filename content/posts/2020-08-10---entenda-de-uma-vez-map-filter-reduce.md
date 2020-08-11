---
title: Entenda de uma vez map(), filter() e reduce().
date: "2020-08-10T22:40:32.169Z"
template: "post"
draft: false
slug: "entenda-de-uma-vez-map-filter-reduce"
category: "Tutoriais"
tags:
  - "Tutoriais"
  - "React"
  - "Web Development"
description: "Entenda de uma vez map(), filter() e reduce()."
socialImage: "/media/errorbounder.png"
---

Hoje tentarei passar o conhecimento que tenho sobre map(), reduce() e filter(). Trataremos de manipulação de array de uma forma mais arrojada, com conceitos de programação funcional.


_**Vantagens**: Redução de complexidade, maior integridade dos dados e código muito mais legível._

_**Desvantagem**: Dependendo do indivíduo, a curva de aprendizado será um pouco mais lenta._

Primeiro de tudo irei mostrar para você a razão de se usar programação funcional.


Vamos considerar a abordagem antiga, um loop for qualquer.

![mapfilterreducefor.png](/media/mapfilterreducefor.png)

. _O que esse for faz?_

Ele está adicionando os valores acima de 5 do array em models.


Vamos entender o que acontece nessa abordagem:

1 .let i = 0

2 .i < array.length

3 .array [i] > 5

4 .models.push(array[i])



São quatro passos para a execução, vejamos como ficaria ao usarmos filter:

![filter.png](/media/filter.png)


Só na estrutura você já nota a diferença, vou explicar pra você:


Os **passos 1, 2, 3 e 4 se tornaram apenas um.** Você não mais precisa definir uma variável i para controlar o loop, não precisa verifica i < array.length, e o array[i] > 5 o filter já faz de forma automática na função. E a diferença mais gritante é que a **função filter, assim como map, retorna um array, então você pode simplesmente atribuir diretamente**, cortando mais um passo, sendo assim, para esse exemplo, temos 4 em 1.

Menos passos e mais otimização, agora você já deve ter notado o poder.


Não se desespere ainda, que agora irei dar uma breve explicação de cada uma das três funções e espero que você saia daqui entendendo.


##map()

. map() irá retornar um novo array, com a aplicação de uma nova função. Ele **mapeia** o array e aplica algo nele.

- Mas como assim, meu consagrado?

Simplesmente você tem um array com uma coisa e quer que o map retorne algo com cada um dos elementos do array (desculpe meus termos científicos)

![map.png](/media/map.png)

Teremos em idadeEmDobro, cada uma das idades do array idades multiplicado por 2. Viu? nada de muito complicado.

##filter()

. filter() filtra o array ,retornando um novo array que passou naquele teste, já vimos ele logo acima mas irei colocar um outro exemplo pra você aqui.

![filteritem.png](/media/filteritem.png)

__O que essa belezinha retorna?__

- Um novo array com todos os nomes cujo tamanho dos nomes seja maior que 6.

Assim como o map ele **percorre cada um dos elementos**, e os que retornarem true para nome.length > 6, farão parte do novo array.


##reduce()

. Por fim reduce(), nessa as pessoas costumam ter um pouco mais de dificuldade de entender, tentarei decifrar para você.


Primeiro gostaria de lhe dizer que _reduce()_ é um dos mais versáteis, você vai encontrar diversas aplicações dele. A mais comum é a utilização para se fazer um somatório de valores, visto que o reduce(), **reduz** um conjunto de valores em apenas um.

![reduce.png](/media/reduce.png)


Vamos entender essa aplicação, nas linhas 2 e 3 temos a execução de um somatório, perceba os atributos que são passados no callback do reducer, no primeiro denominado somatório, é o que chamados de **acumulador**, ele irá armazenar **somatorio + valor** em cada vez que é percorrido o array, e por valor nos referimos como o **valor atual** que está sendo acessado. Assim temos como resultado de total o valor 6, a soma de todos os elementos do array.


Nas linhas 5, 6 temos a mesma função, que irá retorna o mesmo valor para cada interação no array e no fim, irá somar o total com o valor 5, como é verificado na linha 6.


Cobrimos com esse post parte do necessário para se entender as três principais funções presentes no JavaScript funcional. 