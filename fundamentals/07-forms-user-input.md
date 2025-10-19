# Forms & User Input

Now it's time to tackle one of the most common (and traditionally painful) aspects of web development: **forms**. Today, you'll learn the industry-standard approach using React Hook Form and Zod validation that makes forms actually enjoyable to work with!

## 🎯 What You'll Actually Build

By the end of this lesson, you'll have:

- **A registration form** - with complex validation rules and password matching
- **A login form** - simple and clean with shadcn/ui components
- **A feedback form** - with select dropdowns, radio buttons, and textareas
- **A contact form** - with file upload capabilities
- **Reusable form patterns** - that you can use across your entire Learning Hub!

All of this will be demonstrated on your `/advanced/forms` lesson page using React Hook Form + Zod + shadcn/ui - the professional stack you'll use in real projects.

## 📚 What You Need to Know Before Starting

- Completed Lessons 1-6 (React basics, TypeScript, Tailwind + shadcn/ui, Context API)
- Understanding of React state with `useState`
- Familiarity with TypeScript interfaces
- Basic understanding of form elements (input, textarea, select)

---

## Part 1: The Form Handling Problem (30 minutes)

### Remember the LoginDialog from Lesson 6?

In the previous lesson, you built a login form using manual form handling. Let's look at what you had to do:

```tsx
// From Lesson 6 - Manual form handling
export function LoginDialog({ open, onOpenChange }: LoginDialogProps) {
	const { login } = useAuth();

	// Separate state for each field
	const [email, setEmail] = useState("");
	const [password, setPassword] = useState("");
	const [errors, setErrors] = useState<{ email?: string; password?: string }>(
		{}
	);
	const [isSubmitting, setIsSubmitting] = useState(false);

	// Manual validation function
	const validateForm = (): boolean => {
		const newErrors: { email?: string; password?: string } = {};

		if (!email) {
			newErrors.email = "Email is required";
		} else if (!email.includes("@")) {
			newErrors.email = "Please enter a valid email address";
		}

		if (!password) {
			newErrors.password = "Password is required";
		}

		setErrors(newErrors);
		return Object.keys(newErrors).length === 0;
	};

	const handleSubmit = async (e: React.FormEvent) => {
		e.preventDefault();
		if (!validateForm()) return;

		setIsSubmitting(true);
		// ... submit logic
	};

	// ... JSX with manual error display
}
```

That's **a lot of code** for just two fields! Imagine doing this for a form with 10+ fields...

**The Problems:**

1. ❌ Massive state management overhead (one state per field)
2. ❌ Manual validation logic that's error-prone
3. ❌ Repetitive event handlers for every field
4. ❌ Complex error state management
5. ❌ Difficult to test and maintain
6. ❌ No standardised validation schema
7. ❌ Touch state management adds more complexity

### What We Actually Want

Now let's go back and edit that **same login form** from Lesson 6 using React Hook Form + Zod:

```tsx
// React Hook Form + Zod - The modern way
const loginSchema = z.object({
	email: z.string().email("Please enter a valid email address"),
	password: z.string().min(1, "Password is required"),
});

type LoginFormValues = z.infer<typeof loginSchema>;

export function LoginDialog({ open, onOpenChange }: LoginDialogProps) {
	const { login } = useAuth();

	const form = useForm<LoginFormValues>({
		resolver: zodResolver(loginSchema),
		defaultValues: { email: "", password: "" },
	});

	const onSubmit = async (data: LoginFormValues) => {
		// Data is already validated!
		await login(data.email, data.password);
		onOpenChange(false);
		form.reset();
	};

	return (
		<Dialog open={open} onOpenChange={onOpenChange}>
			<DialogContent>
				<DialogHeader>
					<DialogTitle>Log In</DialogTitle>
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
										<Input {...field} />
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
										<Input type="password" {...field} />
									</FormControl>
									<FormMessage />
								</FormItem>
							)}
						/>

						<Button type="submit" disabled={form.formState.isSubmitting}>
							{form.formState.isSubmitting ? "Logging in..." : "Log In"}
						</Button>
					</form>
				</Form>
			</DialogContent>
		</Dialog>
	);
}
```

**Compare the Two Approaches:**

| Manual Approach (Lesson 6)       | React Hook Form + Zod    |
| -------------------------------- | ------------------------ |
| 4 separate `useState` hooks      | 1 `useForm` hook         |
| Manual `validateForm()` function | Zod schema validation    |
| Manual error state management    | Automatic error handling |
| Manual form reset logic          | `form.reset()`           |
| 70+ lines of code                | 40 lines of code         |
| Error-prone                      | Type-safe                |

**Much Better:**

- ✅ Declarative validation with Zod schema
- ✅ Automatic state management (no useState spam!)
- ✅ Built-in error handling and display
- ✅ Type-safe form values with TypeScript inference
- ✅ Accessible by default with shadcn/ui
- ✅ **50% less code** for the same functionality
- ✅ Easy to test and maintain

---

## Part 2: Understanding React Hook Form + Zod (45 minutes)

### Why React Hook Form?

React Hook Form is the industry-standard library for form handling in React because it solves all the common form problems:

**Key Benefits:**

