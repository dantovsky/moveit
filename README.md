This is a [Next.js](https://nextjs.org/) project bootstrapped with [`create-next-app`](https://github.com/vercel/next.js/tree/canary/packages/create-next-app).

## Getting Started

First, run the development server:

```bash
npm run dev
# or
yarn dev
```

Open [http://localhost:3000](http://localhost:3000) with your browser to see the result.

You can start editing the page by modifying `pages/index.js`. The page auto-updates as you edit the file.

[API routes](https://nextjs.org/docs/api-routes/introduction) can be accessed on [http://localhost:3000/api/hello](http://localhost:3000/api/hello). This endpoint can be edited in `pages/api/hello.js`.

The `pages/api` directory is mapped to `/api/*`. Files in this directory are treated as [API routes](https://nextjs.org/docs/api-routes/introduction) instead of React pages.

## Learn More

To learn more about Next.js, take a look at the following resources:

- [Next.js Documentation](https://nextjs.org/docs) - learn about Next.js features and API.
- [Learn Next.js](https://nextjs.org/learn) - an interactive Next.js tutorial.

You can check out [the Next.js GitHub repository](https://github.com/vercel/next.js/) - your feedback and contributions are welcome!

## Deploy on Vercel

The easiest way to deploy your Next.js app is to use the [Vercel Platform](https://vercel.com/new?utm_medium=default-template&filter=next.js&utm_source=create-next-app&utm_campaign=create-next-app-readme) from the creators of Next.js.

Check out our [Next.js deployment documentation](https://nextjs.org/docs/deployment) for more details.


## Anotações

Figma com mais ideias para incrementar no projeto
https://www.figma.com/file/2cp7thFOmNRqoeYJ3u11C0/Move.it-2.0

Acelere sua carreira
Currículo alinhado com o mercado e as atitudes que vão te ajudar a acelerar na direção dos seus objetivos como dev.
https://pages.rocketseat.com.br/ignite/pre-matriculas/15

### Context API - API de Contextos do React

Contextos: formas de fazer comunicação entre vários componentes da app. Ou seja, serve para ter acesso a uma informação a partir de vários lugares.

Na app movit-next, o Context API foi utilizado para que o componente Countdown pudesse se comunicar com o componente ChallengeBox.

**Estrutura básica de um contexto:**

// --- CountdownContext.tsx

```tsx
import { createContext, ReactNode } from "react";

interface CountdownContextData {

}

// type ou interface :: para tipar o "children"
interface CountdownProviderProps {
    children: ReactNode
}

const CountdownContext = createContext({} as CountdownContextData)

export function CountdownProvider({ children }: CountdownProviderProps) {
    return (
        <CountdownContext.Provider value={{}}>
            {children}
        </CountdownContext.Provider>
    )
}
```

#### Exemplo de Config **básica** de um fluxo do Context API


// --- context/MyContext.tsx


```tsx
import { createContext } from 'react'

// For TypeScript use
interface ContextValues {
    fullName: string
    age: 37
}

// Criando um contexto
export const MyContext = createContext({} as ContextValues)
```


// --- pages/_app.tsx


```tsx
import '../styles/global.css'
import { MyContext } from '../contexts/MyContext'

function MyApp({ Component, pageProps }) {
    return (
        <MyContext.Provider value={{ fullName: "Dante Marinho", age: 37 }}>
            <Component {...pageProps} />
        </MyContext.Provider >
    )
}

export default MyApp
```


// --- pages/index.tsx


```tsx
import Head from 'next/head'
import { useContext } from 'react'
import { MyContext } from '../contexts/MyContext'

export default function Home() {

    const { fullName, age } = useContext(MyContext)
    console.log(fullName, age) // Will print "Dante Mrainho 37"

    return (
        <div className={styles.container}>
            <Head>
                <title>Início | My App</title>
            </Head>

            <MyComponent />

            <section>
                <p>Hello World! I'm {fullName}, {age} years old.</p>
            </section>
        </div>
    )
}
```

#### Exemplo de Config mais **aprimorada** de um fluxo do Context API

Nesta opção, em vez de criarmos funções e estados em _app.tsx, concentramos todas estas implementações em outro ficheiro, neste caso, no mesmo ficheiro onde criamos o contexto (pages/MyContext.tsx).

// --- context/MyContext.tsx


```tsx
import { createContext, ReactNode, useState } from 'react'

// For TypeScript use
interface ContextValues {
    fullName: string
    age: 37
    sallary: number
    sayHello: () => void
    moreMoney: (value: number) => void
}

// Criando um contexto
export const MyContext = createContext({} as ContextValues)

// type ou interface :: para tipar o "children"
interface MyProviderProps {
    children: ReactNode
}

export function MyContextProvider({ children }: MyProviderProps) {
    // Put here some states or funcions
    const [sallary, setSallary] = useState(1500)

    function sayHello() {
        console.log('Helooo!!')
    }

    function moreMoney(value: number) {
        setSallary(sallary + value)
    }

    return (
        <MyContext.Provider value={{ fullName: "Dante Marinho", age: 37, sallary, sayHello, moreMoney }}>
            {children}
        </MyContext.Provider>
    )
}
```


// --- pages/_app.tsx


```tsx
import '../styles/global.css'
import { MyContextProvider } from '../contexts/MyContext'

function MyApp({ Component, pageProps }) {
    return (
        <MyContextProvider>
            <Component {...pageProps} />
        </MyContextProvider >
    )
}

export default MyApp
```


// --- pages/index.tsx


```tsx
import Head from 'next/head'
import { useContext } from 'react'
import { MyContext } from '../contexts/MyContext'

export default function Home() {

    const { fullName, age, sayHello, sallary, moreMoney } = useContext(MyContext)
    console.log(fullName, age) // Will print "Dante Mrainho 37"
    sayHello() // Will print "Hellooo!!"

    return (
        <div className={styles.container}>
            <Head>
                <title>Início | My App</title>
            </Head>

            <p>Actual sallary: {sallary}</p>
            <button onClick={() => moreMoney(500)}>Get more 500€</button>

            <MyComponent />

            <section>
                <p>Hello World! I'm {fullName}, {age} years old.</p>
            </section>
        </div>
    )
}
```

## JS Cookie

Os cookies podem ser armazenados tanto no server quanto no client

- js-cookie :: lib JS pura, com uma API amigável para buscar e escrever dados no cookie.
- @types/js-cookie :: lib de terceiro que adiciona typagem a libs que não foram feitas com TypeScript, então ajuda no intellicense do editor.

**Installation**
```
npm i js-cookie
npm i @types/js-cookie -D
```

```tsx
// Uso de cookies para armazenar o level, current-experience e o challengesCompleted
// O useEffect() faz o trigger de sempre observe alguma alteração nas variáveis passadas no array, escreve os novos valores para os cookies
useEffect(() => {
    Cookies.set('level', String(level))
    Cookies.set('currentExperience', String(currentExperience))
    Cookies.set('challengesCompleted', String(challengesCompleted))
}, [level, currentExperience, challengesCompleted])
```

## Criação e funcionamento da modal

// --- LevelUpModal.tsx

```tsx
import { useContext } from 'react'

import { ChallengesContext } from '../contexts/ChallengesContext'
import styles from '../styles/components/LevelUpModal.module.css'

export function LevelUpModal() {
    const { level, closeLevelUpModal } = useContext(ChallengesContext)
    return (
        <div className={styles.overlay}>
            <div className={styles.container}>
                <header>{level}</header>
                <strong>Parabéns</strong>
                <p>Você alcançou um novo level.</p>

                <button type="button" onClick={closeLevelUpModal}>
                    <img src="/icons/close.svg" alt="Fechar modal" />
                </button>
            </div>
        </div>
    )
}
```

// --- LevelUpModal..codule.css

```tsx
.overlay {
    background: rgba(242, 243, 245, 0.8);
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;

    display: flex;
    justify-content: center;
    align-items: center;
}

.container {
    background: var(--white);
    width: 100%;
    max-width: 400px;
    padding: 2rem 3rem;
    border-radius: 5px;
    box-shadow: 0 0 60px rgba(0,0,0,0.05);
    text-align: center;
    position: relative;
}

.container header {
    font-size: 8.75rem;
    font-weight: 600;
    color: var(--blue);
    background: url('/icons/levelup.svg') no-repeat center;
    background-size: contain;
}

.container strong {
    font-size: 2.25rem;
    color: var(--title);
}

.container p {
    font-size: 1.25rem;
    color: var(--text);
    margin-top: 0.25rem;
}

.container button {
    position: absolute;
    right: 0.5rem;
    top: 0.5rem;
    background: transparent;
    border: 0;
    font-size: 0;
}
```

// --- ChallengesContext.tsx

```tsx
import { LevelUpModal } from '../components/LevelUpModal'

(...)

export function ChallengesProvider({ children, ...rest }: ChalllengesProviderProps) {

    (...)

    const [isLevelUpModalOpen, setIsLevelUpModalOpen] = useState(false)

    function levelUp() {
        setLevel(level + 1)
        setIsLevelUpModalOpen(true)
    }

    function closeLevelUpModal() {
        setIsLevelUpModalOpen(false)
    }

    return (
        <ChallengesContext.Provider value={{
            level,
            currentExperience,
            experienceToNextLevel,
            challengesCompleted,
            levelUp,
            startNewChallenge,
            activeChallenge,
            resetChallenge,
            completeChallenge,
            closeLevelUpModal
        }}>
            {children}
            {isLevelUpModalOpen && <LevelUpModal />}
        </ChallengesContext.Provider>
    )
}

```

## Deploy da App React

Opções de plataformas com planos free:
- Netlify
- Vercel

### Deploy na Vercel

Live app:  
https://moveit-dantii.vercel.app

- Criar conta na Vercel
- Download Vercel CLI `npm i -g vercel`
  Se instalar pelo yarn, precisa configurar yarn (yarn global path)
  `vercel login`
  A partir da pasta raiz da app:
  ```
    dantiii@dantiii-neon:~/Desktop/Eventos/NLW 2021-02-18/movit-next$ vercel
    Vercel CLI 21.3.3
    ? Set up and deploy “~/Desktop/Eventos/NLW 2021-02-18/movit-next”? [Y/n] y
    ? Which scope do you want to deploy to? Dante Marinho
    ? Link to existing project? [y/N] n
    ? What’s your project’s name? moveit-dantii
    ? In which directory is your code located? ./
    Auto-detected Project Settings (Next.js):
    - Build Command: `npm run build` or `next build`
    - Output Directory: Next.js default
    - Development Command: next dev --port $PORT
    ? Want to override the settings? [y/N] n
    🔗  Linked to dantiii/moveit-dantii (created .vercel)
    🔍  Inspect: https://vercel.com/dantiii/moveit-dantii/EY9fsCb7Q3ATjqxDJv217SvK8CHR [1s]
    ✅  Production: https://moveit-dantii.vercel.app [copied to clipboard] [38s]
    📝  Deployed to production. Run `vercel --prod` to overwrite later (https://vercel.link/2F).
    💡  To change the domain or build command, go to https://vercel.com/dantiii/moveit-dantii/settings
  ```
- A fazer nova modificação no projeto basta rodar o comando `vercel`, mas desta vez irá criar um novo link provisório para testes. 
- Para mandar novamente para produção basta rodar o comando `vercel --prod`.

## Ideias de melhorias e features para a app

- Adicionar boa documentação, com imagens
- Tornar responsiva
- Integração com PWA
- Logar com Github (OAuth) :: O Next faz isso sem a necessidade de ter que usar um backend. Ver este vídeo:  
  Serverless com ReactJS e Next.js na Vercel | Code/Drops #54  
  https://www.youtube.com/watch?v=Cz55Jmhfw84


## Andamento

Parei em 12:05 do vídeo de 25 fev - NLW 4 Trilha ReactJS...