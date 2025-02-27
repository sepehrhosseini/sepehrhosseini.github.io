---
title: 'Translating Your Next.js Apps'
date: '2021-09-17'
formattedDate: '17 September, 2021'
description: 'If you intend your product to be used by people residing in non-English speaking countries, you will have to make your app multilingual. In this post, we will be looking at how you can quickly & easily translate your app into multiple languages.'
---

# 🌏 Internationalisation & It's Importance

People's accessibility to the web has been a concern of many developers and companies. 

Internationalisation is the process of designing/developing our application to work well with users from different languages and regions.

To understand the importance of Internationalisation, around 989 million people from China have access to the internet. Meanwhile, [Less than 1% of China's population can speak English](https://en.wikipedia.org/wiki/List_of_countries_by_English-speaking_population). 

In this post, we'll look at how we can easily translate our [Next.js] application into different languages. 

# 🚀 Project Setup

For this project, we'll use the following libraries:

 - [Next.js]
 - [next-i18next](https://next-i18next.com/)

```bash
npx create-next-app intl --ts
```

This will setup a new [Next.js] project which will use [TypeScript](https://typescriptlang.org) and [Yarn](https://yarnpkg.com/) for installing packages.

Note: If you just want to use regular [JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript), remove the --ts option

Now that we have our project, let's start the development server

```bash
yarn dev

ready - started server on 0.0.0.0:3000, url: http://localhost:3000
```

Our development server should now be running on [localhost](http://localhost:3000)

# 🗣️ Translating Our App

Now that we have the basics setup and ready to go, we can begin translating our application. 

Let's install the [next-i18next](https://next-i18next.com/) library.

```bash
yarn add next-i18next
```

Let's open our pages/index.tsx file.

At the top, we can setup and use the useTranslation() hook provided by the package.

```tsx
import { useTranslation } from 'next-i18next'
```

and inside our Home component:

```
const { t } = useTranslation('common')
```

Since we're using Next.js, we'll also need to setup our application a bit differently.

At the top of the file, import the serverSideTranslations component:

```tsx
import { serverSideTranslations } from 'next-i18next/serverSideTranslations'
```

Now, below the Home component, we need to use [getStaticProps](https://nextjs.org/docs/basic-features/data-fetching#getstaticprops-static-generation) to setup our translations.

```tsx
export const getStaticProps = async ({ locale }) => ({
  props: {
    ...(await serverSideTranslations(locale, ['common'])),
  },
})
```

Now, let's open our pages/_app.tsx file and make sure we export it with translations. 

```tsx
import '../styles/globals.css'
import type { AppProps } from 'next/app'

import { appWithTranslation } from 'next-i18next'

function MyApp({ Component, pageProps }: AppProps) {
  return <Component {...pageProps} />
}

export default appWithTranslation(MyApp)
```

Awesome! Let's now start to configure our translations with Next.js

At the root of the file, create a new file named next-i18next.config.js

Inside it, paste the following content. Replace the languages in the locale array with the language you wish to translate your app into.

My mother tongue is [Hindi](https://en.wikipedia.org/wiki/Hindi), so I'll be using it in this example to ensure accurate translations. 

```js
module.exports = {
  i18n: {
    defaultLocale: 'en',
    locales: ['en', 'hn'],
  },
}
```

Now, open your next.config.js file. If it's not created already, create it.

Since I'm using [TypeScript](https://typescriptlang.org), it already created the file with some pre-configured settings. 

```js
const { i18n } = require('./next-i18next.config')

/** @type {import('next').NextConfig} */
module.exports = {
  reactStrictMode: true,
  i18n
}
```

Next, we'll need to setup the lines we want to be translated. 

Open up pages/index.tsx

Let's take a look at the header which says "Welcome to Next.js!"

```tsx
<h1 className={styles.title}>
  Welcome to <a href='https://nextjs.org'>Next.js!</a>
</h1>
```

Let's replace this with the following code

```tsx
<h1 className={styles.title}>{t('header')}</h1>
```

Now, if you save it, you'll see instead of "Welcome to Next.js!" it shows "header", and that's because we haven't setup our translation yet.

Create a new file in the directory public/locales/en/common.json

```json
{
  "header": "Welcome to Next.js!",
}
```

Now, when we reload the page, we should see it say "Welcome to Next.js!"

Now to translate it in our secondary language, let's create another new file in public/locales/hn/common.json

The closest translation to "Welcome to Next.js!" in [Hindi](https://en.wikipedia.org/wiki/Hindi) is "Next.js में आपका स्वागत है!" (Using English words for English names).

```json
{
  "header": "Next.js में आपका स्वागत है!"
}
```

Now, when we open up [localhost:3000/hn](http://localhost:3000/hn), we should see our app translated in [Hindi](https://en.wikipedia.org/wiki/Hindi)!

That's it for today. We took at look at how we can translate our Next.js apps in other languages with an example!

[Next.js]: https://nextjs.org