1. **Minimal Re-renders** - Only re-renders the fields that changed, not the entire form
2. **Built-in Validation** - Works seamlessly with Zod for schema-based validation
3. **TypeScript Support** - Automatic type inference from your schemas
4. **Easy Integration** - Works perfectly with shadcn/ui components
5. **Performance** - Optimised for large forms with many fields
6. **Developer Experience** - Clean, declarative code that's easy to maintain

### What is Zod?

Zod is a TypeScript-first schema validation library. Instead of writing manual validation functions, you declare what your data should look like:

```tsx
// Manual validation - The old way ❌
function validateUser(data: any) {
	const errors: Record<string, string> = {};

	if (!data.email || !data.email.includes("@")) {
		errors.email = "Invalid email";
	}

	if (!data.password || data.password.length < 8) {
		errors.password = "Password must be 8+ characters";
	}

	return errors;
}

// Zod schema - The modern way ✅
const userSchema = z.object({
	email: z.string().email("Invalid email address"),
	password: z.string().min(8, "Password must be 8+ characters"),
});

// Automatic TypeScript types!
type User = z.infer<typeof userSchema>;
```

**Why Zod is Amazing:**

- ✅ Write validation rules once, get TypeScript types for free
- ✅ Declarative and easy to read
- ✅ Composable schemas (build complex validation from simple parts)
- ✅ Custom error messages
- ✅ Complex validation with `.refine()` for cross-field validation

### shadcn/ui Form Components

shadcn/ui provides a complete form component system built on React Hook Form:

```tsx
import {
	Form,
	FormControl,
	FormDescription,
	FormField,
	FormItem,
	FormLabel,
	FormMessage,
} from "@/components/ui/form";
```

**Architecture:**

- `<Form>` - Wrapper that provides form context
- `<FormField>` - Connects React Hook Form to individual fields
- `<FormItem>` - Container with proper spacing
- `<FormLabel>` - Accessible label with proper associations
- `<FormControl>` - Wraps the actual input component
- `<FormDescription>` - Optional helper text
- `<FormMessage>` - Automatic error display

---

### How They Work Together

Here's the complete picture of how React Hook Form, Zod, and shadcn/ui work together:

```tsx
// 1. Define your schema with Zod
const loginSchema = z.object({
	email: z.string().email("Invalid email"),
	password: z.string().min(6, "Must be 6+ characters"),
});

// 2. Get TypeScript types automatically
type LoginFormValues = z.infer<typeof loginSchema>;

// 3. Set up React Hook Form with Zod resolver
function LoginForm() {
	const form = useForm<LoginFormValues>({
		resolver: zodResolver(loginSchema), // Connect Zod to React Hook Form
		defaultValues: {
			email: "",
			password: "",
		},
	});

	// 4. Handle form submission (data is already validated!)
	const onSubmit = (data: LoginFormValues) => {
		console.log(data); // Type-safe, validated data
	};

	// 5. Use shadcn/ui components for beautiful, accessible UI
	return (
		<Form {...form}>
			<form onSubmit={form.handleSubmit(onSubmit)}>
				<FormField
					control={form.control}
					name="email"
					render={({ field }) => (
						<FormItem>
							<FormLabel>Email</FormLabel>
							<FormControl>
								<Input {...field} />
							</FormControl>
							<FormMessage /> {/* Automatic error display */}
						</FormItem>
					)}
				/>
			</form>
		</Form>
	);
}
```

**The Magic:**

1. Zod validates your data and provides TypeScript types
2. React Hook Form manages form state and re-renders
3. shadcn/ui provides beautiful, accessible components
4. You get a professional form with minimal code!

---

## Part 3: Building Your Forms Lesson Page (1.5 hours)

### Step 1: Install Required Dependencies

```bash
cd my-dch-react-learning-hub

# shadcn/ui form components (if not already installed)
pnpm dlx shadcn@latest add form
pnpm dlx shadcn@latest add input
pnpm dlx shadcn@latest add textarea
pnpm dlx shadcn@latest add select
pnpm dlx shadcn@latest add checkbox
pnpm dlx shadcn@latest add radio-group

# React Hook Form and Zod (peer dependencies)
pnpm add react-hook-form @hookform/resolvers zod
```

### Step 2: Create the Route

Update `constants/index.ts`:

```tsx
// Update constants/index.ts
export const lessons = [
	// ... existing lessons
	{
		id: 7,
		title: "Forms & User Input",
		path: "/advanced/forms",
		category: "Advanced React",
	},
];
```

In `src/App.tsx`, add the import and route:

```tsx
import FormsLesson from "./pages/advanced/FormsLesson";

<Route path="/advanced/forms" element={<FormsLesson />} />;
```

Create a basic placeholder page at `src/pages/advanced/FormsLesson.tsx`:

```tsx
// src/pages/advanced/FormsLesson.tsx
function FormsLesson() {
	return (
		<div className="container mx-auto px-4 py-12 max-w-4xl">
			<div className="text-center mb-12">
				<h1 className="text-4xl font-bold mb-4">Forms & User Input</h1>
				<p className="text-xl text-muted-foreground">
					Modern form handling with React 19 and React Hook Form
				</p>
			</div>
		</div>
	);
}

export default FormsLesson;
```

Navigate to `/advanced/forms` in your browser to see the placeholder!

### Step 3: Create Registration Form

Now let's build a more complex form with React Hook Form and Zod validation.

Create `src/components/forms-demo/RegistrationForm.tsx`:

```tsx
// src/components/forms-demo/RegistrationForm.tsx
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import * as z from "zod";
import {
	Card,
	CardContent,
	CardDescription,
	CardHeader,
	CardTitle,
} from "@/components/ui/card";
import {
	Form,
	FormControl,
	FormDescription,
	FormField,
	FormItem,
	FormLabel,
	FormMessage,
} from "@/components/ui/form";
import { Input } from "@/components/ui/input";
import { Button } from "@/components/ui/button";
import { Checkbox } from "@/components/ui/checkbox";

// Zod schema with complex validation
const registrationSchema = z
	.object({
		username: z
			.string()
			.min(3, "Username must be at least 3 characters")
			.max(20, "Username must not exceed 20 characters")
			.regex(
				/^[a-zA-Z0-9_]+$/,
				"Username can only contain letters, numbers, and underscores"
			),
		email: z.string().email("Please enter a valid email address"),
		password: z
			.string()
			.min(8, "Password must be at least 8 characters")
			.regex(/[A-Z]/, "Password must contain at least one uppercase letter")
			.regex(/[a-z]/, "Password must contain at least one lowercase letter")
			.regex(/[0-9]/, "Password must contain at least one number"),
		confirmPassword: z.string(),
		agreeToTerms: z.boolean().refine((val) => val === true, {
			message: "You must agree to the terms and conditions",
		}),
	})
	.refine((data) => data.password === data.confirmPassword, {
		message: "Passwords do not match",
		path: ["confirmPassword"],
	});

type RegistrationFormValues = z.infer<typeof registrationSchema>;

export function RegistrationForm() {
	const form = useForm<RegistrationFormValues>({
		resolver: zodResolver(registrationSchema),
		defaultValues: {
			username: "",
			email: "",
			password: "",
			confirmPassword: "",
			agreeToTerms: false,
		},
		mode: "onChange", // Validate on change for better UX
	});

	const onSubmit = async (data: RegistrationFormValues) => {
		// Simulate API call
		await new Promise((resolve) => setTimeout(resolve, 1000));

		console.log("Registration data:", data);
		alert(`Welcome, ${data.username}! Check the console for form data.`);
		form.reset();
	};

	return (
		<Card>
			<CardHeader>
				<CardTitle>React Hook Form + Zod</CardTitle>
				<CardDescription>
					Advanced registration form with schema-based validation
				</CardDescription>
			</CardHeader>
			<CardContent>
				<Form {...form}>
					<form onSubmit={form.handleSubmit(onSubmit)} className="space-y-4">
						<FormField
							control={form.control}
							name="username"
							render={({ field }) => (
								<FormItem>
									<FormLabel>Username</FormLabel>
									<FormControl>
										<Input placeholder="johndoe" {...field} />
									</FormControl>
									<FormDescription>
										3-20 characters, letters, numbers, and underscores only
									</FormDescription>
									<FormMessage />
								</FormItem>
							)}
						/>

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
										<Input type="password" placeholder="••••••••" {...field} />
									</FormControl>
									<FormDescription>
										Must include uppercase, lowercase, and number
									</FormDescription>
									<FormMessage />
								</FormItem>
							)}
						/>

						<FormField
							control={form.control}
							name="confirmPassword"
							render={({ field }) => (
								<FormItem>
									<FormLabel>Confirm Password</FormLabel>
									<FormControl>
										<Input type="password" placeholder="••••••••" {...field} />
									</FormControl>
									<FormMessage />
								</FormItem>
							)}
						/>

						<FormField
							control={form.control}
							name="agreeToTerms"
							render={({ field }) => (
								<FormItem className="flex flex-row items-start space-x-3 space-y-0">
									<FormControl>
										<Checkbox
											checked={field.value}
											onCheckedChange={field.onChange}
										/>
									</FormControl>
									<div className="space-y-1 leading-none">
										<FormLabel>I agree to the terms and conditions</FormLabel>
										<FormDescription>
											By registering, you agree to our Terms of Service
										</FormDescription>
										<FormMessage />
									</div>
								</FormItem>
							)}
						/>

						<Button
							type="submit"
							className="w-full"
							disabled={form.formState.isSubmitting}
						>
							{form.formState.isSubmitting ? "Creating account..." : "Register"}
						</Button>
					</form>
				</Form>
			</CardContent>
		</Card>
	);
}
```

### Step 4: Update FormsLesson Page

Now let's compose everything into the lesson page:

```tsx
// src/pages/advanced/FormsLesson.tsx
import { RegistrationForm } from "@/components/forms-demo/RegistrationForm";

function FormsLesson() {
	return (
		<div className="container mx-auto px-4 py-12 max-w-4xl">
			<div className="text-center mb-12">
				<h1 className="text-4xl font-bold mb-4">Forms & User Input</h1>
				<p className="text-xl text-muted-foreground">
					Professional form handling with React Hook Form + Zod
				</p>
			</div>

			<div className="space-y-8">
				<div className="prose dark:prose-invert max-w-none">
					<h2 className="text-2xl font-semibold mb-4">
						The Professional Stack
					</h2>
					<p className="text-muted-foreground mb-6">
						This form demonstrates the industry-standard approach: React Hook
						Form for state management, Zod for validation, and shadcn/ui for
						beautiful, accessible components. This is the same stack used in
						production applications.
					</p>
				</div>
				<RegistrationForm />
			</div>
		</div>
	);
}

export default FormsLesson;
```

