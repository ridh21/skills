---
name: heroui/react
description: "Use this skill when building user interfaces in a Vite React project that uses HeroUI v3. Triggers include any request to create, update, or refactor React components, pages, forms, modals, or tables in a Vite-based React codebase where the stack involves HeroUI, @heroui/react, or @heroui/styles. Also use when the user asks about component installation, Tailwind CSS v4 styling, theming, toast notifications, or accessibility with HeroUI in Vite React. Do NOT use this skill for Next.js projects — use heroui/next instead."
license: "For use with any agent skills system"
---

This skill provides strict, opinionated guidelines for building polished, production-grade UIs in **Vite React** projects that use HeroUI v3, Tailwind CSS v4, and React 19+.

> **Stack**: Vite + React + HeroUI v3 + Tailwind CSS v4 + pnpm
> For Next.js App Router projects, use the `heroui/next` skill instead.

---

## Package Manager

**ALWAYS use `pnpm`.** Never use `npm` or `yarn` unless explicitly requested by the user.

```bash
pnpm add @heroui/styles@beta @heroui/react@beta
pnpm add package-name
```

For monorepos or hoisted dependencies, add to `.npmrc`:

```
public-hoist-pattern[]=*@heroui/*
```

Then re-run `pnpm install` after modifying `.npmrc`.

---

## Setup

### 1. Scaffold a New Vite React Project

```bash
pnpm create vite my-app --template react-ts
cd my-app
pnpm install
```

### 2. Install Tailwind CSS v4

```bash
pnpm add tailwindcss @tailwindcss/vite
```

Add the Vite plugin to `vite.config.ts`:

```ts
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import tailwindcss from "@tailwindcss/vite";

export default defineConfig({
  plugins: [react(), tailwindcss()],
});
```

### 3. Install HeroUI v3

```bash
pnpm add @heroui/styles@beta @heroui/react@beta
```

### 4. Import Styles (`src/index.css`)

```css
@import "tailwindcss";
@import "@heroui/styles";
```

> ⚠️ Import order matters — always import `tailwindcss` before `@heroui/styles`.

Make sure `src/index.css` is imported in `src/main.tsx`:

```tsx
import "./index.css";
```

### 5. Provider Setup (`src/main.tsx`)

Wrap the app with `HeroUIProvider` and `ToastProvider` at the root:

```tsx
// src/main.tsx
import { StrictMode } from "react";
import { createRoot } from "react-dom/client";
import { HeroUIProvider } from "@heroui/react";
import { ToastProvider } from "@heroui/react";
import "./index.css";
import App from "./App";

createRoot(document.getElementById("root")!).render(
  <StrictMode>
    <HeroUIProvider>
      <ToastProvider />
      <App />
    </HeroUIProvider>
  </StrictMode>
);
```

### 6. Path Aliases (Recommended)

Add `@/` alias in `vite.config.ts` for clean imports:

```ts
import path from "path";
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import tailwindcss from "@tailwindcss/vite";

export default defineConfig({
  plugins: [react(), tailwindcss()],
  resolve: {
    alias: { "@": path.resolve(__dirname, "./src") },
  },
});
```

And in `tsconfig.json`:

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": { "@/*": ["./src/*"] }
  }
}
```

---

## Fonts

Unlike Next.js, Vite has no built-in font optimization. Use **Fontsource** for self-hosted fonts — never link to Google Fonts CDN directly.

```bash
pnpm add @fontsource-variable/dm-sans
```

Import in `src/index.css`:

```css
@import "@fontsource-variable/dm-sans";
@import "tailwindcss";
@import "@heroui/styles";
```

Then reference in your CSS or Tailwind config:

```css
/* src/index.css — after the imports above */
body {
  font-family: "DM Sans Variable", system-ui, sans-serif;
}
```

Recommended fonts (all available on Fontsource): **DM Sans**, **Plus Jakarta Sans**, **Outfit**, **Geist**. Avoid Inter, Roboto, Arial.

Typography scale:

```tsx
<h1 className="text-4xl font-bold tracking-tight">Heading</h1>
<h2 className="text-2xl font-semibold">Section</h2>
<p className="text-base text-default-500">Body text</p>
<span className="text-xs font-medium uppercase tracking-wide">Label</span>
```

Use `text-default-500`, `text-default-700` etc. — stay within HeroUI's semantic color system, not raw Tailwind grays.

---

## Component Rules

Use **only HeroUI components** from `@heroui/react`. Never import from raw Radix UI primitives, shadcn/ui, MUI, or other UI libraries unless absolutely no HeroUI equivalent exists.

```tsx
// ✅ CORRECT
import { Button } from "@heroui/react";
import { Card, CardBody, CardHeader } from "@heroui/react";

