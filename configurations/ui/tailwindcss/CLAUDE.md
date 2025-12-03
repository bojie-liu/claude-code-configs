# Tailwind CSS v4.0 Development Assistant

You are an expert in Tailwind CSS v4.0 with deep knowledge of utility-first styling, responsive design, component patterns, the new CSS-first configuration system, and modern CSS architecture.

## Memory Integration

This CLAUDE.md follows Claude Code memory management patterns:

- **Project memory** - Shared Tailwind CSS design system with team
- **Utility patterns** - Reusable CSS utility combinations
- **Design tokens** - Consistent spacing, colors, and typography
- **Auto-discovery** - Loaded when working with styled components

## Available Commands

Project-specific slash commands for Tailwind development:

- `/tw-component [name]` - Generate component with utility classes
- `/tw-responsive [breakpoints]` - Create responsive design patterns
- `/tw-theme [section]` - Update tailwind.config.js theme
- `/tw-plugin [name]` - Add and configure Tailwind plugin
- `/tw-optimize` - Analyze and optimize CSS bundle size

## Project Context

This project uses **Tailwind CSS v4.0** for styling with:

- **Zero configuration** - no tailwind.config.js needed by default
- **CSS-first configuration** with `@theme` and `@plugin` directives
- **High-performance engine** - 5x faster builds, 100x faster incremental builds
- **Utility-first approach** for rapid development
- **Responsive design** with mobile-first methodology
- **CSS variables and OKLCH** for enhanced design system control
- **Component patterns** for reusable UI elements
- **Dark mode support** with advanced theming
- **Modern plugin ecosystem** with v4-compatible plugins

## 🚀 Major Tailwind v4.0 Changes

### CSS-First Configuration (Recommended)
- **No tailwind.config.js by default** - configure directly in CSS
- **@theme directive** for custom design tokens
- **@plugin directive** for importing plugins
- **@config directive** for optional JavaScript config files
- **Automatic installation** with one line of CSS

## Core Tailwind Principles

### 1. Utility-First Methodology

- **Use utility classes** for styling instead of custom CSS
- **Compose complex components** from simple utilities
- **Maintain consistency** with predefined design tokens
- **Optimize for performance** with automatic CSS purging
- **Embrace constraints** of the design system

### 2. Responsive Design

- **Mobile-first approach** with `sm:`, `md:`, `lg:`, `xl:`, `2xl:` breakpoints
- **Consistent breakpoint usage** across the application
- **Responsive typography** and spacing
- **Flexible grid systems** with CSS Grid and Flexbox
- **Responsive images** and media handling

### 3. Design System Integration

- **Custom color palettes** defined in configuration
- **Consistent spacing scale** using rem units
- **Typography hierarchy** with font sizes and line heights
- **Shadow and border radius** system for depth
- **Animation and transition** utilities for micro-interactions

## Configuration Patterns

### Tailwind v4 Installation & Setup

```css
/* styles.css - Zero configuration approach */
@import "tailwindcss";
```

```css
/* styles.css - With custom theme */
@import "tailwindcss";

@theme {
  --color-brand-50: oklch(97% 0.01 250);
  --color-brand-100: oklch(93% 0.05 250);
  --color-brand-500: oklch(50% 0.15 250);
  --color-brand-900: oklch(20% 0.1 250);

  --font-family-display: "Inter", system-ui, sans-serif;
  --font-size-xl: 1.25rem;
  --font-size-2xl: 1.5rem;
}
```

### Legacy JavaScript Config (Optional)

```javascript
/* tailwind.config.js - Only if needed for complex configuration */
/** @type {import('tailwindcss').Config} */
module.exports = {
  theme: {
    extend: {
      colors: {
        brand: {
          50: 'oklch(97% 0.01 250)',
          500: 'oklch(50% 0.15 250)',
          900: 'oklch(20% 0.1 250)',
        }
      }
    },
  },
  plugins: [],
}
```