Now refresh your browser at `/advanced/forms` - your complete Forms lesson page is live!

---

## 📝 Your Main Challenge: User Feedback Form (45 minutes)

Apply what you've learned by building a complete feedback form!

### Your Task:

Create a feedback form that allows users to submit product feedback with:

1. Name (required, min 2 characters)
2. Email (required, valid email)
3. Product (select dropdown with options)
4. Rating (radio buttons: 1-5 stars)
5. Feedback message (textarea, required, min 10 characters)
6. Subscribe to newsletter (optional checkbox)

### Requirements:

1. Create `src/components/forms-demo/FeedbackForm.tsx`
2. Use React Hook Form + Zod for validation
3. Use shadcn/ui Form components and Select/RadioGroup
4. Show success message on submit
5. Reset form after successful submission

### Starter Code:

```tsx
// src/components/forms-demo/FeedbackForm.tsx
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import * as z from "zod";
import {
	Form,
	FormControl,
	FormField,
	FormItem,
	FormLabel,
	FormMessage,
} from "@/components/ui/form";
import { Input } from "@/components/ui/input";
import { Textarea } from "@/components/ui/textarea";
import { Button } from "@/components/ui/button";
import {
	Select,
	SelectContent,
	SelectItem,
	SelectTrigger,
	SelectValue,
} from "@/components/ui/select";
import { RadioGroup, RadioGroupItem } from "@/components/ui/radio-group";
import { Checkbox } from "@/components/ui/checkbox";

// TODO: Create your Zod schema here
const feedbackSchema = z.object({
	// Your schema here
});

type FeedbackFormValues = z.infer<typeof feedbackSchema>;

export function FeedbackForm() {
	const form = useForm<FeedbackFormValues>({
		resolver: zodResolver(feedbackSchema),
		defaultValues: {
			// Your defaults here
		},
	});

	const onSubmit = async (data: FeedbackFormValues) => {
		// TODO: Handle submission
	};

	return (
		<Form {...form}>
			<form onSubmit={form.handleSubmit(onSubmit)} className="space-y-4">
				{/* TODO: Add your form fields here */}
			</form>
		</Form>
	);
}
```

### Success Criteria:

- ✅ All fields have proper Zod validation
- ✅ Error messages display correctly
- ✅ Form uses shadcn/ui components
- ✅ Success message shows after submission
- ✅ Form resets after submission
- ✅ Fully typed with TypeScript

### Solution:

**⚠️ Important:** Only look at this solution after you've thoroughly attempted the exercise yourself. The learning happens in the struggle, not in copying code!

<details>
<summary>Click to reveal solution</summary>