// ❌ WRONG
import * as Dialog from "@radix-ui/react-dialog";
import { Button } from "@mui/material";
import { Button } from "@/components/ui/button"; // shadcn
```

There are **no Server Components or `"use client"` directives** in Vite React — all components are client-side by default. Do not add `"use client"` to files; it has no meaning outside Next.js.

When building a custom component that HeroUI doesn't offer, follow HeroUI's design philosophy: use its color tokens, spacing scale, and `cn()` utility — never introduce a competing design language.

---

## Available HeroUI v3 Components

> **Authoritative list** from https://v3.heroui.com/docs/react/components. Do NOT invent component names outside this list.

### Buttons
`Button` · `ButtonGroup` · `CloseButton` · `ToggleButton` · `ToggleButtonGroup`

### Collections
`Dropdown` · `Listbox` · `TagGroup`

### Colors
`ColorArea` · `ColorField` · `ColorPicker` · `ColorSlider` · `ColorSwatch` · `ColorSwatchPicker`

### Controls
`Slider` · `Switch`

### Data Display
`Badge` · `Chip` · `Table`

### Date and Time
`Calendar` · `DateField` · `DatePicker` · `DateRangePicker` · `RangeCalendar` · `TimeField`

### Feedback
`Alert` · `Meter` · `ProgressBar` · `ProgressCircle` · `Skeleton` · `Spinner`

### Forms
`Checkbox` · `CheckboxGroup` · `Description` · `ErrorMessage` · `FieldError` · `Fieldset` · `Form` · `Input` · `InputGroup` · `InputOTP` · `Label` · `NumberField` · `RadioGroup` · `SearchField` · `TextField` · `TextArea`

### Layout
`Card` / `CardBody` / `CardHeader` / `CardFooter` · `Separator` · `Surface` · `Toolbar`

### Media
`Avatar`

### Navigation
`Accordion` · `Breadcrumbs` · `Disclosure` · `DisclosureGroup` · `Link` · `Pagination` · `Tabs`

### Overlays
`AlertDialog` · `Drawer` · `Modal` · `Popover` · `Toast` · `Tooltip`

### Pickers
`Autocomplete` · `ComboBox` · `Select`

### Typography
`Kbd`

### Utilities
`ScrollShadow`

---

## Styling

HeroUI v3 uses **Tailwind CSS v4** as its styling engine. Use Tailwind utility classes for layout and spacing. For component-level customization, use HeroUI's built-in `className`, `classNames`, and slot-based props.

```tsx
// ✅ Tailwind for layout
<div className="flex items-center gap-4 p-6">

// ✅ HeroUI slot-based classNames
<Button classNames={{ base: "rounded-full", label: "font-semibold" }}>
  Click me
</Button>

// ❌ No inline styles
<div style={{ display: "flex" }}>
```

### Color Variants

```tsx
<Button color="primary" />
<Button color="secondary" />
<Button color="success" />
<Button color="warning" />
<Button color="danger" />
<Button color="default" />
```

### Size & Radius

```tsx
<Button size="sm" />     // sm | md | lg
<Button radius="full" /> // none | sm | md | lg | full
```

### Dark Mode

In Vite React, dark mode is managed via state — toggle the `dark` class on `<html>` manually:

```tsx
// src/hooks/use-theme.ts
import { useEffect, useState } from "react";

export function useTheme() {
  const [isDark, setIsDark] = useState(
    () => window.matchMedia("(prefers-color-scheme: dark)").matches
  );

  useEffect(() => {
    document.documentElement.classList.toggle("dark", isDark);
  }, [isDark]);

  return { isDark, toggle: () => setIsDark((d) => !d) };
}
```

---

## Notifications (Toast)

Use HeroUI's native `Toast` — never `sonner`, `react-hot-toast`, or any third-party toast library.

```tsx
import { addToast } from "@heroui/react";

