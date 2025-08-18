# Week 1, Day 3-4: React State & Interactivity

Welcome to your first dive into interactive React! Up until now, your React Learning Hub has been static - it shows the same information every time. Today, we're going to bring it to life by learning how to make components respond to user actions and change over time.

## 🎯 What You'll Actually Build

By the end of Day 4, you'll have added to your React Learning Hub:

- **Interactive counter** that responds to button clicks
- **Show/hide sections** that toggle content visibility
- **Simple todo input** that adds items to a growing list
- **Dynamic content** that changes based on user interactions

All of this will be built into your existing `my-dch-react-learning-hub` project, so you can see your skills growing in one place!

## 📚 What You Need to Know Before Starting

- You should have completed Days 1-2 and have your React Learning Hub running
- Basic understanding of variables, arrays, and functions from Day 1
- Your React project should be working at http://localhost:5173

---

## Part 1: Setting Up Your New Lesson Page (15 minutes)

Great news! Your React Learning Hub from Days 1-2 already has the routing and navigation system set up. Now we just need to add today's lesson to the existing structure.

### Step 1: Add the New Lesson to Your Constants

First, let's add the new interactivity lesson to your lesson list. Open `/constants/index.js` and update the lessons array:

```javascript
// constants/index.js
export const lessons = [
	{
		id: 1,
		title: "React Components & JSX",
		path: "/week1/components",
	},
	{
		id: 2,
		title: "State & Interactivity", // Add this new lesson
		path: "/week1/interactivity",
	},
];
```

### Step 2: Create Your InteractivityLesson Component

Create a new file: `src/pages/week1/InteractivityLesson.jsx`

```jsx
// src/pages/week1/InteractivityLesson.jsx

function InteractivityLesson() {
	return <div></div>;
}

export default InteractivityLesson;
```

### Step 3: Add the Route to Your App

Update your `src/App.jsx` to include the new route. Add the import and route:

```jsx
// src/App.jsx - Add the import
import InteractivityLesson from "./pages/week1/InteractivityLesson";

// Then add the new route inside your Routes component:
<Route path="/week1/interactivity" element={<InteractivityLesson />} />;
```

**That's it!** Your React Learning Hub now has a dedicated page for today's interactivity lesson. Visit `/week1/interactivity` to see it!

---

---

## Part 2: Understanding State - Why Static Isn't Enough (30 minutes)

Now that we have our new lesson page set up, let's learn about state! Navigate to your Interactivity lesson page (/week1/interactivity) and we'll build everything there.

### The Problem with Static Components

Open your Components lesson page (/week1/components) and notice that no matter what you do, the information never changes. This is because we're using **variables** that are set once and never change:

```jsx
function ComponentsLesson() {
	const myName = "Sarah"; // This never changes
	const myHobbies = ["reading", "gaming"]; // This never changes either

	return (
		<div>
			<h1>Hi, I'm {myName}</h1>
			{/* This will always show the same name */}
		</div>
	);
}
```

**The question is: What if we want things to change?**

- What if we want a counter that goes up when we click a button?
- What if we want to add new hobbies to our list?
- What if we want to show or hide sections when someone clicks a button?

Regular variables can't do this in React. We need something special called **state**.

### What Is State?

**State** is React's way of remembering information that can change over time. Think of it like a special type of variable that:

1. **Remembers its value** between renders
2. **Triggers re-renders** when it changes (updates the page)
3. **Belongs to the component** where it's created

When state changes, React automatically updates the page to show the new information. This is the magic that makes web pages interactive!

### Your First State Example

Let's create a simple counter to see state in action. We'll add this to your **InteractivityLesson.jsx** page:

