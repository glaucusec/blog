---
author: ["glaucusec"]
title: "Understanding Caching in Next.js 15"
date: "2025-03-13"
description: "Learn about Caching in Next.js 15"
summary: "A step-by-step guide on Caching in Next.js 15"
tags: ["caching", "nextjs"]
categories: ["caching", "frontend", "nextjs"]
series: ["Caching", "Next.js"]
ShowToc: true
TocOpen: true
---

# Mastering Caching in Next.js 15

Next.js 15 provides **powerful caching mechanisms** that optimize performance by reducing redundant computations and API calls. Understanding these caching strategies ensures that your application is **fast, efficient, and scalable**.

This guide covers:
- **Static Site Generation (SSG)**
- **Incremental Static Regeneration (ISR)**
- **Server-Side Rendering (SSR)**
- **Client-Side Data Fetching**
- **Full Route and API Revalidation with `revalidatePath`**
- **Best Practices for Choosing a Caching Strategy**

---

## Static Site Generation (SSG)

**Static Site Generation (SSG)** allows Next.js to generate HTML at **build time**, which is then served as static files. This significantly boosts performance since pages do not require server processing on each request.

### How It Works:
- During development (`npm run dev`), pages regenerate on every reload.
- After building (`npm run build`), static files are pre-generated and **served instantly** on every request.

### When to Use SSG?
Use SSG for pages that **rarely change** and do not depend on user-specific data:
- **Privacy Policy**
- **Contact Us**
- **Blog Posts (if they don’t change often)**
- **Marketing Pages**

### Example:
```tsx
export async function getStaticProps() {
  const data = await fetchData();
  return { props: { data } };
}
```

## Incremental Static Regeneration (ISR)

ISR allows periodic re-generation of static pages without requiring a full site rebuild. This is useful when data updates every few minutes or hours.

### How It Works:
- A cached version of the page is served.
- If the page is older than the revalidation period, Next.js regenerates it in the background.
- The next visitor sees the updated version.

### When to Use ISR?
- Blogs with frequent updates (e.g., refresh content every hour).
- Product pages (e.g., refresh stock availability every 10 minutes).
- News articles (e.g., refresh trending posts every 5 minutes).

```tsx
export const revalidate = 300; // Refreshes every 5 minutes
```
### Special Case: `revalidate = 0`
Setting `revalidate = 0` forces real-time regeneration on every request, making it behave like SSR.


## Server-Side Rendering (SSR)

SSR generates the page on every request and serves fresh data to the user
### When to Use SSR?
Use SSR for dynamic content that must be up-to-date:

- Dashboards (e.g., real-time analytics)
- User-specific pages (e.g., profile settings)
- Highly dynamic content (e.g., stock market updates)
  

```
export async function getServerSideProps() {
  const data = await fetchData();
  return { props: { data } };
}

```

## Client-Side Data Fetching
For user-specific or frequently changing data, fetching data on the client side is ideal.

### When to Use Client-Side Fetching?
- User authentication & session-based data
- Live search results
- Interactive UI elements (e.g., chat messages, notifications)

```tsx
import { useState, useEffect } from "react";

function Dashboard() {
  const [data, setData] = useState(null);

  useEffect(() => {
    fetch("/api/data")
      .then((res) => res.json())
      .then((data) => setData(data));
  }, []);

  return <div>{data ? data.value : "Loading..."}</div>;
}

```

## Revalidating Cached Data with `revalidatePath`
Sometimes, you need to manually refresh a specific route or API when new data is available. Next.js 15 provides the revalidatePath function for this purpose.

### How `revalidatePath` Works:
- Clears the cache for a specific page or API route.
- Forces the page to regenerate on the next request.

### When to Use revalidatePath?
After submitting a form (e.g., update user profile and refresh the page).
After publishing a new blog post (e.g., refresh /blog to display new content).
When a user deletes an item (e.g., update /products list).

### Example: Revalidate a Page After a Form Submission

``` tsx
"use server";

import { revalidatePath } from "next/cache";

export async function updateProfile(formData) {
  await saveUserProfile(formData); // Update user profile in DB
  revalidatePath("/profile"); // Refresh the profile page
}

```

### Example: Revalidate an API Route

```tsx
import { revalidatePath } from "next/cache";

export async function POST(req) {
  await updateDatabase(); // Update some data
  revalidatePath("/api/data"); // Refresh API cache
  return new Response("Cache cleared", { status: 200 });
}

```

## Choosing the Right Caching Strategy


| Strategy              | Best For	 | How it Works |
| :---------------- | ------: | ----: |
| SSG (Static Site Generation)	       |   Pages that rarely change (e.g., privacy policy)	   | Prebuilt at build time and served statically
 |
| ISR (Incremental Static Regeneration)	          |   Pages that update periodically (e.g., blogs, product pages)	   | Cached but regenerated after a set time
 |
| SSR (Server-Side Rendering)	    |  Pages that require fresh data on every request (e.g., dashboards)	   | Generated on each request
 |
| Client-Side Fetching	 |  User-specific or real-time data (e.g., chat, authentication)	   | Data is fetched in the browser after load
 |
 | Revalidate Path	 |  Manually refreshing data after an action (e.g., form submission, new blog post)	   | Clears cache for a specific page or API route
 |