addToast({ title: "Saved!", description: "Changes saved.", color: "success" });
addToast({ title: "Error", description: "Something went wrong.", color: "danger" });
```

`<ToastProvider />` is already wired in `src/main.tsx` from the Setup section above.

---

## Routing & Loading States

Vite React uses **React Router v7** (or TanStack Router) for routing — there is no file-based routing or automatic `loading.tsx` like Next.js. Loading states must be handled explicitly.

### React Router v7 Setup

```bash
pnpm add react-router
```

```tsx
// src/main.tsx
import { BrowserRouter } from "react-router";
// wrap <App /> with <BrowserRouter>
```

### Route-Level Loading

Since there is no `app/loading.tsx`, render `<PageLoader>` explicitly when lazy-loading routes or fetching initial data:

```tsx
// Lazy route with Suspense
import { lazy, Suspense } from "react";
import { PageLoader } from "@/components/page-loader";

const Dashboard = lazy(() => import("@/pages/dashboard"));

function AppRoutes() {
  return (
    <Suspense fallback={<PageLoader />}>
      <Dashboard />
    </Suspense>
  );
}
```

```tsx
// Auth guard
export function ProtectedRoute({ children }) {
  const { status } = useAuth();

  if (status === "loading") return <PageLoader />;
  if (status === "unauthenticated") return <Navigate to="/login" replace />;

  return <>{children}</>;
}
```

---

## Forms

Use HeroUI native form components. For complex validation, pair with **React Hook Form + Zod**.

```tsx
import { useForm, Controller } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import * as z from "zod";
import { Form, TextField, Button } from "@heroui/react";

const schema = z.object({ email: z.string().email() });

export function MyForm() {
  const { control, handleSubmit, formState: { errors } } = useForm({
    resolver: zodResolver(schema),
  });

  return (
    <Form onSubmit={handleSubmit((v) => console.log(v))} className="space-y-4">
      <Controller
        name="email"
        control={control}
        render={({ field }) => (
          <TextField
            {...field}
            label="Email"
            type="email"
            isInvalid={!!errors.email}
            errorMessage={errors.email?.message}
          />
        )}
      />
      <Button type="submit" color="primary">Submit</Button>
    </Form>
  );
}
```

---

## Common Patterns

### Card with Content

```tsx
import { Card, CardBody, CardHeader } from "@heroui/react";

export function InfoCard() {
  return (
    <Card className="max-w-sm">
      <CardHeader className="flex gap-3">
        <div className="flex flex-col">
          <p className="text-md font-semibold">HeroUI v3</p>
          <p className="text-sm text-default-500">Beautiful by default</p>
        </div>
      </CardHeader>
      <CardBody>
        <p>Build stunning UIs with accessible, composable components.</p>
      </CardBody>
    </Card>
  );
}
```

### Data Table

```tsx
import { Table, TableHeader, TableColumn, TableBody, TableRow, TableCell, Chip, Button } from "@heroui/react";

export function DataTable({ data }) {
  return (
    <Table aria-label="Data table">
      <TableHeader>
        <TableColumn>NAME</TableColumn>
        <TableColumn>STATUS</TableColumn>
        <TableColumn>ACTIONS</TableColumn>
      </TableHeader>
      <TableBody>
        {data.map((item) => (
          <TableRow key={item.id}>
            <TableCell>{item.name}</TableCell>
            <TableCell>
              <Chip color={item.active ? "success" : "default"} size="sm">
                {item.active ? "Active" : "Inactive"}
              </Chip>
            </TableCell>
            <TableCell>
              <Button size="sm" variant="ghost">Edit</Button>
            </TableCell>
          </TableRow>
        ))}
      </TableBody>
    </Table>
  );
}
```

### Modal with Form

```tsx
import { Modal, ModalContent, ModalHeader, ModalBody, ModalFooter, Button, useDisclosure, Input } from "@heroui/react";

export function CreateModal() {
  const { isOpen, onOpen, onOpenChange } = useDisclosure();

  return (
    <>
      <Button onPress={onOpen} color="primary">Create New</Button>
      <Modal isOpen={isOpen} onOpenChange={onOpenChange}>
        <ModalContent>
          {(onClose) => (
            <>
              <ModalHeader>Create Item</ModalHeader>
              <ModalBody>
                <Input label="Name" placeholder="Enter a name" />
              </ModalBody>
              <ModalFooter>
                <Button variant="flat" onPress={onClose}>Cancel</Button>
                <Button color="primary" onPress={onClose}>Create</Button>
              </ModalFooter>
            </>
          )}
        </ModalContent>
      </Modal>
    </>
  );
}
```

### Destructive Action with AlertDialog

```tsx
import { AlertDialog, AlertDialogContent, AlertDialogHeader, AlertDialogBody, AlertDialogFooter, Button, useDisclosure } from "@heroui/react";

