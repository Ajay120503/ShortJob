# ShortJob

ShortJob is a full-stack MERN professional community platform for networking, posts, stories, job opportunities, applications, messaging, and admin moderation. It combines a social feed, opportunity board, real-time chat, PWA support, and a powerful admin dashboard.

## Tech Stack

| Layer | Technology |
| --- | --- |
| Frontend | React 19, Vite, Tailwind CSS, DaisyUI 5 |
| Backend | Node.js, Express.js |
| Database | MongoDB, Mongoose |
| Real Time | Socket.io |
| Auth | JWT, httpOnly cookies, email OTP |
| Media | Cloudinary |
| Email | Nodemailer |
| PWA | vite-plugin-pwa, Workbox |

## Current Features

### Social Feed

- Public feed with approved posts.
- Post types: general, job, announcement, achievement, and noticeboard.
- Text posts, multi-image posts, tags, likes, saves, sharing, and delete/edit for owners.
- Feed images preserve the full uploaded content with responsive contained previews and full-image open links.
- Nested comments and replies with comment likes.
- Dedicated post detail conversation layout with a split desktop view, mobile-friendly comment panel, multiline comment input, reply state, and clean empty/loading states.
- Linked job cards inside feed posts.
- Author special color theme support on profile, posts, post detail, jobs, and job detail.
- Saved posts page.

### Stories

- Users can create stories with text or image.
- Stories are moderated before public visibility when moderation is enabled.
- Story viewer modal with progress bars.
- Story avatars do not show online/offline dots for cleaner UI.
- Story media is deleted from Cloudinary during manual or scheduled cleanup.

### Jobs & Opportunities

- Any signed-in user can create jobs/opportunities.
- Job image and organization logo support.
- Required qualifications display cleanly.
- Skills required are shown compactly in cards and sidebars.
- Filters for paid/unpaid, location, and opportunity type.
- Applications with status workflow.
- Applicant dashboard and application kanban with drag-and-drop status updates.
- Applicant records can be exported as formatted Excel and PDF reports by the job poster.
- Improved applicant profile cards with contact, skill, location, timeline, and application context.
- Matched jobs based on profile skills and qualifications.
- Job Q&A, reach stats, quick apply, and applicant tracking.

### Profiles

- Public profiles with profile photo, organization logo, bio, links, skills, qualifications, interests, and location.
- Career timeline entries for background, work, and achievements.
- Current work details and previous work history.
- Follow/unfollow system with followers/following modals.
- Open to Opportunities setting.
- Special user profile theme variants for eligible users.
- Online/offline presence indicator across the platform, with privacy setting.
- Avatar badges are scoped to profile and explore; other pages show clean avatars with only presence.

### Explore

- Search users by name, skill, organization, or profile content.
- Followed users remain searchable.
- Explore excludes already-followed users from discovery sections where appropriate.
- Popular, active, open-to-work, and admin filters.
- Admin/special users display with unique styling.

### Chat

- One-to-one real-time conversations.
- Duplicate conversation prevention.
- Text, image, and file messages.
- Typing indicators, read receipts, unread badges, and online status.
- Custom confirmation modals for clearing/deleting chat.
- Cloudinary cleanup for deleted chat files.

### Notifications

- Real-time notifications through Socket.io.
- Likes, comments, replies, follows, job applications, application updates, messages, and moderation decisions.
- Admin approval/rejection of posts, jobs, and stories notifies the creator.
- Mark all as read, delete individual notifications, and clear all notifications at once.
- Clearing all notifications first marks unread notifications as read before deleting them.

### Authentication & Security

- Register, login, logout, refresh token, email verification, forgot password, and reset password.
- Registration OTP flow.
- Optional login audit setting controlled by admin:
  - Requires location and photo capture at login.
  - Requires a complete visible face capture before login can continue.
  - Uses native browser face detection when available, with a stricter canvas fallback for browsers without native support.
  - Rejects missing, cropped, unclear, or poorly lit face captures.
  - Stores audit records with photo, device, IP, and location.
  - Records expire after 24 hours.
  - Users can delete their own login records.
  - Super admins can delete login records globally.
  - Login audit Cloudinary photos are removed when audit records are deleted.
- Blocked-screen UI matches the application shell while disabling actions.

### Admin Dashboard

- Admin dashboard with users, moderation queue, settings, login records, and content detail pages.
- Manual moderation for posts, jobs, and stories.
- Rule-based moderation with score, severity, decision, and transparent flags.
- Auto moderation and manual review can run in parallel.
- Auto-rejected content remains visible in the admin queue for manual override.
- Admin settings for review queue, auto moderation, content types, rejection reason, login audit, and notifications.
- Trust badge management.
- Super admin permissions for platform-level actions.

### Permissions

