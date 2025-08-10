# Day 3: Props, Types & Interfaces (Interface Segregation)

Components need to talk to each other, but they should only share what's necessary. Today, we learn about props, TypeScript interfaces, and the art of keeping contracts small and focused.

## Learning Objectives

By the end of today, you'll:
- Master props as component communication
- Understand TypeScript interfaces deeply
- Apply Interface Segregation Principle
- Build type-safe, flexible components

## The Contract Analogy

Think of props like a contract between components:
- A pizza delivery contract doesn't include your life story
- It only needs: address, pizza type, payment method
- Same with components - only pass what's needed!

## Part 1: Props Fundamentals

### The Problem with Loose Props

```tsx
// ❌ Without TypeScript - Anything goes!
function Greeting(props) {
  return <h1>Hello, {props.nmae}!</h1>; // Typo! But no error...
}

// This runs but shows "Hello, undefined!"
<Greeting name="Alice" />
```

### The TypeScript Solution

```tsx
// ✅ With TypeScript - Catches errors immediately!
interface GreetingProps {
  name: string;
}

function Greeting({ name }: GreetingProps) {
  return <h1>Hello, {name}!</h1>;
}

// TypeScript error: Property 'nmae' does not exist
```

## Part 2: Interface Segregation Principle

The Interface Segregation Principle says: **Don't force components to depend on props they don't use.**

### Bad Example - Fat Interface

```tsx
// ❌ User object has 20 properties, but we only need 2
interface User {
  id: string;
  name: string;
  email: string;
  password: string;
  address: string;
  phone: string;
  creditCard: string;
  socialSecurity: string;
  // ... 12 more fields
}

interface UserAvatarProps {
  user: User; // Component receives EVERYTHING
}

function UserAvatar({ user }: UserAvatarProps) {
  // But only uses name and email!
  return (
    <div>
      <img src={`/avatar/${user.email}`} alt={user.name} />
      <span>{user.name}</span>
    </div>
  );
}
```

### Good Example - Lean Interface

```tsx
// ✅ Only ask for what you need
interface UserAvatarProps {
  name: string;
  email: string;
}

function UserAvatar({ name, email }: UserAvatarProps) {
  return (
    <div>
      <img src={`/avatar/${email}`} alt={name} />
      <span>{name}</span>
    </div>
  );
}

// Now it can be used with ANY object that has name and email
const user = { id: '1', name: 'Alice', email: 'alice@example.com', age: 25 };
const author = { name: 'Bob', email: 'bob@example.com', books: [] };

<UserAvatar name={user.name} email={user.email} />
<UserAvatar name={author.name} email={author.email} />
```

## Part 3: Building Better Interfaces

Let's create a task display system with proper interfaces:

Create `src/entities/task/model/types.ts`:

```tsx
// Core task interface - the full data model
export interface Task {
  id: string;
  title: string;
  description?: string;
  completed: boolean;
  priority: 'low' | 'medium' | 'high';
  createdAt: Date;
  updatedAt: Date;
  assignee?: string;
  tags?: string[];
}

// Segregated interfaces for different use cases
export interface TaskDisplay {
  id: string;
  title: string;
  completed: boolean;
}

export interface TaskWithPriority extends TaskDisplay {
  priority: 'low' | 'medium' | 'high';
}

export interface TaskActions {
  onToggle: (id: string) => void;
  onDelete: (id: string) => void;
}
```

Now let's build components using these interfaces:

Create `src/shared/ui/Checkbox.tsx`:

```tsx
interface CheckboxProps {
  checked: boolean;
  onChange: (checked: boolean) => void;
  label?: string;
  disabled?: boolean;
}

export function Checkbox({ checked, onChange, label, disabled = false }: CheckboxProps) {
  return (
    <label className="flex items-center space-x-2 cursor-pointer">
      <input
        type="checkbox"
        checked={checked}
        onChange={(e) => onChange(e.target.checked)}
        disabled={disabled}
        className="w-4 h-4 text-blue-600 rounded focus:ring-blue-500"
      />
      {label && <span className="text-gray-700">{label}</span>}
    </label>
  );
}
```

