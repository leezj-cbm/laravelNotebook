# Setting up Tailwind CSS for Laravel Projects AND ShadCN

[link to shadcn setup](#shadcn)

Oficial documentation Vite - https://tailwindcss.com/docs/guides/vite  <---- USE THIS!

Oficial documentation Laravel - https://tailwindcss.tw/docs/guides/laravel - This only shows how to do this with app.blade templating engine

## ChatGPT instruction -  using webpack - works USE VITE!

https://chatgpt.com/share/08858bf6-a6a3-49e5-bfbb-1c18c467b56b

If you are using React within your Laravel project, the setup is slightly different because React will be rendering your components inside a designated element within your Blade template. Here is how you can properly set up your Laravel project to use Tailwind CSS with React:

1. Set up tailwind css and laravel mix

```
npm install -D tailwindcss postcss autoprefixer laravel-mix

npx tailwindcss init -p

```

2. Create and configure webpack.mix.js

```js
const mix = require('laravel-mix');
const tailwindcss = require('tailwindcss');

mix.js('resources/js/app.js', 'public/js')
   .react() // For React support
   .postCss('resources/css/app.css', 'public/css', [
       tailwindcss('./tailwind.config.js'),
   ]);

```
3. Configure Tailwind CSS

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    './resources/**/*.blade.php',
    './resources/**/*.js',
    './resources/**/*.jsx',
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}


```


4. Create CSS File

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

```

5. Update blade template Update your main Blade template (e.g., resources/views/app.blade.php) to include the compiled CSS file and the JavaScript bundle for React: 


```html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Laravel React App</title>
    <link rel="stylesheet" href="{{ mix('css/app.css') }}">
</head>
<body>
    <div id="app"></div>
    <script src="{{ mix('js/app.js') }}"></script>
</body>
</html>


```

6. Compile assets ```npm run dev```

7. Try it out now

```js

// resources/js/components/ExampleComponent.jsx
import React from 'react';

const ExampleComponent = () => {
    return (
        <div className="p-6 max-w-sm mx-auto bg-white rounded-xl shadow-md space-y-4">
            <h1 className="text-3xl font-bold underline">Hello world!</h1>
            <p className="text-gray-500">This is a simple example component.</p>
        </div>
    );
}

export default ExampleComponent;
```

## Shadcn

Official documentation here, but need to fill in the blanks ourselves.

1. Run this command ```npx shadcn-ui@latest init```

2. There will be a weird error : npm ERR! path C:\Users\leezj\digiHUB\UsersleezjAppDataRoamingnpm. Create this folder `digiHUB\UsersleezjAppDataRoamingnpm. ` which is not there. THis will get rid of the error.

3. Run the command above again. You will get another error because it cannot find : ```Failed to load jsconfig.json. Couldn't find tsconfig.json```

4. Create a jsconfig.json with the details below. I choose for @ to point directly at resources/js/ 

```json
{
    "compilerOptions": {
        "baseUrl": ".",
        "paths": {
            "@/": ["resources/js/"],
            "ziggy-js": ["./vendor/tightenco/ziggy"]
        }
    },
    "exclude": ["node_modules", "public"]
}

```

5. Try the command ```npx shadcn-ui@latest init``` again. it should work!

```cli


Would you like to use TypeScript (recommended)? no / yes
Which style would you like to use? › Default
Which color would you like to use as base color? › Slate
Where is your global CSS file? › resources/css/app.css
Do you want to use CSS variables for colors? › no / yes
Where is your tailwind.config.js located? › tailwind.config.js
Configure the import alias for components: › @/Components
Configure the import alias for utils: › @/lib/utils
Are you using React Server Components? › no / yes

```

6. It will automatically update the tailwind.config.js. Check it out to verify.

7. Very important !!! Add in ```resources/css/app.css``` to @vite as below!!! This will solve the issue. Or else app cannot detect the CSS file after installing shadcn.

```html
<!DOCTYPE html>
<html lang="{{ str_replace('_', '-', app()->getLocale()) }}">
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1">

        <title inertia>{{ config('app.name', 'Laravel') }}</title>

        <!-- Scripts -->
        @routes
        @viteReactRefresh
        @vite(['resources/css/app.css', 'resources/js/app.jsx', 'resources/js/Pages/{$page["component"]}.jsx'])
        @inertiaHead
    </head>
    <body class="font-sans antialiased">
        @inertia
    </body>
</html>

```