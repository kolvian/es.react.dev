---
title: React DOM APIs
---

<Intro>

El paquete `react-dom` incluye métodos que sólo son compatibles con aplicaciones web (aquellas que se ejecutan en el entorno del DOM del navegador). No son compatibles con React Native.

</Intro>

---

## APIs {/*apis*/}

Puedes importar las siguientes APIs en tus componentes, pero su uso es poco común:

* [`createPortal`](/reference/react-dom/createPortal) permite renderizar componentes hijos en una parte diferente del árbol del DOM.
* [`flushSync`](/reference/react-dom/flushSync) permite forzar a React a actualizar el estado y el DOM de manera síncrona.

## Resource Preloading APIs {/*resource-preloading-apis*/}

These APIs can be used to make apps faster by pre-loading resources such as scripts, stylesheets, and fonts as soon as you know you need them, for example before navigating to another page where the resources will be used.

[React-based frameworks](/learn/start-a-new-react-project) frequently handle resource loading for you, so you might not have to call these APIs yourself. Consult your framework's documentation for details.

* [`prefetchDNS`](/reference/react-dom/prefetchDNS) lets you prefetch the IP address of a DNS domain name that you expect to connect to.
* [`preconnect`](/reference/react-dom/preconnect) lets you connect to a server you expect to request resources from, even if you don't know what resources you'll need yet.
* [`preload`](/reference/react-dom/preload) lets you fetch a stylesheet, font, image, or external script that you expect to use.
* [`preloadModule`](/reference/react-dom/preloadModule) lets you fetch an ESM module that you expect to use.
* [`preinit`](/reference/react-dom/preinit) lets you fetch and evaluate an external script or fetch and insert a stylesheet.
* [`preinitModule`](/reference/react-dom/preinitModule) lets you fetch and evaluate an ESM module.

---

## Puntos de entrada {/*entry-points*/}

El paquete `react-dom` proporciona dos puntos de entrada adicionales:

* [`react-dom/client`](/reference/react-dom/client) incluye APIs para renderizar componentes de React en el cliente, es decir, en el navegador.
* [`react-dom/server`](/reference/react-dom/server) incluye APIs para renderizar componentes de react en el servidor.

---

## APIs obsoletas {/*deprecated-apis*/}

<Deprecated>

Las siguientes APIs se eliminarán en una próxima versión mayor de React.

</Deprecated>

* [`findDOMNode`](/reference/react-dom/findDOMNode) busca el nodo de DOM más cercano correspondiente a una instancia de componente de clase.
* [`hydrate`](/reference/react-dom/hydrate) monta un árbol en el DOM creado a partir de HTML generado en el servidor. En su lugar, se recomienda usar [`hydrateRoot`](/reference/react-dom/client/hydrateRoot).
* [`render`](/reference/react-dom/render) monta un árbol en el DOM. En su lugar, se recomienda usar [`createRoot`](/reference/react-dom/client/createRoot).
* [`unmountComponentAtNode`](/reference/react-dom/unmountComponentAtNode) desmonta un árbol del DOM. En su lugar, se recomienda usar [`root.unmount()`](/reference/react-dom/client/createRoot#root-unmount).
