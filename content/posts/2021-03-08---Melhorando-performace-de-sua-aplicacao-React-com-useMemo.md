---
title: Melhorando a performance de sua aplicação React com useMemo.
date: "2021-03-08T22:40:32.169Z"
template: "post"
draft: false
slug: "Melhorando-performace-de-sua-aplicacao-React-com-useMemo"
category: "Tutoriais"
tags:
  - "Tutoriais"
  - "React"
  - "Web Development"
  - "Mobile"
description: "Melhorando a performance de sua aplicação React com useMemo"
socialImage: "/media/titleUseMemo.jpg"
---

Fala galera, hoje irei falar como o hook useMemo pode ajudar na performance de seus projetos.

---

## — Memoization

Antes de tudo devemos compreender o conceito de memoization. Memoization em Ciência da Computação é uma técnica que visa nos ajudar a não realizar o mesmo trabalho várias vezes,
seria algo parecido com caching, mas em espoco de função.

- Como em escopo de função?
  Uma função pura, ao receber o mesmo input, deve sempre retornar o mesmo valor. Assim entra a técnica de memoization, já que a função dado o mesmo input retornará o mesmo valor,
  nós sacrificamos espaço em memória, em detrimento de processamento. Ao invés da função realizar todo o processamento para enfim retornar sua saída esperada, nós indicamos um local na memória que já terá esse valor.

O useMemo nada mais é que a implementação de memoization para React.

## — useMemo

Dada a explicação anterior, vejamos um cenário comum para a implementação do useMemo.

Suponhamos que temos dois Swtchers, quando esses dois switchers estiverem ativos, teremos um label abaixo dele nos indicámos o estado como ativado.

![usememo01.png](/media/usememo01.png)

#### Primeiro vamos definir nosso componente label

```javascript
function Label({ active, active2 }) {
  return active && active2 && <p className="App">ACTIVETED</p>;
}
```

Cada active representa o estado booleano de cada switcher.

#### Temos nosso componente principal

```javascript
function App() {
  const [showComponent, setShowComponent] = useState(false);
  const [showComponent2, setShowComponent2] = useState(false);

  const handleChange = () => {
    setShowComponent(!showComponent);
  };
  const handleChange2 = () => {
    setShowComponent2(!showComponent2);
  };

  return (
    <div className="App">
      <Switch
        checked={showComponent}
        onChange={handleChange}
        name="checkedA"
        inputProps={{ "aria-label": "secondary checkbox" }}
      />
      <Switch
        checked={showComponent2}
        onChange={handleChange2}
        name="checkedA"
        inputProps={{ "aria-label": "secondary checkbox" }}
      />
      <Label active={showComponent} active2={showComponent2} />
    </div>
  );
}
```

Possuímos os states, com cada active dos switchers. Temos as funções que irão lidar com a mudança de estado e o nosso return com os componentes.

#### O que há de errado com essa implementação?

Repare no gif abaixo:

![usememo02.gif](/media/usememo02.gif)

Estou usando o react devtools com a opção de highlight ativada. É possível notar que o componente Label é renderizado, mesmo o valor para os dois actives serem false.
Assim temos uma renderização desnecessária, consumindo recurso de processamento.
Basicamente todas as vezes que o estado dos actives mudarem o componente de Label irá renderizar, mesmo o conteúdo não aparecendo na tela.
É aí que entra o useMemo, oras, não necessitamos que enquando (active && active2) === false, que o componente sofra renderização, queremos que esse recurso de processamento apenas seja usado quando, de fato, for para mostrar o label na tela.

#### Utilizando useMemo

```javascript
const memoization = useMemo(
  () => <Label active={showComponent} active2={showComponent2} />,
  [showComponent, showComponent2]
);
```

O useMemo recebe, no caso, o componente que irei retornar e os valores mutaveis que controlam o componente.

```javascript
function App() {
  const [showComponent, setShowComponent] = useState(false);
  const [showComponent2, setShowComponent2] = useState(false);

  const handleChange = () => {
    setShowComponent(!showComponent);
  };
  const handleChange2 = () => {
    setShowComponent2(!showComponent2);
  };

  const memoization = useMemo(
    () => <Label active={showComponent} active2={showComponent2} />,
    [showComponent, showComponent2]
  );

  return (
    <div className="App">
      <Switch
        checked={showComponent}
        onChange={handleChange}
        name="checkedA"
        inputProps={{ "aria-label": "secondary checkbox" }}
      />
      <Switch
        checked={showComponent2}
        onChange={handleChange2}
        name="checkedA"
        inputProps={{ "aria-label": "secondary checkbox" }}
      />
      {memoization}
      {/* <Label active = {showComponent} active2 = {showComponent2} /> */}
    </div>
  );
}
```

Assim, utilizando do useMemo, nosso label só será renderizado na aplicação quando for necessário, quando não for necessário ele irá retornar o mesmo valor, pegando diretamente da memória.
