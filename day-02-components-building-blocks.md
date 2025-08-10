# Day 2: Components as Building Blocks (Single Responsibility Principle)

Yesterday you built the foundation. Today, we're learning about components - the LEGO blocks of React. More importantly, we'll learn why each block should do ONE thing really well.

## Learning Objectives

By the end of today, you'll:
- Understand components as reusable building blocks
- Master the Single Responsibility Principle (SRP)
- Create focused, reusable components
- Build your first component library

## The LEGO Analogy

Think of React components like LEGO blocks:
- A wheel block only knows how to be a wheel
- A window block only knows how to be a window
- You combine them to build a car, house, or spaceship

This is the Single Responsibility Principle: **Each component should do one thing, and do it well.**

## Part 1: What Makes a Good Component?

### Bad Example - Too Many Responsibilities

```tsx
// ❌ This component is trying to do EVERYTHING
function UserProfile() {
  const [user, setUser] = useState(null);
  const [posts, setPosts] = useState([]);
  const [isEditing, setIsEditing] = useState(false);
  
  // Fetching user data - Responsibility #1
  useEffect(() => {
    fetch('/api/user').then(/* ... */);
  }, []);
  
  // Fetching posts - Responsibility #2
  useEffect(() => {
    fetch('/api/posts').then(/* ... */);
  }, []);
  
  // Handling form submission - Responsibility #3
  const handleSubmit = () => {/* ... */};
  
  // Rendering EVERYTHING - Responsibility #4, #5, #6...
  return (
    <div>
      <h1>{user?.name}</h1>
      <button onClick={() => setIsEditing(true)}>Edit</button>
      {isEditing && <form>...</form>}
      <div className="posts">
        {posts.map(post => <div key={post.id}>{post.title}</div>)}
      </div>
    </div>
  );
}
```

### Good Example - Single Responsibility

Let's break it down into focused components:

Create `src/shared/ui/Avatar.tsx`:

```tsx
interface AvatarProps {
  src?: string;
  alt: string;
  size?: 'sm' | 'md' | 'lg';
}

export function Avatar({ src, alt, size = 'md' }: AvatarProps) {
  const sizeClasses = {
    sm: 'w-8 h-8',
    md: 'w-12 h-12',
    lg: 'w-16 h-16'
  };
  
  return (
    <img
      src={src || '/default-avatar.png'}
      alt={alt}
      className={`${sizeClasses[size]} rounded-full object-cover`}
    />
  );
}
```

Create `src/shared/ui/Card.tsx`:

```tsx
interface CardProps {
  children: React.ReactNode;
  className?: string;
}

export function Card({ children, className = '' }: CardProps) {
  return (
    <div className={`bg-white rounded-lg shadow-md p-6 ${className}`}>
      {children}
    </div>
  );
}
```

Create `src/shared/ui/Button.tsx`:

```tsx
interface ButtonProps {
  children: React.ReactNode;
  onClick?: () => void;
  variant?: 'primary' | 'secondary' | 'ghost';
  size?: 'sm' | 'md' | 'lg';
  disabled?: boolean;
}

export function Button({ 
  children, 
  onClick, 
  variant = 'primary',
  size = 'md',
  disabled = false 
}: ButtonProps) {
  const baseClasses = 'font-medium rounded-lg transition-colors focus:outline-none focus:ring-2';
  
  const variantClasses = {
    primary: 'bg-blue-600 text-white hover:bg-blue-700 focus:ring-blue-500',
    secondary: 'bg-gray-200 text-gray-900 hover:bg-gray-300 focus:ring-gray-500',
    ghost: 'bg-transparent text-gray-700 hover:bg-gray-100 focus:ring-gray-500'
  };
  
  const sizeClasses = {
    sm: 'px-3 py-1.5 text-sm',
    md: 'px-4 py-2 text-base',
    lg: 'px-6 py-3 text-lg'
  };
  
  return (
    <button
      onClick={onClick}
      disabled={disabled}
      className={`
        ${baseClasses}
        ${variantClasses[variant]}
        ${sizeClasses[size]}
        ${disabled ? 'opacity-50 cursor-not-allowed' : ''}
      `}
    >
      {children}
    </button>
  );
}
```

## Part 2: Composing Components

Now let's combine these single-purpose components. Create `src/entities/user/ui/UserCard.tsx`:

```tsx
import { Avatar } from '../../../shared/ui/Avatar';
import { Card } from '../../../shared/ui/Card';
import { Button } from '../../../shared/ui/Button';

interface User {
  id: string;
  name: string;
  email: string;
  avatar?: string;
}

interface UserCardProps {
  user: User;
  onEdit?: () => void;
}

export function UserCard({ user, onEdit }: UserCardProps) {
  return (
    <Card>
      <div className="flex items-center space-x-4">
        <Avatar src={user.avatar} alt={user.name} size="lg" />
        <div className="flex-1">
          <h3 className="text-lg font-semibold text-gray-900">{user.name}</h3>
          <p className="text-gray-600">{user.email}</p>
        </div>
        {onEdit && (
          <Button onClick={onEdit} variant="secondary" size="sm">
            Edit
          </Button>
        )}
      </div>
    </Card>
  );
}
```

