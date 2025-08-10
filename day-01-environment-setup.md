# Day 1: Environment Setup & Project Structure

Welcome to your React journey! Today, we're setting up a professional development environment and learning about modern project architecture. Think of this as building the foundation of a house - get it right, and everything else becomes easier.

## Learning Objectives

By the end of today, you'll:
- Have a fully functional React + TypeScript + Tailwind CSS development environment
- Understand Feature-Sliced Design architecture
- Create your first organized project structure
- Run your first React application

## Prerequisites

Make sure you have:
- Node.js 18+ installed (check with `node --version`)
- A code editor (VS Code recommended)
- Basic terminal/command line knowledge

## Part 1: Creating Your Project

Let's start by creating a new React project with Vite. Open your terminal and run:

```bash
npm create vite@latest my-react-journey -- --template react-ts
cd my-react-journey
npm install
```

### Why Vite?
Vite is like a turbo engine for your development. It starts instantly and updates your browser the moment you save a file. No more waiting!

## Part 2: Adding Tailwind CSS

Now let's add Tailwind CSS v4.1 for styling:

```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

Update your `tailwind.config.js`:

```javascript
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./index.html",
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

Replace the content in `src/index.css`:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

## Part 3: Feature-Sliced Design Architecture

Now, here's where we get professional. Instead of throwing files everywhere, we'll organize our code like a well-arranged library.

### The Architecture

Delete everything in the `src` folder except `main.tsx` and `index.css`. Let's create our structure:

```bash
# Run these commands in your terminal
mkdir -p src/app
mkdir -p src/pages
mkdir -p src/widgets
mkdir -p src/features
mkdir -p src/entities
mkdir -p src/shared/ui
mkdir -p src/shared/lib
mkdir -p src/shared/config
```

### What Each Folder Means

Think of these folders as different sections in a department store:

- **`/app`** - The main entrance (global styles, providers, app initialization)
- **`/pages`** - Different floors (HomePage, AboutPage, etc.)
- **`/widgets`** - Complete sections (Header, Sidebar, ProductList)
- **`/features`** - Specific actions (AddToCart, UserLogin)
- **`/entities`** - The products (User, Product, Order)
- **`/shared`** - Common utilities everyone uses (buttons, helpers, configs)

### The Golden Rule

**Lower layers cannot import from higher layers!**
- ✅ A page can import from widgets
- ❌ A widget cannot import from pages
- ✅ Shared can be imported by anyone
- ❌ Shared cannot import from anywhere else

## Part 4: Your First Component

Let's create a simple App component with proper architecture:

Create `src/app/App.tsx`:

```tsx
interface AppProps {
  title?: string;
}

export function App({ title = "My React Journey" }: AppProps) {
  return (
    <div className="min-h-screen bg-gray-50">
      <header className="bg-white shadow-sm">
        <div className="max-w-7xl mx-auto px-4 py-6">
          <h1 className="text-3xl font-bold text-gray-900">{title}</h1>
        </div>
      </header>
      <main className="max-w-7xl mx-auto px-4 py-8">
        <p className="text-gray-600">
          Welcome to Day 1! Your environment is set up correctly.
        </p>
      </main>
    </div>
  );
}
```

Update `src/main.tsx`:

```tsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import { App } from './app/App'
import './index.css'

ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
)
```

## Part 5: Run Your Application

```bash
npm run dev
```

Visit `http://localhost:5173` and you should see your app running!

## Practice Exercises

### Exercise 1: Easy - Add a Subtitle
Modify the `App` component to accept a `subtitle` prop and display it below the title. Use TypeScript to make it optional with a default value.

### Exercise 2: Medium - Create a Shared Component
1. Create a Button component in `src/shared/ui/Button.tsx`
2. Make it accept `children`, `onClick`, and `variant` (primary/secondary) props
3. Use Tailwind classes to style it differently based on variant
4. Import and use it in your App component

### Exercise 3: Hard - Respect the Architecture
Create a simple "Counter" feature following FSD:
1. Create a Counter entity in `src/entities/counter/model.ts` with a type definition
2. Create a useCounter hook in `src/features/counter/hooks/useCounter.ts`
3. Create a Counter widget in `src/widgets/counter/Counter.tsx`
4. Use it in your App component

Remember: Each layer can only import from layers below it!

## Common Mistakes to Avoid

1. **Don't skip TypeScript types** - They're like safety rails, preventing bugs before they happen
2. **Don't put everything in one folder** - Use the architecture from day one
3. **Don't ignore the import rules** - They keep your code clean and maintainable

## Reflection Questions

1. Why do you think we separate code into different layers?
2. How is this different from putting all components in one folder?
3. What benefits do you see in using TypeScript with React?

## Quick Reference

```
Project Structure:
src/
├── app/          # App-wide settings
├── pages/        # Route pages
├── widgets/      # Complete UI blocks
├── features/     # User interactions
├── entities/     # Business logic
└── shared/       # Reusable utilities

Import Rule: Lower → Higher ✅ | Higher → Lower ❌
```

## Next Steps

Tomorrow, we'll dive into creating components and understanding how they work as building blocks. We'll also explore the Single Responsibility Principle - making sure each component does one thing really well.

Pro tip: Spend 15 minutes exploring the project structure. Create a few empty folders and files to get comfortable with the organization. The more familiar you are with the structure, the easier tomorrow will be!