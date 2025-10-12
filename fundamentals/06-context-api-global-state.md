# Context API & Global State

Welcome to Advanced React! You've mastered React fundamentals, TypeScript, and professional styling with Tailwind and shadcn/ui. But there's a problem lurking in your code: **prop drilling**. If you've ever passed user data through 5 components just to show a profile picture in the navbar, you know the pain. Today, we're solving that forever with the Context API!

## 🎯 What You'll Actually Build

By the end of this lesson, you'll have:

- **An authentication context** - manages user login/logout across your entire app
- **A secret component** - blurred until you log in (no prop drilling!)
- **A login modal** - beautiful shadcn Dialog with Form using react-hook-form
- **Modular components** - following SOLID principles from previous lessons

All of this will be demonstrated on your `/advanced/context` lesson page, with contexts that can be used across your entire Learning Hub!

## 📚 What You Need to Know Before Starting

- Completed Lessons 1-5 (React basics, TypeScript, Tailwind + shadcn/ui)
- Understanding of React state with `useState`
- Familiarity with component props and composition
- Basic TypeScript interfaces
- SOLID principles from previous lessons

---

## Part 1: The Prop Drilling Problem (30 minutes)

### The Pain You've Definitely Felt

Imagine you need to show user information in multiple places:

- The navigation bar (avatar and name)
- Protected lesson pages (check if logged in)
- Profile pages (show full user details)
- Settings pages (update user preferences)

Here's what you'd have to do WITHOUT Context API:

```tsx
// App.tsx - Top level
function App() {
	const [user, setUser] = useState<User | null>(null);

	return (
		<div>
			<Navigation user={user} onLogout={() => setUser(null)} />
			<Routes>
				<Route path="/" element={<HomePage user={user} />} />
				<Route path="/lessons" element={<LessonsPage user={user} />} />
				<Route path="/profile" element={<ProfilePage user={user} />} />
			</Routes>
		</div>
	);
}

// Navigation.tsx - Doesn't use user, just passes it down
function Navigation({ user, onLogout }: NavigationProps) {
	return (
		<nav>
			<Logo />
			<Menu />
			<UserMenu user={user} onLogout={onLogout} />
		</nav>
	);
}

// LessonsPage.tsx - Passes user to children
function LessonsPage({ user }: LessonsPageProps) {
	return (
		<div>
			<ProtectedSection user={user} />
		</div>
	);
}
```

**The Problems:**

1. ❌ Components that don't care about user must accept props just to pass them down
2. ❌ Every intermediate component needs user in its props
3. ❌ Adding new user features means updating many components
4. ❌ Testing becomes harder (must mock props everywhere)
5. ❌ Component signatures become cluttered

### What We Actually Want

```tsx
// App.tsx - Provide auth once at the top
function App() {
	return (
		<AuthProvider>
			<Navigation /> {/* No user props! */}
			<Routes>
				<Route path="/" element={<HomePage />} />
				<Route path="/lessons" element={<LessonsPage />} />
			</Routes>
		</AuthProvider>
	);
}

// Any component can access auth directly
function ProtectedSection() {
	const { user } = useAuth(); // No props needed!

	if (!user) return <BlurredContent />;
	return <SecretContent />;
}
```

**Much Better:**

- ✅ No prop drilling through components that don't care
- ✅ Any component can access user when needed
- ✅ Clean component signatures
- ✅ Easy to add new user-aware components
- ✅ Simpler testing

---

## Part 2: Understanding Context API (45 minutes)

### What Is Context?

Think of Context like **Wi-Fi in your home**:

- The **Provider** broadcasts data (like a Wi-Fi router)
- **Consumers** connect to get the data (like your devices)
- No cables (props) needed between the router and devices

### When Should You Use Context?

**Good Use Cases:**

- ✅ User authentication (needed across many components)
- ✅ Shopping cart (accessed from multiple places)
- ✅ User preferences (applied across the app)
- ✅ Notifications (triggered from anywhere)