Create `src/entities/task/ui/TaskItem.tsx`:

```tsx
import { Checkbox } from '../../../shared/ui/Checkbox';
import { TaskDisplay } from '../model/types';

interface TaskItemProps {
  task: TaskDisplay; // Only needs display data
  onToggle: (id: string) => void;
}

export function TaskItem({ task, onToggle }: TaskItemProps) {
  return (
    <div className="flex items-center p-3 bg-white rounded-lg shadow-sm">
      <Checkbox
        checked={task.completed}
        onChange={() => onToggle(task.id)}
      />
      <span className={`ml-3 ${task.completed ? 'line-through text-gray-500' : ''}`}>
        {task.title}
      </span>
    </div>
  );
}
```

Create `src/shared/ui/Badge.tsx`:

```tsx
interface BadgeProps {
  children: React.ReactNode;
  variant?: 'default' | 'success' | 'warning' | 'danger';
  size?: 'sm' | 'md';
}

export function Badge({ children, variant = 'default', size = 'sm' }: BadgeProps) {
  const variantClasses = {
    default: 'bg-gray-100 text-gray-800',
    success: 'bg-green-100 text-green-800',
    warning: 'bg-yellow-100 text-yellow-800',
    danger: 'bg-red-100 text-red-800'
  };
  
  const sizeClasses = {
    sm: 'px-2 py-0.5 text-xs',
    md: 'px-3 py-1 text-sm'
  };
  
  return (
    <span className={`
      inline-flex items-center font-medium rounded-full
      ${variantClasses[variant]}
      ${sizeClasses[size]}
    `}>
      {children}
    </span>
  );
}
```

Create `src/entities/task/ui/TaskItemWithPriority.tsx`:

```tsx
import { TaskItem } from './TaskItem';
import { Badge } from '../../../shared/ui/Badge';
import { TaskWithPriority } from '../model/types';

interface TaskItemWithPriorityProps {
  task: TaskWithPriority; // Extends TaskDisplay with priority
  onToggle: (id: string) => void;
}

const priorityConfig = {
  low: { variant: 'default' as const, label: 'Low' },
  medium: { variant: 'warning' as const, label: 'Medium' },
  high: { variant: 'danger' as const, label: 'High' }
};

export function TaskItemWithPriority({ task, onToggle }: TaskItemWithPriorityProps) {
  const config = priorityConfig[task.priority];
  
  return (
    <div className="flex items-center justify-between">
      <TaskItem task={task} onToggle={onToggle} />
      <Badge variant={config.variant} size="sm">
        {config.label}
      </Badge>
    </div>
  );
}
```

## Part 4: Advanced Interface Patterns

### Optional vs Required Props

```tsx
interface ButtonProps {
  children: React.ReactNode;    // Required
  onClick?: () => void;         // Optional
  disabled?: boolean;           // Optional with default
  type?: 'button' | 'submit';   // Optional with specific values
}
```

### Extending Interfaces

```tsx
interface BaseInputProps {
  value: string;
  onChange: (value: string) => void;
}

interface TextInputProps extends BaseInputProps {
  placeholder?: string;
  maxLength?: number;
}

interface NumberInputProps extends BaseInputProps {
  min?: number;
  max?: number;
  step?: number;
}
```

### Union Types for Flexibility

```tsx
interface SuccessAlert {
  type: 'success';
  message: string;
}

interface ErrorAlert {
  type: 'error';
  message: string;
  code: number;
}

type Alert = SuccessAlert | ErrorAlert;

function AlertDisplay({ alert }: { alert: Alert }) {
  if (alert.type === 'error') {
    // TypeScript knows alert.code exists here!
    return <div>Error {alert.code}: {alert.message}</div>;
  }
  return <div>Success: {alert.message}</div>;
}
```

## Practice Exercises

### Exercise 1: Easy - Specific Props
Take this fat interface and break it down:

```tsx
// Start with this:
interface ProductCardProps {
  product: {
    id: string;
    name: string;
    price: number;
    description: string;
    category: string;
    stock: number;
    images: string[];
    reviews: Review[];
  };
}

// Refactor to:
// 1. ProductSummaryProps (just id, name, price)
// 2. ProductImageProps (just images)
// 3. ProductStockProps (just stock)
```

### Exercise 2: Medium - Composable Interfaces
Create a flexible form field system:

1. Base `FieldProps` interface
2. `TextFieldProps` extending base
3. `SelectFieldProps` extending base
4. `CheckboxFieldProps` extending base

Each should only include relevant props!

### Exercise 3: Hard - Type-Safe Event System
Create an event handling system where:
1. Different components emit different event types
2. Each event has its own data structure
3. TypeScript ensures you handle the right data

```tsx
// Hint: Start with:
type TaskEvent = 
  | { type: 'toggle'; id: string }
  | { type: 'delete'; id: string }
  | { type: 'edit'; id: string; newTitle: string };
```

## Repetition Drills

### Drill 1: Three Ways to Pass Data
Create the same "User Display" three different ways:

```tsx
// Version 1: Full object
<UserDisplay1 user={fullUserObject} />

// Version 2: Specific props
<UserDisplay2 name={user.name} role={user.role} />

// Version 3: Render props pattern
<UserDisplay3 renderName={() => user.name} />
```

### Drill 2: Progressive Enhancement
Start with a simple interface and gradually add features:

1. `SimpleButton` - just text and onClick
2. `IconButton` - adds icon prop
3. `LoadingButton` - adds loading state
4. `AdvancedButton` - adds all features

Each builds on the previous, showing interface evolution.

## Common Mistakes to Avoid

1. **The Kitchen Sink Interface**
   ```tsx
   // ❌ Bad: Everything in one interface
   interface ComponentProps {
     // UI props
     className?: string;
     style?: CSSProperties;
     // Data props
     user?: User;
     posts?: Post[];
     // Event props
     onClick?: () => void;
     onChange?: () => void;
     onSubmit?: () => void;
     // Feature flags
     showAvatar?: boolean;
     showPosts?: boolean;
     enableEdit?: boolean;
   }
   ```

2. **The Any Escape Hatch**
   ```tsx
   // ❌ Bad: Giving up on types
   interface Props {
     data: any;
     config: any;
   }
   ```

3. **The Optional Everything**
   ```tsx
   // ❌ Bad: When everything is optional, nothing is clear
   interface Props {
     title?: string;
     onClick?: () => void;
     user?: User;
     // What does this component actually need?
   }
   ```

## Reflection Questions

1. Why is it better to pass `name` and `email` instead of the entire `user` object?
2. How does interface segregation make components more reusable?
3. When would you choose to extend an interface vs create a new one?

## Quick Reference

```typescript
// Interface Basics
interface Props {
  required: string;        // Must provide
  optional?: string;       // Can omit
  withDefault?: boolean;   // Can omit (default in component)
  union: 'a' | 'b' | 'c'; // Must be one of these
  callback: (x: number) => void;
}

// Extending
interface Extended extends Base {
  newProp: string;
}

// Combining
interface Combined extends A, B {
  ownProp: string;
}

// Utility Types
Partial<Props>    // All props optional
Required<Props>   // All props required
Pick<Props, 'a'>  // Only selected props
Omit<Props, 'a'>  // All except selected
```

## Mini Project: Task Card System

Create a complete task card system:

1. Define segregated interfaces for:
   - Task data (id, title, completed)
   - Task metadata (priority, tags)
   - Task actions (toggle, edit, delete)
   
2. Build components that use only what they need:
   - SimpleTaskCard (just data)
   - PriorityTaskCard (data + metadata)
   - InteractiveTaskCard (data + actions)

3. Show how the same task object can be displayed differently based on context

Tomorrow, we'll add state to our components and learn how to manage data changes while keeping our interfaces clean!