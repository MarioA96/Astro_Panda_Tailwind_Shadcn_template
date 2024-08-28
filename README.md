# Astro-PandaCss-Tailwind-Shadcn_template

## Guia para integrar Panda con Tailwind (*y eventualmente si se desea con shadcn*)

1. #### Primero debes de instalar Astro
    - Desde aqui: [Astro Build](https://docs.astro.build/en/install-and-setup/#start-a-new-project)

2. #### Luego Debes de instalar Panda (sigue todo los pasos, eventualmente los modificaremos)
    -  Desde aqui: [Panda](https://panda-css.com/docs/installation/astro#install-panda)

3. #### Instala desde la documentacion de Shadcn [a Tailwind](https://ui.shadcn.com/docs/installation/astro) (dale si a todo) **PERO**, antes del paso de ```Run the shadcn-ui init command to setup your project: ...``` omitelo por ahora...

4. #### Agrega la dependencia ```sh npm i autoprefixer```

5. #### Luego revisa el directorio llamado: Panda-Tailwind_template, en este revisa primero el apartado de package.json, que luzca algo parecido

6. #### Despues en archivo ```astro.config.mjs```, remueve la integration de tailwind

```mjs
    //Ejemplo:

    import { defineConfig } from 'astro/config';
    import react from "@astrojs/react";

    // https://astro.build/config
    export default defineConfig({
        integrations: [react()]
    });
```


7. #### Eventualmente en el archivo ```postcss.config.cjs``` debe lucir algo como:
```cjs
    module.exports = {
        plugins: {
            tailwindcss: {},
            '@pandacss/dev/postcss': {},
            autoprefixer: {}
	    }
    };
```

8. #### Agrega en el archivo de ```panda.config.ts```:

```ts
	layers: {},
```


9. #### Despues modifica el archivo de ```tailwind.config.ts``` agregando esto:

```ts
	corePlugins: {
		preflight: false,
	}
```


10. #### Y finalmente el ```index.css``` en el src (**src/index.css**) debe lucir algo como esto:

```css
    @tailwind base;
    @tailwind components;
    @tailwind utilities;

    @layer reset, base, tokens, recipes, utilities;

    /* if you decide to rename layers, you can use it like below */
    /* @layer panda_reset, panda_base, panda_tokens, panda_recipes, panda_utilities; */
```

11. #### Para la parte de integrar ahora Shadcn debemos recurrir a la [documentacion oficial](https://ui.shadcn.com/docs/installation/astro)

- Para esto nos centraremos en la parte de instalacion de tailwind de parte de shadcn:
```npx astro add tailwind``` dando Yes a todo lo que surja

12. #### Editamos el archivo ```tsconfig.json``` luciendo algo como:

```json
    {
    "compilerOptions": {
        // ...
        "baseUrl": ".",
        "paths": {
        "@/*": [
            "./src/*"
        ]
        }
        // ...
    }
    }
```

13. #### Ahora instalamos mediante la linea de comandos lo siguiente: ```npx shadcn-ui@latest init```

- Como configuración recomendada es la siguiente:
```shell
    yes
    Default
    Slate
    .src/styles/global.css
    yes
    <<blank>>
    tailwind.config.mjs
    @/components
    @/lib/utils
    no
    yes
```

14. #### Por el momento evitaremos los siguientes del sitio pasos para llevar a cabo nuestra propia configuracion para evitar colisiones con las clases y dependencias de PandaCss

- Lo que haya generado el global.css agregalo al index.css, ya que ese es al que vamos a estar sirviendo, ¡cuidando el orden!

15. #### Probablemente tengas 2 archivos ```tailwind.config.js``` y otro ```tailwind.config.mjs```, ambos solo asegurate tengan el:

```js
    corePlugins: { preflight: false }
```

- Si es necesario borra el tailwind.config.mjsm de igual forma no hay problema, asi como el global.css. Tambien asegurate que ```tailwind.config.js``` sea ```export default { ... }```

- El archivo ```postcss.config.cjs``` permanece igual:
```js
    module.exports = {
        plugins: {
            tailwindcss: {},
            '@pandacss/dev/postcss': {},
            autoprefixer: {}
        }
    };
```

16. #### Nos volvemos a asegurar que ```astro.config.mjs``` luzca como:

```js
    import { defineConfig } from 'astro/config';
    import react from "@astrojs/react";

    // https://astro.build/config
    export default defineConfig({
    integrations: [react()]
    });
```

17. #### Y listo, ahora si puedes hacer la prueba agregando el boton

```shell 
    npx shadcn-ui@latest add button
```

18. #### Y reuniendo todo en el index.astro como:

```tsx
    ---
    import '../index.css'

    import { css } from '../../styled-system/css';
    import { Button } from '@/components/ui/button';
    ---

        <h1 class={ css({ fontSize: '4xl', fontWeight: 'bold' }) }>Panda</h1>
        <h1 class="text-3xl font-bold">Tailwind</h1>
        <Button>Shadcn</Button>
```

#### BONUS - Podemos ejecutar el comando: ``` npm run prepare ``` y para ver el sitio con ``` npm run dev ```

### RESULTADO ESPERADO: 
[imagen resultado](/public/resultado.png)