```tsx
// src/components/forms-demo/FeedbackForm.tsx
import { useState } from "react";
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import * as z from "zod";
import {
	Card,
	CardContent,
	CardDescription,
	CardHeader,
	CardTitle,
} from "@/components/ui/card";
import {
	Form,
	FormControl,
	FormDescription,
	FormField,
	FormItem,
	FormLabel,
	FormMessage,
} from "@/components/ui/form";
import { Input } from "@/components/ui/input";
import { Textarea } from "@/components/ui/textarea";
import { Button } from "@/components/ui/button";
import {
	Select,
	SelectContent,
	SelectItem,
	SelectTrigger,
	SelectValue,
} from "@/components/ui/select";
import { RadioGroup, RadioGroupItem } from "@/components/ui/radio-group";
import { Checkbox } from "@/components/ui/checkbox";

const feedbackSchema = z.object({
	name: z.string().min(2, "Name must be at least 2 characters"),
	email: z.string().email("Please enter a valid email address"),
	product: z.string().min(1, "Please select a product"),
	rating: z.string().min(1, "Please select a rating"),
	feedback: z.string().min(10, "Feedback must be at least 10 characters"),
	subscribe: z.boolean().default(false),
});

type FeedbackFormValues = z.infer<typeof feedbackSchema>;

export function FeedbackForm() {
	const [submitted, setSubmitted] = useState(false);

	const form = useForm<FeedbackFormValues>({
		resolver: zodResolver(feedbackSchema),
		defaultValues: {
			name: "",
			email: "",
			product: "",
			rating: "",
			feedback: "",
			subscribe: false,
		},
	});

	const onSubmit = async (data: FeedbackFormValues) => {
		// Simulate API call
		await new Promise((resolve) => setTimeout(resolve, 1000));

		console.log("Feedback submitted:", data);
		setSubmitted(true);
		form.reset();

		// Hide success message after 3 seconds
		setTimeout(() => setSubmitted(false), 3000);
	};

	return (
		<Card>
			<CardHeader>
				<CardTitle>Product Feedback</CardTitle>
				<CardDescription>
					Help us improve by sharing your feedback
				</CardDescription>
			</CardHeader>
			<CardContent>
				{submitted && (
					<div className="mb-4 bg-green-50 text-green-700 px-4 py-3 rounded-md">
						Thank you for your feedback! We appreciate your input.
					</div>
				)}

				<Form {...form}>
					<form onSubmit={form.handleSubmit(onSubmit)} className="space-y-4">
						<FormField
							control={form.control}
							name="name"
							render={({ field }) => (
								<FormItem>
									<FormLabel>Name</FormLabel>
									<FormControl>
										<Input placeholder="John Doe" {...field} />
									</FormControl>
									<FormMessage />
								</FormItem>
							)}
						/>

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
							name="product"
							render={({ field }) => (
								<FormItem>
									<FormLabel>Product</FormLabel>
									<Select
										onValueChange={field.onChange}
										defaultValue={field.value}
									>
										<FormControl>
											<SelectTrigger>
												<SelectValue placeholder="Select a product" />
											</SelectTrigger>
										</FormControl>
										<SelectContent>
											<SelectItem value="learning-hub">Learning Hub</SelectItem>
											<SelectItem value="todo-app">Todo App</SelectItem>
											<SelectItem value="dashboard">Dashboard</SelectItem>
											<SelectItem value="other">Other</SelectItem>
										</SelectContent>
									</Select>
									<FormMessage />
								</FormItem>
							)}
						/>

						<FormField
							control={form.control}
							name="rating"
							render={({ field }) => (
								<FormItem>
									<FormLabel>Rating</FormLabel>
									<FormControl>
										<RadioGroup
											onValueChange={field.onChange}
											defaultValue={field.value}
											className="flex gap-4"
										>
											{[1, 2, 3, 4, 5].map((rating) => (
												<FormItem
													key={rating}
													className="flex items-center space-x-2 space-y-0"
												>
													<FormControl>
														<RadioGroupItem value={String(rating)} />
													</FormControl>
													<FormLabel className="font-normal">
														{rating} ⭐
													</FormLabel>
												</FormItem>
											))}
										</RadioGroup>
									</FormControl>
									<FormMessage />
								</FormItem>
							)}
						/>

						<FormField
							control={form.control}
							name="feedback"
							render={({ field }) => (
								<FormItem>
									<FormLabel>Your Feedback</FormLabel>
									<FormControl>
										<Textarea
											placeholder="Tell us what you think..."
											rows={4}
											{...field}
										/>
									</FormControl>
									<FormDescription>
										Minimum 10 characters required
									</FormDescription>
									<FormMessage />
								</FormItem>
							)}
						/>

						<FormField
							control={form.control}
							name="subscribe"
							render={({ field }) => (
								<FormItem className="flex flex-row items-start space-x-3 space-y-0">
									<FormControl>
										<Checkbox
											checked={field.value}
											onCheckedChange={field.onChange}
										/>
									</FormControl>
									<div className="space-y-1 leading-none">
										<FormLabel>Subscribe to our newsletter</FormLabel>
										<FormDescription>
											Get updates about new features and improvements
										</FormDescription>
									</div>
								</FormItem>
							)}
						/>

						<Button
							type="submit"
							className="w-full"
							disabled={form.formState.isSubmitting}
						>
							{form.formState.isSubmitting
								? "Submitting..."
								: "Submit Feedback"}
						</Button>
					</form>
				</Form>
			</CardContent>
		</Card>
	);
}
```

</details>

---

## 🏃‍♂️ Practice Drills

Build muscle memory with these form handling exercises.

### Drill 1: Login Form (20 minutes)

**Your Task:** Create a simple login form with email and password fields.

**Focus:** Basic React Hook Form + Zod validation patterns.

**Requirements:**

1. Create `src/components/forms-demo/LoginForm.tsx`
2. Use React Hook Form with Zod schema
3. Fields:
   - Email (required, valid email)
   - Password (required, min 6 characters)
   - Remember me (optional checkbox)
4. Use shadcn/ui Form components
5. Show loading state on submit
6. Log form data to console on submit

**Hints:**

```tsx
const loginSchema = z.object({
	email: z.string().email("Invalid email address"),
	password: z.string().min(6, "Password must be at least 6 characters"),
	rememberMe: z.boolean().default(false),
});
```

**When to use:** Authentication forms, simple login pages, password-protected sections.

**Solution:**

**⚠️ Important:** Only look at this solution after you've thoroughly attempted the exercise yourself. The learning happens in the struggle, not in copying code!

<details>
<summary>Click to reveal solution</summary>

```tsx
// src/components/forms-demo/LoginForm.tsx
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import * as z from "zod";
import {
	Card,
	CardContent,
	CardDescription,
	CardHeader,
	CardTitle,
} from "@/components/ui/card";
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
import { Checkbox } from "@/components/ui/checkbox";

const loginSchema = z.object({
	email: z.string().email("Invalid email address"),
	password: z.string().min(6, "Password must be at least 6 characters"),
	rememberMe: z.boolean().default(false),
});

type LoginFormValues = z.infer<typeof loginSchema>;

export function LoginForm() {
	const form = useForm<LoginFormValues>({
		resolver: zodResolver(loginSchema),
		defaultValues: {
			email: "",
			password: "",
			rememberMe: false,
		},
	});

	const onSubmit = async (data: LoginFormValues) => {
		// Simulate API call
		await new Promise((resolve) => setTimeout(resolve, 1000));

		console.log("Login data:", data);
		alert("Login successful! Check console for data.");
	};

	return (
		<Card className="max-w-md mx-auto">
			<CardHeader>
				<CardTitle>Log In</CardTitle>
				<CardDescription>Enter your credentials to continue</CardDescription>
			</CardHeader>
			<CardContent>
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
										<Input type="password" placeholder="••••••" {...field} />
									</FormControl>
									<FormMessage />
								</FormItem>
							)}
						/>

						<FormField
							control={form.control}
							name="rememberMe"
							render={({ field }) => (
								<FormItem className="flex flex-row items-start space-x-3 space-y-0">
									<FormControl>
										<Checkbox
											checked={field.value}
											onCheckedChange={field.onChange}
										/>
									</FormControl>
									<FormLabel className="font-normal">Remember me</FormLabel>
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
			</CardContent>
		</Card>
	);
}
```