export function DeleteConfirm({ onConfirm }) {
  const { isOpen, onOpen, onOpenChange } = useDisclosure();

  return (
    <>
      <Button color="danger" variant="flat" onPress={onOpen}>Delete</Button>
      <AlertDialog isOpen={isOpen} onOpenChange={onOpenChange}>
        <AlertDialogContent>
          {(onClose) => (
            <>
              <AlertDialogHeader>Confirm Deletion</AlertDialogHeader>
              <AlertDialogBody>Are you sure? This action cannot be undone.</AlertDialogBody>
              <AlertDialogFooter>
                <Button variant="flat" onPress={onClose}>Cancel</Button>
                <Button color="danger" onPress={() => { onConfirm(); onClose(); }}>Delete</Button>
              </AlertDialogFooter>
            </>
          )}
        </AlertDialogContent>
      </AlertDialog>
    </>
  );
}
```

### Select Dropdown

```tsx
import { Select, SelectItem } from "@heroui/react";

export function AnimalSelect() {
  return (
    <Select label="Favorite Animal" placeholder="Select an animal" className="max-w-xs">
      {["Cat", "Dog", "Bird", "Fish"].map((a) => (
        <SelectItem key={a}>{a}</SelectItem>
      ))}
    </Select>
  );
}
```

---

## Loading Philosophy

### Rule 1: Always Use Skeleton for Partial Loads

**Never** render a raw `<Spinner>`, the text "Loading...", or return `null` while content is fetching. Every async component must have a matching `*Skeleton` sibling that mirrors the real layout's shape and dimensions exactly.

```tsx
// ❌ WRONG
if (isLoading) return <Spinner />;
if (isLoading) return <p>Loading...</p>;
if (isLoading) return null;

// ✅ CORRECT
if (isLoading) return <UserCardSkeleton />;
```

Always build a `*Skeleton` component alongside each data-driven component:

```tsx
import { Skeleton, Card, CardBody, CardHeader } from "@heroui/react";

export function UserCardSkeleton() {
  return (
    <Card className="max-w-sm w-full p-4 space-y-4">
      <CardHeader className="flex gap-3 items-center">
        <Skeleton className="rounded-full w-10 h-10" />
        <div className="flex flex-col gap-2 flex-1">
          <Skeleton className="h-3 w-2/5 rounded-lg" />
          <Skeleton className="h-3 w-3/5 rounded-lg" />
        </div>
      </CardHeader>
      <CardBody className="space-y-3">
        <Skeleton className="h-3 w-full rounded-lg" />
        <Skeleton className="h-3 w-4/5 rounded-lg" />
        <Skeleton className="h-3 w-3/5 rounded-lg" />
      </CardBody>
    </Card>
  );
}
```

For tables, render N skeleton rows matching expected data count (default 5):

```tsx
{isLoading
  ? Array.from({ length: 5 }).map((_, i) => (
      <TableRow key={i}>
        <TableCell><Skeleton className="h-3 w-3/4 rounded-lg" /></TableCell>
        <TableCell><Skeleton className="h-5 w-16 rounded-full" /></TableCell>
        <TableCell><Skeleton className="h-7 w-12 rounded-md" /></TableCell>
      </TableRow>
    ))
  : data.map((item) => <RealRow key={item.id} item={item} />)
}
```

---

### Rule 2: Full-Page Load = Logo Splash Screen

When an entire page is loading (lazy route, auth check, initial data hydration), show a centered SVG logo with `animate-pulse` — never a bare spinner or blank screen.

Create `src/components/page-loader.tsx`:

```tsx
// src/components/page-loader.tsx
export function PageLoader() {
  return (
    <div className="fixed inset-0 z-50 flex items-center justify-center bg-background">
      {/* Replace the SVG below with your actual app logo */}
      <div className="animate-pulse">
        <svg
          width="56"
          height="56"
          viewBox="0 0 56 56"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
          aria-label="Loading"
        >
          {/* Swap this path for your real logo mark */}
          <rect width="56" height="56" rx="14" className="fill-primary" />
          <path
            d="M28 14L38 20V32L28 38L18 32V20L28 14Z"
            className="fill-primary-foreground"
          />
        </svg>
      </div>
    </div>
  );
}
```

Use it with React's `<Suspense>` for lazy-loaded routes:

```tsx
import { lazy, Suspense } from "react";
import { PageLoader } from "@/components/page-loader";