**Bad Use Cases:**

- ❌ Form input values (use local state)
- ❌ Hover states (use local state)
- ❌ Data only 1-2 components need (use props)
- ❌ Theme (shadcn/ui handles this!)

**The Rule:** Use Context when 3+ components at different nesting levels need the same data.

### Context API Structure

Every Context has three parts:

```tsx
// 1. CREATE the context
const AuthContext = createContext<AuthContextType | undefined>(undefined);

// 2. PROVIDE the context (wrap your app)
<AuthContext.Provider value={authData}>
	<YourApp />
</AuthContext.Provider>;

// 3. CONSUME the context (use in components)
const auth = useContext(AuthContext);
```

---

## Part 3: Building the Authentication Context (1 hour)

### Step 1: Install Required Components

```bash
cd my-dch-react-learning-hub

pnpm dlx shadcn@latest add dialog
pnpm dlx shadcn@latest add form
pnpm dlx shadcn@latest add input
pnpm dlx shadcn@latest add label
pnpm dlx shadcn@latest add avatar
pnpm dlx shadcn@latest add badge

# Form requires these peer dependencies
pnpm add react-hook-form @hookform/resolvers zod
```

### Step 2: Create the Route First

Before building the components, let's create the route so you can navigate to your lesson page as you build!

```tsx
// Update constants/index.ts
export const lessons = [
	// ... existing lessons
	{
		id: 6,
		title: "Context API & Global State",
		path: "/advanced/context",
		category: "Advanced React",
	},
];

// In src/App.tsx, add the import and route
import ContextLesson from "./pages/advanced/ContextLesson";

<Route path="/advanced/context" element={<ContextLesson />} />;
```

Now create a basic placeholder page at `src/pages/advanced/ContextLesson.tsx`:

```tsx
// src/pages/advanced/ContextLesson.tsx
function ContextLesson() {
	return (
		<div className="container mx-auto px-4 py-12 max-w-4xl">
			<div className="text-center mb-12">
				<h1 className="text-4xl font-bold mb-4">Context API & Global State</h1>
				<p className="text-xl text-muted-foreground">
					Building this lesson page...
				</p>
			</div>
		</div>
	);
}

export default ContextLesson;
```

Now navigate to `/advanced/context` in your browser - you should see the placeholder! We'll build the real components next.

### Step 3: Create the Auth Context

Create `src/contexts/AuthContext.tsx`:

```tsx
// src/contexts/AuthContext.tsx
import {
	createContext,
	useContext,
	useState,
	useEffect,
	ReactNode,
} from "react";

interface User {
	id: string;
	name: string;
	email: string;
	avatar?: string;
}

interface AuthContextType {
	user: User | null;
	isAuthenticated: boolean;
	login: (email: string, password: string) => Promise<void>;
	logout: () => void;
}

const AuthContext = createContext<AuthContextType | undefined>(undefined);

interface AuthProviderProps {
	children: ReactNode;
}

export function AuthProvider({ children }: AuthProviderProps) {
	const [user, setUser] = useState<User | null>(() => {
		const savedUser = localStorage.getItem("user");
		return savedUser ? JSON.parse(savedUser) : null;
	});

	useEffect(() => {
		if (user) {
			localStorage.setItem("user", JSON.stringify(user));
		} else {
			localStorage.removeItem("user");
		}
	}, [user]);

	const isAuthenticated = user !== null;

	const login = async (email: string, password: string): Promise<void> => {
		// Simulate API call
		await new Promise((resolve) => setTimeout(resolve, 500));

		const userData: User = {
			id: crypto.randomUUID(),
			name: email.split("@")[0],
			email: email,
			avatar: `https://api.dicebear.com/7.x/avataaars/svg?seed=${email}`,
		};

		setUser(userData);
	};

	const logout = () => {
		setUser(null);
	};

	const value: AuthContextType = {
		user,
		isAuthenticated,
		login,
		logout,
	};

	return <AuthContext.Provider value={value}>{children}</AuthContext.Provider>;
}

