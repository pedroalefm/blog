---
title: Lide com seus erros no React 16 com Error Boundaries.
date: "2020-08-10T22:40:32.169Z"
template: "post"
draft: false
slug: "lide-com-seus-erros-react16-error-bounderies"
category: "Tutoriais"
tags:
  - "Tutoriais"
  - "React"
  - "Web Development"
description: "Lide com seus erros no React 16 com Error Bounderies."
socialImage: "/media/errorbounder.png"
---

Olá meus caros, hoje vou falar de uma funcionalidade que chegou junto com o React 16, nossa forma de lidar com erros, os Error Boundaries.
---
— Primeiro de tudo, o que é Error boundaries?
---
Error boundaries são components React que capturam erros JavaScript em qualquer lugar na sua árvore de components. Eles oferecem um log desses erros e mostram uma Fallback UI.
Vale observar que os Erros boundaries interceptam os erros durante o rendering da aplicação, nos métodos do ciclo de vida e nos construtores dos componentes.
Caso você não saiba o que é uma Fallback UI, ela é simplesmente uma interface que você exibirá quando um erro for capturado.

##. componentDidCatch()
Esse é o método que faz com que a mágica aconteça, primeiro devo dizer que ele é um Lifecycle method, imagine ele como uma palavra mágica dentro da sua aplicação, essa palavra mágica vai invocar um estado diferente na aplicação. Pretendo fazer um post mais no futuro sobre o ciclo de vida de uma aplicação React, mas por enquanto saiba que essa não é a única, o ciclo de vida passa por quatro categorias Mounting, Updating, Unmounting e Error handling.

Antes de lhe mostrar como implementar um Error Boundary, gostaria de fazer um adendo de quando usar isso. Basicamente depende de você. Você pode fazer com que ele seja mais genérico ou mais específico. Você pode colocar ele no topo da sua árvore, prevenindo erros no componente App, ou colocar no mais específico componente de sua aplicação.

##Criando um Error Boundary
Abaixo temos a criação do componente Error Boundary
![code1errorbounder.png](/media/code1errorbounder.png)

Não há muita diferença para um componente React, apenas a implementação da função componentDidCatch(), nada de muito especial. Vaja como é simples implementar:!

![code2errorbounder.png](/media/code2errorbounder.png)


Vale ressaltar que apenas os erros que ocorrem nos filhos do Error Boundary serão tratados por ele e que apenas class components podem ser Error Boundary, então não esqueça de implementar da maneira correta. Uma curiosidade é que ele não se vigia, ele não captura seus próprios erros, por exemplo, no caso de uma fatalidade em mostrar sua linda Fallback UI, ele não exibirá seu próprio erro, nesse caso o erro vai subindo até o Error Boundary mais acima dele.

##Observação sobre a implementação
. Ser generalista ou especialista?

Eu digo que depende do que você quer, mas não sou o dono da razão.
Por exemplo, tenho um componente que vai me exibir aleatoriamente 5 alunos de uma sala de aula e eu implemento ele duas vezes para que me mostre 10 (Abstraia a lógica, apenas presta atenção da árvore de componentes). Algo assim:

![code3errorbounder.png](/media/code3errorbounder.png)

Suponha que apenas o componente da linha 2 deu erro. O que vai acontecer é que o erro será capturado independente se o componente da linha 3 deu ou não erro. Mas e se mesmo que o primeiro der erro nós ainda quisermos os dados vindos do componente da linha 3?


Devemos, no caso, sermos mais específicos


![code4errorbounder.png](/media/code4errorbounder.png)


Conclusão

Só gostaria de ressaltar que a forma que você usa seus error boundaries depende apenas de você, mas tenha sempre cuidado, pois ele afeta todos os componentes que estão abaixo na sua árvore de componentes.
Espero que o post não tenha ficado muito longo, é uma funcionalidade simples para um nome bonito e que agrega muito poder.
