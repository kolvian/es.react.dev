---
title: renderToString
---

<Pitfall>

`renderToString` no es compatible con transmisión (*streaming*) o espera de datos. [Ver alternativas.](#alternatives)

</Pitfall>

<Intro>

`renderToString` renderiza un árbol de React como una cadena de HTML.

```js
const html = renderToString(reactNode)
```

</Intro>

<InlineToc />

---

## Referencia {/*reference*/}

### `renderToString(reactNode)` {/*rendertostring*/}

En el servidor, llama a `renderToString` para renderizar tu aplicación a HTML.

```js
import { renderToString } from 'react-dom/server';

const html = renderToString(<App />);
```

En el cliente, llama a [`hydrateRoot`](/reference/react-dom/client/hydrateRoot) para hacer que el HTML generado por el servidor sea interactivo.

[Ver más ejemplos a continuación.](#usage)

#### Parámetros {/*parameters*/}

* `reactNode`: Un nodo de React que deseas renderizar como HTML. Por ejemplo, un nodo JSX como `<App />`.

#### Returns {/*returns*/}

Una cadena de caracteres HTML.

#### Advertencias {/*caveats*/}

* `renderToString` tiene un soporte limitado para Suspense. Si un componente suspende, `renderToString` inmediatamente envía su fallback como HTML.

* `renderToString` funciona en el navegador, pero [no se recomienda](#removing-rendertostring-from-the-client-code) usarlo en el código del cliente.

---

## Uso {/*usage*/}

### Renderizar un árbol de React como HTML en una cadena de caracteres {/*rendering-a-react-tree-as-html-to-a-string*/}

Llama a `renderToString` para renderizar tu aplicación como una cadena de caracteres HTML que puedes enviar con la respuesta del servidor:

```js {5-6}
import { renderToString } from 'react-dom/server';

// La sintaxis del manejador de rutas depende de tu framework de backend.
app.use('/', (request, response) => {
  const html = renderToString(<App />);
  response.send(html);
});
```

Esto producirá la salida HTML inicial no interactiva de tus componentes de React. En el cliente, deberás llamar a [`hydrateRoot`](/reference/react-dom/client/hydrateRoot) para *hidratar* ese HTML generado por el servidor y hacerlo interactivo.


<Pitfall>

`renderToString` no es compatible con transmisión o espera de datos. [Ver alternativas.](#alternatives)

</Pitfall>

---

## Alternativas {/*alternatives*/}

### Migración de `renderToString` a un método de transmisión en el servidor {/*migrating-from-rendertostring-to-a-streaming-method-on-the-server*/}

`renderToString` devuelve una cadena de caracteres de inmediato, por lo que no admite transmisión (*streaming*) o espera de datos.

<<<<<<< HEAD:beta/src/content/reference/react-dom/server/renderToString.md
Cuando sea posible, recomendamos utilizar estas alternativas totalmente funcionales:
=======
When possible, we recommend using these fully-featured alternatives:
>>>>>>> f37ef36b070730ce8abd68860388179ed4284638:src/content/reference/react-dom/server/renderToString.md

* Si utilizas Node.js, utiliza [`renderToPipeableStream`.](/reference/react-dom/server/renderToPipeableStream)
* Si utilizas Deno o una versión moderna de runtime con [Web Streams](https://developer.mozilla.org/en-US/docs/Web/API/Streams_API), utiliza [`renderToReadableStream`.](/reference/react-dom/server/renderToReadableStream)

Puedes continuar utilizando `renderToString` si tu entorno de servidor no admite *streams*.

---

### Eliminar `renderToString` del código del cliente {/*removing-rendertostring-from-the-client-code*/}

A veces, se usa `renderToString` en el cliente para convertir algún componente en HTML.

```js {1-2}
// 🚩 Innecesario: usar renderToString en el cliente
import { renderToString } from 'react-dom/server';

const html = renderToString(<MyIcon />);
console.log(html); // Por ejemplo, "<svg>...</svg>"
```

Importar `react-dom/server` **en el cliente**  aumenta innecesariamente el tamaño de tu paquete y debe evitarse.  Si necesitas renderizar algún componente como HTML en el navegador, utiliza [`createRoot`](/reference/react-dom/client/createRoot) y lee el HTML desde el DOM:

```js
import { createRoot } from 'react-dom/client';
import { flushSync } from 'react-dom';

const div = document.createElement('div');
const root = createRoot(div);
flushSync(() => {
  root.render(<MyIcon />);
});
console.log(div.innerHTML); // Por ejemplo, "<svg>...</svg>"
```

La llamada a [`flushSync`](/reference/react-dom/flushSync) es necesaria para que el DOM se actualice antes de leer su propiedad [`innerHTML.`](https://developer.mozilla.org/en-US/docs/Web/API/Element/innerHTML)

---

## Solución de problemas {/*troubleshooting*/}

### Cuando un componente se suspende, el HTML siempre contiene un fallback {/*when-a-component-suspends-the-html-always-contains-a-fallback*/}

`renderToString` no es compatible completamente con Suspense.

<<<<<<< HEAD:beta/src/content/reference/react-dom/server/renderToString.md
Si algún componente se suspende (Por ejemplo, porque está definido con [`lazy`](/reference/react/lazy) o busca datos), `renderToString` no esperará a que se resuelva su contenido. En su lugar, `renderToString` encontrará el límite de [`<Suspense>`](/reference/react/Suspense) más cercano por encima y renderizará su prop `fallback` en el HTML. El contenido no aparecerá hasta que se cargue el código del cliente.
=======
If some component suspends (for example, because it's defined with [`lazy`](/reference/react/lazy) or fetches data), `renderToString` will not wait for its content to resolve. Instead, `renderToString` will find the closest [`<Suspense>`](/reference/react/Suspense) boundary above it and render its `fallback` prop in the HTML. The content will not appear until the client code loads.

To solve this, use one of the [recommended streaming solutions.](#migrating-from-rendertostring-to-a-streaming-method-on-the-server) They can stream content in chunks as it resolves on the server so that the user sees the page being progressively filled in before the client code loads.
>>>>>>> f37ef36b070730ce8abd68860388179ed4284638:src/content/reference/react-dom/server/renderToString.md

Para resolver esto, utiliza una de las [soluciones de streaming recomendadas.](#migrating-from-rendertostring-to-a-streaming-method-on-the-server) Pueden transmitir contenido en trozos a medida que se resuelve en el servidor para que el usuario vea cómo se rellena la página progresivamente antes de que se cargue el código del cliente.