```css
/* Reference JavaScript config in CSS */
@import "tailwindcss";
@config "tailwind.config.js";
```

### Tailwind v4 CSS-First Design System

```css
/* styles.css - Modern v4 approach */
@import "tailwindcss";

@theme {
  /* OKLCH Color System (better than HSL) */
  --color-brand-50: oklch(97% 0.01 250);
  --color-brand-100: oklch(93% 0.05 250);
  --color-brand-200: oklch(87% 0.08 250);
  --color-brand-300: oklch(78% 0.12 250);
  --color-brand-400: oklch(68% 0.15 250);
  --color-brand-500: oklch(50% 0.18 250);
  --color-brand-600: oklch(42% 0.15 250);
  --color-brand-700: oklch(35% 0.12 250);
  --color-brand-800: oklch(28% 0.08 250);
  --color-brand-900: oklch(20% 0.05 250);
  --color-brand-950: oklch(13% 0.02 250);

  /* Enhanced Gray Scale */
  --color-gray-50: oklch(99% 0 0);
  --color-gray-100: oklch(97% 0 0);
  --color-gray-200: oklch(93% 0 0);
  --color-gray-300: oklch(85% 0 0);
  --color-gray-400: oklch(70% 0 0);
  --color-gray-500: oklch(55% 0 0);
  --color-gray-600: oklch(45% 0 0);
  --color-gray-700: oklch(35% 0 0);
  --color-gray-800: oklch(25% 0 0);
  --color-gray-900: oklch(15% 0 0);
  --color-gray-950: oklch(8% 0 0);

  /* Typography System */
  --font-family-sans: "Inter Variable", ui-sans-serif, system-ui, sans-serif;
  --font-family-mono: "JetBrains Mono Variable", ui-monospace, monospace;
  --font-family-display: "Cal Sans", ui-sans-serif, system-ui, sans-serif;

  /* Enhanced Spacing */
  --spacing-18: 4.5rem;
  --spacing-88: 22rem;

  /* Animation System */
  --animate-fade-in: fade-in 0.5s ease-in-out;
  --animate-slide-up: slide-up 0.3s ease-out;
  --animate-bounce-gentle: bounce-gentle 2s infinite;

  /* Animation Keyframes */
  --keyframes-fade-in: {
    from { opacity: 0; }
    to { opacity: 1; }
  };

  --keyframes-slide-up: {
    from { transform: translateY(10px); opacity: 0; }
    to { transform: translateY(0); opacity: 1; }
  };

  --keyframes-bounce-gentle: {
    0%, 100% { transform: translateY(-5%); }
    50% { transform: translateY(0); }
  };
}

/* Import v4-compatible plugins */
@plugin "tailwindcss/typography";
@plugin "@tailwindcss/forms";
@plugin "@tailwindcss/container-queries";
```

### Advanced Configuration with CSS Variables

```javascript
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      colors: {
        background: 'hsl(var(--background))',
        foreground: 'hsl(var(--foreground))',
        primary: {
          DEFAULT: 'hsl(var(--primary))',
          foreground: 'hsl(var(--primary-foreground))',
        },
        secondary: {
          DEFAULT: 'hsl(var(--secondary))',
          foreground: 'hsl(var(--secondary-foreground))',
        },
        muted: {
          DEFAULT: 'hsl(var(--muted))',
          foreground: 'hsl(var(--muted-foreground))',
        },
        accent: {
          DEFAULT: 'hsl(var(--accent))',
          foreground: 'hsl(var(--accent-foreground))',
        },
        destructive: {
          DEFAULT: 'hsl(var(--destructive))',
          foreground: 'hsl(var(--destructive-foreground))',
        },
        border: 'hsl(var(--border))',
        input: 'hsl(var(--input))',
        ring: 'hsl(var(--ring))',
      },
      borderRadius: {
        lg: 'var(--radius)',
        md: 'calc(var(--radius) - 2px)',
        sm: 'calc(var(--radius) - 4px)',
      },
    },
  },
}
```

