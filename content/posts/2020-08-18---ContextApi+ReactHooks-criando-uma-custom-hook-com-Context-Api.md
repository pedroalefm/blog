---
title: Context Api + React Hooks, criando uma custom hook com Context Api.
date: "2020-08-18T22:40:32.169Z"
template: "post"
draft: false
slug: "ContextApi+ReactHooks-criando-uma-custom-hook-com-Context-Api"
category: "Tutoriais"
tags:
  - "Tutoriais"
  - "React"
  - "Web Development"
  - "Mobile"
description: "Context Api + React Hooks, criando uma custom hook com Context Api"
socialImage: "/media/errorbounder.png"
---

Fala galera, hoje irei mostrar a vocês um pequeno tutorial de como utilizar Context Api + hooks, assim criando uma hook customizada.

---

## — Primeiramente, o que é Context Api ?

Utilizamos Context quando precisamos pegar dados, tidos como globais, e ao invés de passarmos para cada nó na árvore de component, criamos uma espécie de state global, em que os components poderão acessar os dados armazenados em um único local. Quem utiliza Redux já está acostumado com essa metodologia de criar um estado global, Context Api é algo já fornecido pelo React, sendo algo que até mesmo o Redux implementa em seu código.

Logo, se você tem algum dado que precisa ser acessado globalmente, Context Api é uma opção.

![reactree.jpg](/media/reacttree.jpg)

Nesse tutorial irei mostrar um context, que criei em um pet project que estou mexendo. Basicamente ele serve para armazenar dados do usuário, após ele fazer a autenticação.

#### Primeiro passo é criar o context instanciando createContext() fornecido pelo React

```javascript
import React, { createContext } from "react";

export const UserContext = createContext({});
```

Importamos React, pois esse é um arquivo tsx/jsx e importamos createContext para criarmos o UserContext.
O valor inicial do Context é um objeto vazio.

#### Agora iremos criar nosso provider, ele que "prover" os valores do nosso context.

```javascript
import React, { createContext, useState } from "react";

interface PropUserContext {
  children: any;
}

export const UserContext = createContext({});

export default function UserProvider(props: PropUserContext) {
  const [user, setUser] = useState({});

  return (
    <UserContext.Provider value={{ user, setUser }}>
      {props.children}
    </UserContext.Provider>
  );
}
```

Note que foi adicionado o useState, nosso UserProvider possui um state próprio, nesse state é armazenado os dados do usuário.

```javascript
export default function UserProvider(props: PropUserContext) {
  const [user, setUser] = useState({});

  return (
    <UserContext.Provider value={{ user, setUser }}>
      {props.children}
    </UserContext.Provider>
  );
}
```

Nosso UserProvider retorna exatamente o Provider do nosso Context, o value é o que ficará disponível para nossos components que devem ter acesso aos dados globais. Acessamos assim o user e a função setUser, para alterar o estado que estamos considerando como global.

Apenas para registrar, estou usando Typescript, por isso a tipagem e a interface em relação as props.

#### Nosso último passo em nosso arquivo de context é criar a nossa hook

```javascript
export function useUser() {
  const currentContext = useContext(UserContext);
  const { user, setUser } = currentContext;
  return { user, setUser };
}
```

Antes de tudo foi feito um novo import, importamos useContext, hook fornecida pelo React para utilizarmos com Context Api

```javascript
import React, { createContext, useState, useContext } from "react";
```

Assim nossa hook, que tem como funcionalidade recuperar e setar dados do usuário, em um store "global" está criada.

```javascript
import React, { createContext, useState, useContext } from "react";

interface PropUserContext {
  children: any;
}

export const UserContext = createContext({});

export default function UserProvider(props: PropUserContext) {
  const [user, setUser] = useState({});

  return (
    <UserContext.Provider value={{ user, setUser }}>
      {props.children}
    </UserContext.Provider>
  );
}

export function useUser() {
  const currentContext = useContext(UserContext);
  const { user, setUser } = currentContext;
  return { user, setUser };
}
```

Esse é o nosso arquivo ContextUser.tsx e para usarmos isso é muito simples.

#### Em App.tsx devemos fornecer o provider para nossos components.

```javascript
import React from "react";
import UserProvider from "./src/context/User";
const App = () => {
  return (
    <>
      <UserProvider>
        <Login />
        <Home />
      </UserProvider>
    </>
  );
};

export default App;
```

Assim todos os components, que são children de UserProvider, irão ter acesso aos valores.

#### Em Login.tsx vamos utilizar nossa hook

```javascript
import { useUser } from "../../context/User";
```

Usando desestruction para pegar os values do nosso context

```javascript
const { user, setUser } = useUser();
```

Em minha função de Login, armazeno o usuário autenticado no nosso store global.

```javascript
async function login() {
  const userLogged = await loginUser(email, password);
  setUser(userLogged);
}
```

Assim se quisermos acessar os dados do usuário precisamos apenas utilizar nossa hook.

```javascript
import { useUser } from "../../context/User";
const { user, setUser } = useUser();
```

Em user já teremos disponivel os dados do nosso usuário.

<br>
<br>
<br>

Context Api são de fato uma opção simples para termos um store global em nossa aplicação, e nesse tutorial tentei demonstrar uma maneira diferente de se utilizar isso, implementando através do conceito de hooks do React.
