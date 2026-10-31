# TASKLY — Plan together. Ship faster.

![TASKLY Brand Banner](https://images.unsplash.com/photo-1618005182384-a83a8bd57fbe?w=1200&auto=format&fit=crop&q=80)

**TASKLY** is a production-ready, full-stack collaborative project management platform built for modern engineering and design teams. It combines fluid Kanban boards with real-time WebSocket collaboration, task assignments, checklist tracking, comments with @mentions and emoji reactions, and in-app notifications.

---

## 🎨 Brand Identity & Design Direction

- **Product Name:** TASKLY
- **Tagline:** *"Plan together. Ship faster."*
- **Design Direction:** Minimal, professional, energetic, and highly usable with clean flat surfaces and subtle elevation.
- **Color Palette:**
  - **Primary / Accent:** `#A3E635` (Graphite Lime)
  - **Light Lime / Highlights:** `#D9F99D`
  - **Dark Surfaces & Sidebar:** `#20242C`
  - **Canvas Background:** `#F9FAFB`
  - **Neutral Border & Dividers:** `#E5E7EB`
- **Typography:** Plus Jakarta Sans & Inter
- **Micro-Interactions:** Smooth drag-and-drop card movement, live typing indicator banner, member online presence rings, and celebratory confetti on completing tasks.

---

## 🚀 Key Features

### 1. Real-Time Kanban Boards
- **Dynamic Columns:** Standard workflow (`Backlog`, `To Do`, `In Progress`, `Review`, `Done`) plus custom column creation, renaming, and deletion.
- **Drag & Drop:** Fluid card movement across columns and reordering within columns with optimistic UI updates and server synchronization.
- **Filters & Search:** Instant filtering by priority (`Low`, `Medium`, `High`, `Urgent`), assignee, and title/description search.
- **Task Cards:** Display priority badges, colored project tags, checklist progress bars, overdue alerts, comment counts, attachment counters, and assignee avatars.

### 2. Rich Task Detail Modal
- **Editable Title & Metadata:** Direct inline editing of task title, priority, status, and due dates.
- **Markdown / Rich Description:** Full support for formatted task briefs and specifications.
- **Interactive Checklists:** Create multiple checklists, track completed/total subtasks with progress bars.
- **Task Attachments:** Upload and manage link and file attachments with quick-open external links.
- **Comments with @Mentions & Reactions:**
  - Real-time comment stream.
  - Teammate mentions (`@username`) trigger automatic in-app notifications.
  - Emoji reaction picker (`👍`, `❤️`, `🚀`, `👀`, `🎉`) with one-click toggles and live counts.
  - Live typing indicator: *"Riya Sharma is typing..."*.
- **Activity History:** Full chronological audit log of movements, edits, and completions.

### 3. In-App Notification Center
- **Bell Icon Badge:** Real-time unread badge counter in top navbar.
- **Notification Drawer:** Tabs for *All* and *Unread* notifications.
- **Event Triggers:** Task assignments, due dates / overdue alerts, comment @mentions, and project invitations.
- **One-Click Deep Linking:** Click any notification to navigate directly to the relevant project and task.
- **Mark as Read:** Individual and "Mark all as read" bulk actions.

### 4. Project & Team Management
- **Role-Based Access Control:** `OWNER`, `ADMIN`, `MEMBER`.
- **Project Invitations:** Invite collaborators via email or username.
- **Privacy Controls:** Public team projects or Private restricted boards.
- **Color & Icon Personalization:** 7 tailored brand accent hues.

### 5. Global Search (`Ctrl + K` / `Cmd + K`)
- Instant fuzzy search across project titles, descriptions, and task cards.
- Quick keyboard-driven navigation.

---

## 🛠️ Technology Stack

| Layer | Technology |
|---|---|
| **Frontend** | React 19, TypeScript, Vite |
| **Styling** | Tailwind CSS v4, Lucide React Icons |
| **Real-time** | Socket.IO Client |
| **Backend** | Node.js, Express, TypeScript (`tsx`) |
| **WebSockets** | Socket.IO Server with room-based pub/sub |
| **Database** | SQLite (out-of-the-box) / PostgreSQL compatible |
| **ORM** | Prisma ORM 5.x |
| **Authentication** | JWT Access & Refresh Tokens, `bcryptjs` password hashing |
| **Validation** | Zod schema validation (Client & Server) |
| **Delight** | Canvas-Confetti on task completion |

---

## 🗄️ Database Schema

The database models are defined in [schema.prisma](file:///c:/Users/mudra_pethkar__/CodeAlpha_ProjectManagementTool/server/prisma/schema.prisma):

- `User`: id, name, email, passwordHash, avatarUrl, role, bio, timestamps
- `Project`: id, name, description, color, icon, isPrivate, archived, ownerId, timestamps
- `ProjectMember`: id, projectId, userId, role (`OWNER`, `ADMIN`, `MEMBER`), joinedAt
- `Invitation`: id, projectId, email, token, role, status (`PENDING`, `ACCEPTED`, `DECLINED`), expiresAt
- `Board`: id, projectId, name, position
- `List`: id, boardId, name, position
- `Task`: id, listId, title, description, position, dueDate, priority, status, createdById, archived
- `TaskAssignee`: id, taskId, userId
- `Label`: id, projectId, name, color
- `TaskLabel`: id, taskId, labelId
- `Checklist`: id, taskId, title, position
- `ChecklistItem`: id, checklistId, text, completed, position
- `Attachment`: id, taskId, fileName, fileUrl, fileSize, uploadedBy
- `Comment`: id, taskId, authorId, body, edited, reactions (JSON)
- `Activity`: id, projectId, taskId, actorId, action, metadata (JSON)
- `Notification`: id, userId, type, title, body, resourceType, resourceId, read

---

## 🔌 API Endpoints Summary

### Authentication
- `POST /api/auth/register` — Create a new user account
- `POST /api/auth/login` — Sign in and receive JWT access & refresh tokens
- `POST /api/auth/refresh` — Refresh expired access token
- `POST /api/auth/logout` — Revoke session
- `GET /api/auth/me` — Retrieve current authenticated user profile
- `PATCH /api/auth/me` — Update name, bio, avatar, or password
- `POST /api/auth/forgot-password` — Generate password reset token
- `POST /api/auth/reset-password` — Set new password with reset token

### Projects & Invitations
- `GET /api/projects` — List user's projects with progress metrics
- `POST /api/projects` — Create project with default board & lists
- `GET /api/projects/:projectId` — Get project details & team roster
- `PATCH /api/projects/:projectId` — Update project metadata & privacy
- `DELETE /api/projects/:projectId` — Delete project (Owner only)
- `POST /api/projects/:projectId/invitations` — Invite member by email
- `GET /api/projects/:projectId/invitations` — List pending invitations
- `POST /api/invitations/:invitationId/accept` — Accept project invitation
- `POST /api/invitations/:invitationId/decline` — Decline project invitation
- `PATCH /api/projects/:projectId/members/:memberId` — Update member role
- `DELETE /api/projects/:projectId/members/:memberId` — Remove member

### Kanban Board & Lists
- `GET /api/projects/:projectId/board` — Get board with lists & tasks
- `POST /api/projects/:projectId/board/lists` — Create new list/column
- `PATCH /api/projects/:projectId/board/lists/:listId` — Rename list
- `DELETE /api/projects/:projectId/board/lists/:listId` — Delete list

### Tasks & Checklists
- `POST /api/lists/:listId/tasks` — Create new task card
- `GET /api/tasks/:taskId` — Get full task details
- `PATCH /api/tasks/:taskId` — Update task title, description, priority, due date
- `PATCH /api/tasks/:taskId/move` — Move task between lists / reorder
- `DELETE /api/tasks/:taskId` — Delete task
- `POST /api/tasks/:taskId/assignees` — Toggle assignee
- `POST /api/tasks/:taskId/labels` — Toggle label
- `POST /api/tasks/:taskId/checklists` — Create checklist
- `POST /api/checklists/:checklistId/items` — Add checklist item
- `PATCH /api/checklist-items/:itemId` — Toggle item completed / edit text
- `DELETE /api/checklist-items/:itemId` — Delete checklist item
- `POST /api/tasks/:taskId/attachments` — Add attachment
- `DELETE /api/attachments/:attachmentId` — Delete attachment

### Comments & Reactions
- `POST /api/tasks/:taskId/comments` — Add comment (with @mention detection)
- `PATCH /api/comments/:commentId` — Edit comment
- `DELETE /api/comments/:commentId` — Delete comment
- `POST /api/comments/:commentId/reaction` — Toggle emoji reaction

### Notifications & Search
- `GET /api/notifications` — Fetch user notifications & unread count
- `PATCH /api/notifications/:notificationId/read` — Mark notification read
- `PATCH /api/notifications/read-all` — Mark all notifications read
- `GET /api/activities` — Global activity feed for dashboard
- `GET /api/search?q=:query` — Global search across projects and tasks

---

## ⚡ WebSocket Event Model

Socket.IO rooms:
- `project:${projectId}`: For board collaborative updates.
- `user:${userId}`: For personal notifications.

| Event Name | Direction | Payload Description |
|---|---|---|
| `project:join` | Client -> Server | Join project room with user info |
| `project:leave` | Client -> Server | Leave project room |
| `project:presence` | Server -> Client | Array of active online teammates on the board |
| `task:typing` | Bi-directional | `{ taskId, userName, isTyping }` comment typing indicator |
| `task:created` | Server -> Client | New task card added to list |
| `task:updated` | Server -> Client | Updated task metadata |
| `task:moved` | Server -> Client | Task card moved between lists or reordered |
| `task:deleted` | Server -> Client | Task card removed |
| `list:created` | Server -> Client | New Kanban column created |
| `list:updated` | Server -> Client | List renamed |
| `list:deleted` | Server -> Client | List removed |
| `comment:created` | Server -> Client | New comment added to task |
| `comment:reaction_updated` | Server -> Client | Emoji reaction toggled on comment |
| `notification:new` | Server -> Client | Personal in-app notification delivered live |

---

## 🏁 Quick Start & Local Setup

### 1. Prerequisites
- Node.js `v18+` (tested on Node.js `v24.19.0`)
- npm `v9+`

### 2. Installation
Clone the repository and install all dependencies:
```bash
# Install root, backend, and frontend packages
npm run install:all
```

### 3. Initialize Database & Seed Demo Data
```bash
# Push Prisma schema to SQLite dev.db
npm run prisma:push

# Seed rich demo data (users, projects, tasks, comments, activities)
npm run prisma:seed
```

### 4. Run Development Servers
Start both backend API (`http://localhost:5000`) and frontend Vite dev server (`http://localhost:5173`) concurrently:
```bash
npm run dev
```

Visit **`http://localhost:5173`** in your browser!

---

## 👥 Demo Accounts (1-Click Login)

The seed script creates 4 team members with password `Password123!`:

| Name | Role | Email | Password |
|---|---|---|---|
| **Alex Morgan** | Project Lead / Owner | `alex@taskly.dev` | `Password123!` |
| **Riya Sharma** | Senior Product Designer | `riya@taskly.dev` | `Password123!` |
| **Marcus Vance** | Full Stack Engineer | `marcus@taskly.dev` | `Password123!` |
| **Elena Rostova** | QA & Release Lead | `elena@taskly.dev` | `Password123!` |

> 💡 **Tip:** On the landing page or login page, click any of the **1-Click Demo Login** buttons to immediately enter the dashboard as that team member without typing credentials!

---

## 🧪 Testing

Run backend security and unit tests:
```bash
npm run test
```

Build production bundles:
```bash
npm run build
```

---

## 🌐 Deployment Instructions

### Frontend (Vercel / Netlify / Cloudflare Pages)
1. Set the root directory to `client`.
2. Build command: `npm run build`.
3. Output directory: `dist`.
4. Environment variables:
   - `VITE_API_URL`: Your deployed backend URL + `/api` (e.g., `https://api.taskly.yourdomain.com/api`).
   - `VITE_SOCKET_URL`: Your deployed backend WebSocket URL.

### Backend (Railway / Render / Fly.io)
1. Set the root directory to `server`.
2. Build command: `npm run build`.
3. Start command: `node dist/index.js`.
4. Environment variables:
   - `PORT`: `5000` (or dynamic provided by host).
   - `DATABASE_URL`: Connection string for PostgreSQL (e.g. Neon, Supabase, Railway Postgres) or SQLite.
   - `JWT_SECRET`: Random 64-character secret.
   - `JWT_REFRESH_SECRET`: Random 64-character secret.
   - `CLIENT_URL`: URL of your frontend app.
5. In `server/prisma/schema.prisma`, to switch from SQLite to PostgreSQL, simply change:
   ```prisma
   datasource db {
     provider = "postgresql"
     url      = env("DATABASE_URL")
   }
   ```
   Then run `npx prisma db push`.

---

## 📄 License
MIT © TASKLY Team. Built for CodeAlpha Project Management Tool.