- General users can use core platform features: posts, stories, jobs, applications, chat, profile, and explore.
- Admins can review content, run rule checks, approve/reject content, inspect users, view login records, and update admin notes.
- Super admins can:
  - Promote users to admin.
  - Remove admin access.
  - Block/unblock users.
  - Delete users.
  - Grant/revoke trust badges.
  - Update platform settings.
  - Delete login records.
- Top contributor and trust badges no longer grant admin permissions by themselves.

### Moderation & Fake Detection

- Rule-only detector for posts, jobs, and stories.
- Detects scam patterns, unsafe topics, fee/deposit requests, off-platform contact funnels, short links, suspicious compensation, weak job details, repeated spam, low context, and expired deadlines.
- Returns score, decision, severity, reason, and detailed flags.
- Runs on content creation and through admin manual rule check.
- Scheduled auto moderation processes pending content when the review window expires.

### Media Cleanup

- Cloudinary cleanup is handled for profile changes, post deletion, job deletion/update, stories, chat files, login records, and user deletion.
- Keeps database and storage in sync.

### UI & Branding

- Warm professional teal/coral theme.
- FontAwesome brand icon usage.
- Updated PWA icons and favicon.
- Mobile bottom bar uses the current user avatar for Profile.
- Sidebar, bottom bar, admin UI, right sidebar, explore, jobs, saved posts, profile, job cards, post cards, comments, and blocked screen share the current app design language.
- Responsive layouts use fuller mobile width with tighter side spacing and larger-screen split panels where useful.
- Landing page uses updated visual sections and subtle scroll animations.

## Project Structure

```text
ShortJob/
├── client/
│   ├── public/
│   │   ├── favicon.svg
│   │   ├── icon.svg
│   │   ├── icons/
│   │   └── manifest.webmanifest
│   ├── src/
│   │   ├── components/
│   │   │   ├── admin/
│   │   │   ├── auth/
│   │   │   ├── common/
│   │   │   ├── job/
│   │   │   ├── post/
│   │   │   └── profile/
│   │   ├── context/
│   │   ├── pages/
│   │   │   └── admin/
│   │   ├── store/
│   │   ├── utils/
│   │   ├── App.jsx
│   │   ├── main.jsx
│   │   └── index.css
│   ├── package.json
│   └── vite.config.js
├── server/
│   ├── config/
│   ├── controllers/
│   ├── middlewares/
│   ├── models/
│   ├── routes/
│   ├── utils/
│   ├── package.json
│   └── server.js
├── .gitignore
└── README.md
```

## Setup

### Prerequisites

- Node.js 18+
- MongoDB or MongoDB Atlas
- Cloudinary account
- Email account or SMTP credentials for Nodemailer

### Install Dependencies

```bash
cd client
npm install

cd ../server
npm install
```

### Environment Variables

Create `server/.env`:

```env
MONGODB_URI=mongodb+srv://<user>:<password>@<cluster>/<database>
JWT_SECRET=your_jwt_secret
JWT_REFRESH_SECRET=your_refresh_secret
CLIENT_URL=http://localhost:5173

EMAIL_USER=your_email@example.com
EMAIL_PASSWORD=your_email_app_password

CLOUDINARY_CLOUD_NAME=your_cloudinary_cloud
CLOUDINARY_API_KEY=your_cloudinary_key
CLOUDINARY_API_SECRET=your_cloudinary_secret

PORT=5000
NODE_ENV=development
```

### Run Locally

Terminal 1:

```bash
cd server
node server.js
```

Terminal 2:

```bash
cd client
npm run dev
```

Default URLs:

- Frontend: `http://localhost:5173`
- Backend API: `http://localhost:5000/api`

## Useful Commands

```bash
# Build frontend
cd client
npm run build

# Preview frontend build
cd client
npm run preview

# Run backend
cd server
node server.js
```

## API Overview

### Auth

| Method | Endpoint | Description |
| --- | --- | --- |
| POST | `/api/auth/register` | Start registration |
| POST | `/api/auth/verify-registration-otp` | Verify registration OTP |
| POST | `/api/auth/login` | Login |
| POST | `/api/auth/login-audit` | Complete login audit when required |
| POST | `/api/auth/logout` | Logout |
| GET | `/api/auth/me` | Current user |
| POST | `/api/auth/forgot-password` | Send password reset OTP |
| POST | `/api/auth/reset-password` | Reset password |
| POST | `/api/auth/refresh-token` | Refresh token |

### Users

