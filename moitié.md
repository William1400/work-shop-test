### Change folder vers le
**App.tsx**

importez fetchQuizQuestions et ouvrez la console dans le navigateur pour voir les paramètres, et la variété des difficultés qui ont été générées 
```
// Types 
import { QuestionState, Difficulty } from './API';



//dans la function App() {
    console.log(fetchQuizQuestions(TOTAL_QUESTION, Difficulty.EASY));

```



 Créer le fichier et mettre **utils.ts**

ici, nous créons un *array* pour passer la fonction aléatoire avec random et générer aléatoirement nos questions 

```
// quick fix random const
export const shuffleArray = (array: any[]) =>

    [...array].sort(() => Math.random() - 0.5);

// aller regarder la promise localhost
```

Change folder vers **App.tsx**

nous appelons notre nouvelle fonction pour l'application,
et passer le nouveau paramètre de difficulté et son type et une nouvelle constante où nous donnerons le nombre de questions de l'application et ses types
```
import { fetchQuizQuestions } from './API';


export type AnswerObject = {

    question: string;
    answer: string;
    correct: boolean;
    correctAnswer: string;
}

// nombre de question que l'on va chercher dans l'api
const TOTAL_QUESTIONS = 15;



// Styles
```

Toujours en **App.tsx**
transformer le journal de la console en commentaire  et maintenant nous commençons à déclarer les actions qui vont se produire comme le game over et la continuation du jeu (sans game over), et faire une nouvelle console pour voir les questions.

```


// Dans le function App() {

    // console.log(fetchQuizQuestions(TOTAL_QUESTION, Difficulty.EASY));
    console.log(questions);


    // creer la fonction qui lance le jeu dans le
    //const startGame = async () => {

        setLoading(true);
        setGameOver(false);

        const newQuestions = await fetchQuizQuestions(TOTAL_QUESTIONS, Difficulty.EASY);

        setQuestions(newQuestions);
        setScore(0);
        setUserAnswers([]);
        setNumber(0);
        setLoading(false);
    };

    //avant le 
    //const checkAnswer = (e: React.MouseEvent<HTMLButtonElement>) => { };

    //const nextQuestion = () => { };

```



Encore dans **App.tsx**

remplacez le code dans le return par celui ci-dessous 

maintenant nous donnons vie à l'application et à sa fonctionnalité, et les fonctions comme démarrer le jeu, répondre aux questions, avancer, vérifier si c'est correct ou non et la fin du jeu en cas d'erreurs. regardez bien les codes car on a passé les paramètres et on a créé les fonctionnalités avec typescript
```
    return (

        <div className="App" >

            <h1>React Quiz</h1>
            {gameOver || userAnswers.length === TOTAL_QUESTIONS ? (

                <button className="start" onClick={startGame}>Start</button>
            ) : null}
            {!gameOver ? <p className="score">Score: {score}</p> : null}
            {loading && <p>Loading Question...</p>}
            {!loading && !gameOver && (

                {/* <QuestionCard
                       
                       questionNumber={number + 1}
                       totalQuestions={TOTAL_QUESTIONS}
                       question={questions[number].question}
                       answers={questions[number].answers}
                       userAnswer={userAnswers ? userAnswers[number] : undefined}
                       callback={checkAnswer}
                /> */}
            )}

            {!gameOver && !loading && userAnswers.length === number + 1 && number !== TOTAL_QUESTIONS - 1 ? (

                <button className="next" onClick={nextQuestion}>Next Question</button>
            ) : null}

        </div >
    );
}


```


Change folder vers le **QuestionCard.tsx**

nous retournons à notre QuestionCard où nous allons déclarer les paramètres et déclarer nos valeurs booléennes
```
// changer le bloc avec:

<div>
    {answers.map(answer => (
        <div key={answer}>
            <button disabled={userAnswer ? true : false} value={answer} onClick={callback}>
                <span dangerouslySetInnerHTML={{ __html: answer }}></span>
            </button>
        </div>
    ))}
</div>

```


Nous revenons à **App.tsx**
maintenant dans la constante *checkAnswer*

ici nous créons notre score complet pour voir le nombre de hits 
```
// Implementer checkAnswer Fonction

//dans le cadre de la constante const checkAnswer = (e: React.MouseEvent<HTMLButtonElement>) => {

    if (!gameOver) {

        //Users Answer
        const answer = e.currentTarget.value;

        // Check answer against correct answer
        const correct = questions[number].correct_answer === answer;

        // Add score if answer is correct
        if (correct) setScore(prev => prev + 1);
        
        // Save answer in the array for user userAnswers
        const answerObject = {

            question: questions[number].question,
            answer,
            correct,
            correctAnswer: questions[number].correct_answer,
        };

        setUserAnswers((prev) => [...prev, answerObject]);
    }
//};

//test localhost
```

Maintenant, dans la fonction **nextQuestion**, mettez ce code 

ajouter aussi ces lignes supplémentaires pour rendre le code précédent fonctionnel


```


// Implementer NextQuestion Fonction
//maintenant dans la fonction const nextQuestion = () => {

    // Move on the next question if not the last question
    const nextQuestion = number + 1;

    if (nextQuestion === TOTAL_QUESTIONS) {

        setGameOver(true);
    } else {

        setNumber(nextQuestion);
    }
//};

//test localhost
```


Change ficher vers le
**QuestionCard.tsx**

et pour finir d'importer les objets sur votre carte de questions
```

// import types 

import { AnswerObject } from '../App';

```