# Introducción a Astro

Astro es un framework de JavaScript optimizado para construir sitios web rápidos y orientados a contenido.
En esta ocación trabajaremos con la Api de SpaceX y sus diferentes lanzamietos para tener como ejemplo de base el funcionamiento de astro.
Aquí tienes una breve introducción sobre qué es Astro, para qué sirve y cómo funciona:

## ¿Qué es Astro?

Astro es un framework que se centra en mejorar el rendimiento de los sitios web mediante la renderización de componentes en el servidor y el envío de HTML ligero al navegador, sin sobrecarga innecesaria de JavaScript.

## Para qué sirve Astro?

Astro es ideal para construir sitios web como:

* Sitios de marketing
* Blogs
* Páginas de e-commerce
* Documentación
* Portafolios
* Páginas de aterrizaje (landing pages)
* Aplicaciones SaaS
* Más...

## Cómo funciona Astro

1. Renderizado en el servidor: Astro renderiza los componentes en el servidor antes de enviar el HTML al navegador, lo que mejora el rendimiento y las métricas de SEO.
2. HTML ligero: Solo se envía el HTML necesario, eliminando JavaScript innecesario.
3. Compatibilidad con contenido: Astro trabaja con contenido de diversas fuentes, como sistemas de gestión de contenido (CMS), APIs externas y archivos del sistema de archivos.
4. Extensibilidad: Puedes extender Astro con tus herramientas favoritas, incluyendo componentes de UI, bibliotecas CSS, temas, integraciones y más.
5. Flexibilidad: Astro es compatible con todos los principales frameworks de UI, como React, Vue, Svelte, y más.

```js
---
import BuyButton from '../components/BuyButton.jsx';
import { getProductDetails } from 'ecommerce-package';
import ProductPageLayout from '../layouts/ProductPageLayout.astro';

const product = await getProductDetails(Astro.params.slug);
---

<ProductPageLayout>
  <img src={product.imageUrl} alt={product.imageAlt} />
  <h2>{product.name}</h2>
  <BuyButton id={product.id} client:load />
</ProductPageLayout>

```

Astro es una herramienta poderosa para construir sitios web rápidos y eficientes, con una arquitectura flexible que permite integrar diversas tecnologías y herramientas.

```js
yarn astro add --help
```

# Fetching de Datos / Iteración de Elementos

Astro permite obtener datos de APIs o fuentes externas en tiempo de construcción o mediante renderizado dinámico. La iteración de elementos en listas se realiza mediante estructuras familiares como `.map()` en JavaScript.

Ejemplo:

```js
---
const res = await fetch('https://jsonplaceholder.typicode.com/posts');
const posts = await res.json();
---

<ul>
  {posts.map((post) => (
    <li key={post.id}>
      <a href={`/posts/${post.id}`}>{post.title}</a>
    </li>
  ))}
</ul>

```
# Tipos en TypeScript + Console Ninja

Astro soporta TypeScript, lo que permite definir tipos de datos, mejorando la confiabilidad y evitando errores. `console.log` es una herramienta útil para inspeccionar datos y depurar problemas en el navegador o en el servidor.

```js
---
import type { APIPost } from '../types';

interface APIPost {
  id: number;
  title: string;
}

const res = await fetch('https://jsonplaceholder.typicode.com/posts');
const posts: APIPost[] = await res.json();
---

<script>
  console.log('Posts fetched:', posts);
</script>

<ul>
  {posts.map((post) => (
    <li key={post.id}>{post.title}</li>
  ))}
</ul>

```

# Renderizado Condicional

Astro permite mostrar u ocultar contenido basado en condiciones, lo que es útil para personalizar la interfaz según el estado del usuario o los datos disponibles.

```js
---
const user = { name: 'Jane', loggedIn: true };
---

{user.loggedIn ? (
  <p>Welcome, {user.name}!</p>
) : (
  <p>Please log in.</p>
)}

```

# Páginas de Error 404

Una página personalizada de error 404 permite ofrecer una mejor experiencia de usuario cuando alguien intenta acceder a una ruta no existente.

Ejemplo:
Crea `src/pages/404.astro:`
```js
---
const title = "Page Not Found";
---

<html>
  <head>
    <title>{title}</title>
  </head>
  <body>
    <h1>404 - {title}</h1>
    <p>Oops! The page you're looking for doesn't exist.</p>
    <a href="/">Go back to Home</a>
  </body>
</html>

```
# Páginas Dinámicas (getStaticPaths)