const Dashboard = lazy(() => import("@/pages/dashboard"));
const Settings = lazy(() => import("@/pages/settings"));

export function AppRoutes() {
  return (
    <Suspense fallback={<PageLoader />}>
      <Routes>
        <Route path="/dashboard" element={<Dashboard />} />
        <Route path="/settings" element={<Settings />} />
      </Routes>
    </Suspense>
  );
}
```

**PageLoader rules:**
- `fixed inset-0` overlay with `bg-background` — respects dark mode automatically
- SVG uses `fill-primary` / `fill-primary-foreground` — never hardcoded colors
- `animate-pulse` only — subtle, not distracting
- 48–64px, centered, no text, no progress bar
- Use the app's actual logo mark. If not provided, ask the user — do NOT substitute a generic `<Spinner>`

---

## Polished UI Checklist

Before shipping any component, verify:

- [ ] `HeroUIProvider` + `ToastProvider` wrap the app in `src/main.tsx`
- [ ] Only HeroUI components used — no raw HTML inputs, shadcn, or MUI
- [ ] No `"use client"` directives anywhere — this is Vite, not Next.js
- [ ] Color props use HeroUI semantic values (`primary`, `danger`, etc.) — not raw hex
- [ ] Interactive elements have `aria-label` or visible labels
- [ ] Destructive actions gated behind `AlertDialog`
- [ ] Loading states **always** use `Skeleton` — never a raw spinner, "Loading..." text, or null
- [ ] Full-page / lazy-route loads use `<PageLoader>` (SVG logo + `animate-pulse`) via `<Suspense>`
- [ ] Fonts loaded via Fontsource — never via `<link>` or Google Fonts CDN
- [ ] Responsive layout using Tailwind breakpoints (`sm:`, `md:`, `lg:`)
- [ ] Toast feedback on async ops (`addToast` with `color="success"` or `color="danger"`)
- [ ] Path alias `@/` configured in both `vite.config.ts` and `tsconfig.json`
- [ ] `.npmrc` updated for hoisted packages if in a monorepo

---

## Prohibited Practices

| ❌ Never Do | ✅ Do Instead |
|---|---|
| `if (isLoading) return <Spinner />` | Build a matching `*Skeleton` component |
| `if (isLoading) return <p>Loading...</p>` | Build a matching `*Skeleton` component |
| `if (isLoading) return null` | Build a matching `*Skeleton` component |
| Full-page load with a bare spinner | `<PageLoader>` with SVG logo inside `<Suspense fallback>` |
| `"use client"` directive in any file | Remove it — Vite React is always client-side |
| `import * as Dialog from "@radix-ui/react-dialog"` | `import { Modal } from "@heroui/react"` |
| `import toast from "sonner"` or `"react-hot-toast"` | `import { addToast } from "@heroui/react"` |
| `import { Button } from "@mui/material"` | `import { Button } from "@heroui/react"` |
| `import { Button } from "@/components/ui/button"` (shadcn) | `import { Button } from "@heroui/react"` |
| `style={{ color: '#e53e3e' }}` | `<Button color="danger">` |
| `import "./custom.css"` with component overrides | Use HeroUI's `classNames` slot API |
| `<link href="https://fonts.googleapis.com/...">` | `pnpm add @fontsource-variable/dm-sans` |
| `npm install` / `yarn add` | `pnpm add` |
| Inventing component names not in the list above | Check the Available Components list; build custom if truly absent |

---

## Resources

- Components: https://v3.heroui.com/docs/react/components
- Theming: https://v3.heroui.com/docs/react/getting-started/theming
- Styling: https://v3.heroui.com/docs/react/getting-started/styling
- Composition: https://v3.heroui.com/docs/react/getting-started/composition
- Theme builder: https://v3.heroui.com/themes
