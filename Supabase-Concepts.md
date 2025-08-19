# Supabase Concepts

This document covers key Supabase concepts with practical examples using React and PostgreSQL. Topics include authentication, role-based access control, CRUD operations, edge functions, image storage, real-time data, and Row Level Security (RLS).

---

## 1. Authentication

Supabase provides authentication via email, OAuth, and third-party providers. Here’s how to set up email authentication in React:

```jsx
import { createClient } from "@supabase/supabase-js";

const supabase = createClient(
  "https://your-project.supabase.co",
  "public-anon-key"
);

// Sign up
const signUp = async (email, password) => {
  const { user, error } = await supabase.auth.signUp({ email, password });
};

// Sign in
const signIn = async (email, password) => {
  const { user, error } = await supabase.auth.signInWithPassword({
    email,
    password,
  });
};
```

---

## 2. Role Based Access Control (RBAC)

RBAC is managed via PostgreSQL roles and policies. Example: restrict access to `admin` role.

```sql
-- Create role
CREATE ROLE admin;

-- Grant privileges
GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA public TO admin;
```

In Supabase, use RLS policies to enforce roles:

```sql
CREATE POLICY "Admins only" ON public.my_table
  FOR ALL
  USING (auth.role() = 'admin');
```

---

## 3. CRUD Operation

Basic CRUD in React using Supabase client:

```jsx
// Create
await supabase.from("tasks").insert([{ title: "New Task" }]);

// Read
const { data } = await supabase.from("tasks").select("*");

// Update
await supabase.from("tasks").update({ title: "Updated Task" }).eq("id", 1);

// Delete
await supabase.from("tasks").delete().eq("id", 1);
```

---

## 4. Edge Function

Edge Functions allow you to run server-side code close to your users.

Example: Create an edge function in `supabase/functions/hello.ts`:

```ts
import { serve } from "https://deno.land/x/sift/mod.ts";

serve(async (req) => {
  return new Response("Hello from Edge Function!");
});
```

Deploy with:

```
supabase functions deploy hello
```

Call from React:

```js
const res = await fetch("https://your-project.functions.supabase.co/hello");
const text = await res.text();
```

---

## 5. Image Storing

Supabase Storage allows you to store and serve images.

```jsx
// Upload image
const uploadImage = async (file) => {
  const { data, error } = await supabase.storage
    .from("avatars")
    .upload("public/avatar.png", file);
};

// Get image URL
const { publicURL } = supabase.storage
  .from("avatars")
  .getPublicUrl("public/avatar.png");
```

---

## 6. Real Time Data

Supabase supports real-time subscriptions using PostgreSQL's replication.

```jsx
supabase
  .channel("public:tasks")
  .on(
    "postgres_changes",
    { event: "*", schema: "public", table: "tasks" },
    (payload) => {
      console.log("Change received!", payload);
    }
  )
  .subscribe();
```

---

## 7. RLS - Row Level Security

RLS lets you define fine-grained access policies.

Enable RLS:

```sql
ALTER TABLE tasks ENABLE ROW LEVEL SECURITY;
```

Example policy: Only allow users to access their own tasks.

```sql
CREATE POLICY "User can access own tasks" ON tasks
  FOR SELECT USING (auth.uid() = user_id);
```

---

## References

- [Supabase Docs](https://supabase.com/docs)
- [React Integration](https://supabase.com/docs/guides/with-react)
- [Edge Functions](https://supabase.com/docs/guides/functions)
- [RLS](https://supabase.com/docs/guides/auth/row-level-security)