```jsx
// Update your src/pages/week1/InteractivityLesson.jsx file
import { useState } from "react";
import { Link } from "react-router-dom";

function InteractivityLesson() {
	// This is state! It starts at 0 and can change
	const [count, setCount] = useState(0);

	return (
		<div>
			<div
				style={{
					backgroundColor: "#e74c3c",
					color: "white",
					padding: "20px",
					borderRadius: "10px",
					marginBottom: "30px",
					textAlign: "center",
				}}
			>
				<h1>Days 3-4: State & Interactivity</h1>
				<p>
					Learn to make your React components come alive with state and events!
				</p>
			</div>

			{/* NEW: Add this counter section */}
			<div
				style={{
					backgroundColor: "white",
					padding: "20px",
					borderRadius: "10px",
					boxShadow: "0 2px 10px rgba(0,0,0,0.1)",
					marginTop: "20px",
					textAlign: "center",
				}}
			>
				<h2 style={{ color: "#2c3e50" }}>My First Interactive Component!</h2>
				<p style={{ fontSize: "24px", margin: "20px 0" }}>Count: {count}</p>
				<button
					onClick={() => setCount(count + 1)}
					style={{
						backgroundColor: "#3498db",
						color: "white",
						border: "none",
						padding: "10px 20px",
						borderRadius: "5px",
						fontSize: "16px",
						cursor: "pointer",
					}}
				>
					Click me!
				</button>
			</div>
		</div>
	);
}

export default InteractivityLesson;
```

**Save your file and click the button!** 🎉

Notice how the number changes every time you click? That's state in action!

### Breaking Down useState

Let's understand what just happened:

```jsx
const [count, setCount] = useState(0);
```

**What this means:**

1. `useState(0)` - Creates state with an initial value of 0
2. `count` - The current value of the state
3. `setCount` - The function to change the state
4. `[count, setCount]` - Array destructuring to get both values

**The pattern is always:**

```jsx
const [valueName, setValueName] = useState(initialValue);
```

### How State Updates Work

When you click the button:

```jsx
onClick={() => setCount(count + 1)}
```

**Here's what happens:**

1. Button is clicked
2. `setCount(count + 1)` runs (if count is 3, this becomes `setCount(4)`)
3. React sees the state changed
4. React re-renders the component with the new state value
5. The page updates to show the new count

**Important:** React doesn't change the existing `count` variable - it creates a whole new version of the component with the new value!

---

## Part 2: Event Handling - Responding to User Actions (1 hour)

### Understanding Events in React

Events are things that happen in your app - clicking buttons, typing in inputs, submitting forms, hovering over elements. React gives us a way to "listen" for these events and run code when they happen.

### Common Event Types

Let's add several different event types to your InteractivityLesson page. **Update your InteractivityLesson.jsx** to include these new state variables and sections:

```jsx
function InteractivityLesson() {
	const [count, setCount] = useState(0);
	const [message, setMessage] = useState("No button clicked yet");
	const [inputValue, setInputValue] = useState("");

	return (
		<div>
			<div
				style={{
					backgroundColor: "#e74c3c",
					color: "white",
					padding: "20px",
					borderRadius: "10px",
					marginBottom: "30px",
					textAlign: "center",
				}}
			>
				<h1>Days 3-4: State & Interactivity</h1>
				<p>
					Learn to make your React components come alive with state and events!
				</p>
			</div>

			{/* Your counter from the previous section */}
			{/* ... counter code ... */}

			{/* NEW: Event examples section */}
			<div
				style={{
					backgroundColor: "white",
					padding: "20px",
					borderRadius: "10px",
					boxShadow: "0 2px 10px rgba(0,0,0,0.1)",
					marginTop: "20px",
				}}
			>
				<h2 style={{ color: "#2c3e50" }}>Event Handling Examples</h2>

				{/* onClick Event */}
				<div style={{ marginBottom: "20px" }}>
					<h3>Click Events</h3>
					<button
						onClick={() => setMessage("Left button was clicked!")}
						style={{
							backgroundColor: "#e74c3c",
							color: "white",
							border: "none",
							padding: "10px 20px",
							borderRadius: "5px",
							marginRight: "10px",
							cursor: "pointer",
						}}
					>
						Click Me (Red)
					</button>
					<button
						onClick={() => setMessage("Right button was clicked!")}
						style={{
							backgroundColor: "#2ecc71",
							color: "white",
							border: "none",
							padding: "10px 20px",
							borderRadius: "5px",
							cursor: "pointer",
						}}
					>
						Click Me (Green)
					</button>
					<p style={{ marginTop: "10px", fontStyle: "italic" }}>{message}</p>
				</div>

				{/* onChange Event */}
				<div style={{ marginBottom: "20px" }}>
					<h3>Input Events</h3>
					<input
						type="text"
						placeholder="Type something here..."
						value={inputValue}
						onChange={(event) => setInputValue(event.target.value)}
						style={{
							padding: "10px",
							borderRadius: "5px",
							border: "1px solid #ddd",
							width: "300px",
							fontSize: "16px",
						}}
					/>
					<p style={{ marginTop: "10px" }}>
						You typed: <strong>{inputValue}</strong>
					</p>
				</div>
			</div>
		</div>
	);
}
```

