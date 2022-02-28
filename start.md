# QUIZZ PROJECT

Programmation d'un quizz React TypeScript à l'aide d'une API externe.

### Installation

- clone ce repo
- cd app
- ``npm install``

### Première étape

Voici à quoi doit ressembler votre App.tsx au départ

**App.tsx**
```
import React, { useState } from 'react';

// Components

// Types 

// Styles

function App() {

    return (

        <>
            <div className="App">
                Welcome
            </div>
        </>
    );
}

export default App;
```
Dans le même repertoire, nous allons ensuite créer un dossier **components**.
Nous allons y intégrer notre premier component: **QuestionCard.tsx**

**QuestionCard.tsx**

```
import React from 'react';

// types

// styles

// type

const QuestionCard = () => (

    <div>
    QuestionCard
    </div>
);

export default QuestionCard;
```
## Seconde étape

Nous pouvons maintenant importer notre component QuestionCard.tsx dans l'App.tsx


```
import QuestionCard from './components/QuestionCard';
```
Nous aurons besoin de 3 fonctions que nous allons déjà initiés:
- startGame() : qui démarre le quizz
- checkAnswer(``e: React.MouseEvent<HTMLButtonElement>``) : qui vérifie si la réponse selectionnée est la bonne.
- NextQuestion() : qui passe à la question suivante.
  

Dans **App.tsx**, à l'interieur de la fonction App
```
const startGame = async () => {};

const checkAnswer = (e: React.MouseEvent<HTMLButtonElement>) => {};

const nextQuestion = () => {};

```

<!-- 
**App.tsx**
```
///App.tsx 2

import React, { useState } from 'react';


// Components
import QuestionCard from './components/QuestionCard';

// Types 


// Styles


function App() {

    const startGame = async () => {};

    const checkAnswer = (e: React.MouseEvent<HTMLButtonElement>) => {};

    const nextQuestion = () => {};

    return (

            <div className="App" >
            
                <h1>React Quiz</h1>
            
                <QuestionCard/>
                
                <button className="next" onClick={nextQuestion}>Next Question</button>
               
            
            </div>
    );
}

export default App;
``` -->

Nous allons maintenant préparer les props pour notre component:
**QuestionCard**

Comme vu précédement, il faut définir le **type** des props:

```
type Props = {

    question: string;
    answers: string[];
    callback: (e: React.MouseEvent<HTMLButtonElement>) => void;
    userAnswer: AnswerObject | undefined;
    questionNumber: number;
    totalQuestions: number;
};
```
On passe ensuite en paramètre les props dans la fonction **QuestionCard**
```
const QuestionCard: React.FC<Props> = '{
    question,
    answers,
    callback,
    userAnswer,
    questionNumber,
    totalQuestions
}
```

<!-- Voici à quoi doit ressembler votre **QuestionCard**
```
import React from 'react';

// types

// styles

// type
type Props = {

    question: string;
    answers: string[];
    callback: (e: React.MouseEvent<HTMLButtonElement>) => void;
    userAnswer: AnswerObject | undefined;
    questionNumber: number;
    totalQuestions: number;
};

const QuestionCard: React.FC<Props> = ({
    
    question,
    answers,
    callback,
    userAnswer,
    questionNumber,
    totalQuestions
}) => (

    <div>
        <p className="number">
            Question: {questionNumber} / {totalQuestions}
        </p>
        <p dangerouslySetInnerHTML={{ __html: question }}/>
        <div>
            {answers.map(answer => (
        
                <button disabled={userAnswer ? true : false} value={answer} onClick={callback}>
                    <span dangerouslySetInnerHTML= {{ __html: answer }}></span>
                </button>
            ))}
        </div>
    </div>
);

export default QuestionCard;
``` -->
## Troisième étape
Nous allons maintenant implémenter les Hooks et fetch l'API du Quizz

**App.tsx**