export function useAuth() {
	const context = useContext(AuthContext);

	if (context === undefined) {
		throw new Error("useAuth must be used within an AuthProvider");
	}

	return context;
}
```

### Step 4: Wrap Your App

Update `src/App.tsx`:

```tsx
// src/App.tsx
import { BrowserRouter, Routes, Route } from "react-router-dom";
import { AuthProvider } from "./contexts/AuthContext";

function App() {
	return (
		<AuthProvider>
			<BrowserRouter>{/* Your routes */}</BrowserRouter>
		</AuthProvider>
	);
}

export default App;
```

---

## Part 4: Building Modular Components (1.5 hours)

Following SOLID principles, we'll create separate, focused components:

### Step 5: Create AuthStatus Component

Displays current authentication status (Single Responsibility Principle).

Create `src/components/context-demo/AuthStatus.tsx`:

```tsx
// src/components/context-demo/AuthStatus.tsx
import { useAuth } from "../../contexts/AuthContext";
import {
	Card,
	CardContent,
	CardDescription,
	CardHeader,
	CardTitle,
} from "@/components/ui/card";
import { Avatar, AvatarFallback, AvatarImage } from "@/components/ui/avatar";
import { Badge } from "@/components/ui/badge";
import { Button } from "@/components/ui/button";

interface AuthStatusProps {
	onLoginClick: () => void;
}

export function AuthStatus({ onLoginClick }: AuthStatusProps) {
	const { user, isAuthenticated, logout } = useAuth();

	return (
		<Card>
			<CardHeader>
				<CardTitle>Your Current Status</CardTitle>
				<CardDescription>
					This component uses useAuth() - no props for user data!
				</CardDescription>
			</CardHeader>
			<CardContent>
				{isAuthenticated && user ? (
					<div className="flex items-center justify-between">
						<div className="flex items-center gap-4">
							<Avatar className="h-16 w-16">
								<AvatarImage src={user.avatar} alt={user.name} />
								<AvatarFallback>{user.name[0].toUpperCase()}</AvatarFallback>
							</Avatar>
							<div>
								<p className="text-lg font-semibold">{user.name}</p>
								<p className="text-sm text-muted-foreground">{user.email}</p>
								<Badge className="mt-2">Authenticated</Badge>
							</div>
						</div>
						<Button onClick={logout} variant="outline">
							Log Out
						</Button>
					</div>
				) : (
					<div className="text-center py-4">
						<p className="text-muted-foreground mb-4">
							You're not logged in. Log in to unlock the secret content!
						</p>
						<Button onClick={onLoginClick}>Log In</Button>
					</div>
				)}
			</CardContent>
		</Card>
	);
}
```

### Step 6: Create SecretComponent

The protected content that's blurred when logged out (Open/Closed Principle - easily extendable).

Create `src/components/context-demo/SecretComponent.tsx`:

```tsx
// src/components/context-demo/SecretComponent.tsx
import { useAuth } from "../../contexts/AuthContext";
import {
	Card,
	CardContent,
	CardDescription,
	CardHeader,
	CardTitle,
} from "@/components/ui/card";
import { Button } from "@/components/ui/button";

interface SecretComponentProps {
	onLoginClick: () => void;
}