### Understanding the Event Object

In the input example above, notice this line:

```jsx
onChange={(event) => setInputValue(event.target.value)}
```

**What's happening:**

1. `event` - An object containing information about what happened
2. `event.target` - The element that triggered the event (the input field)
3. `event.target.value` - The current text in the input field

This pattern is very common in React forms!

### Event Handler Functions

Instead of writing all our event logic inline, we can create functions:

```jsx
function InteractivityLesson() {
	const [count, setCount] = useState(0);

	// Event handler function
	const handleIncrement = () => {
		setCount(count + 1);
	};

	const handleDecrement = () => {
		setCount(count - 1);
	};

	const handleReset = () => {
		setCount(0);
	};

	return (
		<div>
			<p>Count: {count}</p>
			<button onClick={handleIncrement}>+</button>
			<button onClick={handleDecrement}>-</button>
			<button onClick={handleReset}>Reset</button>
		</div>
	);
}
```

**Why use separate functions?**

- Cleaner, more readable code
- Easier to add complex logic later
- Functions can be reused
- Better for debugging

---

## Part 3: Conditional Rendering - Showing Different Things (1 hour)

### What Is Conditional Rendering?

Conditional rendering means showing different content based on the current state. It's like having an if-statement for your JSX.

### Basic Show/Hide Pattern

Let's add a section to your Learning Hub that can be shown or hidden:

```jsx
function InteractivityLesson() {
	const [showDetails, setShowDetails] = useState(false);

	// ... your other state and variables

	return (
		<div
			style={
				{
					/* your styles */
				}
			}
		>
			{/* Your existing content */}

			{/* NEW: Conditional rendering section */}
			<div
				style={{
					backgroundColor: "white",
					padding: "20px",
					borderRadius: "10px",
					boxShadow: "0 2px 10px rgba(0,0,0,0.1)",
					marginTop: "20px",
				}}
			>
				<h2 style={{ color: "#2c3e50" }}>About My Learning Journey</h2>

				<button
					onClick={() => setShowDetails(!showDetails)}
					style={{
						backgroundColor: showDetails ? "#e74c3c" : "#3498db",
						color: "white",
						border: "none",
						padding: "10px 20px",
						borderRadius: "5px",
						marginBottom: "15px",
						cursor: "pointer",
					}}
				>
					{showDetails ? "Hide Details" : "Show Details"}
				</button>

				{showDetails && (
					<div
						style={{
							backgroundColor: "#f8f9fa",
							padding: "15px",
							borderRadius: "5px",
							borderLeft: "4px solid #3498db",
						}}
					>
						<h3>Why I'm Learning React</h3>
						<p>
							I'm learning React because I want to build modern web
							applications. So far I've learned about components, JSX, and now
							state management!
						</p>
						<p>My goals for this week:</p>
						<ul>
							<li>Master useState and event handling</li>
							<li>Build interactive components</li>
							<li>Understand when and why to use state</li>
						</ul>
					</div>
				)}
			</div>
		</div>
	);
}
```

### Understanding Conditional Rendering Syntax

There are several ways to do conditional rendering in React:

#### Method 1: && Operator (Most Common)

```jsx
{
	showDetails && <div>This only shows when showDetails is true</div>;
}
```

**How it works:**

- If `showDetails` is `true`, React shows the content after `&&`
- If `showDetails` is `false`, React shows nothing

#### Method 2: Ternary Operator (? :)

```jsx
{
	showDetails ? <div>Details are shown</div> : <div>Details are hidden</div>;
}
```

**How it works:**

- If condition before `?` is true, show first option
- If condition before `?` is false, show second option

#### Method 3: If Statement (Less Common in JSX)

```jsx
function InteractivityLesson() {
	const [showDetails, setShowDetails] = useState(false);

	let detailsContent = null;
	if (showDetails) {
		detailsContent = <div>Details content here</div>;
	}

	return <div>{detailsContent}</div>;
}
```

### Dynamic Button Text and Styling

Notice in our example how the button changes based on state:

```jsx
<button
	onClick={() => setShowDetails(!showDetails)}
	style={{
		backgroundColor: showDetails ? "#e74c3c" : "#3498db", // Red if true, blue if false
		// ... other styles
	}}
>
	{showDetails ? "Hide Details" : "Show Details"}{" "}
	{/* Different text based on state */}
</button>
```

This creates a much better user experience!

---

## Part 4: Working with Lists and Dynamic Content (1.5 hours)

### Adding Items to Lists

One of the most common interactive patterns is adding items to a list. Let's create a simple hobby tracker:

```jsx
function InteractivityLesson() {
	// ... your existing state
	const [newHobby, setNewHobby] = useState("");
	const [hobbies, setHobbies] = useState(["reading", "gaming", "cooking"]);

	const handleAddHobby = () => {
		if (newHobby.trim() !== "") {
			// Don't add empty hobbies
			setHobbies([...hobbies, newHobby]); // Add new hobby to existing list
			setNewHobby(""); // Clear the input
		}
	};

	return (
		<div
			style={
				{
					/* your styles */
				}
			}
		>
			{/* Your existing content */}

			{/* NEW: Dynamic hobby list */}
			<div
				style={{
					backgroundColor: "white",
					padding: "20px",
					borderRadius: "10px",
					boxShadow: "0 2px 10px rgba(0,0,0,0.1)",
					marginTop: "20px",
				}}
			>
				<h2 style={{ color: "#2c3e50" }}>My Growing Hobby List</h2>

				<div style={{ marginBottom: "20px" }}>
					<input
						type="text"
						placeholder="Enter a new hobby..."
						value={newHobby}
						onChange={(e) => setNewHobby(e.target.value)}
						style={{
							padding: "10px",
							borderRadius: "5px",
							border: "1px solid #ddd",
							width: "250px",
							marginRight: "10px",
							fontSize: "16px",
						}}
					/>
					<button
						onClick={handleAddHobby}
						style={{
							backgroundColor: "#2ecc71",
							color: "white",
							border: "none",
							padding: "10px 20px",
							borderRadius: "5px",
							cursor: "pointer",
							fontSize: "16px",
						}}
					>
						Add Hobby
					</button>
				</div>

				<ul style={{ listStyle: "none", padding: 0 }}>
					{hobbies.map((hobby, index) => (
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

				<p style={{ marginTop: "15px", color: "#7f8c8d" }}>
					Total hobbies: {hobbies.length}
				</p>
			</div>
		</div>
	);
}
```

### Understanding Array State Updates

**Important Rule:** Never mutate state arrays directly!

```jsx
// ❌ Wrong way (mutates existing array)
hobbies.push(newHobby);
setHobbies(hobbies);

// ✅ Right way (creates new array)
setHobbies([...hobbies, newHobby]);
```

**Why?** React needs to see that the state has changed. If you modify the existing array, React thinks nothing changed and won't re-render!

### The Spread Operator (...) Explained

```jsx
const oldArray = [1, 2, 3];
const newArray = [...oldArray, 4]; // [1, 2, 3, 4]
```

The spread operator (`...`) "spreads out" all items from an existing array into a new array. It's perfect for React state updates!

---

## 📝 Your Main Challenge: Build an Interactive Counter with Features (45 minutes)

Now it's time to apply everything you've learned! You'll build a feature-rich counter component.

### Your Task:

Add a new section to your **InteractivityLesson.jsx** page with a counter that has these features:

1. **Display current count**
2. **Increment button (+1)**
3. **Decrement button (-1)**
4. **Reset button (back to 0)**
5. **Step size input** (change how much each click adds/subtracts)
6. **History display** (show last 5 actions taken)

### Step-by-Step Guide:

**Step 1: Set up the state**

Add these new state variables to your **InteractivityLesson.jsx** component:

```jsx
function InteractivityLesson() {
	// ... your existing state

	// New state for the advanced counter
	const [counterValue, setCounterValue] = useState(0);
	const [stepSize, setStepSize] = useState(1);
	const [history, setHistory] = useState([]);

	// ... rest of your component
}
```

**Step 2: Create event handlers**

```jsx
const handleIncrement = () => {
	const newValue = counterValue + stepSize;
	setCounterValue(newValue);
	setHistory([...history, `+${stepSize} (now ${newValue})`].slice(-5)); // Keep only last 5
};

const handleDecrement = () => {
	const newValue = counterValue - stepSize;
	setCounterValue(newValue);
	setHistory([...history, `-${stepSize} (now ${newValue})`].slice(-5));
};

const handleReset = () => {
	setCounterValue(0);
	setHistory([...history, "Reset to 0"].slice(-5));
};
```

