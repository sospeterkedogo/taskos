# TaskOS - Professional Task Command Center

A high-performance, distraction-free task management system built for velocity and clarity. Designed with a Swiss/Editorial aesthetic to maintain focus on what matters: Execution.

## Tech Stack

Framework: Next.js 14 (App Router)

Styling: Tailwind CSS (Brutalist/Minimal theme)

Database: Supabase (PostgreSQL)

Auth: Supabase Auth (Magic Link)

Icons: Lucide React

## Key Features

Mission Control: High-level dashboard with velocity tracking and progress visualization.

Timeline Logic: Tasks sorted by urgency and due dates (Overdue, Today, Upcoming).

Cloud Native: Access your tasks from any device; data syncs in real-time.

Secure: Row Level Security (RLS) ensures you only see your own data.

## Database Schema (Critical)

You must run this SQL in your Supabase SQL Editor to set up the backend:

-- 1. Create the Table
create table todos (
  id uuid default gen_random_uuid() primary key,
  user_id uuid references auth.users not null,
  title text not null,
  description text,
  completed boolean default false,
  priority text default 'MEDIUM', -- 'LOW', 'MEDIUM', 'HIGH'
  due_date date,
  created_at timestamp with time zone default timezone('utc'::text, now()) not null
);

-- 2. Enable Row Level Security (Security Policy)
alter table todos enable row level security;

-- 3. Create Policies (Who can do what?)
create policy "Individuals can create their own todos." on todos for
    insert with check (auth.uid() = user_id);

create policy "Individuals can view their own todos." on todos for
    select using (auth.uid() = user_id);

create policy "Individuals can update their own todos." on todos for
    update using (auth.uid() = user_id);

create policy "Individuals can delete their own todos." on todos for
    delete using (auth.uid() = user_id);

-- 4. Enable Realtime (Optional but recommended)
-- Go to Database -> Replication in Supabase Dashboard and enable 'todos'


## Environment Setup

Create a .env.local file in your root:

NEXT_PUBLIC_SUPABASE_URL=your_project_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_anon_key


## Installation

npm install @supabase/supabase-js date-fns lucide-react
npm run dev


© 2025 Sospeter. Built for productivity.