export function SecretComponent({ onLoginClick }: SecretComponentProps) {
	const { isAuthenticated } = useAuth();

	return (
		<Card className="relative overflow-hidden">
			<CardHeader>
				<CardTitle>Secret Content (Protected by Context)</CardTitle>
				<CardDescription>
					This component checks auth without any props being passed!
				</CardDescription>
			</CardHeader>
			<CardContent>
				<div
					className={
						isAuthenticated ? "" : "blur-md select-none pointer-events-none"
					}
				>
					<div className="bg-gradient-to-r from-purple-500 to-pink-500 text-white p-8 rounded-lg">
						<h3 className="text-2xl font-bold mb-4">
							🎉 You Unlocked the Secret!
						</h3>
						<p className="text-lg mb-4">
							This content was blurred until you logged in. This component
							accessed your authentication status using{" "}
							<code className="bg-white/20 px-2 py-1 rounded">useAuth()</code>-
							no props passed from parent components!
						</p>
						<div className="bg-white/20 p-4 rounded">
							<p className="font-mono text-sm">
								const &#123; isAuthenticated &#125; = useAuth();
							</p>
						</div>
					</div>
				</div>

				{!isAuthenticated && (
					<div className="absolute inset-0 flex items-center justify-center bg-background/80 backdrop-blur-sm">
						<Button size="lg" onClick={onLoginClick}>
							🔒 Log In to Unlock
						</Button>
					</div>
				)}
			</CardContent>
		</Card>
	);
}
```

### Step 7: Create LoginDialog

Modal with proper shadcn Form integration (Dependency Inversion - depends on abstractions).

Create `src/components/context-demo/LoginDialog.tsx`:

```tsx
// src/components/context-demo/LoginDialog.tsx
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import * as z from "zod";
import { useAuth } from "../../contexts/AuthContext";
import {
	Dialog,
	DialogContent,
	DialogDescription,
	DialogHeader,
	DialogTitle,
} from "@/components/ui/dialog";
import {
	Form,
	FormControl,
	FormField,
	FormItem,
	FormLabel,
	FormMessage,
} from "@/components/ui/form";
import { Input } from "@/components/ui/input";
import { Button } from "@/components/ui/button";

const loginSchema = z.object({
	email: z.string().email("Please enter a valid email address"),
	password: z.string().min(1, "Password is required"),
});

type LoginFormValues = z.infer<typeof loginSchema>;

interface LoginDialogProps {
	open: boolean;
	onOpenChange: (open: boolean) => void;
}

export function LoginDialog({ open, onOpenChange }: LoginDialogProps) {
	const { login } = useAuth();

	const form = useForm<LoginFormValues>({
		resolver: zodResolver(loginSchema),
		defaultValues: {
			email: "",
			password: "",
		},
	});

	const onSubmit = async (data: LoginFormValues) => {
		try {
			await login(data.email, data.password);
			onOpenChange(false);
			form.reset();
		} catch (error) {
			console.error("Login failed:", error);
		}
	};

	return (
		<Dialog open={open} onOpenChange={onOpenChange}>
			<DialogContent>
				<DialogHeader>
					<DialogTitle>Log In</DialogTitle>
					<DialogDescription>
						Enter any email and password to log in (mock authentication)
					</DialogDescription>
				</DialogHeader>

				<Form {...form}>
					<form onSubmit={form.handleSubmit(onSubmit)} className="space-y-4">
						<FormField
							control={form.control}
							name="email"
							render={({ field }) => (
								<FormItem>
									<FormLabel>Email</FormLabel>
									<FormControl>
										<Input
											type="email"
											placeholder="you@example.com"
											{...field}
										/>
									</FormControl>
									<FormMessage />
								</FormItem>
							)}
						/>

						<FormField
							control={form.control}
							name="password"
							render={({ field }) => (
								<FormItem>
									<FormLabel>Password</FormLabel>
									<FormControl>
										<Input
											type="password"
											placeholder="Enter any password"
											{...field}
										/>
									</FormControl>
									<FormMessage />
								</FormItem>
							)}
						/>

						<Button
							type="submit"
							className="w-full"
							disabled={form.formState.isSubmitting}
						>
							{form.formState.isSubmitting ? "Logging in..." : "Log In"}
						</Button>
					</form>
				</Form>
			</DialogContent>
		</Dialog>
	);
}
```

### Step 8: Update ContextLesson Page

Clean composition of all components (Interface Segregation - focused interfaces).

Update `src/pages/advanced/ContextLesson.tsx` to use all the components we created:

```tsx
// src/pages/advanced/ContextLesson.tsx
import { useState } from "react";
import { AuthStatus } from "../../components/context-demo/AuthStatus";
import { SecretComponent } from "../../components/context-demo/SecretComponent";
import { LoginDialog } from "../../components/context-demo/LoginDialog";