**Step 3: Build the UI** (Add this to your JSX)

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
	<h2 style={{ color: "#2c3e50" }}>Advanced Interactive Counter</h2>

	{/* Counter Display */}
	<div
		style={{
			fontSize: "48px",
			fontWeight: "bold",
			textAlign: "center",
			margin: "20px 0",
			color: counterValue >= 0 ? "#2ecc71" : "#e74c3c",
		}}
	>
		{counterValue}
	</div>

	{/* Step Size Input */}
	<div style={{ marginBottom: "20px", textAlign: "center" }}>
		<label style={{ marginRight: "10px" }}>Step size:</label>
		<input
			type="number"
			value={stepSize}
			onChange={(e) => setStepSize(Number(e.target.value))}
			style={{
				padding: "5px",
				width: "80px",
				textAlign: "center",
				border: "1px solid #ddd",
				borderRadius: "3px",
			}}
		/>
	</div>

	{/* Buttons */}
	<div style={{ textAlign: "center", marginBottom: "20px" }}>
		<button onClick={handleDecrement} style={buttonStyle("#e74c3c")}>
			-{stepSize}
		</button>
		<button
			onClick={handleReset}
			style={{ ...buttonStyle("#95a5a6"), margin: "0 10px" }}
		>
			Reset
		</button>
		<button onClick={handleIncrement} style={buttonStyle("#2ecc71")}>
			+{stepSize}
		</button>
	</div>

	{/* History */}
	<div style={{ marginTop: "20px" }}>
		<h3 style={{ color: "#2c3e50" }}>Recent Actions:</h3>
		{history.length === 0 ? (
			<p style={{ color: "#7f8c8d", fontStyle: "italic" }}>No actions yet</p>
		) : (
			<ul style={{ listStyle: "none", padding: 0 }}>
				{history.map((action, index) => (
					<li
						key={index}
						style={{
							backgroundColor: "#f8f9fa",
							padding: "8px",
							margin: "3px 0",
							borderRadius: "3px",
							fontSize: "14px",
						}}
					>
						{action}
					</li>
				))}
			</ul>
		)}
	</div>
</div>
```

**Step 4: Add the button style helper** (add this before your return statement)

```jsx
const buttonStyle = (bgColor) => ({
	backgroundColor: bgColor,
	color: "white",
	border: "none",
	padding: "10px 20px",
	borderRadius: "5px",
	fontSize: "16px",
	cursor: "pointer",
	minWidth: "80px",
});
```

### Challenge Extensions (If You Finish Early):

1. **Add a double/half feature**: Buttons to multiply or divide by 2
2. **Color coding**: Change the counter color based on the value (negative = red, zero = gray, positive = green)
3. **Limits**: Add maximum and minimum limits that prevent going beyond certain values
4. **Sound effects**: Research how to play sounds when buttons are clicked (hint: HTML5 Audio API)

---

## 🏃‍♂️ Practice Drills

> ⚠️ **Important Reminder:** These drills are for practicing concepts you just learned. Don't ask AI to solve them - that defeats the purpose! Use AI to understand concepts or get unstuck, not to skip the learning.

### Drill 1: Show/Hide Content Toggle (15 minutes)

**Your Task:** Add a new section to your **InteractivityLesson.jsx** page that shows/hides your goals or future plans.

**Requirements:**

- Button that says "Show My Goals" or "Hide My Goals"
- Content that appears/disappears when clicked
- Button color should change based on state (visible vs hidden)

**Hint:**

```jsx
const [showGoals, setShowGoals] = useState(false);
```

### Drill 2: Simple Todo Input (20 minutes)

**Your Task:** Add a mini todo list section to your **InteractivityLesson.jsx** page where you can add items (don't worry about removing them yet).

**Requirements:**

- Text input for new todos
- "Add Todo" button
- List that shows all todos
- Input should clear after adding a todo
- Show total number of todos

**Hint:**

```jsx
const [newTodo, setNewTodo] = useState("");
const [todos, setTodos] = useState([]);
```

### Drill 3: Dynamic Content Switcher (15 minutes)

**Your Task:** Add a section to your **InteractivityLesson.jsx** page with multiple buttons that each show different content.

**Requirements:**

- 3 buttons: "About Me", "My Skills", "My Goals"
- Clicking each button shows different content
- Only one section visible at a time
- Active button should look different from inactive ones

**Hint:**

```jsx
const [activeSection, setActiveSection] = useState("about");
```

---

## 🎯 Checkpoint Questions

Before moving on to Day 5-6, make sure you can answer these:

1. **What is state and why do we need it?** (Memory that can change and triggers re-renders)
2. **What's the difference between props and state?** (Props come from parent, state is owned by component)
3. **Why can't we mutate state directly?** (React won't know it changed and won't re-render)
4. **What does the spread operator (...) do?** (Creates a new array/object with existing items plus new ones)
5. **What's the difference between && and ternary operator for conditional rendering?** (&& shows nothing if false, ternary shows alternative)

### Quick Self-Test:

Look at this code and predict what happens:

```jsx
const [count, setCount] = useState(5);
const [message, setMessage] = useState("Hello");