## Part 3: Building a Component System

Let's create more building blocks. Create `src/shared/ui/Text.tsx`:

```tsx
interface TextProps {
  children: React.ReactNode;
  as?: 'h1' | 'h2' | 'h3' | 'h4' | 'p' | 'span';
  size?: 'xs' | 'sm' | 'base' | 'lg' | 'xl' | '2xl';
  weight?: 'normal' | 'medium' | 'semibold' | 'bold';
  color?: 'primary' | 'secondary' | 'success' | 'danger' | 'muted';
  className?: string;
}

export function Text({
  children,
  as: Component = 'p',
  size = 'base',
  weight = 'normal',
  color = 'primary',
  className = ''
}: TextProps) {
  const sizeClasses = {
    xs: 'text-xs',
    sm: 'text-sm',
    base: 'text-base',
    lg: 'text-lg',
    xl: 'text-xl',
    '2xl': 'text-2xl'
  };
  
  const weightClasses = {
    normal: 'font-normal',
    medium: 'font-medium',
    semibold: 'font-semibold',
    bold: 'font-bold'
  };
  
  const colorClasses = {
    primary: 'text-gray-900',
    secondary: 'text-gray-700',
    success: 'text-green-600',
    danger: 'text-red-600',
    muted: 'text-gray-500'
  };
  
  return (
    <Component 
      className={`
        ${sizeClasses[size]}
        ${weightClasses[weight]}
        ${colorClasses[color]}
        ${className}
      `}
    >
      {children}
    </Component>
  );
}
```

## Part 4: Practice Through Repetition

Now let's practice SRP by building variations of the same concept.

### Exercise 1: Easy - Icon Button
Create an IconButton component that:
- Only handles displaying an icon with a clickable area
- Reuses the styling logic from Button
- Has a single responsibility: "Be a clickable icon"

### Exercise 2: Medium - Input Component
Create an Input component in `src/shared/ui/Input.tsx` that:
- Only handles text input display and basic events
- Doesn't validate (that's another component's job)
- Doesn't fetch data (that's another component's job)
- Just displays an input field beautifully

Example structure:
```tsx
interface InputProps {
  value: string;
  onChange: (value: string) => void;
  placeholder?: string;
  type?: 'text' | 'email' | 'password';
  disabled?: boolean;
}
```

### Exercise 3: Hard - Compose a Form
Using ONLY the components you've built:
1. Create a `src/features/user-edit/ui/UserEditForm.tsx`
2. It should use Input, Button, and Text components
3. The form itself should only handle:
   - Collecting values
   - Calling onSubmit
   - NOT validation, NOT API calls, JUST form state

## Repetition Drills

Let's build the same concept three different ways to master SRP:

### Drill 1: Status Badge
```tsx
// Version 1: Simple
<Badge status="active">Active</Badge>

// Version 2: With icon
<Badge status="active" icon={<CheckIcon />}>Active</Badge>

// Version 3: With custom colors
<Badge color="green" bgColor="green-100">Custom</Badge>
```

Build all three versions, keeping each focused on displaying a badge.

### Drill 2: List Item
Build three versions of a list item:
1. Simple text list item
2. List item with icon
3. List item with action button

Each should be a separate component, NOT one component with many options.

## Common Mistakes to Avoid

1. **The Swiss Army Knife Component**
   ```tsx
   // ❌ Bad: Too many responsibilities
   <SuperComponent 
     showAvatar
     showBadge
     showButton
     handleEdit
     handleDelete
     fetchData
   />
   ```

2. **The Know-It-All Component**
   ```tsx
   // ❌ Bad: Component knows too much about business logic
   function Button() {
     const user = useUser(); // Why does a button need user data?
     const api = useApi();   // Why does a button need API access?
   }
   ```

3. **The Shape-Shifter Component**
   ```tsx
   // ❌ Bad: Too many variants = too many responsibilities
   <Card 
     variant="user-profile-with-posts-and-comments-and-likes"
   />
   ```

## Reflection Questions

1. Look at your Button component. What is its single responsibility?
2. If you needed to add API calls to Button, why would that be wrong?
3. How does breaking components down make them more reusable?

## Quick Reference

```
Single Responsibility Principle (SRP):
- One component = One job
- Like LEGO blocks - specific and combinable
- If you use "and" to describe it, split it

Good Signs:
✅ Component name is clear and specific
✅ Props are simple and focused
✅ Easy to test in isolation
✅ Reusable in many places

Bad Signs:
❌ Component has 10+ props
❌ Contains business logic AND UI
❌ Hard to name (UserProfileAndPostsAndComments)
❌ Lots of conditional rendering
```

## Challenge Project

Create a "Recipe Card" component system:
1. `RecipeCard` - The container
2. `RecipeImage` - Just the image
3. `RecipeTitle` - Just the title
4. `RecipeTime` - Cook time display
5. `RecipeDifficulty` - Difficulty badge
6. `RecipeIngredientCount` - Ingredient counter

Each component should do ONE thing. Combine them to create beautiful recipe cards!

Tomorrow, we'll dive deep into Props and TypeScript interfaces - learning how components talk to each other while staying independent!