function ContextLesson() {
	const [isLoginOpen, setIsLoginOpen] = useState(false);

	const handleLoginClick = () => {
		setIsLoginOpen(true);
	};

	return (
		<div className="container mx-auto px-4 py-12 max-w-4xl">
			<div className="text-center mb-12">
				<h1 className="text-4xl font-bold mb-4">Context API & Global State</h1>
				<p className="text-xl text-muted-foreground">
					Eliminate prop drilling with global state management
				</p>
			</div>

			<div className="space-y-8">
				<AuthStatus onLoginClick={handleLoginClick} />
				<SecretComponent onLoginClick={handleLoginClick} />
			</div>

			<LoginDialog open={isLoginOpen} onOpenChange={setIsLoginOpen} />
		</div>
	);
}

export default ContextLesson;
```

Now refresh your browser at `/advanced/context` - your complete Context API lesson page is live!

**What We've Achieved:**

1. **Single Responsibility** - Each component has one job
2. **Separation of Concerns** - Auth logic in context, UI in components
3. **Reusability** - Components can be used elsewhere
4. **Testability** - Easy to test in isolation
5. **Maintainability** - Changes are localized

---

## 📝 Your Main Challenge: Add User Menu to Navigation (30 minutes)

Now apply what you've learned to your actual app navigation!

### Your Task:

Create a modular `UserMenu` component for your navigation bar.

### Requirements:

1. Create `src/components/UserMenu.tsx`
2. Use `useAuth()` hook (no props for user data)
3. Show login button when logged out
4. Show avatar + logout button when logged in
5. Add to your main Navigation component

### Hint:

```tsx
// src/components/UserMenu.tsx
import { useAuth } from "../contexts/AuthContext";
import { Button } from "@/components/ui/button";
import { Avatar, AvatarFallback, AvatarImage } from "@/components/ui/avatar";

