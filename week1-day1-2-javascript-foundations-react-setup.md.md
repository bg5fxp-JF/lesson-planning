# Week 1, Day 1-2: Your First Steps into React

Welcome to programming! In these first two days, we'll learn the essential building blocks and set up your React Learning Hub - a special project where you'll build and showcase everything you learn. Think of this as learning to speak a new language - we'll start with the most important words first.

## 🎯 What You'll Actually Build

By the end of Day 2, you'll have:

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

### Create Your React Project

Now let's create your React Learning Hub project:

```bash
# Create a new React project (this takes a minute)
pnpm create vite@latest my-dch-react-learning-hub --template react

# Go into the project folder
cd my-dch-react-learning-hub

# opens project in VS Code
code .

# Download all the tools we need (this also takes a minute)
pnpm install

# Start your development server
pnpm dev
```

**🎉 Success!** Open your browser and go to http://localhost:5173

You should see a page with a spinning React logo. **This is your React Learning Hub running on your computer!**

### Initialize Git for Your Project

Since we'll be committing and pushing changes throughout our learning journey, let's set up Git for this project:

```bash
# Stop the development server first (Ctrl+C in terminal)
# Then initialize git
git init

# Add all files to git
git add .

# Make your first commit
git commit -m "Initial React Learning Hub setup"
```

**What just happened?** We created a Git repository for your project so you can track all the changes you make as you learn!

In VS Code You should see a file tree on the left side. This is your Learning Hub project!

### Understanding What You Created

```
my-dch-react-learning-hub/
├── src/
│   ├── App.jsx         ← This is where we'll build your hub
│   └── main.jsx        ← This starts everything (don't touch)
├── index.html          ← The basic HTML page
└── package.json        ← Project settings (don't touch)
```

**The only file we care about right now is `App.jsx` - this is where you'll build your learning journey!**

---

## Part 3: Building Your Learning Hub Homepage (1.5 hours)

### Step 1: Understanding What We're Looking At

Open `src/App.jsx` in VS Code. You'll see something like this:

```jsx
function App() {
	return (
		<div>
			<h1>Vite + React</h1>
			{/* ... other code ... */}
		</div>
	);
}
```

**What is this?** This is a React component - think of it as a custom HTML tag that we create ourselves!

Let's clear it out and start fresh:

```jsx
// src/App.jsx - Replace everything with this
function App() {
	return (
		<div>
			<h1>Hello World!</h1>
		</div>
	);
}

export default App;
```

**Save the file and check your browser - you should see "Hello World!"**

### Step 2: Adding Your Name (Understanding Variables in JSX)

Let's make it personal by adding your name:

```jsx
function App() {
	const myName = "Sarah"; // Put YOUR name here!

	return (
		<div>
			<h1>Hello World!</h1>
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

Let's add more information about yourself:

```jsx
function App() {
	const myName = "Sarah";
	const myAge = 25;
	const myCity = "Boston";

	return (
		<div>
			<h1>Welcome to My React Learning Hub!</h1>
			<p>Hi! My name is {myName}.</p>
			<p>
				I'm {myAge} years old and I live in {myCity}.
			</p>
			<p>I'm learning React!</p>
		</div>
	);
}
```

**Try it:** Change the values to match your real information and see the website update!

### Step 4: Adding a List of Hobbies (Understanding Arrays)

Now let's add your hobbies using what we learned about arrays:

```jsx
function App() {
	const myName = "Sarah";
	const myAge = 25;
	const myCity = "Boston";
	const myHobbies = ["reading", "cooking", "gaming", "hiking"];

	return (
		<div>
			<h1>Welcome to My React Learning Hub!</h1>
			<p>Hi! My name is {myName}.</p>
			<p>
				I'm {myAge} years old and I live in {myCity}.
			</p>
			<p>I'm learning React!</p>

			<h2>My Hobbies:</h2>
			<ul>
				{myHobbies.map((hobby) => (
					<li>{hobby}</li>
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
function App() {
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
					Welcome to My React Learning Hub!
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

export default App;
```

**Congratulations! You just built your React Learning Hub homepage!** 🎉

---

## 📝 Your Challenge: Customize Your Learning Hub! (30 minutes)

Now it's your turn to make this Learning Hub truly yours. Here's what to do:

### Your Task:

Modify the code to include YOUR real information:

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

**Your Task:** Add a new variable called `myJob` (or `mySchool`) to your Learning Hub and display it.

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

In Days 3-4, we'll add navigation to your Learning Hub and introduce TypeScript! You'll learn how to create different pages for each lesson and how TypeScript makes development safer and more enjoyable.

Remember: Every expert was once a beginner. You're doing great! 🌟
