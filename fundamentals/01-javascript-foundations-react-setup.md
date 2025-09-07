# JavaScript Foundations & React Setup

Welcome to programming! In this lesson, we'll learn the essential building blocks and set up your React Learning Hub - a special project where you'll build and showcase everything you learn. Think of this as learning to speak a new language - we'll start with the most important words first.

## 🎯 What You'll Actually Build

By the end of this lesson, you'll have:

- **Your React Learning Hub** - a project that will grow with you as you learn
- **Your first page** showing your personal information
- Your name, hobbies, and goals displayed dynamically
- Everything styled and looking professional!

## 📚 What You Need to Know

- **Nothing!** We'll teach you everything from scratch
- A computer with internet access
- Ability to create folders and files
- Curiosity and patience (programming is like learning any skill - it takes practice!)

---

## Part 1: Essential JavaScript for React (1 hour)

Before we start with React, we need to learn just enough JavaScript to understand what's happening. Don't worry - we'll only cover exactly what you need!

### Getting Started with StackBlitz

1. Go to [stackblitz.com](https://stackblitz.com)
2. Click "Sign up" (you can use your Google/GitHub account)
3. Once logged in, click "Create New" → "JavaScript" (vanilla JS project)
4. You'll see a code editor on the left and a preview on the right
5. Clear everything in the `index.js` file - we'll start fresh!

> 🤖 **AI Learning Tip:** As you learn these concepts, you might be tempted to ask AI to "write the code for me." Instead, use AI to **understand concepts** and **test your knowledge**. Read our [AI Learning Companion Guide](./02-ai-learning-companion-guide.md) to learn how to use AI as your tutor, not your crutch!

### Understanding console.log() - Your First Tool

`console.log()` is how we see what our code is doing. It prints messages to the console (a special area for programmers):

```javascript
// This prints "Hello World" to the console
console.log("Hello World");

// You can print numbers
console.log(42);

// You can print multiple things
console.log("My age is", 25);
```

**To see the console in StackBlitz:** Look at the bottom of the preview area - there's a "Console" tab. Click it!

### Comments - Notes in Your Code

Comments are notes for humans. The computer ignores them completely:

```javascript
// This is a single-line comment - it starts with two slashes
// I can write anything here and it won't run

console.log("This code runs"); // I can also put comments at the end of lines

/* 
   This is a multi-line comment
   I can write multiple lines here
   Great for longer explanations!
*/

// Use comments to:
// 1. Explain what your code does
// 2. Temporarily disable code (just add // in front)
// 3. Leave notes for yourself or others
```

### Data Types - Different Kinds of Information

JavaScript has different types of data, like a toolbox with different tools:

#### 1. Strings (Text)

```javascript
// Strings are text wrapped in quotes
const myName = "Sarah"; // Double quotes
const myCity = "Boston"; // Single quotes (both work!)
const myHobby = `Reading`; // Backticks (special - we'll see why!)

console.log(myName); // Prints: Sarah
console.log(typeof myName); // Prints: string
```

#### 2. Numbers

```javascript
// Numbers don't need quotes
const myAge = 25; // Whole number (integer)
const myHeight = 5.8; // Decimal number
const temperature = -10; // Negative number

console.log(myAge); // Prints: 25
console.log(typeof myAge); // Prints: number

// You can do math with numbers
console.log(myAge + 5); // Prints: 30
console.log(myAge * 2); // Prints: 50
```

#### 3. Booleans (True/False)

```javascript
// Booleans are yes/no, on/off, true/false values
const isStudent = true;
const isRaining = false;
const hasLicense = true;

console.log(isStudent); // Prints: true
console.log(typeof isStudent); // Prints: boolean

// Booleans help us make decisions in code
if (isStudent) {
	console.log("You get a student discount!");
}
```

#### 4. Undefined and Null

```javascript
// undefined means "no value assigned yet"
let myFutureJob; // We declared it but didn't give it a value
console.log(myFutureJob); // Prints: undefined

// null means "intentionally empty"
const myPet = null; // I don't have a pet
console.log(myPet); // Prints: null
```

### Variables - Storing Information

Think of variables as labeled boxes where you store information:

```javascript
// Creating a variable (like putting "John" in a box labeled "name")
const name = "John";
const age = 25;
const city = "New York";

// Using variables
console.log("Hello, my name is " + name); // "Hello, my name is John"
```

**Template Literals - A Better Way to Combine Text:**

```javascript
// Instead of: "Hello, my name is " + name + " and I live in " + city
// We can write:
const greeting = `Hello, my name is ${name} and I live in ${city}`;
console.log(greeting); // "Hello, my name is John and I live in New York"
```

### Functions - Reusable Instructions

Functions are like recipes - a set of instructions you can use over and over:

```javascript
// Old way to write functions
function sayHello(personName) {
	return `Hello ${personName}!`;
}

// React way to write functions (arrow functions)
const sayHello = (personName) => {
	return `Hello ${personName}!`;
};

// Even shorter for simple functions
const sayHello = (personName) => `Hello ${personName}!`;

// Using the function
sayHello("Sarah"); // "Hello Sarah!"
```

### Arrays - Lists of Things

Arrays store multiple items in order:

```javascript
const hobbies = ["reading", "gaming", "cooking"];
const numbers = [1, 2, 3, 4, 5];
const friends = ["Alice", "Bob", "Charlie"];

// Getting items from an array (starts counting from 0)
hobbies[0]; // "reading" (first item)
hobbies[1]; // "gaming" (second item)
```

**The Most Important Array Method - .map():**

```javascript
const friends = ["Alice", "Bob", "Charlie"];

// Transform each friend into a greeting
const greetings = friends.map((friend) => `Hello ${friend}!`);
// Result: ["Hello Alice!", "Hello Bob!", "Hello Charlie!"]

// This is how we'll create lists in React!
```

### Objects - Storing Related Information

Objects group related information together:

```javascript
const person = {
	name: "Sarah",
	age: 28,
	city: "Boston",
};

// Getting information from objects
person.name; // "Sarah"
person.age; // 28
person["name"]; // "Sarah" (another way)
```

**Destructuring - Extracting Information:**

```javascript
const person = { name: "Sarah", age: 28, city: "Boston" };

// Instead of:
const name = person.name;
const age = person.age;

// We can write:
const { name, age } = person;

console.log(name); // "Sarah"
console.log(age); // 28
```

### How This Becomes React

Here's the magic - everything we just learned becomes React:

```jsx
// JavaScript array:
const hobbies = ["reading", "gaming", "cooking"]

// Becomes React list:
{hobbies.map(hobby => <li>{hobby}</li>)}

// JavaScript template literal:
const greeting = `Hello ${name}!`

// Becomes React content:
<h1>Hello {name}!</h1>

// JavaScript object:
const person = { name: "Sarah", age: 28 }

// Becomes React data:
<div>Hi, I'm {person.name} and I'm {person.age} years old</div>
```

**The Connection:** React takes the JavaScript you know and makes it create web pages!

---

## Part 2: Setting Up Your React Project (15 minutes)

**Prerequisites:** Before starting this section, make sure you've completed:

- [Environment Setup Guide](./00-environment-setup-beginner.md) - Install VS Code, Node.js, and pnpm
- [Git & GitHub Setup](./01-git-github-setup-simple.md) - Set up version control

If you haven't done these yet, please complete them first!

### Get Your React Learning Hub Project

We've prepared a starter React project for you! Let's clone it to your computer:

```bash
# Clone the React Learning Hub starter project
git clone https://github.com/bg5fxp-JF/my-dch-react-learning-hub.git

# Go into the project folder
cd my-dch-react-learning-hub

# Opens project in VS Code
code .

# Download all the tools we need (this takes a minute)
pnpm install

# Start your development server
pnpm dev
```

**🎉 Success!** Open your browser and go to http://localhost:5173

You should see your React Learning Hub with a navigation bar and dashboard page. **This is your React Learning Hub running on your computer!**

### Initialize Your Own Git Repository

Now let's set up your own Git repository to track your learning journey:

```bash
# Stop the development server first (Ctrl+C in terminal)
# Remove the existing git history (from the cloned repo)
rm -rf .git

# Initialize your own fresh git repository
git init

# Add all files to git
git add .

# Make your first commit
git commit -m "Initial React Learning Hub setup - starting my journey!"
```

**What just happened?** You now have your own Git repository for tracking all the changes you'll make as you learn! The starter code is now yours to modify and experiment with.

In VS Code You should see a file tree on the left side. This is your Learning Hub project!

### Understanding What You Created

```
my-dch-react-learning-hub/
├── src/
│   ├── App.jsx              ← Main app with routing already set up
│   ├── main.jsx             ← This starts everything (don't touch)
│   ├── pages/               ← Your lesson pages go here
│   │   ├── Dashboard.jsx    ← Your learning hub homepage
│   │   └── fundamentals/    ← React fundamentals lessons
│   └── components/          ← Reusable UI components
│       └── default/         ← Navigation and cards
├── index.html               ← The basic HTML page
└── package.json             ← Project settings
```

**The starter project includes:**

- React Router for navigation between pages
- A Dashboard component as your homepage
- Pre-built navigation and lesson card components
- Organized folder structure for your learning journey

---

## Part 3: Building Your First Component Lesson (1.5 hours)

### Step 1: Understanding What We're Looking At

Your project already has navigation set up! Let's explore:

1. **Open your browser** at http://localhost:5173 - You'll see the Dashboard with lesson cards
2. **Click on "React Components"** card - This takes you to your first lesson page!

Open `src/App.jsx` to see how routing works:

```jsx
function App() {
	return (
		<Router>
			<div>
				<Navigation /> {/* Navigation bar at the top */}
				<Routes>
					<Route path="/" element={<Dashboard />} /> {/* Hub for all lessons */}
					<Route
						path="/fundamentals/components"
						element={<ComponentsLesson />}
					/> {/* Your first lesson! */}
				</Routes>
			</div>
		</Router>
	);
}
```

**What is this?**

- `Dashboard` = Your hub for navigating between all lessons
- `ComponentsLesson` = Where you'll write your first React code!
- The navigation happens automatically when you click lesson cards

### Step 2: Working on Your First Lesson Page

Open `src/pages/fundamentals/ComponentsLesson.jsx` - THIS is where we'll build this lesson!

You'll see it already has some starter code. Let's modify it to add your personal information:

```jsx
// src/pages/fundamentals/ComponentsLesson.jsx
function ComponentsLesson() {
	const myName = "Sarah"; // Put YOUR name here!

	return (
		<div>
			<h1>Learning React Components</h1>
			<p>My name is {myName}</p>
		</div>
	);
}
```

**Key Points:**

- We created a variable `myName`
- In JSX (the HTML-looking part), we use `{myName}` to show the variable's value
- The curly braces `{}` mean "this is JavaScript, not regular text", if you were to remove the `{}` you'd just see "My name is myName" on your browser

### Step 3: Adding More Information

Let's expand the ComponentsLesson with more information about yourself:

```jsx
function ComponentsLesson() {
	const myName = "Sarah";
	const myAge = 25;
	const myCity = "Boston";

	return (
		<div>
			<h1>Learning React Components</h1>
			<h2>About Me</h2>
			<p>Hi! My name is {myName}.</p>
			<p>
				I'm {myAge} years old and I live in {myCity}.
			</p>
			<p>I'm learning React components!</p>
		</div>
	);
}
```

**Try it:** Change the values to match your real information and see the page update!

### Step 4: Adding a List of Hobbies (Understanding Arrays)

Now let's add your hobbies using what we learned about arrays:

```jsx
function ComponentsLesson() {
	const myName = "Sarah";
	const myAge = 25;
	const myCity = "Boston";
	const myHobbies = ["reading", "cooking", "gaming", "hiking"];

	return (
		<div>
			<h1>Learning React Components</h1>
			<h2>About Me</h2>
			<p>Hi! My name is {myName}.</p>
			<p>
				I'm {myAge} years old and I live in {myCity}.
			</p>
			<p>I'm learning React components!</p>

			<h2>My Hobbies:</h2>
			<ul>
				{myHobbies.map((hobby) => (
					<li key={hobby}>{hobby}</li>
				))}
			</ul>
		</div>
	);
}
```

**What just happened?**

- We created an array of hobbies
- We used `myHobbies.map()` to turn each hobby into a `<li>` element
- React automatically displays all the `<li>` elements as a list!

### Step 5: Making It Look Better (Adding Styles)

Let's add some styling to make it look more professional:

```jsx
function ComponentsLesson() {
	const myName = "Sarah";
	const myAge = 25;
	const myCity = "Boston";
	const myHobbies = ["reading", "cooking", "gaming", "hiking"];

	return (
		<div
			style={{
				maxWidth: "600px",
				margin: "0 auto",
				padding: "40px 20px",
				fontFamily: "Arial, sans-serif",
				lineHeight: "1.6",
			}}
		>
			<div
				style={{
					backgroundColor: "#f0f8ff",
					padding: "20px",
					borderRadius: "10px",
					marginBottom: "30px",
					textAlign: "center",
				}}
			>
				<h1 style={{ color: "#2c3e50", marginBottom: "10px" }}>
					Learning React Components
				</h1>
				<p style={{ fontSize: "18px", color: "#34495e" }}>
					Hi! My name is {myName}.
				</p>
			</div>

			<div
				style={{
					backgroundColor: "white",
					padding: "20px",
					borderRadius: "10px",
					boxShadow: "0 2px 10px rgba(0,0,0,0.1)",
					marginBottom: "20px",
				}}
			>
				<h2 style={{ color: "#2c3e50" }}>About Me</h2>
				<p>
					I'm {myAge} years old and I live in {myCity}.
				</p>
				<p>
					I'm building this Learning Hub to track my React journey and showcase
					everything I learn!
				</p>
			</div>

			<div
				style={{
					backgroundColor: "white",
					padding: "20px",
					borderRadius: "10px",
					boxShadow: "0 2px 10px rgba(0,0,0,0.1)",
				}}
			>
				<h2 style={{ color: "#2c3e50" }}>My Hobbies</h2>
				<ul style={{ listStyle: "none", padding: 0 }}>
					{myHobbies.map((hobby, index) => (
						<li
							key={index}
							style={{
								backgroundColor: "#ecf0f1",
								padding: "10px",
								margin: "5px 0",
								borderRadius: "5px",
								borderLeft: "4px solid #3498db",
							}}
						>
							{hobby}
						</li>
					))}
				</ul>
			</div>
		</div>
	);
}

export default ComponentsLesson;
```

**Congratulations! You just built your first React component lesson!** 🎉

**Navigation tip:** Click the logo or "Home" in the navigation to go back to your Dashboard!

---

## 📝 Your Challenge: Customize Your Component Lesson! (30 minutes)

Now it's your turn to make this component lesson truly yours. Here's what to do:

### Your Task:

Modify the ComponentsLesson.jsx to include YOUR real information:

1. **Change the personal details** to match you
2. **Add/remove hobbies** that you actually enjoy
3. **Add a new section** called "My Goals" with 3-4 things you want to achieve
4. **Change the colors** to ones you like better

### Step-by-Step Guide:

**Step 1: Update Your Information**

```jsx
// Change these to YOUR real info:
const myName = "Your Name Here";
const myAge = 25; // Your real age
const myCity = "Your City";
const myHobbies = ["hobby1", "hobby2", "hobby3"]; // YOUR hobbies
```

**Step 2: Add a Goals Section**
Add this before the closing `</div>`:

```jsx
<div
	style={{
		backgroundColor: "white",
		padding: "20px",
		borderRadius: "10px",
		boxShadow: "0 2px 10px rgba(0,0,0,0.1)",
		marginTop: "20px",
	}}
>
	<h2 style={{ color: "#2c3e50" }}>My Goals</h2>
	<ul style={{ listStyle: "none", padding: 0 }}>
		{/* Add your goals here - copy the pattern from hobbies! */}
	</ul>
</div>
```

**Step 3: Change Colors (Optional)**
Try changing `#f0f8ff` to a different color like:

- `#ffe6e6` (light red)
- `#e6ffe6` (light green)
- `#fff0e6` (light orange)

### Need Help?

- **Can't think of goals?** Try: "Learn React", "Build a portfolio", "Get a job in tech"
- **Stuck on the goals list?** Copy the exact pattern from the hobbies section
- **Colors not working?** Make sure you use quotes around the color code
- **Getting errors?** Don't ask AI to "fix this for me" - instead ask "Can you help me understand what this error means?"

> 🤖 **Remember:** If you're stuck, use the **30-60-90 Rule** from our [AI Learning Guide](./02-ai-learning-companion-guide.md): Try for 30 seconds yourself, think about what specifically confuses you for 60 seconds, then ask AI a targeted question about your specific confusion.

---

## 🏃‍♂️ Practice Drills

> ⚠️ **Critical Learning Moment:** These drills are where real learning happens! **Don't ask AI to solve them for you.** Instead, struggle with them first - that's how your brain builds programming muscle memory. Use AI to **understand concepts** or **get unstuck**, not to skip the challenge.
>
> **Smart AI Usage:** "I'm trying to add a variable but it's not showing up. Can you explain how variables work in JSX?" ✅  
> **Harmful AI Usage:** "Complete this drill for me" ❌

### Drill 1: Add More Information (10 minutes)

**Your Task:** Add a new variable called `myJob` (or `mySchool`) to your ComponentsLesson and display it.

**Hint:**

```jsx
const myJob = "Student"; // or whatever you do
// Then use {myJob} somewhere in your JSX
```

### Drill 2: Change a Color (10 minutes)

**Your Task:** Find one of the background colors in your code and change it to your favorite color.

**Hint:** Look for `backgroundColor: '#f0f8ff'` and change the `#f0f8ff` part to a different color like:

- `#ffeeee` (light pink)
- `#eeffee` (light green)
- `#eeeeff` (light blue)

### Drill 3: Add More Hobbies (10 minutes)

**Your Task:** Add 2 more hobbies to your hobbies array.

**Remember:** Arrays use square brackets `[]` and commas between items:

```jsx
const myHobbies = ["reading", "cooking", "your new hobby here"];
```

---

## 🎯 Checkpoint Questions

Before moving on, make sure you can answer these:

1. **Why does React exist?** (It solves the problem of keeping UI in sync with data)
2. **What is a component?** (A reusable piece of UI, like a custom HTML tag)
3. **What are props?** (Data passed to components to make them dynamic)
4. **What does JSX compile to?** (Regular JavaScript function calls)

---

## 📚 Resources for Curious Minds

- [React Official Docs - Thinking in React](https://react.dev/learn/thinking-in-react)
- [Why React Re-renders (Visual Guide)](https://www.joshwcomeau.com/react/why-react-re-renders/)
- [React Component Patterns](https://www.patterns.dev/posts/react-patterns)

---

## What's Next?

In the next lesson, we'll dive into **React State & Interactivity**! You'll learn how to make your components come alive by adding:

- **Interactive counters** that respond to button clicks
- **Show/hide sections** that toggle content visibility
- **Dynamic lists** where you can add new items
- **Event handling** to respond to user actions
- **Conditional rendering** to show different content based on state

You'll add a new lesson page to your existing React Learning Hub and build these interactive features step by step. By the end, your static components will become truly dynamic!

Remember: Every expert was once a beginner. You're doing great! 🌟