// When this button is clicked, what will count and message be?
<button
	onClick={() => {
		setCount(count + 1);
		setMessage("Count is now " + count);
	}}
>
	Click me
</button>;
```

**Answer:** `count` will be 6, but `message` will be "Count is now 5" because state updates are asynchronous and `count` hasn't updated yet when we set the message.

---

## 📚 Understanding the React Re-render Cycle

Before we finish, it's important to understand what happens when state changes:

### The React Update Process:

1. **Event occurs** (user clicks button)
2. **Event handler runs** (your function executes)
3. **State setter called** (`setCount(newValue)`)
4. **React schedules re-render** (doesn't happen immediately)
5. **Component function runs again** (with new state values)
6. **DOM updates** (page shows new content)

### Why This Matters:

```jsx
const handleClick = () => {
	console.log("Before:", count); // Shows old value
	setCount(count + 1);
	console.log("After:", count); // Still shows old value!
	// The new value isn't available until the next render
};
```

This is why you can't immediately use the new state value after setting it. React batches updates for performance!

---

## Common Mistakes and How to Fix Them

### Mistake 1: Trying to Use State Immediately After Setting It

```jsx
// ❌ This won't work as expected
const handleClick = () => {
	setCount(count + 1);
	console.log(count); // Still shows old value
};

// ✅ Use useEffect if you need to react to state changes
// (We'll learn this in later lessons)
```

### Mistake 2: Modifying State Directly

```jsx
// ❌ Never do this
const handleAddHobby = () => {
	hobbies.push(newHobby); // Mutates existing array
	setHobbies(hobbies); // React doesn't detect the change
};

// ✅ Always create new arrays/objects
const handleAddHobby = () => {
	setHobbies([...hobbies, newHobby]);
};
```

### Mistake 3: Forgetting Event Object Parameter

```jsx
// ❌ This will cause an error
<input onChange={() => setValue(event.target.value)} />

// ✅ Include the event parameter
<input onChange={(event) => setValue(event.target.value)} />
```

---

## What's Next?

In Days 5-6, we'll build on today's state concepts to create more complex applications with:

- **Dynamic lists** where you can add AND remove items
- **Component communication** - sharing data between different components
- **More complex state** - managing multiple related pieces of information
- **Real application patterns** that you'll use in professional development

**For future lessons, you'll follow the same simple pattern:**

1. **Add the new lesson** to `/constants/index.js`
2. **Create the new component** in the appropriate folder
3. **Add the route** to your App.jsx
4. **Build all exercises** on that dedicated lesson page

This pattern keeps your Learning Hub organized and makes it easy to review concepts as you learn new ones. Every new lesson will get its own "room" in your Learning Hub!

You're building a solid foundation! State and interactivity are the heart of React applications. 🌟

---

## 🤖 AI Learning Companion Tips

**Great ways to use AI while practicing:**

- ✅ "Can you explain why my component isn't re-rendering when I click the button?"
- ✅ "I'm getting an error about 'Cannot read property'. What does this mean?"
- ✅ "Help me understand the difference between onClick and onSubmit"

**Don't do this:**

- ❌ "Write the todo list component for me"
- ❌ "Complete this exercise"
- ❌ "Give me the solution to drill 2"

Remember: The struggle is where the learning happens! Use AI to understand concepts, not to skip the work.
