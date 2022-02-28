# GET STARTED REACT TYPESCRIPT

## Installation

Pour démarrer un nouveau projet react app avec TypeScript, vous pouvez exécuter :

```npx create-react-app my-app --template typescript```

Pour ajouter TypeScript à un projet React App existant, commencez par l'installer :

```npm install --save typescript @types/node @types/react @types/react-dom @types/jest```

## BASIC PROPS

Définir des props pour un component

```
interface AppProps{
  headerText: string;
  message?: string;
}

function App({ headerText, message }: AppProps) {
  return (
    <div className="App">
      <h1>{headerText}</h1>
      {message && <p>{message}</p>}
    </div>
  );
}
```

Attention ! Il faut définir le type des props avant de les utiliser.
Ici ``{ headerText, message}`` ont le type définit par **AppProps**

ici:
```
interface AppProps {
    headerText: string;
    message?: string;
}
```

Props optionnel:

Il faut préciser si le props est optionnel au risque d'avoir une erreur. Ici donc il peut y avoir ou pas de messages dans le components.

``message?: string``


## FUNCTION COMPONENTS
## HOOKS
**useState**:

- Type Inference vs Type Annotation
- Union Type
- Organizing interfaces
  
```
interface User{
    name: string;
    age: number;
}

function App () {
    const [user, setUser] = useState<User | null>(null);

    const fetchUser = () => setUser({
        name: 'Michel',
        age: 23,
})


  return (
    <div className="App">
        <h1>Use State</h1>
        <button onClick={fetchUser}> Fetch User On Click</button>
        {user && <p>{`Bonjour ${user.name}`}</p>}
    </div>
  );
}

```
## CLASS COMPONENTS
## TYPING defaultProps
## FORMS & EVENTS
## CONTEXT