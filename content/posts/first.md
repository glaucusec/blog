---
author: ["glaucusec"]
title: "Using Google Fonts with @next/font in Tailwind CSS v4"
date: "2025-01-30"
description: "Learn how to integrate and optimize Google Fonts in a Next.js project using @next/font with Tailwind CSS v4."
summary: "A step-by-step guide on adding and configuring Google Fonts in Tailwind CSS v4 with @next/font in Next.js."
tags: ["tailwindcss", "nextjs", "google-fonts", "web-performance"]
categories: ["development", "frontend", "nextjs"]
series: ["Next.js & Tailwind Guide"]
ShowToc: true
TocOpen: true
---

## Selecting the required fonts

- Add the required fonts from NextJS to your layout file.
- For me i have added my fonts on a seperate `fonts.ts` file, but you can add the fonts directly to the layout file

```javascript
// fonts.ts
import { Inter, Poppins } from "next/font/google";

export const poppins = Poppins({
  weight: ["400", "600"],
  subsets: ["latin"],
  variable: "--font-poppins",
});

export const inter = Inter({
  subsets: ["latin"],
  variable: "--font-inter",
});
```

- The **Poppins** font is not a variable font, so we need to manually include the required font weights.
  On the other hand, **Inter** is a variable font, allowing it to adapt to any font weight as needed without requiring separate font-weight files.

## Adding the fonts to layout.tsx file

```react
// layout.tsx
import { inter, poppins } from "@/lib/fonts";

export default function RootLayout({
  children,
}: {
  children: React.ReactNode,
}) {
  return (
    <html lang="en">
      <body className={`${inter.className} ${poppins.variable}`}>
        {children}
      </body>
    </html>
  );
}
```

- Here I need the inter font to be applied on my entire nextjs application. we are adding the taiwind generated classname of the `inter` font.
- But for poppins I want the font to be loaded as a CSS variable

### Adding CSS Variable

- Notice on the `fonts.ts` file we have provided a `variable` name. we can use that on the `globals.css`
- On tailwind v4, they removed the config file and everything is simple and can be added from `globals.css` itself.

```css
--font-poppins: var(--font-poppins);
```

- Add the css code like this and now you can use `font-poppins` classname on any element.
- For example, adding the classname like this will apply the font styles.

```html
 <div className="font-poppins container
```