`getStaticPaths` permite generar rutas dinámicas en tiempo de construcción. Esto es ideal para sitios con contenido basado en datos, como blogs o catálogos.
Ejm Archivo: `src/pages/posts/[id].astro`

```js
---
export async function getStaticPaths() {
  const res = await fetch('https://jsonplaceholder.typicode.com/posts');
  const posts = await res.json();

  return posts.map((post) => ({
    params: { id: post.id.toString() },
  }));
}

const { id } = Astro.params;
const res = await fetch(`https://jsonplaceholder.typicode.com/posts/${id}`);
const post = await res.json();
---

<h1>{post.title}</h1>
<p>{post.body}</p>
<a href="/">Go back to Home</a>

```

# Páginas Dinámicas (SSR - Server-Side Rendering)

SSR permite renderizar páginas dinámicamente en el servidor para cada solicitud, ideal para contenido que cambia frecuentemente o depende de la sesión del usuario.

Ejemplo:

Archivo: `src/pages/api/[id].astro`

```js
---
export const prerender = false;

const { id } = Astro.params;
const res = await fetch(`https://jsonplaceholder.typicode.com/posts/${id}`);
const post = await res.json();
---

<h1>{post.title}</h1>
<p>{post.body}</p>

```
# Hybrid View Transitions (Introducción)

Las transiciones de vista híbridas permiten crear experiencias fluidas entre páginas, combinando la velocidad del prerenderizado con transiciones animadas

Ejemplo:
1. Habilita en `astro.config.mjs:`

```js
export default defineConfig({
  experimental: {
    hybridRendering: true,
  },
});

```
2. Usa un framework (React, Solid.js) con transiciones animadas.

# Componentes Interactivos (Las Islas)

Astro utiliza un enfoque de "islas", donde solo las partes interactivas de la página cargan JavaScript, mejorando el rendimiento general.

Ejemplo:
```js
---
// Usamos un componente de React
import Counter from '../components/Counter.jsx';
---

<h1>Welcome to Astro!</h1>
<Counter client:load />

```
Archivo `Counter.jsx:`

```js
import { useState } from 'react';

export default function Counter() {
  const [count, setCount] = useState(0);
  return (
    <button onClick={() => setCount(count + 1)}>
      Count: {count}
    </button>
  );
}

```
# Persistencia de Información

```tsx
---
import HeaderButton from "./HeaderButton.astro";
import {Counter} from "../components/Counter.tsx"

---

<header transition:persist class="py-8 px-4 mx-auto max-w-xl lg:py-16 lg:px-6">
    <div class="mx-auto text-center mb-8 lg:mb-16">
        <h2 class="mb-4 text-5xl tracking-tight font-extrabold text-white">
            Space Launches
        </h2>
        <p class="font-light text-gray-500 sm:text-xl dark:text-gray-400">
            All the information about SpaceX Launches
        </p>
    </div>

    <Counter client:visible />
    <Counter client:visible />
    
    <nav class="flex flex-col items-center justify-between w-full text-center md:flex-row">
    </nav>

</header>
```
# Persistencia de Información en Formato Markdown

Astro permite integrar contenido en formato Markdown fácilmente, ideal para blogs, documentación o cualquier sitio basado en texto estructurado.

Ejemplo:
Archivo: `src/content/posts/my-post.md`

```js
---
title: "My First Post"
date: "2024-11-01"
---

This is a blog post written in Markdown.

```
Archivo: `src/pages/posts/[slug].astro`
```js
---
import { getCollection } from 'astro:content';

export async function getStaticPaths() {
  const posts = await getCollection('posts');
  return posts.map((post) => ({
    params: { slug: post.slug },
  }));
}

const { slug } = Astro.params;
const post = await getCollection('posts', slug);
---

<h1>{post.data.title}</h1>
<p>{post.body}</p>

```
# Conclusión
Astro 3 ofrece herramientas poderosas y flexibles que permiten optimizar el desarrollo web, adaptándose a proyectos dinámicos, estáticos o híbridos. Para más detalles, consulta la documentación oficial de Astro.


![Tux, the Linux mascot](/src//assets/images/image.png)