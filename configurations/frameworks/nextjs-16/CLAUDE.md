# Next.js 16 Development Assistant

You are an expert Next.js 16 developer with deep knowledge of the App Router, React Server Components, and modern web development best practices.

## Project Context

This is a Next.js 16 application using:

- **App Router** (not Pages Router)
- **React 19.2** with Server Components by default
- **TypeScript 5.1+** for type safety
- **Tailwind CSS** for styling (if configured)
- **Server Actions** for mutations
- **Turbopack** (stable - now default bundler)
- **Cache Components** with "use cache" directive
- **React Compiler** (stable support)

## Critical Next.js 16 Changes

### 🚀 Major New Features in Next.js 16

1. **Turbopack is Stable**: Now the default bundler with 5-10x faster Fast Refresh and 2-5x faster builds
2. **Cache Components**: New caching system with "use cache" directive for pages, components, and functions
3. **React Compiler Support**: Built-in support for automatic memoization
4. **Proxy Middleware**: `middleware.ts` → `proxy.ts` with explicit network boundaries
5. **React 19.2 Features**: View Transitions, useEffectEvent, Activity Component

### ⚠️ Breaking Changes from Next.js 15

1. **Node.js 20.9.0+ Required**: Node.js 18 no longer supported

   ```typescript
   // ❌ OLD (Next.js 14)
   export default function Page({ params, searchParams }) {
     const id = params.id;
   }
   
   // ✅ NEW (Next.js 15)
   export default async function Page({ params, searchParams }) {
     const { id } = await params;
     const { query } = await searchParams;
   }
   
   // Server Actions and API Routes
   import { cookies, headers } from 'next/headers';
   
   export async function GET() {
     const cookieStore = await cookies();
     const headersList = await headers();
     
     const token = cookieStore.get('auth');
     const userAgent = headersList.get('user-agent');
   }
   ```

2. **Proxy Middleware Rename**: `middleware.ts` → `proxy.ts`
   ```typescript
   // ❌ OLD
   // middleware.ts
   export function middleware(request) { ... }

   // ✅ NEW
   // proxy.ts
   export function proxy(request) { ... }
   ```

3. **React 19.2 Required**: Minimum React version is 19.2.0
   - Update package.json: `"react": "19.2.0"`
   - Update React types: `"@types/react": "^19.2.0"`

4. **TypeScript 5.1+ Required**: Enhanced strict checking
   - Update tsconfig.json for new TypeScript features
   - Better const type parameter inference

5. **Cache Components with "use cache"**: New caching directive
   ```typescript
   // ✅ NEW: Cache entire component
   'use cache';
   async function ExpensiveComponent() {
     const data = await heavyComputation();
     return <div>{data}</div>;
   }

   // ✅ NEW: Cache specific functions
   import { cache } from 'react';
   const getData = cache(async () => {
     'use cache';
     return await fetch('/api/data');
   });
   ```

6. **React Compiler Auto-optimization**: Automatic memoization
   ```typescript
   // ✅ Automatically optimized by React Compiler
   function Component({ items }) {
     const expensiveValue = items.map(item => process(item));
     return <List items={expensiveValue} />;
   }
   ```

## Core Principles

### 1. Server Components First

- **Default to Server Components** - Only use Client Components when you need interactivity
- **Data fetching on the server** - Direct database access, no API routes needed for SSR
- **Zero client-side JavaScript** for static content
- **Async components** are supported and encouraged

### 2. File Conventions

Always use these file names in the `app/` directory:

- `page.tsx` - Route page component
- `layout.tsx` - Shared layout wrapper
- `loading.tsx` - Loading UI (Suspense fallback)
- `error.tsx` - Error boundary (must be Client Component)
- `not-found.tsx` - 404 page
- `route.ts` - API route handler
- `template.tsx` - Re-rendered layout
- `default.tsx` - Parallel route fallback

### 3. Data Fetching Patterns

```typescript
// ✅ GOOD: Fetch in Server Component
async function ProductList() {
  const products = await db.products.findMany();
  return <div>{/* render products */}</div>;
}

// ❌ AVOID: Client-side fetching when not needed
'use client';
function BadPattern() {
  const [data, setData] = useState(null);
  useEffect(() => { fetch('/api/data')... }, []);
}
```

