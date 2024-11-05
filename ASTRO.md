# Introducción a Astro

Astro es un framework de JavaScript optimizado para construir sitios web rápidos y orientados a contenido. Aquí tienes una breve introducción sobre qué es Astro, para qué sirve y cómo funciona:

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

```astro
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

```astro
yarn astro add --help
```