## Component Patterns

### Layout Components

```jsx
// Responsive Container
function Container({ children, className = "" }) {
  return (
    <div className={`mx-auto max-w-7xl px-4 sm:px-6 lg:px-8 ${className}`}>
      {children}
    </div>
  );
}

// Responsive Grid
function Grid({ children, cols = 1, className = "" }) {
  const colsMap = {
    1: 'grid-cols-1',
    2: 'grid-cols-1 md:grid-cols-2',
    3: 'grid-cols-1 md:grid-cols-2 lg:grid-cols-3',
    4: 'grid-cols-1 md:grid-cols-2 lg:grid-cols-4',
  };
  
  return (
    <div className={`grid gap-6 ${colsMap[cols]} ${className}`}>
      {children}
    </div>
  );
}

// Responsive Stack
function Stack({ children, spacing = 'md', className = "" }) {
  const spacingMap = {
    sm: 'space-y-2',
    md: 'space-y-4',
    lg: 'space-y-6',
    xl: 'space-y-8',
  };
  
  return (
    <div className={`flex flex-col ${spacingMap[spacing]} ${className}`}>
      {children}
    </div>
  );
}
```

### Interactive Components

```jsx
// Animated Button
function Button({ children, variant = 'primary', size = 'md', className = "", ...props }) {
  const baseClasses = 'inline-flex items-center justify-center rounded-md font-medium transition-all duration-200 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-offset-2 disabled:pointer-events-none disabled:opacity-50';
  
  const variants = {
    primary: 'bg-brand-600 text-white hover:bg-brand-700 focus-visible:ring-brand-500',
    secondary: 'bg-gray-100 text-gray-900 hover:bg-gray-200 focus-visible:ring-gray-500',
    outline: 'border border-gray-300 bg-transparent hover:bg-gray-50 focus-visible:ring-gray-500',
    ghost: 'hover:bg-gray-100 focus-visible:ring-gray-500',
  };
  
  const sizes = {
    sm: 'h-8 px-3 text-sm',
    md: 'h-10 px-4',
    lg: 'h-11 px-6 text-lg',
  };
  
  return (
    <button
      className={`${baseClasses} ${variants[variant]} ${sizes[size]} ${className}`}
      {...props}
    >
      {children}
    </button>
  );
}

// Card Component
function Card({ children, className = "", hover = false }) {
  return (
    <div className={`
      rounded-lg border border-gray-200 bg-white p-6 shadow-sm
      ${hover ? 'transition-shadow hover:shadow-md' : ''}
      dark:border-gray-800 dark:bg-gray-900
      ${className}
    `}>
      {children}
    </div>
  );
}
```

### Form Components

```jsx
// Input Field
function Input({ label, error, className = "", ...props }) {
  return (
    <div className="space-y-1">
      {label && (
        <label className="block text-sm font-medium text-gray-700 dark:text-gray-300">
          {label}
        </label>
      )}
      <input
        className={`
          block w-full rounded-md border border-gray-300 px-3 py-2 text-sm
          placeholder-gray-400 shadow-sm transition-colors
          focus:border-brand-500 focus:outline-none focus:ring-1 focus:ring-brand-500
          disabled:cursor-not-allowed disabled:bg-gray-50 disabled:text-gray-500
          dark:border-gray-600 dark:bg-gray-800 dark:text-white
          dark:placeholder-gray-500 dark:focus:border-brand-400
          ${error ? 'border-red-500 focus:border-red-500 focus:ring-red-500' : ''}
          ${className}
        `}
        {...props}
      />
      {error && (
        <p className="text-sm text-red-600 dark:text-red-400">{error}</p>
      )}
    </div>
  );
}

// Select Field
function Select({ label, error, children, className = "", ...props }) {
  return (
    <div className="space-y-1">
      {label && (
        <label className="block text-sm font-medium text-gray-700 dark:text-gray-300">
          {label}
        </label>
      )}
      <select
        className={`
          block w-full rounded-md border border-gray-300 px-3 py-2 text-sm
          shadow-sm transition-colors focus:border-brand-500 focus:outline-none
          focus:ring-1 focus:ring-brand-500 disabled:cursor-not-allowed
          disabled:bg-gray-50 disabled:text-gray-500
          dark:border-gray-600 dark:bg-gray-800 dark:text-white
          dark:focus:border-brand-400
          ${error ? 'border-red-500 focus:border-red-500 focus:ring-red-500' : ''}
          ${className}
        `}
        {...props}
      >
        {children}
      </select>
      {error && (
        <p className="text-sm text-red-600 dark:text-red-400">{error}</p>
      )}
    </div>
  );
}
```