</details>

### Drill 2: Contact Form with File Upload (20 minutes)

**Your Task:** Create a contact form that accepts file uploads with validation.

**Focus:** Working with file inputs and custom validation.

**Requirements:**

1. Create `src/components/forms-demo/FileUploadForm.tsx`
2. Use React Hook Form with file validation
3. Fields:
   - Name (required)
   - File (required, max 5MB, accept images only)
   - Description (optional, textarea)
4. Show file size and name after selection
5. Validate file size and type

**Hints:**

```tsx
const fileUploadSchema = z.object({
	name: z.string().min(1, "Name is required"),
	file: z
		.instanceof(FileList)
		.refine((files) => files.length > 0, "File is required")
		.refine(
			(files) => files[0]?.size <= 5000000,
			"File size must be less than 5MB"
		)
		.refine(
			(files) =>
				["image/jpeg", "image/png", "image/gif"].includes(files[0]?.type),
			"Only JPEG, PNG, and GIF files are allowed"
		),
	description: z.string().optional(),
});
```

**When to use:** Profile picture uploads, document submissions, file attachments.

**Solution:**

**⚠️ Important:** Only look at this solution after you've thoroughly attempted the exercise yourself. The learning happens in the struggle, not in copying code!

<details>
<summary>Click to reveal solution</summary>

```tsx
// src/components/forms-demo/FileUploadForm.tsx
import { useState } from "react";
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import * as z from "zod";
import {
	Card,
	CardContent,
	CardDescription,
	CardHeader,
	CardTitle,
} from "@/components/ui/card";
import {
	Form,
	FormControl,
	FormDescription,
	FormField,
	FormItem,
	FormLabel,
	FormMessage,
} from "@/components/ui/form";
import { Input } from "@/components/ui/input";
import { Textarea } from "@/components/ui/textarea";
import { Button } from "@/components/ui/button";

const MAX_FILE_SIZE = 5000000; // 5MB
const ACCEPTED_IMAGE_TYPES = ["image/jpeg", "image/png", "image/gif"];

const fileUploadSchema = z.object({
	name: z.string().min(1, "Name is required"),
	file: z
		.instanceof(FileList)
		.refine((files) => files.length > 0, "File is required")
		.refine(
			(files) => files[0]?.size <= MAX_FILE_SIZE,
			"File size must be less than 5MB"
		)
		.refine(
			(files) => ACCEPTED_IMAGE_TYPES.includes(files[0]?.type),
			"Only JPEG, PNG, and GIF files are allowed"
		),
	description: z.string().optional(),
});

type FileUploadFormValues = z.infer<typeof fileUploadSchema>;

export function FileUploadForm() {
	const [selectedFile, setSelectedFile] = useState<File | null>(null);

	const form = useForm<FileUploadFormValues>({
		resolver: zodResolver(fileUploadSchema),
		defaultValues: {
			name: "",
			description: "",
		},
	});

	const onSubmit = async (data: FileUploadFormValues) => {
		// Simulate file upload
		await new Promise((resolve) => setTimeout(resolve, 1500));

		const file = data.file[0];
		console.log("Upload data:", {
			name: data.name,
			fileName: file.name,
			fileSize: file.size,
			fileType: file.type,
			description: data.description,
		});

		alert("File uploaded successfully! Check console for details.");
		form.reset();
		setSelectedFile(null);
	};

	return (
		<Card className="max-w-md mx-auto">
			<CardHeader>
				<CardTitle>File Upload</CardTitle>
				<CardDescription>Upload an image file (max 5MB)</CardDescription>
			</CardHeader>
			<CardContent>
				<Form {...form}>
					<form onSubmit={form.handleSubmit(onSubmit)} className="space-y-4">
						<FormField
							control={form.control}
							name="name"
							render={({ field }) => (
								<FormItem>
									<FormLabel>Name</FormLabel>
									<FormControl>
										<Input placeholder="My image" {...field} />
									</FormControl>
									<FormMessage />
								</FormItem>
							)}
						/>

						<FormField
							control={form.control}
							name="file"
							render={({ field: { onChange, value, ...field } }) => (
								<FormItem>
									<FormLabel>File</FormLabel>
									<FormControl>
										<Input
											type="file"
											accept="image/jpeg,image/png,image/gif"
											onChange={(e) => {
												onChange(e.target.files);
												setSelectedFile(e.target.files?.[0] || null);
											}}
											{...field}
										/>
									</FormControl>
									<FormDescription>JPEG, PNG, or GIF (max 5MB)</FormDescription>
									<FormMessage />
								</FormItem>
							)}
						/>

						{selectedFile && (
							<div className="p-3 bg-muted rounded-md text-sm">
								<p className="font-medium">{selectedFile.name}</p>
								<p className="text-muted-foreground">
									{(selectedFile.size / 1024 / 1024).toFixed(2)} MB
								</p>
							</div>
						)}

						<FormField
							control={form.control}
							name="description"
							render={({ field }) => (
								<FormItem>
									<FormLabel>Description (Optional)</FormLabel>
									<FormControl>
										<Textarea
											placeholder="Describe your image..."
											rows={3}
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
							{form.formState.isSubmitting ? "Uploading..." : "Upload File"}
						</Button>
					</form>
				</Form>
			</CardContent>
		</Card>
	);
}
```

