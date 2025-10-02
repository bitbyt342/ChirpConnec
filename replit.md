# Twitter/X-like Social Platform

## Project Overview
A full-featured social networking platform similar to Twitter/X with user authentication, posting, social features, Blue Tick verification subscriptions, ad campaigns, and admin controls.

## Tech Stack
- **Frontend**: React, TypeScript, Tailwind CSS, shadcn/ui, Wouter (routing), TanStack Query
- **Backend**: Node.js, Express, TypeScript
- **Database**: PostgreSQL (Neon-backed via Replit)
- **Authentication**: Replit Auth (OIDC) - supports Google, GitHub, X, Apple, email/password
- **Payments**: Stripe for Blue Tick subscriptions
- **File Upload**: Multer with base64 data URL encoding

## Key Features
1. **Authentication**: Replit Auth integration with session management
2. **Posts**: Create/view posts with text and images, like/comment/repost
3. **Social**: Follow/unfollow users, view followers/following
4. **Verification**: Blue Tick subscription via Stripe (monthly/annual plans)
5. **Ads**: Business ad campaign creation and management
6. **Admin**: Content moderation, user management, reporting system
7. **Search**: User and hashtag search functionality
8. **Trending**: Hashtag tracking and trending topics

## Database Schema
- `sessions`: Session storage for auth
- `users`: User profiles with verification status
- `posts`: User posts with content and images
- `likes`: Post likes
- `comments`: Post comments
- `reposts`: Post reposts
- `follows`: User follow relationships
- `hashtags`: Trending hashtags
- `ad_campaigns`: Business advertising campaigns
- `reports`: Content and user reports

## Authentication Flow
1. Landing page with login/signup buttons
2. Redirect to `/api/login` which triggers Replit Auth
3. OIDC flow with callback to `/api/callback`
4. User creation/update in database via `upsertUser`
5. Session stored in PostgreSQL
6. Protected routes use `isAuthenticated` middleware
7. Logout via `/api/logout` with OIDC end session

## API Routes
- **Auth**: `/api/auth/user`, `/api/login`, `/api/logout`, `/api/callback`
- **Users**: `/api/users/search`, `/api/users/:id`, `/api/users/:id/followers`, `/api/users/:id/following`
- **Posts**: `/api/posts`, `/api/posts/:id`, `/api/posts/:id/like`, `/api/posts/:id/comments`, `/api/posts/:id/repost`
- **Follow**: `/api/users/:id/follow`, `/api/users/:id/unfollow`
- **Subscriptions**: `/api/subscribe/create-checkout-session`, `/api/subscribe/portal`
- **Ads**: `/api/ad-campaigns`, `/api/ad-campaigns/:id`
- **Admin**: `/api/admin/users`, `/api/admin/reports`, `/api/admin/users/:id/verify`, `/api/admin/users/:id/suspend`

## Pages
- `/` - Landing page (public)
- `/home` - Main feed (protected)
- `/profile/:id` - User profile (public)
- `/subscribe` - Blue Tick subscription (protected)
- `/admin` - Admin dashboard (admin only)
- `/ads` - Ad campaign manager (protected)

## Recent Changes (October 1, 2025)
- Fixed TypeScript type errors in User interface usage
- Fixed null safety issues for nullable database fields
- Updated react-icons from SiTwitter to SiX (v5+ compatibility)
- Installed multer for file uploads
- Fixed Stripe API version to "2025-09-30.clover"
- Application running successfully on port 5000

## Environment Variables
- `DATABASE_URL`: PostgreSQL connection string
- `SESSION_SECRET`: Session encryption secret
- `STRIPE_SECRET_KEY`: Stripe API secret key
- `VITE_STRIPE_PUBLIC_KEY`: Stripe publishable key (frontend)
- `REPL_ID`: Replit application ID
- `REPLIT_DOMAINS`: Comma-separated list of domains for auth callbacks
- `ISSUER_URL`: OIDC issuer URL (defaults to https://replit.com/oidc)

## Development
- Start: `npm run dev` (runs Express + Vite on port 5000)
- Database push: `npm run db:push`
- All workflows managed by Replit, auto-restart on changes

## Testing Strategy
- E2E tests using Playwright for UI/UX workflows
- Test authentication flow with OIDC claim injection
- Test posting, social features, payments, and admin functions
- Database operations tested via direct queries in test plans