## Responsive Design Patterns

### Mobile-First Approach

```jsx
// Responsive Navigation
function Navigation() {
  return (
    <nav className="
      flex flex-col space-y-4
      md:flex-row md:items-center md:space-x-6 md:space-y-0
    ">
      <a href="/" className="
        text-gray-700 hover:text-brand-600
        md:text-sm
        lg:text-base
      ">
        Home
      </a>
      <a href="/about" className="
        text-gray-700 hover:text-brand-600
        md:text-sm
        lg:text-base
      ">
        About
      </a>
    </nav>
  );
}

// Responsive Hero Section
function Hero() {
  return (
    <section className="
      px-4 py-12 text-center
      sm:px-6 sm:py-16
      md:py-20
      lg:px-8 lg:py-24
      xl:py-32
    ">
      <h1 className="
        text-3xl font-bold tracking-tight text-gray-900
        sm:text-4xl
        md:text-5xl
        lg:text-6xl
        xl:text-7xl
      ">
        Welcome to Our Site
      </h1>
      <p className="
        mt-4 text-lg text-gray-600
        sm:mt-6 sm:text-xl
        lg:mt-8 lg:text-2xl
      ">
        Building amazing experiences with Tailwind CSS
      </p>
    </section>
  );
}
```

### Container Queries

```jsx
// Using container queries for component-level responsiveness
function ProductCard() {
  return (
    <div className="@container">
      <div className="
        flex flex-col space-y-4
        @md:flex-row @md:space-x-4 @md:space-y-0
        @lg:space-x-6
      ">
        <img className="
          h-48 w-full object-cover
          @md:h-32 @md:w-32
          @lg:h-40 @lg:w-40
        " />
        <div className="flex-1">
          <h3 className="
            text-lg font-semibold
            @lg:text-xl
          ">
            Product Name
          </h3>
        </div>
      </div>
    </div>
  );
}
```

## Dark Mode Implementation

### CSS Variables Approach

```css
/* globals.css */
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  :root {
    --background: 0 0% 100%;
    --foreground: 222.2 84% 4.9%;
    --primary: 221.2 83.2% 53.3%;
    --primary-foreground: 210 40% 98%;
    --secondary: 210 40% 96%;
    --secondary-foreground: 222.2 84% 4.9%;
  }
  
  .dark {
    --background: 222.2 84% 4.9%;
    --foreground: 210 40% 98%;
    --primary: 217.2 91.2% 59.8%;
    --primary-foreground: 222.2 84% 4.9%;
    --secondary: 217.2 32.6% 17.5%;
    --secondary-foreground: 210 40% 98%;
  }
}
```

### Theme Toggle Component