</details>

### Drill 3: Multi-Step Booking Form (20 minutes)

**Your Task:** Create a multi-step form with progress tracking.

**Focus:** Managing form state across multiple steps.

**Requirements:**

1. Create `src/components/forms-demo/BookingForm.tsx`
2. Use React Hook Form with 3 steps:
   - Step 1: Personal info (name, email, phone)
   - Step 2: Booking details (date, time, guests)
   - Step 3: Review and confirm
3. Show progress indicator
4. Validate each step before proceeding
5. Allow going back to edit previous steps

**Hints:**

- Use a `step` state to track current step
- Use `form.trigger()` to validate specific fields
- Show different form fields based on current step

**When to use:** Checkout flows, onboarding wizards, complex data entry.

**Solution:**

**⚠️ Important:** Only look at this solution after you've thoroughly attempted the exercise yourself. The learning happens in the struggle, not in copying code!

<details>
<summary>Click to reveal solution</summary>

```tsx
// src/components/forms-demo/BookingForm.tsx
import { useState } from "react";
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import * as z from "zod";
import {
	Card,
	CardContent,
	CardDescription,
	CardHeader,
	CardTitle,
} from "@/components/ui/card";
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
import { Progress } from "@/components/ui/progress";

const bookingSchema = z.object({
	// Step 1: Personal Info
	name: z.string().min(2, "Name must be at least 2 characters"),
	email: z.string().email("Invalid email address"),
	phone: z.string().regex(/^\+?[\d\s-]+$/, "Invalid phone number"),

	// Step 2: Booking Details
	date: z.string().min(1, "Date is required"),
	time: z.string().min(1, "Time is required"),
	guests: z.string().refine((val) => {
		const num = parseInt(val);
		return num >= 1 && num <= 10;
	}, "Guests must be between 1 and 10"),
});

type BookingFormValues = z.infer<typeof bookingSchema>;

export function BookingForm() {
	const [step, setStep] = useState(1);
	const totalSteps = 3;

	const form = useForm<BookingFormValues>({
		resolver: zodResolver(bookingSchema),
		defaultValues: {
			name: "",
			email: "",
			phone: "",
			date: "",
			time: "",
			guests: "2",
		},
		mode: "onChange",
	});

	const nextStep = async () => {
		let fieldsToValidate: (keyof BookingFormValues)[] = [];

		if (step === 1) {
			fieldsToValidate = ["name", "email", "phone"];
		} else if (step === 2) {
			fieldsToValidate = ["date", "time", "guests"];
		}

		const isValid = await form.trigger(fieldsToValidate);

		if (isValid && step < totalSteps) {
			setStep(step + 1);
		}
	};

	const prevStep = () => {
		if (step > 1) {
			setStep(step - 1);
		}
	};

	const onSubmit = async (data: BookingFormValues) => {
		// Simulate booking submission
		await new Promise((resolve) => setTimeout(resolve, 1000));

		console.log("Booking confirmed:", data);
		alert("Booking confirmed! Check console for details.");
		form.reset();
		setStep(1);
	};

	const progress = (step / totalSteps) * 100;

	return (
		<Card className="max-w-2xl mx-auto">
			<CardHeader>
				<CardTitle>Table Booking</CardTitle>
				<CardDescription>
					Step {step} of {totalSteps}
				</CardDescription>
				<Progress value={progress} className="mt-2" />
			</CardHeader>
			<CardContent>
				<Form {...form}>
					<form onSubmit={form.handleSubmit(onSubmit)} className="space-y-6">
						{/* Step 1: Personal Info */}
						{step === 1 && (
							<div className="space-y-4">
								<h3 className="text-lg font-semibold">Personal Information</h3>

								<FormField
									control={form.control}
									name="name"
									render={({ field }) => (
										<FormItem>
											<FormLabel>Full Name</FormLabel>
											<FormControl>
												<Input placeholder="John Doe" {...field} />
											</FormControl>
											<FormMessage />
										</FormItem>
									)}
								/>

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
									name="phone"
									render={({ field }) => (
										<FormItem>
											<FormLabel>Phone Number</FormLabel>
											<FormControl>
												<Input placeholder="+1 234 567 8900" {...field} />
											</FormControl>
											<FormMessage />
										</FormItem>
									)}
								/>
							</div>
						)}

						{/* Step 2: Booking Details */}
						{step === 2 && (
							<div className="space-y-4">
								<h3 className="text-lg font-semibold">Booking Details</h3>

								<FormField
									control={form.control}
									name="date"
									render={({ field }) => (
										<FormItem>
											<FormLabel>Date</FormLabel>
											<FormControl>
												<Input type="date" {...field} />
											</FormControl>
											<FormMessage />
										</FormItem>
									)}
								/>

								<FormField
									control={form.control}
									name="time"
									render={({ field }) => (
										<FormItem>
											<FormLabel>Time</FormLabel>
											<FormControl>
												<Input type="time" {...field} />
											</FormControl>
											<FormMessage />
										</FormItem>
									)}
								/>

								<FormField
									control={form.control}
									name="guests"
									render={({ field }) => (
										<FormItem>
											<FormLabel>Number of Guests</FormLabel>
											<FormControl>
												<Input type="number" min="1" max="10" {...field} />
											</FormControl>
											<FormMessage />
										</FormItem>
									)}
								/>
							</div>
						)}

						{/* Step 3: Review */}
						{step === 3 && (
							<div className="space-y-4">
								<h3 className="text-lg font-semibold">Review Your Booking</h3>

								<div className="bg-muted p-4 rounded-lg space-y-2">
									<div className="grid grid-cols-2 gap-2">
										<p className="font-medium">Name:</p>
										<p>{form.getValues("name")}</p>

										<p className="font-medium">Email:</p>
										<p>{form.getValues("email")}</p>

										<p className="font-medium">Phone:</p>
										<p>{form.getValues("phone")}</p>

										<p className="font-medium">Date:</p>
										<p>{form.getValues("date")}</p>

										<p className="font-medium">Time:</p>
										<p>{form.getValues("time")}</p>

										<p className="font-medium">Guests:</p>
										<p>{form.getValues("guests")}</p>
									</div>
								</div>
							</div>
						)}

						{/* Navigation Buttons */}
						<div className="flex justify-between pt-4">
							{step > 1 && (
								<Button type="button" variant="outline" onClick={prevStep}>
									Back
								</Button>
							)}

							{step < totalSteps ? (
								<Button type="button" onClick={nextStep} className="ml-auto">
									Next
								</Button>
							) : (
								<Button
									type="submit"
									className="ml-auto"
									disabled={form.formState.isSubmitting}
								>
									{form.formState.isSubmitting
										? "Confirming..."
										: "Confirm Booking"}
								</Button>
							)}
						</div>
					</form>
				</Form>
			</CardContent>
		</Card>
	);
}
```

