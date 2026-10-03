---
title: 'Next.Js'
date: '2026-10-02'
draft: false
---

## Module 1: Introduction & Environment Setup

- **Welcome & Project Overview:** Course introduction and Property Pulse project walkthrough    
      
    
- **Core Concepts:** Understanding Next.js as a full-stack React framework (SSR, SSG, SEO, API routes)    
      
    
- **Environment Setup:** Installing VS Code extensions, Node.js/npm, Git, React DevTools, and MongoDB Atlas/Compass    
      
    
- **Project Initialization:** Setting up theme files, creating a Next.js app with Tailwind CSS, and configuring dev servers
    
      
  

## Module 2: Fundamentals & Component Architecture

- **Layouts & Pages:** Setting up the App Router layout, home page, and global Tailwind styles
    
      
    
- **Metadata & Assets:** Managing page metadata (title, description, keywords) and static assets
    
      
    
- **File-Based Routing:** Implementing nested routes (`/properties`), dynamic `[id]` routes, and catch-all patterns
    
      
    
- **Server vs. Client Components:** Understanding default server rendering, the `"use client"` directive, and navigation hooks (`useRouter`, `useParams`, `useSearchParams`, `usePathname`)
    
      
    
- **Core Components:** Building dynamic Navbars, active link highlighters, footers, hero sections, property cards, custom loading spinners (`loading.js`), and custom 404 pages (`not-found.js`)
    
      
    

## Module 3: Database Integration & Data Management

- **Database Configuration:** Creating MongoDB Atlas clusters, setting up MongoDB Compass, and managing URI variables in `.env`
    
      
    
- **Mongoose Integration:** Configuring database connection logic (`config/database.js`) and defining `User` and `Property` Mongoose schemas
    
      
    
- **Data Retrieval:** Fetching properties using server components and lean queries for optimized performance
    
      
    

## Module 4: Authentication & Protected Routes

- **Next Auth & OAuth Setup:** Understanding session management, configuring Google OAuth Credentials, and establishing the `/api/auth` catch-all route
    
      
    
- **Authentication State:** Integrating session providers, creating sign-in/sign-out buttons, and rendering user profile imagery dynamically
    
      
    
- **Database Synchronization:** Writing logic to create or attach users to the database upon Google OAuth login
    
      
    
- **Route Protection:** Securing restricted pages (`/properties/add`, `/profile`, `/messages`) using Next.js root middleware and matchers
    
      
    

## Module 5: Property CRUD & Media Management

- **Server Actions:** Handling form submissions directly on the server without manual client API calls
    
      
    
- **Data Formatting:** Parsing form inputs, amenities lists, and seller details
    
      
    
- **Image Management:** Converting uploads to Base64, integrating Cloudinary for media storage, and displaying images via responsive grids and lightboxes (PhotoSwipe)
    
      
    
- **Listing Management:** Prefilling update forms, executing database updates, handling property deletion with image cleanup, and revalidating paths
    
      
    

## Module 6: Interactive Features & Location Services

- **Geocoding & Maps:** Translating property addresses to latitude/longitude via Google Geocoding and rendering Mapbox maps with custom markers
    
      
    
- **Bookmarking:** Toggling saved properties, checking bookmark state, and rendering a dedicated "Saved Properties" page
    
      
    
- **Social Integration & UI Feedback:** Implementing social sharing buttons (`react-share`) and toast notifications (`react-toastify`) for action feedback
    
      
    

## Module 7: Search, Filtering & Pagination

- **Search Engine:** Creating client search forms, passing URL query parameters, and running dynamic MongoDB regex queries across names, descriptions, and locations
    
      
    
- **Pagination:** Implementing offset/limit logic (`skip` and `limit`) with Mongoose and building reusable page controls
    
      
    

## Module 8: In-App Messaging & Global State

- **Messaging Architecture:** Defining the `Message` model for sender, recipient, property, and body fields
    
      
    
- **Form Hooks:** Using `useActionState` and `useFormStatus` to monitor pending states and display submission feedback
    
      
    
- **Inbox System:** Building user inbox pages, displaying message cards, and enabling mark-as-read and delete actions
    
      
    
- **Global Context:** Setting up React Context to manage real-time unread message counters across the navigation bar and inbox
    
      
    

## Module 9: Optimization & Deployment

- **Featured Listings:** Querying and highlighting featured properties on the home page
    
      
    
- **Vercel Deployment:** Pushing code to GitHub, setting up production environment variables, assigning domains, and validating live authentication, search, and database actions
