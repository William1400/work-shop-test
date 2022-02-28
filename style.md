et pour rendre l'application plus belle 
n'oubliez pas d'importer chaque style à votre destination 

Change folder 
App.styles.ts

En typescript, nous importons le style avec un createGlobalStyle dans un fichier de type script
avec ce code nous rendons la position des éléments plus harmonieux comme par exemple l'image de fond, la position du titre et du bouton.

```

import styled, { createGlobalStyle } from 'styled-components';

//@ts-ignore => ignore-Warning
import bgImage from './images/quizzpng.png';

// creer un fonction globale pour le style
export const GlobalStyle = createGlobalStyle`

    html {
        height: 100%;
    }

    body {
        background-image: url(${bgImage});
        background-size: cover;

        margin: 0;
        padding: 0 20px; 

        display: flex;
        justify-content: center;
        align-content: center;
        align-items: center;

        color: white;
    }

    * {

        box-sizing: border-box;
        font-family: 'Catamaran', sans-serif;
    }
`;
```
2
avec le code suivant vous rendrez notre titre, score et textes plus stylisés 
```

// creer un div de style dans une constante
export const Wrapper = styled.div`

    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 20px;

    > p {

        color:#fff;
    }

    .score {

        color: #fff;
        font-size: 2rem;
        margin: 0;
    }

    h1 {
        font-family: Fascinate Inline, Haettenschweiler, 'Arial Narrow Bold', sans-serif;
        background-image: linear-gradient(180deg, #fff, #87F1FF);
        background-size: 100%;
        background-clip: text;

        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
        -moz-background-clip: text;
        -moz-text-fill-color: transparent;
        filter: drop-shadow(2px 2px #0085A3);
        font-size: 70px;
        font-weight: 400;
        text-align: center;
        margin: 2àpx;
    }
```

3
ici, nous pouvons rendre le bouton de démarrage et le bouton suivant plus jolis et plus harmonieux. 
```

.start, .next {

        cursor: pointer;
        background: linear-gradient(180deg, #fff, #FFCC91);
        border: 2px solid #D38558;
        box-shadow: 0px 5px 10px rgba(0, 0, 0, 0.25);
        border-radius: 10px;
        height: 40px;
        margin: 20px 0;
        padding: 0 40px;
    }

    .start {

        max-width: 200px;
    }
`
```

Change folder 
App.tsx

il est maintenant temps d'importer notre style ts dans notre application principale

```
// Import Styles
import { GlobalStyle, Wrapper } from './App.styles';

// change return() block

return (

    <>
        <GlobalStyle />
        <Wrapper>
            <h1>React Quiz</h1>
            {gameOver || userAnswers.length === TOTAL_QUESTIONS ? (

                <button className="start" onClick={startGame}>Start</button>
            ) : null}
            {!gameOver ? <p className="score">Score: {score}</p> : null}
            {loading && <p>Loading Question...</p>}
            {!loading && !gameOver && (

                <QuestionCard
                    questionNumber={number + 1}
                    totalQuestions={TOTAL_QUESTIONS}
                    question={questions[number].question}
                    answers={questions[number].answers}
                    userAnswer={userAnswers ? userAnswers[number] : undefined}
                    callback={checkAnswer}
                />
            )}
            {!gameOver && !loading && userAnswers.length === number + 1 && number !== TOTAL_QUESTIONS - 1 ? (

                <button className="next" onClick={nextQuestion}>Next Question</button>
            ) : null}
        </Wrapper>
    </>
);
```
Change folder 
QuestionCard.styles.tsx
Maintenant, nous allons faire notre style de carte de quiz. 
avec ce code, nous stylisons la lettre de la question et les éléments qu'elle contient

```
import styled from 'styled-components';

export const Wrapper = styled.div`

    max-width: 1100px;
    background: #ebfeff;
    border-radius: 10px;
    border: 2px solid #0085A3;
    padding:20px;
    box-shadow: 0 5px 10px rgba(0,0,0,0.25);
    text-align:center;

    p {
        fonst-size 1rem;
    }
`
```
2
dans le code suivant, nous donnons un style aux boutons de question de sorte qu'ils deviennent verts pour les bonnes réponses et rouges pour les mauvaises réponses. 

```
type ButtonWrapperProps = {

    correct: boolean;
    userClicked: boolean;
}

export const ButtonWrapper = styled.div<ButtonWrapperProps>`

    transition: all 0.3 ease-in-out;

    :hover {

        opacity: 0.8;
    }

    button {

        cursor: pointer;
        user-select: none;
        font-size: 1rem;
        width: 100%;
        height: 40px;
        margin: 5px 0 0;

        /* change  le background color du boutton selon la situation */
        background: ${({ correct, userClicked }) => 
            correct
                ? 'linear-gradient(90deg, #56ffA4, #59BC86)'
                : !correct && userClicked
                ? 'linear-gradient(90deg, #FF5656, #C16868)'
                : 'linear-gradient(90deg, #56ccff,  #6eafb4)'
        };
        border: 2px solid #FFF;
        box-shadow: 1px 2px 0px rgba(0, 0, 0, 0.1);
        border-radius: 10px;
        color: #fff;
        text-shadow: 0px 1px 0px rgba(0, 0, 0, 0.25);
    }
   
`
```

et c'est tout, vous avez maintenant un quiz complet et bien conçu pour jouer et montrer vos compétences en matière de typescriopt