```
const [loading, setLoading] = useState(false);
const [questions, setQuestions] = useState<QuestionState[]>([]);
const [number, setNumber] = useState(0);
const [userAnswers, setUserAnswers] = useState<AnswerObject[]>([]);
const [score, setScore] = useState(0);
const [gameOver, setGameOver] = useState(true);

```

Nous avons assignés 2 types que nous n'avons pas défini encore **QuestionState** et **AnswerObject**.
Crée un fichier API.ts (src/**API.ts**);

Définissons **QuestionState**:
**API.ts**
```
//il faut détailler l'objet question
export type Question = {

    category: string;
    correct_answer: string;
    difficulty: string;
    incorrect_answers: string[];
    question: string;
    type: string;
}

// export question via Question State
export type QuestionState = Question & { answers: string[]};
```
Ensuite préparons un objet qui nous servira pour l'API: **Difficulty**;

```
// export objet question difficulter
export enum Difficulty {

    EASY = "easy",
    MEDIUM = "medium",
    HARD = "hard",
}
```
**enum** permet de définir un ensemble de constantes nommées. L'utilisation des enums peut faciliter la documentation, ou la création d'un ensemble de cas distincts. TypeScript fournit à la fois des enums numériques et des enums basés sur des chaînes de caractères.


Dans ce fichier nous allons fetch l'API externe:

```
// fetch asynchrone sur l'API + les deux objets que l'on a besoin 
export const fetchQuizQuestions = async (amount: number, difficulty: Difficulty) => {

    const endpoint = `https://opentdb.com/api.php?amount=${amount}&difficulty=${difficulty}&type=multiple`;
    const data = await (await fetch(endpoint)).json();
    
    return data.results.map((question: Question) => ({

        ...question,
        answers: shuffleArray([
            
            ...question.incorrect_answers, question.correct_answer,
        ]),
    }));
    
};
```

**shuffleArray()**


**App.tsx**

```

function App() {

    const [loading, setLoading] = useState(false);
    const [questions, setQuestions] = useState<QuestionState[]>([]);
    const [number, setNumber] = useState(0);
    const [userAnswers, setUserAnswers] = useState<AnswerObject[]>([]);
    const [score, setScore] = useState(0);
    const [gameOver, setGameOver] = useState(true);

    const startGame = async () => {};

    const checkAnswer = (e: React.MouseEvent<HTMLButtonElement>) => {};

    const nextQuestion = () => {};

    return (

            <div className="App" >
            
                <h1>React Quiz</h1>
            
                <QuestionCard
                       
                       questionNumber={number + 1}
                       totalQuestions={TOTAL_QUESTIONS}
                       question={questions[number].question}
                       answers={questions[number].answers}
                       userAnswer={userAnswers ? userAnswers[number] : undefined}
                       callback={checkAnswer}
                />
                
                <button className="next" onClick={nextQuestion}>Next Question</button>
               
            
            </div>
    );
}

```

<!-- **API.ts**
```
import { shuffleArray } from './utils';

//il faut détailler l'objet question
export type Question = {

    category: string;
    correct_answer: string;
    difficulty: string;
    incorrect_answers: string[];
    question: string;
    type: string;
}

// export question via Question State
export type QuestionState = Question & { answers: string[]};

// export objet question difficulter
export enum Difficulty {

    EASY = "easy",
    MEDIUM = "medium",
    HARD = "hard",
}

// fetch asynchrone sur l'API + les deux objets que l'on a besoin 
export const fetchQuizQuestions = async (amount: number, difficulty: Difficulty) => {

    const endpoint = `https://opentdb.com/api.php?amount=${amount}&difficulty=${difficulty}&type=multiple`;
    const data = await (await fetch(endpoint)).json();
    
    return data.results.map((question: Question) => ({

        ...question,
        answers: shuffleArray([
            
            ...question.incorrect_answers, question.correct_answer,
        ]),
    }));
    
};
``` -->

Maintenant, allez au milieu du code [ici](./moitié.md)