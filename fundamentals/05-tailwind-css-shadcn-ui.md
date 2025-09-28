# Tailwind CSS v4 + shadcn/ui Components

Welcome back! You've built some brilliant interactive components with React and TypeScript, but let's be honest - those inline styles are getting a bit tedious, aren't they? Today, we're going to transform how you style your components using Tailwind CSS v4 and discover how professional component libraries are built with shadcn/ui!

## 🎯 What You'll Actually Build

By the end of this lesson, you'll have added to your React Learning Hub:

- **A new styling playground route** - `/fundamentals/styling`
- **All your previous components restyled** - using Tailwind utility classes
- **Understanding of professional styling** - how real companies style their apps
- **shadcn/ui integration** - using professionally built components where it makes sense
- **Responsive designs** - that work beautifully on mobile, tablet, and desktop

All of this will be built into your existing `my-dch-react-learning-hub` project, giving your Learning Hub a professional look!

## 📚 What You Need to Know Before Starting

- You should have completed Lessons 1-4 and have your React Learning Hub running
- Your TypeScript components from previous lessons
- Your todo app, counter, and other components you've built
- Basic understanding that CSS makes things look nice (we'll teach you the rest!)

---

## Part 1: Setting Up Your New Lesson Route (15 minutes)

Following the same pattern as before, let's add today's lesson to your existing React Learning Hub structure.

### Step 1: Add the New Lesson to Your Constants

Open `constants/index.ts` and update the lessons array:

```typescript
// constants/index.ts
export const lessons = [
	{
		id: 1,
		title: "React Components & JSX",
		path: "/fundamentals/components",
		category: "React Fundamentals",
	},
	{
		id: 2,
		title: "State & Interactivity",
		path: "/fundamentals/interactivity",
		category: "React Fundamentals",
	},
	{
		id: 3,
		title: "Dynamic Lists & Communication",
		path: "/fundamentals/lists",
		category: "React Fundamentals",
	},
	{
		id: 4,
		title: "TypeScript Integration",
		path: "/fundamentals/typescript",
		category: "React Fundamentals",
	},
	{
		id: 5,
		title: "Tailwind CSS & shadcn/ui", // Add this new lesson
		path: "/fundamentals/styling",
		category: "React Fundamentals",
	},
];
```

### Step 2: Install Tailwind CSS v4

The brilliant thing about Tailwind v4 is how simple the setup is. Let's add it to your project:

```bash
cd my-dch-react-learning-hub
pnpm add tailwindcss@next @tailwindcss/vite@next
```

Update your `vite.config.ts`:

```typescript
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import tailwindcss from "@tailwindcss/vite";

export default defineConfig({
	plugins: [react(), tailwindcss()],
	resolve: {
		alias: {
			"@": "/src",
		},
	},
});
```

Create or update `src/index.css` with just this single line:

```css
@import "tailwindcss";
```

Make sure your `main.tsx` imports this CSS:

```typescript
// src/main.tsx
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App.tsx";
import "./index.css"; // Make sure this line exists

ReactDOM.createRoot(document.getElementById("root")!).render(
	<React.StrictMode>
		<App />
	</React.StrictMode>
);
```

That's it! Tailwind v4 automatically detects all your components and generates only the CSS you actually use. No configuration files needed!

### Step 3: Install Tailwind IntelliSense (IMPORTANT!)

Before we go any further, install the Tailwind CSS IntelliSense extension in VS Code. This is absolutely essential:

1. Open VS Code
2. Go to Extensions (⌘+Shift+X on Mac, Ctrl+Shift+X on Windows)
3. Search for "Tailwind CSS IntelliSense"
4. Install the official extension by Tailwind Labs

**Why is this crucial?** IntelliSense gives you:

- Autocomplete for Tailwind classes as you type
- Documentation on hover
- Warnings when you use classes incorrectly
- Colour previews for colour classes

Without this extension, you'll be guessing class names. With it, you'll be flying!

### Step 4: Create Your StylingLesson Component

Create a new file: `src/pages/fundamentals/StylingLesson.tsx`

```tsx
// src/pages/fundamentals/StylingLesson.tsx

function StylingLesson() {
	return (
		<div className="max-w-4xl mx-auto p-8 space-y-8">
			<div className="bg-blue-50 border-l-4 border-blue-500 p-6 rounded">
				<h1 className="text-4xl font-bold text-blue-900 mb-2">
					Tailwind CSS & shadcn/ui
				</h1>
				<p className="text-blue-700">
					Learn professional styling with utility-first CSS and component
					libraries
				</p>
			</div>

			<div className="bg-white rounded-lg shadow-md p-6 border border-gray-200">
				<h2 className="text-2xl font-bold text-gray-900 mb-4">
					Your Styling Playground
				</h2>
				<p className="text-gray-700 mb-4">
					This is where you'll experiment with Tailwind and shadcn/ui!
				</p>
			</div>
		</div>
	);
}

export default StylingLesson;
```

### Step 5: Add the Route

Update your router configuration to include the new route:

```tsx
// src/App.tsx or wherever your routes are defined
import StylingLesson from './pages/fundamentals/StylingLesson'

// Add to your routes:
{
  path: '/fundamentals/styling',
  element: <StylingLesson />
}
```

Now start your dev server and navigate to `/fundamentals/styling` - you should see your new lesson page with Tailwind styling!

```bash
pnpm dev
```

---

## Part 2: Tailwind CSS Fundamentals (1-2 hours)

### What Is Tailwind CSS?

Instead of writing CSS like this:

```css
.button {
	background-color: blue;
	color: white;
	padding: 8px 16px;
	border-radius: 4px;
}
```

Tailwind lets you write utility classes directly in your JSX:

```tsx
<button className="bg-blue-500 text-white px-4 py-2 rounded">Click me</button>
```

**Benefits:**

- No context switching between files
- No thinking of class names
- Only the CSS you use gets included
- Easy to make responsive designs
- Consistent spacing and colours

### Essential Tailwind Concepts

#### 1. Spacing (Padding & Margin)

Tailwind uses a spacing scale where numbers represent multiples of 0.25rem (4px):

```tsx
// Padding
p - 4; // padding: 1rem (16px) on all sides
px - 6; // padding-left and padding-right: 1.5rem (24px)
py - 2; // padding-top and padding-bottom: 0.5rem (8px)
pt - 8; // padding-top: 2rem (32px)
pb - 4; // padding-bottom: 1rem (16px)

// Margin
m - 4; // margin: 1rem (16px) on all sides
mx - auto; // margin-left and margin-right: auto (centres element)
mb - 6; // margin-bottom: 1.5rem (24px)
mt -
	8 - // margin-top: 2rem (32px)
	mt -
	4; // negative margin-top (for overlapping elements)

// Gap (for flex/grid)
gap - 4; // gap: 1rem between flex/grid items
gap - x - 2; // horizontal gap only
gap - y - 6; // vertical gap only

// Space between children
space - y - 4; // adds margin-top to all children except first
space - x - 2; // adds margin-left to all children except first
```

**Example:**

```tsx
<div className="p-6 space-y-4">
	<h2 className="text-2xl mb-2">Title</h2>
	<p>Paragraph 1</p>
	<p>Paragraph 2</p>
	<p>Paragraph 3</p>
</div>
```

#### 2. Typography

```tsx
// Font Size
text-xs     // 0.75rem (12px)
text-sm     // 0.875rem (14px)
text-base   // 1rem (16px) - default
text-lg     // 1.125rem (18px)
text-xl     // 1.25rem (20px)
text-2xl    // 1.5rem (24px)
text-3xl    // 1.875rem (30px)
text-4xl    // 2.25rem (36px)

// Font Weight
font-light    // 300
font-normal   // 400
font-medium   // 500
font-semibold // 600
font-bold     // 700

// Text Alignment
text-left
text-center
text-right

// Text Colour
text-gray-900  // very dark grey
text-gray-500  // medium grey
text-blue-600  // blue
text-red-500   // red
```

**Example:**

```tsx
<div>
	<h1 className="text-4xl font-bold text-gray-900">Main Heading</h1>
	<p className="text-lg text-gray-600">Subtitle text</p>
	<p className="text-sm text-gray-500">Small helper text</p>
</div>
```

#### 3. Colours

Tailwind provides colours from 50 (lightest) to 950 (darkest):

```tsx
// Background
bg - blue - 500; // solid background
bg - gray - 100; // very light grey background

// Text
text - blue - 600; // blue text
text - red - 500; // red text

// Border
border - gray - 300; // grey border

// Colour scales: 50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 950
```

**Example:**

```tsx
<div className="bg-blue-50 border border-blue-200 text-blue-900 p-4">
  Light blue info box
</div>

<div className="bg-red-50 border border-red-200 text-red-900 p-4">
  Light red error box
</div>
```

#### 4. Layout with Flexbox

```tsx
// Basic Flex
flex; // display: flex
flex - col; // flex-direction: column (vertical)
flex - row; // flex-direction: row (horizontal - default)

// Alignment
items - start; // align-items: flex-start (top)
items - center; // align-items: center (middle)
items - end; // align-items: flex-end (bottom)

// Justify Content
justify - start; // justify-content: flex-start (left/top)
justify - center; // justify-content: center (centre)
justify - end; // justify-content: flex-end (right/bottom)
justify - between; // justify-content: space-between

// Flex Sizing
flex - 1; // flex: 1 1 0% (grow to fill space)
flex - none; // don't grow or shrink
```

**Example:**

```tsx
// Horizontal layout with gap
<div className="flex gap-4 items-center">
  <button className="px-4 py-2 bg-blue-500 text-white">Button 1</button>
  <button className="px-4 py-2 bg-blue-500 text-white">Button 2</button>
</div>

// Vertical layout
<div className="flex flex-col gap-2">
  <p>Item 1</p>
  <p>Item 2</p>
  <p>Item 3</p>
</div>

// Space between elements
<div className="flex justify-between items-center">
  <h2>Title</h2>
  <button>Action</button>
</div>
```

#### 5. Layout with Grid

```tsx
// Basic Grid
grid              // display: grid
grid-cols-2       // 2 equal columns
grid-cols-3       // 3 equal columns
grid-cols-4       // 4 equal columns

// Custom columns
grid-cols-[200px_1fr]  // first col 200px, second fills space
```

**Example:**

```tsx
<div className="grid grid-cols-3 gap-4">
	<div className="bg-gray-100 p-4">Card 1</div>
	<div className="bg-gray-100 p-4">Card 2</div>
	<div className="bg-gray-100 p-4">Card 3</div>
</div>
```

#### 6. Borders and Shadows

```tsx
// Borders
border; // 1px border
border - 2; // 2px border
border - 4; // 4px border

// Border Sides
border - t; // top border only
border - l; // left border only
border - l - 4; // thick left border

// Border Radius
rounded; // border-radius: 0.25rem (4px)
rounded - md; // border-radius: 0.375rem (6px)
rounded - lg; // border-radius: 0.5rem (8px)
rounded - full; // border-radius: 9999px (circle)

// Shadows
shadow - sm; // small shadow
shadow; // medium shadow
shadow - md; // medium-large shadow
shadow - lg; // large shadow
```

**Example:**

```tsx
<div className="border border-gray-300 rounded-lg shadow-md p-6">
	Card with border, rounded corners, and shadow
</div>
```

#### 7. Responsive Design with Breakpoints

This is where Tailwind truly shines! You can apply different styles at different screen sizes:

```tsx
// Breakpoints:
// sm: 640px   (mobile landscape / small tablet)
// md: 768px   (tablet)
// lg: 1024px  (desktop)
// xl: 1280px  (large desktop)
// 2xl: 1536px (extra large desktop)

// Mobile-first approach (default is mobile, add larger sizes)
<div className="text-sm md:text-base lg:text-lg">
  Small on mobile, base on tablet, large on desktop
</div>

<div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
  1 column mobile, 2 columns tablet, 3 columns desktop
</div>

<div className="p-4 md:p-6 lg:p-8">
  Less padding on mobile, more on larger screens
</div>
```

**Real Example:**

```tsx
function ResponsiveCard() {
	return (
		<div
			className="
      p-4 md:p-6 lg:p-8
      text-sm md:text-base
      bg-white
      rounded-lg
      shadow-md
      max-w-sm md:max-w-md lg:max-w-lg
    "
		>
			<h2 className="text-xl md:text-2xl lg:text-3xl font-bold mb-4">
				Responsive Card
			</h2>
			<p className="text-gray-600">This card adapts to screen size!</p>
		</div>
	);
}
```

#### 8. Hover, Focus, and Other States

```tsx
// Hover
hover: bg - blue - 600; // background changes on hover
hover: text - white; // text colour changes on hover
hover: shadow - lg; // shadow increases on hover

// Focus (for inputs, buttons)
focus: ring - 2; // adds focus ring
focus: ring - blue - 500; // blue focus ring
focus: outline - none; // removes default outline

// Active (when clicking)
active: bg - blue - 700;

// Disabled
disabled: opacity - 50;
disabled: cursor - not - allowed;
```

**Example:**

```tsx
<button
	className="
  px-4 py-2
  bg-blue-500 text-white
  rounded-md
  hover:bg-blue-600
  active:bg-blue-700
  focus:ring-2 focus:ring-blue-300
  transition-colors
"
>
	Hover over me!
</button>
```

#### 9. Transitions and Animations

```tsx
// Transition
transition              // transition all properties
transition-colors       // only transition colours
transition-transform    // only transition transforms

// Duration
duration-150           // 150ms
duration-300           // 300ms (default)
duration-500           // 500ms

// Timing
ease-in
ease-out
ease-in-out
```

**Example:**

```tsx
<button
	className="
  px-4 py-2 bg-blue-500 text-white
  transform hover:scale-105
  transition-all duration-200
"
>
	Hover to scale up!
</button>
```

### Learning More Tailwind

**The Tailwind documentation is brilliant!** Visit https://tailwindcss.com/docs

Key sections to explore:

- Core Concepts → Utility-First Fundamentals
- Layout → Flexbox, Grid, Container
- Spacing → Padding, Margin, Space Between
- Typography → Font Size, Font Weight, Text Colour
- Backgrounds → Background Colour, Gradients
- Borders → Border Width, Border Radius
- Effects → Box Shadow, Opacity

**Pro tip:** Keep the Tailwind docs open in a tab. With IntelliSense helping you, you'll quickly memorise the most common classes!

---

## Part 3: Understanding Flexbox with Knights of the Flexbox Table (30-45 minutes)

Before you start practising with Tailwind Play, you need to understand **Flexbox** - the layout system that powers most modern web designs. Flexbox makes it easy to align items, distribute space, and create responsive layouts.

**But instead of reading about it, you're going to play a game!**

### 🎮 Play Knights of the Flexbox Table

Head to **https://knightsoftheflexboxtable.com/**

This is an interactive game that teaches you Flexbox by having you arrange knights on a table. Work through ALL the levels - they progressively teach you every important Flexbox concept.

**What you'll learn:**

- `justify-content` - how to align items horizontally
- `align-items` - how to align items vertically
- `flex-direction` - row vs column layouts
- `flex-wrap` - how items wrap to new lines
- `align-content` - how wrapped lines are spaced
- And more!

**Time estimate:** 30-45 minutes to complete all levels

**Pro tip:** Take notes as you play! Write down what each property does. You'll use these concepts constantly in Tailwind.

### Why This Matters for Tailwind

In Tailwind, Flexbox properties become utility classes:

| Flexbox CSS                      | Tailwind Class    |
| -------------------------------- | ----------------- |
| `display: flex`                  | `flex`            |
| `flex-direction: row`            | `flex-row`        |
| `flex-direction: column`         | `flex-col`        |
| `justify-content: center`        | `justify-center`  |
| `justify-content: space-between` | `justify-between` |
| `align-items: center`            | `items-center`    |
| `align-items: start`             | `items-start`     |
| `gap: 1rem`                      | `gap-4`           |

Once you understand Flexbox from the game, using it in Tailwind will be effortless!

---

## Part 4: Tailwind Practice Drills (Use Tailwind Play!)

Now that you understand Flexbox, let's practice Tailwind in isolation. Head to **https://play.tailwindcss.com/** - this is an online Tailwind playground where you can experiment!

### Drill 1: Create a Profile Card (15 minutes)

In Tailwind Play, create a profile card with:

- A coloured header section
- A circular avatar (use a div with a background colour)
- Name and title text
- A button at the bottom
- Hover effects on the button
- Proper spacing throughout

#### End Goal:

![[./images/twDrill1.png]]

**Starter code for Tailwind Play:**

```html
<div class="min-h-screen bg-gray-100 p-8">
	<!-- Your profile card here -->
</div>
```

<details>
<summary>Solution</summary>

```html
<div class="min-h-screen bg-gray-100 p-8 flex items-center justify-center">
	<div class="bg-white rounded-lg shadow-lg overflow-hidden max-w-sm w-full">
		<!-- Header -->
		<div class="bg-gradient-to-r from-blue-500 to-purple-600 h-32"></div>

		<!-- Avatar -->
		<div class="flex justify-center -mt-16">
			<div
				class="w-32 h-32 rounded-full bg-gray-300 border-4 border-white shadow-lg flex items-center justify-center text-4xl font-bold text-gray-600"
			>
				AJ
			</div>
		</div>

		<!-- Content -->
		<div class="px-6 py-4 text-center">
			<h2 class="text-2xl font-bold text-gray-900 mb-1">Alex Johnson</h2>
			<p class="text-gray-600 mb-4">Frontend Developer</p>
			<p class="text-sm text-gray-500 mb-6">
				Passionate about building beautiful user interfaces with React and
				Tailwind CSS.
			</p>

			<button
				class="
        w-full
        px-6 py-3
        bg-blue-500 text-white
        rounded-lg
        font-medium
        hover:bg-blue-600
        active:bg-blue-700
        transition-colors
        duration-200
      "
			>
				View Profile
			</button>
		</div>
	</div>
</div>
```

</details>

### Drill 2: Build a Responsive Grid (15 minutes)

Create a grid of 6 cards that:

- Shows 1 column on mobile
- Shows 2 columns on tablet
- Shows 3 columns on desktop
- Each card has an image placeholder, title, and description
- Cards have hover effects

#### End Goal:

![[./images/twDrill2.png]]

<details>
<summary>Solution</summary>

```html
<div class="min-h-screen bg-gray-50 p-8">
	<h1 class="text-4xl font-bold text-center mb-8 text-gray-900">
		Our Services
	</h1>

	<div
		class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6 max-w-6xl mx-auto"
	>
		<!-- Card 1 -->
		<div
			class="bg-white rounded-lg shadow-md overflow-hidden hover:shadow-xl transition-shadow duration-300"
		>
			<div class="h-48 bg-gradient-to-br from-blue-400 to-blue-600"></div>
			<div class="p-6">
				<h3 class="text-xl font-bold text-gray-900 mb-2">Web Design</h3>
				<p class="text-gray-600">
					Beautiful, responsive websites tailored to your needs.
				</p>
			</div>
		</div>

		<!-- Card 2 -->
		<div
			class="bg-white rounded-lg shadow-md overflow-hidden hover:shadow-xl transition-shadow duration-300"
		>
			<div class="h-48 bg-gradient-to-br from-purple-400 to-purple-600"></div>
			<div class="p-6">
				<h3 class="text-xl font-bold text-gray-900 mb-2">Development</h3>
				<p class="text-gray-600">
					Modern web applications built with React and TypeScript.
				</p>
			</div>
		</div>

		<!-- Card 3 -->
		<div
			class="bg-white rounded-lg shadow-md overflow-hidden hover:shadow-xl transition-shadow duration-300"
		>
			<div class="h-48 bg-gradient-to-br from-green-400 to-green-600"></div>
			<div class="p-6">
				<h3 class="text-xl font-bold text-gray-900 mb-2">Consulting</h3>
				<p class="text-gray-600">
					Expert advice for your web development projects.
				</p>
			</div>
		</div>

		<!-- Card 4 -->
		<div
			class="bg-white rounded-lg shadow-md overflow-hidden hover:shadow-xl transition-shadow duration-300"
		>
			<div class="h-48 bg-gradient-to-br from-red-400 to-red-600"></div>
			<div class="p-6">
				<h3 class="text-xl font-bold text-gray-900 mb-2">SEO</h3>
				<p class="text-gray-600">Optimise your site for search engines.</p>
			</div>
		</div>

		<!-- Card 5 -->
		<div
			class="bg-white rounded-lg shadow-md overflow-hidden hover:shadow-xl transition-shadow duration-300"
		>
			<div class="h-48 bg-gradient-to-br from-yellow-400 to-yellow-600"></div>
			<div class="p-6">
				<h3 class="text-xl font-bold text-gray-900 mb-2">Analytics</h3>
				<p class="text-gray-600">Track and understand your user behaviour.</p>
			</div>
		</div>

		<!-- Card 6 -->
		<div
			class="bg-white rounded-lg shadow-md overflow-hidden hover:shadow-xl transition-shadow duration-300"
		>
			<div class="h-48 bg-gradient-to-br from-pink-400 to-pink-600"></div>
			<div class="p-6">
				<h3 class="text-xl font-bold text-gray-900 mb-2">Support</h3>
				<p class="text-gray-600">24/7 support for all your technical needs.</p>
			</div>
		</div>
	</div>
</div>
```

</details>

### Drill 3: Create a Navigation Bar (15 minutes)

Build a navbar that:

- Has your logo/name on the left
- Has navigation links in the centre (or right)
- Has a call-to-action button
- Is sticky at the top
- Has a subtle shadow
- Responds to different screen sizes

#### End Goal:

![[./images/twDrill3-1.png]]
![[./images/twDrill3-2.png]]

<details>
<summary>Solution</summary>

```html
<div class="min-h-screen bg-gray-50">
	<!-- Navbar -->
	<nav class="sticky top-0 bg-white shadow-md z-50">
		<div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
			<div class="flex justify-between items-center h-16">
				<!-- Logo -->
				<div class="flex-shrink-0">
					<span class="text-2xl font-bold text-blue-600">MyBrand</span>
				</div>

				<!-- Navigation Links (hidden on mobile) -->
				<div class="hidden md:flex space-x-8">
					<a
						href="#"
						class="text-gray-700 hover:text-blue-600 transition-colors"
						>Home</a
					>
					<a
						href="#"
						class="text-gray-700 hover:text-blue-600 transition-colors"
						>About</a
					>
					<a
						href="#"
						class="text-gray-700 hover:text-blue-600 transition-colors"
						>Services</a
					>
					<a
						href="#"
						class="text-gray-700 hover:text-blue-600 transition-colors"
						>Contact</a
					>
				</div>

				<!-- CTA Button -->
				<button
					class="
          px-4 py-2
          bg-blue-600 text-white
          rounded-md
          font-medium
          hover:bg-blue-700
          transition-colors
        "
				>
					Get Started
				</button>
			</div>
		</div>
	</nav>

	<!-- Page Content -->
	<div class="max-w-7xl mx-auto px-4 py-16">
		<h1 class="text-4xl font-bold text-gray-900 mb-4">Welcome to MyBrand</h1>
		<p class="text-gray-600 text-lg">
			Scroll down to see the sticky navigation bar in action.
		</p>

		<!-- Add some height to enable scrolling -->
		<div class="mt-96 pt-96">
			<p class="text-gray-600">Keep scrolling...</p>
		</div>
	</div>
</div>
```

</details>

---

## Part 5: Understanding shadcn/ui Through the Button Component (1 hour)

Now that you understand Tailwind, let's dive into how professional component libraries are built. We're going to examine shadcn/ui's Button component in detail.

### What Is shadcn/ui?

shadcn/ui is NOT a traditional component library that you install via npm. Instead, it's a collection of **copy-paste components** that you own and can customise. When you add a component, the actual code is copied into your project.

**Why is this brilliant?**

- You own the code - no black box
- Fully customisable
- No package version conflicts
- Built with accessibility in mind
- Uses modern patterns (TypeScript, Tailwind)

### Setting Up shadcn/ui

First, install the required dependencies:

```bash
pnpm add class-variance-authority clsx tailwind-merge
pnpm add -D @types/node
```

Create `src/lib/utils.ts`:

```typescript
import { clsx, type ClassValue } from "clsx";
import { twMerge } from "tailwind-merge";

export function cn(...inputs: ClassValue[]) {
	return twMerge(clsx(inputs));
}
```

> #### Facing installation problems?
>
> Take a look at their [docs](https://ui.shadcn.com/docs/installation/vite).

**What does the `cn` function do?**

The `cn` (className) function intelligently combines Tailwind class names:

```typescript
// clsx allows conditional classes
cn("px-4 py-2", condition && "bg-blue-500");

// twMerge prevents conflicts - if you have both bg-blue-500 and bg-red-500,
// it keeps only the last one
cn("bg-blue-500", "bg-red-500"); // Result: "bg-red-500"

// This is super useful when you want component variants!
```

Create `components.json` in your project root:

```json
{
	"$schema": "https://ui.shadcn.com/schema.json",
	"style": "new-york",
	"rsc": false,
	"tsx": true,
	"tailwind": {
		"config": "",
		"css": "src/index.css",
		"baseColor": "zinc",
		"cssVariables": true,
		"prefix": ""
	},
	"aliases": {
		"components": "@/components",
		"utils": "@/lib/utils",
		"ui": "@/components/ui",
		"lib": "@/lib",
		"hooks": "@/hooks"
	}
}
```

Now install the Button component:

```bash
pnpm dlx shadcn@latest add button
```

**What just happened?** The CLI created `src/components/ui/button.tsx` with the Button component code. You now OWN this code!

### Anatomy of the Button Component

Let's examine the Button component that was just created. Open `src/components/ui/button.tsx` and you'll see something like this:

```typescript
// src/components/ui/button.tsx
import * as React from "react";
import { cva, type VariantProps } from "class-variance-authority";
import { cn } from "@/lib/utils";

const buttonVariants = cva(
	// Base styles applied to ALL button variants
	"inline-flex items-center justify-center gap-2 whitespace-nowrap rounded-md text-sm font-medium transition-colors focus-visible:outline-none focus-visible:ring-1 focus-visible:ring-ring disabled:pointer-events-none disabled:opacity-50",
	{
		variants: {
			// Different visual styles
			variant: {
				default:
					"bg-primary text-primary-foreground shadow hover:bg-primary/90",
				destructive:
					"bg-destructive text-destructive-foreground shadow-sm hover:bg-destructive/90",
				outline:
					"border border-input bg-background shadow-sm hover:bg-accent hover:text-accent-foreground",
				secondary:
					"bg-secondary text-secondary-foreground shadow-sm hover:bg-secondary/80",
				ghost: "hover:bg-accent hover:text-accent-foreground",
				link: "text-primary underline-offset-4 hover:underline",
			},
			// Different sizes
			size: {
				default: "h-9 px-4 py-2",
				sm: "h-8 rounded-md px-3 text-xs",
				lg: "h-10 rounded-md px-8",
				icon: "h-9 w-9",
			},
		},
		defaultVariants: {
			variant: "default",
			size: "default",
		},
	}
);

export interface ButtonProps
	extends React.ButtonHTMLAttributes<HTMLButtonElement>,
		VariantProps<typeof buttonVariants> {
	asChild?: boolean;
}

const Button = React.forwardRef<HTMLButtonElement, ButtonProps>(
	({ className, variant, size, ...props }, ref) => {
		return (
			<button
				className={cn(buttonVariants({ variant, size, className }))}
				ref={ref}
				{...props}
			/>
		);
	}
);
Button.displayName = "Button";

export { Button, buttonVariants };
```

**Let's break this down step by step:**

#### 1. The `cva` Function (class-variance-authority)

`cva` stands for "class variance authority" - it creates variant-based component styles:

```typescript
const buttonVariants = cva(
  "base classes here",  // Always applied
  {
    variants: {
      variant: { ... },   // Different styles
      size: { ... }       // Different sizes
    },
    defaultVariants: { ... }
  }
)
```

This is the secret sauce that makes shadcn/ui components so flexible!

#### 2. Base Styles (Always Applied)

```typescript
"inline-flex items-center justify-center gap-2 whitespace-nowrap rounded-md text-sm font-medium transition-colors focus-visible:outline-none focus-visible:ring-1 focus-visible:ring-ring disabled:pointer-events-none disabled:opacity-50";
```

These classes ensure every button:

- Uses Flexbox for layout (`inline-flex items-center justify-center`)
- Has consistent spacing (`gap-2`)
- Text doesn't wrap (`whitespace-nowrap`)
- Has rounded corners (`rounded-md`)
- Has smooth colour transitions (`transition-colors`)
- Has proper focus states for keyboard navigation (`focus-visible:ring-1`)
- Has disabled state styling (`disabled:opacity-50`)

#### 3. Variant Styles

```typescript
variant: {
  default: "bg-primary text-primary-foreground shadow hover:bg-primary/90",
  destructive: "bg-destructive text-destructive-foreground shadow-sm hover:bg-destructive/90",
  outline: "border border-input bg-background shadow-sm hover:bg-accent hover:text-accent-foreground",
  // etc...
}
```

Each variant defines a complete visual style. The `/90` syntax means "90% opacity" - so `hover:bg-primary/90` makes the background slightly lighter on hover.

#### 4. Size Variants

```typescript
size: {
  default: "h-9 px-4 py-2",
  sm: "h-8 rounded-md px-3 text-xs",
  lg: "h-10 rounded-md px-8",
  icon: "h-9 w-9",
}
```

Different size options for different contexts.

#### 5. The `cn` Function in Action

Notice how the Button component uses `cn`:

```typescript
className={cn(buttonVariants({ variant, size, className }))}
```

This combines:

1. The base styles from `cva`
2. The variant styles based on props
3. Any additional className passed by the user

If there are conflicts, the last one wins (thanks to `twMerge`)!

#### 6. Using the Button

Now you can use the Button with different variants:

```tsx
import { Button } from "@/components/ui/button";

function MyComponent() {
	return (
		<div className="flex gap-4">
			<Button>Default</Button>
			<Button variant="destructive">Delete</Button>
			<Button variant="outline">Cancel</Button>
			<Button variant="ghost">Ghost</Button>
			<Button size="sm">Small</Button>
			<Button size="lg">Large</Button>
		</div>
	);
}
```

#### 7. Customising the Button

Because you own the code, you can add your own variants! Open `src/components/ui/button.tsx` and add:

```typescript
variant: {
  default: "...",
  destructive: "...",
  outline: "...",
  // Add your own!
  success: "bg-green-600 text-white shadow hover:bg-green-700",
  warning: "bg-yellow-500 text-black shadow hover:bg-yellow-600",
}
```

Now use it:

```tsx
<Button variant="success">Save</Button>
<Button variant="warning">Warning</Button>
```

### Key Takeaways About shadcn/ui

1. **You own the code** - it's copied into your project
2. **Built with `cva`** - for managing variants elegantly
3. **Uses the `cn` utility** - for combining classes intelligently
4. **Fully customisable** - edit the files as you like
5. **Type-safe** - full TypeScript support
6. **Accessible** - keyboard navigation, focus management built-in

---

## Part 6: shadcn/ui Playground (Open-Ended Exploration)

Now it's your turn to explore! Head to **https://ui.shadcn.com/docs/components** and browse the available components.

### Your Mission

1. **Browse the shadcn/ui documentation** - look at different components
2. **Pick 2-3 components that interest you** - maybe Card, Input, Checkbox?
3. **Install them using the CLI**:
   ```bash
   pnpm dlx shadcn@latest add card
   pnpm dlx shadcn@latest add input
   pnpm dlx shadcn@latest add checkbox
   ```
4. **Experiment in your StylingLesson.tsx page** - try them out!
5. **Look at the source code** - open the generated files in `src/components/ui/`
6. **Customise them** - change colours, add variants, make them yours!

### Recommended Components to Explore

Start with these beginner-friendly components:

- **Card** - for layouts and sections
- **Input** - for form inputs
- **Label** - for form labels
- **Checkbox** - for checkboxes
- **Badge** - for tags and status indicators
- **Separator** - for visual dividers

### Example: Adding and Using a Card

```bash
pnpm dlx shadcn@latest add card
```

Now use it in your StylingLesson component:

```tsx
// In StylingLesson.tsx
import {
	Card,
	CardContent,
	CardDescription,
	CardHeader,
	CardTitle,
} from "@/components/ui/card";
import { Button } from "@/components/ui/button";

function StylingLesson() {
	return (
		<div className="max-w-4xl mx-auto p-8 space-y-6">
			<Card>
				<CardHeader>
					<CardTitle>My First shadcn/ui Card</CardTitle>
					<CardDescription>
						This is a professionally styled card component
					</CardDescription>
				</CardHeader>
				<CardContent>
					<p className="text-gray-600 mb-4">
						Cards are perfect for grouping related content together.
					</p>
					<Button>Learn More</Button>
				</CardContent>
			</Card>

			{/* Try adding more cards! */}
			<div className="grid grid-cols-1 md:grid-cols-2 gap-6">
				<Card>
					<CardHeader>
						<CardTitle>Card 2</CardTitle>
					</CardHeader>
					<CardContent>
						<p>Content here</p>
					</CardContent>
				</Card>

				<Card>
					<CardHeader>
						<CardTitle>Card 3</CardTitle>
					</CardHeader>
					<CardContent>
						<p>Content here</p>
					</CardContent>
				</Card>
			</div>
		</div>
	);
}
```

### When to Use shadcn/ui vs Plain Tailwind

**Use plain Tailwind when:**

- You need something simple (a div with padding and margins)
- You want full control without overhead
- It's a one-off design

**Use shadcn/ui when:**

- You need interactive components (buttons, inputs, checkboxes)
- You want consistent styling across your app
- You need built-in accessibility
- You want to save time and follow best practices

---

## 🏋️ Main Exercise: Convert Your Previous Components to Tailwind (60-90 minutes)

Now for the main challenge! Take your components from previous lessons and give them a professional makeover with Tailwind and shadcn/ui.

### Requirements

Go through your previous lesson pages and convert them to use Tailwind:

#### 1. Convert Your Counter Component (Lesson 2)

**Current version:** Uses inline styles
**New version:** Should use Tailwind classes + shadcn/ui Button

**Where to find it:** Your InteractivityLesson page (`src/pages/fundamentals/InteractivityLesson.tsx`)

**Suggestions:**

- Replace all inline `style={{...}}` with Tailwind classes
- Use `flex` and `gap` for button layout
- Use shadcn/ui `Button` component for all buttons
- Use Tailwind typography classes (`text-4xl`, `font-bold`) for the count display
- Add hover effects with `hover:` modifier
- Make it responsive with breakpoint classes

#### 2. Convert Your Todo App (Lesson 3)

**Current version:** Uses inline styles
**New version:** Should use Tailwind classes + shadcn/ui components

**Where to find it:** Your ListsLesson page (`src/pages/fundamentals/ListsLesson.tsx`)

**Suggestions:**

- Use shadcn/ui `Button` for add/delete actions
- Use shadcn/ui `Input` for the text input
- Use shadcn/ui `Checkbox` for todo completion
- Optionally use shadcn/ui `Card` to wrap sections
- Add hover effects on todo items (`hover:bg-gray-50`)
- Use Tailwind for all spacing (`space-y-4`, `gap-2`, etc.)
- Make it responsive

#### 3. Convert All Other Components

Look through your InteractivityLesson and ListsLesson pages. For each component with inline styles:

1. **Replace inline `style={{...}}`** with equivalent Tailwind classes
2. **Use shadcn/ui components** where appropriate:
   - Buttons → `<Button>`
   - Text inputs → `<Input>`
   - Checkboxes → `<Checkbox>`
   - Containers → consider `<Card>` if it makes sense
3. **Add interactive states** with `hover:`, `focus:`, `active:` modifiers
4. **Make responsive** with `md:`, `lg:` breakpoints
5. **Ensure consistent spacing** with Tailwind's spacing scale

### Example: Converting a Simple Component

**Before (inline styles):**

```tsx
function SimpleCard() {
	return (
		<div
			style={{
				backgroundColor: "white",
				padding: "24px",
				borderRadius: "8px",
				boxShadow: "0 1px 3px rgba(0,0,0,0.1)",
				maxWidth: "400px",
			}}
		>
			<h2
				style={{ fontSize: "24px", fontWeight: "bold", marginBottom: "16px" }}
			>
				Title
			</h2>
			<p style={{ color: "#666", marginBottom: "16px" }}>
				Description text here
			</p>
			<button
				style={{
					backgroundColor: "#3b82f6",
					color: "white",
					padding: "8px 16px",
					borderRadius: "6px",
					border: "none",
					cursor: "pointer",
				}}
			>
				Click me
			</button>
		</div>
	);
}
```

**After (Tailwind + shadcn/ui):**

```tsx
import { Button } from "@/components/ui/button";

function SimpleCard() {
	return (
		<div className="bg-white p-6 rounded-lg shadow-md max-w-md">
			<h2 className="text-2xl font-bold mb-4">Title</h2>
			<p className="text-gray-600 mb-4">Description text here</p>
			<Button>Click me</Button>
		</div>
	);
}
```

**Benefits:**

- Much shorter and cleaner code
- Professional button with built-in hover states and accessibility
- Easy to make responsive
- Consistent with your design system

### Success Criteria

Your converted components should:

✅ Use Tailwind utility classes instead of inline styles
✅ Use shadcn/ui components for buttons, inputs, checkboxes where appropriate
✅ Have hover and focus states
✅ Be responsive (test at different screen widths!)
✅ Have consistent spacing using Tailwind's spacing scale
✅ Look professional and polished

**Pro tip:** Work on one component at a time. Convert it, test it, then move to the next!

---

## 🎓 What You've Learned

Congratulations! You've just levelled up your styling game. Here's what you can now do:

✅ **Understand Flexbox** from playing Knights of the Flexbox Table
✅ **Use Tailwind CSS** for rapid UI development
✅ **Understand responsive design** with breakpoints
✅ **Use shadcn/ui components** for professional UIs
✅ **Understand `cva` and `cn`** for component variants
✅ **Customise component libraries** because you own the code
✅ **Build accessible interfaces** with built-in best practices
✅ **Work like professional developers** using industry-standard tools

## 📝 Quick Reference

### Essential Tailwind Cheat Sheet

```tsx
// Spacing
p-4 px-6 py-2 m-4 mx-auto gap-4 space-y-4

// Typography
text-sm text-lg text-2xl font-bold text-gray-600

// Layout - Flexbox
flex flex-col items-center justify-between gap-4

// Layout - Grid
grid grid-cols-3 gap-6

// Colours
bg-blue-500 text-white border-gray-300

// Effects
hover:bg-blue-600 shadow-md rounded-lg transition-colors

// Responsive
sm: md: lg: xl: 2xl:
```

### shadcn/ui Quick Start

```bash
# Install dependencies (one time)
pnpm add class-variance-authority clsx tailwind-merge

# Add components (as needed)
pnpm dlx shadcn@latest add button
pnpm dlx shadcn@latest add card
pnpm dlx shadcn@latest add input

# Use them in your components
import { Button } from '@/components/ui/button'
<Button variant="outline">Click me</Button>
```

### Useful Resources

- **Tailwind CSS Docs:** https://tailwindcss.com/docs
- **shadcn/ui Docs:** https://ui.shadcn.com/docs/components
- **Tailwind Play (practice):** https://play.tailwindcss.com/
- **Flexbox Game:** https://knightsoftheflexboxtable.com/

## 🚀 Next Steps

In the next lesson, we'll learn about **Context API & Global State Management** - how to share state across your entire application without passing props through every component. Your beautiful Tailwind styling will make that state management even more impressive!

## 💡 Pro Tips

1. **Keep Tailwind docs open** - you'll reference them constantly at first
2. **Use IntelliSense religiously** - it'll save you so much time
3. **Mobile-first approach** - style for mobile, then add larger breakpoints
4. **Explore shadcn/ui docs** - see what components are available
5. **Customise components** - don't be afraid to edit files in `components/ui/`
6. **Use the `cn` utility** - it's your best friend for combining classes
7. **Master Flexbox first** - understanding Flexbox deeply will make everything easier

---