export function UserMenu() {
	const { user, isAuthenticated, logout } = useAuth();

	if (!isAuthenticated || !user) {
		return (
			<Button variant="default" asChild>
				<a href="/advanced/context">Log In</a>
			</Button>
		);
	}

	return (
		<div className="flex items-center gap-3">
			<div className="flex items-center gap-2">
				<Avatar className="h-8 w-8">
					<AvatarImage src={user.avatar} alt={user.name} />
					<AvatarFallback>{user.name[0].toUpperCase()}</AvatarFallback>
				</Avatar>
				<span className="text-sm font-medium hidden md:inline">
					{user.name}
				</span>
			</div>
			<Button variant="outline" size="sm" onClick={logout}>
				Log Out
			</Button>
		</div>
	);
}
```

**Success Criteria:**

- ✅ Component uses useAuth() hook
- ✅ No user props passed
- ✅ Works across all pages
- ✅ Follows single responsibility principle

---

## 🏃‍♂️ Practice Drills

Focus on understanding context patterns through repetition.

### Drill 1: Shopping Cart Context (15 minutes)

**Your Task:** Create a basic shopping cart context.

**Focus:** Understanding the context structure through repetition.

**Requirements:**

1. Create `src/contexts/CartContext.tsx`
2. Define a `CartItem` interface with:
   - `id` (string)
   - `name` (string)
   - `quantity` (number)
3. Define `CartContextType` interface with:
   - `items` (array of CartItem)
   - `totalItems` (number - calculated from items)
   - `addItem` function
   - `clearCart` function
4. Create the context with `createContext`
5. Create `CartProvider` component that:
   - Stores items in state
   - Calculates totalItems
   - Provides add and clear functions
6. Create `useCart` custom hook with error handling

**Hints:**

- Use `reduce()` to calculate totalItems: `items.reduce((sum, item) => sum + item.quantity, 0)`
- Pattern is identical to AuthContext, just different data

**When to use:** E-commerce sites, learning platforms with course purchases, any app with a shopping cart.

### Drill 2: Settings Context (15 minutes)

**Your Task:** Create a user preferences context.

**Focus:** Managing settings with update and reset functions.

**Requirements:**

1. Create `src/contexts/SettingsContext.tsx`
2. Define a `Settings` interface with:
   - `fontSize` (type: `"small" | "medium" | "large"`)
   - `language` (type: `"en" | "es" | "fr"`)
   - `notifications` (boolean)
3. Define `SettingsContextType` interface with:
   - `settings` (Settings object)
   - `updateSettings` function (accepts `Partial<Settings>`)
   - `resetSettings` function
4. Create a `defaultSettings` object with sensible defaults
5. Create the context and provider
6. In the provider:
   - Initialize state with defaultSettings
   - `updateSettings` should merge new values: `setSettings(prev => ({ ...prev, ...newSettings }))`
   - `resetSettings` should restore defaults
7. Create `useSettings` custom hook

**Hints:**

- `Partial<Settings>` means all properties are optional
- Use spread operator to merge objects

**When to use:** Apps with user preferences, accessibility settings, localisation needs.

### Drill 3: Notification Context (15 minutes)

**Your Task:** Create a notification system context with auto-dismiss.

**Focus:** Working with arrays and setTimeout.

**Requirements:**

1. Create `src/contexts/NotificationContext.tsx`
2. Define a `Notification` interface with:
   - `id` (string)
   - `message` (string)
   - `type` (type: `"success" | "error" | "info"`)
3. Define `NotificationContextType` interface with:
   - `notifications` (array of Notification)
   - `addNotification` function (accepts message and type)
   - `removeNotification` function (accepts id)
4. Create the context and provider
5. In the provider:
   - Store notifications in state
   - `addNotification` should:
     - Generate a unique id with `crypto.randomUUID()`
     - Add notification to array
     - Set a timeout to remove it after 3 seconds
   - `removeNotification` should filter out the notification by id
6. Create `useNotifications` custom hook

**Hints:**

- Use `setTimeout(() => removeNotification(id), 3000)` for auto-dismiss
- Filter: `setNotifications(prev => prev.filter(n => n.id !== id))`

**When to use:** Toast notifications, success/error messages, system alerts.

---

## 🎯 Checkpoint Questions

1. **What problem does Context solve?** (Eliminates prop drilling)

2. **When should you use Context?** (When 3+ components at different levels need the same data)

3. **What are the three parts of Context?** (createContext, Provider, useContext/custom hook)

4. **Why create a custom hook?** (Error handling, enforces provider usage, TypeScript support)

5. **Why did we split into separate components?** (Single Responsibility Principle, easier testing, better reusability)

### Quick Self-Test:

What's wrong here?

```tsx
interface AppContextType {
	user: User;
	cart: Cart;
	settings: Settings;
	notifications: Notification[];
}
```

**Answer:** All state in one context means components re-render when ANY value changes, even if they only use one piece. Split into separate contexts (AuthProvider, CartProvider, etc.).

---

## 🤖 AI Learning Tips

**Good questions:**

- ✅ "Why is my context causing re-renders?"
- ✅ "Should I use Context or props here?"
- ✅ "How do I test components with Context?"

**Avoid:**

- ❌ "Build my context system"
- ❌ "Convert everything to Context"

**30-60-90 Rule:** Try for 30s, think for 60s, then ask targeted questions.

## 📚 Resources

- [React Context Docs](https://react.dev/reference/react/useContext)
- [shadcn/ui Form](https://ui.shadcn.com/docs/components/form)
- [React Hook Form](https://react-hook-form.com/)