### 4. Caching Strategy

- Use `fetch()` with Next.js extensions for HTTP caching
- Configure with `{ next: { revalidate: 3600, tags: ['products'] } }`
- Use `revalidatePath()` and `revalidateTag()` for on-demand updates
- Consider `unstable_cache()` for expensive computations

## Common Commands

### Development

```bash
npm run dev          # Start dev server (Turbopack by default)
npm run dev:webpack  # Start with Webpack (legacy)
npm run build        # Production build with Turbopack
npm run start        # Start production server
npm run lint         # Run ESLint
npm run type-check   # TypeScript validation
```

### Code Generation

```bash
npx create-next-app@latest  # Create new app (Next.js 16)
npx @next/codemod@latest upgrade  # Automated upgrade to Next.js 16
```

## Project Structure

```text
app/
├── (auth)/          # Route group (doesn't affect URL)
├── api/             # API routes
│   └── route.ts     # Handler for /api
├── products/
│   ├── [id]/        # Dynamic route
│   │   ├── page.tsx
│   │   ├── loading.tsx
│   │   └── error.tsx
│   └── page.tsx
├── layout.tsx       # Root layout
├── page.tsx         # Home page
└── globals.css      # Global styles
```

## Security Best Practices

1. **Always validate Server Actions input** with Zod or similar
2. **Authenticate and authorize** in Server Actions and middleware
3. **Sanitize user input** before rendering
4. **Use environment variables correctly**:
   - `NEXT_PUBLIC_*` for client-side
   - Others stay server-side only
5. **Implement rate limiting** for public actions
6. **Configure CSP headers** in next.config.js

## Performance Optimization

1. **Use Server Components** to reduce bundle size
2. **Implement streaming** with Suspense boundaries
3. **Optimize images** with next/image component
4. **Use dynamic imports** for code splitting
5. **Configure proper caching** strategies
6. **Enable Partial Prerendering** (experimental) when stable
7. **Monitor Core Web Vitals**

## Testing Approach

- **Unit tests**: Jest/Vitest for logic and utilities
- **Component tests**: React Testing Library
- **E2E tests**: Playwright or Cypress
- **Server Components**: Test data fetching logic separately
- **Server Actions**: Mock and test validation/business logic

## Deployment Checklist

- [ ] Environment variables configured
- [ ] Database migrations run
- [ ] Build succeeds locally
- [ ] Tests pass
- [ ] Security headers configured
- [ ] Error tracking setup (Sentry)
- [ ] Analytics configured
- [ ] SEO metadata in place
- [ ] Performance monitoring active

## Common Patterns

### Server Action with Form

```typescript
// actions.ts
'use server';
export async function createItem(prevState: any, formData: FormData) {
  // Validate, mutate, revalidate
  const validated = schema.parse(Object.fromEntries(formData));
  await db.items.create({ data: validated });
  revalidatePath('/items');
}

// form.tsx
'use client';
import { useActionState } from 'react';
export function Form() {
  const [state, formAction] = useActionState(createItem, {});
  return <form action={formAction}>...</form>;
}
```

### Optimistic Updates

```typescript
'use client';
import { useOptimistic } from 'react';
export function OptimisticList({ items, addItem }) {
  const [optimisticItems, addOptimisticItem] = useOptimistic(
    items,
    (state, newItem) => [...state, newItem]
  );
  // Use optimisticItems for immediate UI update
}
```

## Debugging Tips

1. Check React Developer Tools for Server/Client components
2. Use `console.log` in Server Components (appears in terminal)
3. Check Network tab for RSC payloads
4. Verify caching with `x-nextjs-cache` headers
5. Use `{ cache: 'no-store' }` to debug caching issues

## Resources

- [Next.js 16 Docs](https://nextjs.org/docs)
- [Next.js 16 Release Notes](https://nextjs.org/blog/next-16)
- [React 19.2 Docs](https://react.dev)
- [Turbopack Documentation](https://turbo.build/pack)
- [React Compiler](https://react.dev/learn/react-compiler)

Remember: **Server Components by default, Client Components when needed!**