| Method | Endpoint | Description |
| --- | --- | --- |
| GET | `/api/users/search` | Search users |
| GET | `/api/users/:id` | Get profile |
| PUT | `/api/users/:id` | Update profile |
| POST | `/api/users/:id/follow` | Follow/unfollow |
| GET | `/api/users/:id/posts` | User posts |
| GET | `/api/users/:id/jobs` | User jobs |
| GET | `/api/users/:id/followers` | Followers |
| GET | `/api/users/:id/following` | Following |
| PATCH | `/api/users/me/opportunity-status` | Toggle open to opportunities |
| POST | `/api/users/me/badges` | Update profile badges |
| PUT | `/api/users/:id/timeline` | Update career timeline |
| GET | `/api/users/online` | Online user IDs |
| GET | `/api/users/me/login-history` | My login records |
| DELETE | `/api/users/me/login-history/:id` | Delete my login record |

### Posts & Comments

| Method | Endpoint | Description |
| --- | --- | --- |
| GET | `/api/posts` | Feed |
| POST | `/api/posts` | Create post |
| GET | `/api/posts/:id` | Post detail |
| PUT | `/api/posts/:id` | Update post |
| DELETE | `/api/posts/:id` | Delete post |
| POST | `/api/posts/:id/like` | Toggle like |
| POST | `/api/posts/:id/save` | Toggle save |
| GET | `/api/posts/saved` | Saved posts |
| GET | `/api/posts/:postId/comments` | Comments |
| POST | `/api/posts/:postId/comments` | Add comment |
| POST | `/api/comments/:id/reply` | Reply |
| POST | `/api/comments/:id/like` | Toggle comment like |
| DELETE | `/api/comments/:id` | Delete comment |

### Jobs

| Method | Endpoint | Description |
| --- | --- | --- |
| GET | `/api/jobs` | List jobs |
| POST | `/api/jobs` | Create job |
| GET | `/api/jobs/:id` | Job detail |
| PUT | `/api/jobs/:id` | Update job |
| DELETE | `/api/jobs/:id` | Delete job |
| POST | `/api/jobs/:id/apply` | Apply |
| GET | `/api/jobs/:id/applicants` | Applicants |
| PUT | `/api/applications/:id/status` | Update application status |
| GET | `/api/jobs/applications/my` | My applications |
| GET | `/api/jobs/matched` | Matched jobs |

### Stories

| Method | Endpoint | Description |
| --- | --- | --- |
| GET | `/api/stories` | Get visible stories |
| POST | `/api/stories` | Create story |
| POST | `/api/stories/:id/view` | Mark story viewed |
| DELETE | `/api/stories/:id` | Delete story |

### Chat

| Method | Endpoint | Description |
| --- | --- | --- |
| GET | `/api/chat/conversations` | Conversations |
| POST | `/api/chat/conversations` | Create/get conversation |
| DELETE | `/api/chat/conversations/:id` | Delete conversation |
| DELETE | `/api/chat/conversations/:id/messages` | Clear conversation messages |
| GET | `/api/chat/conversations/:id/messages` | Messages |
| POST | `/api/chat/messages` | Send message |
| PUT | `/api/chat/messages/:id/read` | Mark read |

### Notifications

| Method | Endpoint | Description |
| --- | --- | --- |
| GET | `/api/notifications` | Notifications |
| PUT | `/api/notifications/read-all` | Mark all read |
| DELETE | `/api/notifications/:id` | Delete notification |
| DELETE | `/api/notifications/clear-all` | Mark unread notifications read, then delete all |

### Admin

| Method | Endpoint | Description |
| --- | --- | --- |
| GET | `/api/admin/users` | List users |
| GET | `/api/admin/users/:id` | User detail |
| PUT | `/api/admin/users/:id/notes` | Update admin notes |
| PUT | `/api/admin/users/:id/block` | Block user, super admin |
| PUT | `/api/admin/users/:id/unblock` | Unblock user, super admin |
| DELETE | `/api/admin/users/:id` | Delete user, super admin |
| PUT | `/api/admin/users/:id/grant-badge` | Grant trust badge, super admin |
| PUT | `/api/admin/users/:id/revoke-badge` | Revoke trust badge, super admin |
| PUT | `/api/admin/users/:id/make-admin` | Promote admin, super admin |
| PUT | `/api/admin/users/:id/remove-admin` | Remove admin, super admin |
| GET | `/api/admin/settings` | Read settings |
| PUT | `/api/admin/settings` | Update settings, super admin |
| GET | `/api/admin/queue` | Moderation queue |
| GET | `/api/admin/content/:type/:id` | Content detail |
| PUT | `/api/admin/content/:type/:id/run-check` | Run rule check |
| PUT | `/api/admin/content/:type/:id/approve` | Approve content |
| PUT | `/api/admin/content/:type/:id/reject` | Reject content |
| GET | `/api/admin/login-records` | Login audit records |
| GET | `/api/admin/login-records/:id` | Login audit detail |
| DELETE | `/api/admin/login-records/:id` | Delete login record, super admin |

## Notes