```jsx
// Theme toggle with smooth transitions
function ThemeToggle() {
  const [theme, setTheme] = useState('light');
  
  const toggleTheme = () => {
    const newTheme = theme === 'light' ? 'dark' : 'light';
    setTheme(newTheme);
    document.documentElement.classList.toggle('dark', newTheme === 'dark');
  };
  
  return (
    <button
      onClick={toggleTheme}
      className="
        rounded-lg p-2 transition-colors duration-200
        hover:bg-gray-100 dark:hover:bg-gray-800
        focus:outline-none focus:ring-2 focus:ring-brand-500
      "
      aria-label="Toggle theme"
    >
      {theme === 'light' ? (
        <MoonIcon className="h-5 w-5 text-gray-700 dark:text-gray-300" />
      ) : (
        <SunIcon className="h-5 w-5 text-gray-700 dark:text-gray-300" />
      )}
    </button>
  );
}
```

## Performance Optimization

### Tailwind v4 Performance Features

**Built-in Optimizations:**
- **High-performance engine**: Up to 5x faster full builds
- **Incremental builds**: Over 100x faster incremental rebuilds
- **Automatic optimization**: No manual purging configuration needed
- **Smart content detection**: Automatically finds and processes template files

**CLI Installation:**
```bash
# Install the new CLI package
npm install -g @tailwindcss/cli@next

# Initialize project (creates minimal setup)
tailwindcss init

# Build with watch mode
tailwindcss --input src/styles.css --output dist/styles.css --watch
```

**Content Detection (Auto-configured):**
```css
/* styles.css - Content detection is automatic */
@import "tailwindcss";

/* Manual content specification (optional) */
@source "src/**/*.{js,ts,jsx,tsx}";
@source "components/**/*.{js,ts,jsx,tsx}";
```

### Custom Utilities

```css
/* Custom utilities for common patterns */
@layer utilities {
  .text-balance {
    text-wrap: balance;
  }
  
  .animation-delay-200 {
    animation-delay: 200ms;
  }
  
  .animation-delay-400 {
    animation-delay: 400ms;
  }
  
  .mask-gradient-to-r {
    mask-image: linear-gradient(to right, transparent, black 20%, black 80%, transparent);
  }
}
```

### Component Layer

```css
@layer components {
  .btn {
    @apply inline-flex items-center justify-center rounded-md px-4 py-2 text-sm font-medium transition-colors focus-visible:outline-none focus-visible:ring-2 disabled:pointer-events-none disabled:opacity-50;
  }
  
  .btn-primary {
    @apply bg-brand-600 text-white hover:bg-brand-700 focus-visible:ring-brand-500;
  }
  
  .card {
    @apply rounded-lg border border-gray-200 bg-white p-6 shadow-sm dark:border-gray-800 dark:bg-gray-900;
  }
  
  .input {
    @apply block w-full rounded-md border border-gray-300 px-3 py-2 text-sm shadow-sm transition-colors placeholder:text-gray-400 focus:border-brand-500 focus:outline-none focus:ring-1 focus:ring-brand-500 disabled:cursor-not-allowed disabled:bg-gray-50 dark:border-gray-600 dark:bg-gray-800 dark:text-white;
  }
}
```

## Animation and Motion

### Custom Animations

```javascript
// Advanced animations in Tailwind config
module.exports = {
  theme: {
    extend: {
      animation: {
        'spin-slow': 'spin 3s linear infinite',
        'pulse-fast': 'pulse 1s cubic-bezier(0.4, 0, 0.6, 1) infinite',
        'bounce-x': 'bounceX 1s infinite',
        'fade-in-up': 'fadeInUp 0.5s ease-out',
        'slide-in-right': 'slideInRight 0.3s ease-out',
        'scale-in': 'scaleIn 0.2s ease-out',
      },
      keyframes: {
        bounceX: {
          '0%, 100%': { transform: 'translateX(-25%)' },
          '50%': { transform: 'translateX(0)' },
        },
        fadeInUp: {
          '0%': { opacity: '0', transform: 'translateY(20px)' },
          '100%': { opacity: '1', transform: 'translateY(0)' },
        },
        slideInRight: {
          '0%': { opacity: '0', transform: 'translateX(20px)' },
          '100%': { opacity: '1', transform: 'translateX(0)' },
        },
        scaleIn: {
          '0%': { opacity: '0', transform: 'scale(0.95)' },
          '100%': { opacity: '1', transform: 'scale(1)' },
        },
      },
    },
  },
}
```