</details>

---

## 🎯 Checkpoint Questions

1. **What three libraries make up the professional form handling stack?** (React Hook Form, Zod, and shadcn/ui)

2. **What is the main benefit of using Zod schemas?** (Declarative validation with automatic TypeScript type inference)

3. **How does `z.infer<typeof schema>` help you?** (Automatically creates TypeScript types from your Zod schema, keeping validation and types in sync)

4. **Why use shadcn/ui Form components instead of plain HTML inputs?** (Built-in accessibility, automatic error display, consistent styling, and proper ARIA labels)

5. **What does the `resolver` prop do in React Hook Form?** (Connects external validation libraries like Zod to React Hook Form)

6. **What is `.refine()` used for in Zod schemas?** (Custom validation logic, especially cross-field validation like password matching)

### Quick Self-Test:

What's wrong here?

```tsx
const form = useForm({
	defaultValues: {
		email: "",
	},
});

// Later...
const email = form.getValues("emial"); // Typo!
```

**Answer:** No TypeScript type safety! Without a typed schema and `useForm<FormValues>()`, typos in field names won't be caught at compile time. Always use Zod schema with `z.infer<typeof schema>` for type safety.

---

## 🤖 AI Learning Tips

**Good questions:**

- ✅ "Why is my form not validating on submit?"
- ✅ "How do I validate one field based on another's value?"
- ✅ "What's the best way to handle file uploads in React Hook Form?"

**Avoid:**

- ❌ "Build my entire form for me"
- ❌ "Write all validation rules for me"

**30-60-90 Rule:** Try for 30s, think for 60s, then ask targeted questions.

## 📚 Resources

- [React Hook Form Documentation](https://react-hook-form.com/)
- [React Hook Form - Get Started](https://react-hook-form.com/get-started)
- [Zod Schema Validation](https://zod.dev/)
- [Zod - Basic Usage](https://zod.dev/?id=basic-usage)
- [shadcn/ui Form Components](https://ui.shadcn.com/docs/components/form)
- [Form Accessibility (WCAG)](https://www.w3.org/WAI/tutorials/forms/)

## 💡 Common Patterns You'll Use

### Pattern 1: Optional Fields

```tsx
const schema = z.object({
	email: z.string().email(),
	phone: z.string().optional(), // Can be undefined
	bio: z.string().optional(),
});
```

### Pattern 2: Conditional Validation

```tsx
const schema = z
	.object({
		hasAddress: z.boolean(),
		address: z.string(),
	})
	.refine((data) => !data.hasAddress || data.address.length > 0, {
		message: "Address is required when 'Has Address' is checked",
		path: ["address"],
	});
```

### Pattern 3: Custom Error Messages

```tsx
const schema = z.object({
	password: z
		.string()
		.min(8, "Password must be at least 8 characters") // Custom message
		.regex(/[A-Z]/, "Must contain an uppercase letter") // Custom message
		.regex(/[0-9]/, "Must contain a number"), // Custom message
});
```

### Pattern 4: Watching Form Values

```tsx
function MyForm() {
	const form = useForm();

	// Watch a specific field
	const emailValue = form.watch("email");

	// Watch all fields
	const allValues = form.watch();

	return <form>...</form>;
}
```
