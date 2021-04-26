# React

**JSX** es la sintaxis de react, esta sintaxis existe para que sea mas facil escribir react. Un transpilador (como Babel o Typescript) lo convierte a js para que todos los navegadores puedan comprenderlo. 

 ```js
 <div className="foo">
   <Sum a={1} b={4} />
 </div>
 ```

Se podría traducir ha esto en **JS**

```js
React.createElement('div', {class: 'foo'}, React.createElement(Sum, {a:1, b:4}, null))
```



## State, Props and Keys

El ***state\*** es un objeto plano que se administra dentro del componente (similar a una variable dentro de una *function*)

Las ***props\*** son propiedades (entradas arbitrarias) que se pasan al componente (así como un parámetro) y devuelven a React elementos que describen lo que debe aparecer en pantalla

Las ***key\*** ayudan a react a identificar que items han cambiado, agregados o eliminados. El fin es que cada elemento tenga una identidad estable entre sus hermano, no globales. Normalmente se usan las ***id\*** de los elementos del array, pero también se puede usar el ***index\*** (no recomendable, usar como ultima instancia)

## Hooks

Los hooks solo funcionan en componentes funcionales, como el siguiente;

```js
example = () => { console.log('hi') }
```

Todos los **hooks** parten con la palabra **use*** y todos de alguna manera contribuyen al **lifecycle** del componente.



### useState

...

### useEffect

Toma dos parámetros, el primero el una función de *callback* que es cuando el componente se monta (similar a componentDidMount) dentro de esta podemos retornar una función que se ejecutaría cuando el componente sale (similar a componentWillUnmount). El segundo parámetro es un *array* que contiene una lista de dependencias del componente, lo que vaya ahí, vez que se actualice para que el *useEffect* se ejecute de nuevo, si se deja vacío solo se ejecutara una vez, si no se coloca se ejecutara siempre.

```js
const ExampleEffect = (props) => {
  useEffect(() => {
    console.log('hello')
    setTimeout(() => { alert('hello') }, 2000)
    return (() => {
        console.log('cleanup on change of props');
    })
  }, [])
  
  return (
    <div className="effectExample">
    	//some content
  	</div>
  )
}
```



### useContext

Comparte una variable que puede ser llamada desde cualquier componente de la aplicación, debería usarse cuando esta data se puede considerar *global* (UI theme, authenticated user, etc).

```js
export const ExampleContext = React.createContext()

// On our wrapper
const someValue = 'Some Value'

<ExampleContext.Provider value={someValue}>
  <ExampleComponent />
</ExampleContext.Provider>

// On ExampleComponent
import { ExampleContext } from './Wrapper'

const context = useContext(ExampleContext)

<div className="example">
  <div className="exampleValue">Here is our {context}</div>
</div>

// Output: 'Here is our Some Value'
```



### useReducer

Es una funcion que toma un **state** previo como primer parámetro y un **action** como segundo parametro para retornar un nuevo **state** y dentro de en un **dispatch method**. Sirve para manejar estados logicos complejos y tiene una similitud a *.reducer()*

```js
const initialState = 0
const reducer = (state, action) => {
  switch (action) {
    case 'increment': return state + 1
    case 'decrement': return state - 1
    case 'reset': return 0
    default: throw new Error('Error inesperado')
  }
}

const ReducerExample = () => {
  const [count, dispatch] = useReducer(reducer, initialState)
  return (
    <div className="example">
    	{count}
			<button onClick={() => dispatch('increment')}>+1</button>
			<button onClick={() => dispatch('decrement')}}>-1</button>
			<button onClick={() => dispatch('reset')}}>reset to 0</button>
		</div>
  )
}
```



### useCallback

...



### useMemo

...



## Presentational/Container Components

Los **presentational components** renderizan *data* a traves de **props**, son inconcientes de la *data* que renderizaran, contienen muy poca lógica, no saben si la *data* que renderizaran cambiara y normalmente son renderizados dentro de un *container component*.

Por otro lado, los **container components** tiene la logica de la app, por ejemplo pueden hacer los *fetch a la data*, cambiar los *state*, entre otras cosas, tambien rederizan los *presentional components* en el metodo render.



## Higher Order Component (HOC)

Los **HOCs** son una funcion que toma un componente como argumento y retornan un nuevo componente. La idea de estos es descomponer la logica en funciones mas pequeñas y simples para ser reutilizadas a lo largo de la app, así evitando efectos secundarios y hacer que sea mas fácil debuguear y mantener la app.



## Composition

La composicion en react se encarga de resolver dos problemas comunes, **containment** y **specialization**, asi evitando los problemas de los *class components* que traen por si la *inheritance*. 

**containment:** Con el podemos pasar lo que queramos como *children* al componente padre, de esta manera no heredamos funciones ni relaciones de estos y por ende es mas facil cambiarlo en el futuro.

**specialization:** Existen ciertos casos que son muy similares unos con otros, para evitar tener que tener muchos elementos que comparten herencia. La composicion puede generar componentes mas especificos usando uno ya hecho y pasando nuevos elementos a traves de *props*.



***Tip**: Puedes colocar la palabra **debugger** en alguna parte del para forzar el debugger del navegador.