### Staggered Animations

```jsx
// Staggered animation component
function StaggeredList({ items }) {
  return (
    <div className="space-y-4">
      {items.map((item, index) => (
        <div
          key={item.id}
          className={`
            animate-fade-in-up opacity-0
            animation-delay-${index * 100}
          `}
          style={{ animationFillMode: 'forwards' }}
        >
          {item.content}
        </div>
      ))}
    </div>
  );
}
```

## Common Patterns and Solutions

### Truncated Text

```jsx
// Text truncation with tooltips
function TruncatedText({ text, maxLength = 100 }) {
  const truncated = text.length > maxLength;
  const displayText = truncated ? `${text.slice(0, maxLength)}...` : text;
  
  return (
    <span 
      className={`${truncated ? 'cursor-help' : ''}`}
      title={truncated ? text : undefined}
    >
      {displayText}
    </span>
  );
}

// CSS-only truncation
function CSSLimTruncate() {
  return (
    <p className="truncate">This text will be truncated if it's too long</p>
    // Or for multiple lines:
    <p className="line-clamp-3">
      This text will be clamped to 3 lines and show ellipsis
    </p>
  );
}
```

### Aspect Ratio Containers

```jsx
// Responsive aspect ratio containers
function AspectRatioImage({ src, alt, ratio = 'aspect-video' }) {
  return (
    <div className={`relative overflow-hidden rounded-lg ${ratio}`}>
      <img 
        src={src}
        alt={alt}
        className="absolute inset-0 h-full w-full object-cover"
      />
    </div>
  );
}

// Custom aspect ratios
function CustomAspectRatio() {
  return (
    <div className="aspect-[4/3]">
      {/* Content with 4:3 aspect ratio */}
    </div>
  );
}
```

### Focus Management

```jsx
// Accessible focus styles
function FocusExample() {
  return (
    <div className="space-y-4">
      <button className="
        rounded-md bg-brand-600 px-4 py-2 text-white
        focus:outline-none focus:ring-2 focus:ring-brand-500 focus:ring-offset-2
        focus-visible:ring-2 focus-visible:ring-brand-500
      ">
        Accessible Button
      </button>
      
      <input className="
        rounded-md border border-gray-300 px-3 py-2
        focus:border-brand-500 focus:outline-none focus:ring-1 focus:ring-brand-500
        invalid:border-red-500 invalid:focus:border-red-500 invalid:focus:ring-red-500
      " />
    </div>
  );
}
```

## Plugin Ecosystem

### Typography Plugin

```javascript
// @tailwindcss/typography configuration
module.exports = {
  plugins: [
    require('@tailwindcss/typography')({
      className: 'prose',
    }),
  ],
  theme: {
    extend: {
      typography: {
        DEFAULT: {
          css: {
            maxWidth: 'none',
            color: 'inherit',
            a: {
              color: 'inherit',
              textDecoration: 'none',
              fontWeight: '500',
            },
            'a:hover': {
              color: '#0ea5e9',
            },
          },
        },
      },
    },
  },
}
```

### Forms Plugin

```javascript
// @tailwindcss/forms configuration
module.exports = {
  plugins: [
    require('@tailwindcss/forms')({
      strategy: 'class', // or 'base'
    }),
  ],
}
```

## Resources

- [Tailwind CSS Documentation](https://tailwindcss.com/docs)
- [Tailwind UI Components](https://tailwindui.com)
- [Headless UI](https://headlessui.com)
- [Heroicons](https://heroicons.com)
- [Tailwind Play](https://play.tailwindcss.com)
- [Tailwind Community](https://github.com/tailwindlabs/tailwindcss/discussions)

Remember: **Utility-first, mobile-first, performance-first. Embrace constraints, compose with utilities, and maintain consistency!**
