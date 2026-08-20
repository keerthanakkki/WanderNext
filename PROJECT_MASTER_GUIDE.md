# 🏠 WanderNest — PROJECT MASTER GUIDE

## Complete Reverse Engineering & Interview Preparation Document

> **Purpose:** This document teaches you the WanderNest project so deeply that you can confidently explain it in placement interviews for product-based and service-based companies (8–18 LPA packages). Every section builds understanding step-by-step from absolute basics.

---

## 📑 TABLE OF CONTENTS

| Section | Title | Page |
|---------|-------|------|
| 1 | [Executive Summary](#section-1-executive-summary) | Overview & Pitches |
| 2 | [Problem Statement](#section-2-problem-statement) | Why This Project Exists |
| 3 | [Solution Overview](#section-3-solution-overview) | How It Solves The Problem |
| 4 | [Technology Stack](#section-4-technology-stack) | Every Tool Explained |
| 5 | [High Level Architecture (HLD)](#section-5-high-level-architecture-hld) | System Design |
| 6 | [Low Level Design (LLD)](#section-6-low-level-design-lld) | Module-by-Module |
| 7 | [Folder Structure](#section-7-complete-folder-structure-explanation) | Every Folder Explained |
| 8 | [Code Flow Walkthrough](#section-8-code-flow-walkthrough) | From Boot to Response |
| 9 | [Database Design](#section-9-database-design) | Schemas & Relations |
| 10 | [API Documentation](#section-10-api-documentation) | Every Route |
| 11 | [Authentication & Security](#section-11-authentication--security) | Login, Sessions, Safety |
| 12 | [Project Features](#section-12-project-features) | Feature Deep-Dive |
| 13 | [Interview Story](#section-13-interview-story) | Ready-Made Answers |
| 14 | [Frequently Asked Interview Questions](#section-14-frequently-asked-interview-questions) | 50+ Q&A |
| 15 | [Deep Dive Follow-Up Questions](#section-15-deep-dive-follow-up-questions) | 30+ Follow-Ups |
| 16 | [Failure Scenarios](#section-16-failure-scenarios) | What If X Breaks? |
| 17 | [Scalability Discussion](#section-17-scalability-discussion) | 100 to 100K Users |
| 18 | [Optimizations](#section-18-optimizations) | What Can Be Improved |
| 19 | [Future Scope](#section-19-future-scope) | 20 Enhancements |
| 20 | [Resume & Placement Preparation](#section-20-resume--placement-preparation) | Ready-to-Use Descriptions |
| 21 | [Project Revision Sheet](#section-21-project-revision-sheet) | Night-Before-Interview |
| 22 | [Mock Interview](#section-22-mock-interview) | Full Mock Session |
| 23 | [Cheat Sheet](#section-23-cheat-sheet) | 30-Minute Quick Ref |

---

# SECTION 1: EXECUTIVE SUMMARY

## 1. Project Name
**WanderNest**

## 2. One-line Description
A full-stack Airbnb-inspired travel accommodation listing platform built with Node.js, Express.js, MongoDB, and EJS, featuring user authentication, cloud image uploads, interactive maps, reviews, and category-based browsing.

## 3. Elevator Pitch (30 Seconds)
> "WanderNest is a full-stack web application I built that works like Airbnb. Users can browse travel accommodations, create their own listings with images and location maps, leave star-rated reviews, and search by destination or category. I built it using Node.js and Express on the backend, MongoDB for data storage, EJS for dynamic pages, Cloudinary for image uploads, Mapbox for interactive maps, and Passport.js for secure authentication. It follows the MVC architecture pattern and implements RESTful APIs."

## 4. Interview Introduction (1 Minute)
> "I developed WanderNest, a full-stack travel accommodation platform inspired by Airbnb. The problem I wanted to solve was creating a platform where travelers can discover unique stays and property owners can list their accommodations with rich media and location information.
>
> On the backend, I used Node.js with Express.js following the MVC architecture — Models with Mongoose for MongoDB, Controllers for business logic, and Routes for URL handling. User authentication is handled by Passport.js with session-based login. I integrated Cloudinary for cloud-based image uploads using Multer as middleware, and Mapbox GL JS for interactive maps with geocoding to convert location names to coordinates.
>
> The frontend uses EJS templating with ejs-mate for layouts, styled with Bootstrap 5. I implemented server-side validation using Joi, flash messages for user feedback, and authorization middleware to ensure only listing owners can edit or delete their properties. The application supports features like category filtering (Mountains, Castles, Pools, etc.), search functionality, star-rated reviews, and a tax toggle display."

## 5. Interview Introduction (2 Minutes)
> "I developed WanderNest, a comprehensive full-stack travel accommodation platform that replicates core Airbnb functionality. Let me walk you through the architecture and key decisions.
>
> **Architecture:** I followed the MVC pattern — separating concerns into Models (Mongoose schemas for Listings, Reviews, Users), Views (EJS templates with a layout system), and Controllers (business logic handlers). Routes are modularized into separate files for listings, reviews, and users, making the codebase maintainable.
>
> **Backend:** Built on Node.js with Express.js. I chose MongoDB with Mongoose ODM because the accommodation data is naturally document-oriented — each listing has nested image objects, arrays of review references, and GeoJSON geometry data for maps. This is a perfect use case for NoSQL.
>
> **Authentication & Authorization:** I implemented Passport.js with the Local Strategy and passport-local-mongoose for password hashing with PBKDF2. Sessions are stored in MongoDB using connect-mongo for persistence. I built custom middleware for authorization — `isLoggedIn` checks authentication, `isOwner` ensures only the listing creator can edit/delete, and `isReviewAuthor` protects review deletion.
>
> **Cloud Services:** Images are uploaded to Cloudinary via Multer with multer-storage-cloudinary, eliminating local storage dependency. Location coordinates are resolved through the Mapbox Geocoding API and rendered on interactive maps using Mapbox GL JS.
>
> **Validation & Error Handling:** Server-side validation uses Joi schemas for both listings and reviews. I created a custom `ExpressError` class extending the native Error class for consistent error handling, and a `wrapAsync` utility to catch rejected promises in async route handlers without repetitive try-catch blocks.
>
> **Key Features:** Category-based filtering (10 categories like Trending, Mountains, Castles), title-based search with MongoDB regex, star ratings using the Starability CSS library, tax toggle display, and full CRUD operations with image replacement on edit.
>
> **Deployment:** The app is configured for deployment with session secrets and database URLs managed through environment variables via dotenv."

## 6. What Problem Does This Solve?
Travelers need a single platform to **discover, compare, and book** unique accommodations worldwide. Property owners need a way to **list their properties** with rich media (images, maps, descriptions) and receive **user feedback** through reviews. WanderNest bridges this gap by providing a marketplace connecting travelers with property owners.

## 7. Why Does This Problem Matter?
- The global vacation rental market is worth **$100+ billion**
- Travelers waste hours comparing properties across scattered websites
- Property owners struggle to reach potential guests without a dedicated platform
- Trust between strangers (guests and hosts) requires review systems and verified profiles

## 8. Who Are the Target Users?
| User Type | Description |
|-----------|-------------|
| **Travelers** | People looking for unique accommodations (cottages, villas, treehouses) |
| **Property Owners** | Hosts who want to list their properties and reach guests |
| **Budget Travelers** | Users who want to compare prices and see tax breakdowns |
| **Adventure Seekers** | Users browsing by category (Mountains, Camping, Arctic) |

## 9. What Makes This Solution Useful?
- **Visual Discovery:** High-quality images with Cloudinary CDN delivery
- **Location Awareness:** Interactive Mapbox maps showing exact property locations
- **Trust Building:** Star-rated review system with author attribution
- **Smart Browsing:** 10 category filters + text search
- **Owner Control:** Only owners can edit/delete their listings
- **Responsive Design:** Works on desktop, tablet, and mobile

## 10. Real-World Applications
- Airbnb, Booking.com, Vrbo, OYO — all follow similar architectures
- The MVC pattern, REST APIs, authentication, and cloud storage concepts apply to **any** web application
- This project demonstrates **production-level** patterns: session management, input validation, authorization, error handling

---

### Resume Description Version
> **WanderNest — Full-Stack Travel Accommodation Platform**
> Built an Airbnb-inspired web app using Node.js, Express.js, MongoDB, EJS, and Bootstrap. Implemented MVC architecture with RESTful APIs, Passport.js authentication, Cloudinary image uploads, Mapbox geocoding/maps, Joi validation, and authorization middleware. Features include CRUD listings, star-rated reviews, category filtering, search, and tax toggle display.

### LinkedIn Description Version
> 🏠 Built **WanderNest** — a full-stack Airbnb clone that lets users list, discover, and review travel accommodations worldwide.
>
> 🔧 **Tech Stack:** Node.js | Express.js | MongoDB | Mongoose | EJS | Bootstrap 5 | Passport.js | Cloudinary | Mapbox | Joi
>
> ✅ **Key Features:** User authentication & authorization, cloud image uploads, interactive maps with geocoding, star-rated reviews, category-based filtering, responsive design
>
> 🏗️ **Architecture:** MVC pattern with modular routing, custom error handling middleware, session-based auth with MongoDB store, server-side validation

### HR Interview Explanation Version
> "I built a travel accommodation website called WanderNest, similar to Airbnb. Users can sign up, log in, browse properties with photos and maps, filter by categories like Mountains or Castles, search by destination name, create their own property listings with uploaded images, and leave star-rated reviews. I used popular industry technologies like Node.js for the server, MongoDB for the database, and integrated cloud services for images and maps. The project taught me how real-world applications handle user authentication, data validation, file uploads, and building maintainable code using design patterns like MVC."

---

# SECTION 2: PROBLEM STATEMENT

## Simple Explanation
Imagine you want to travel and stay in a unique place — not a regular hotel, but a treehouse, a castle, or a beachfront cottage. How do you find these places? You'd have to search across dozens of websites, compare prices, check photos, and read reviews. There's no single platform that lets you do all of this while also seeing the exact location on a map.

On the other side, imagine you own a beautiful cottage by the lake. How do you reach travelers who might want to stay there? You need a platform where you can create a listing, upload photos, set a price, and receive reviews from past guests.

## Technical Explanation
The problem requires a **multi-tenant web application** with:
- **User management:** Registration, authentication, session persistence
- **Content management:** CRUD operations on listings with associated media (images) and metadata (location, price, category)
- **Social features:** Review/rating system with author attribution
- **Search & discovery:** Text-based search with regex matching, category-based filtering
- **Geospatial features:** Forward geocoding (location string → coordinates) and map rendering
- **Cloud storage:** Scalable image storage separate from the application server
- **Authorization:** Role-based access control (owner-only edit/delete)

## Challenges Faced by Users
| Challenge | Impact |
|-----------|--------|
| Scattered information | Travelers check 5–10 websites to find one stay |
| No location context | Listings without maps make it hard to understand proximity |
| Trust deficit | Without reviews, guests can't verify listing quality |
| No visual richness | Text-only listings fail to convey property appeal |
| No categorization | Users can't quickly browse "Mountains" or "Castles" |
| Owner frustration | Listing a property without a platform limits reach |

## Why Existing Solutions Are Insufficient (In Context of Learning)
While Airbnb exists commercially, building a clone teaches you:
- How authentication and authorization actually work under the hood
- How MVC architecture separates concerns in real applications
- How cloud services (Cloudinary, Mapbox) integrate with backends
- How to handle file uploads in Node.js
- How NoSQL databases model real-world data with references

## Interview Explanation
> "The core problem is connecting travelers with property owners. Travelers need to discover, evaluate (through images, maps, reviews), and compare accommodations. Property owners need to list their properties with rich media and receive guest feedback. I built WanderNest to solve both sides of this marketplace problem while learning production-level web development patterns."

---

# SECTION 3: SOLUTION OVERVIEW

## How the Project Solves the Problem
WanderNest creates a **two-sided marketplace**:
1. **Property Owners** sign up, create listings with title, description, price, location, category, and uploaded image
2. **Travelers** browse listings, filter by category, search by name, view on maps, and leave reviews
3. **The Platform** handles authentication, authorization, data storage, image hosting, and geocoding

## Core Idea
A web application following the **MVC pattern** with **RESTful routing** where:
- **Models** define data structure (Listing, Review, User)
- **Views** render dynamic HTML using EJS templates
- **Controllers** contain business logic
- **Routes** map HTTP methods + URLs to controller actions
- **Middleware** handles cross-cutting concerns (auth, validation, error handling)

## End-to-End User Journey

### Journey 1: Browsing & Reviewing (Traveler)
```
User visits homepage (/listings)
    → Express serves the request
    → listingController.index queries MongoDB for all listings
    → EJS renders index.ejs with listing cards
    → User sees property cards with images, titles, prices
    → User clicks category filter (e.g., "Mountains")
    → Route /listings/category/Mountains queries filtered results
    → User clicks a listing card
    → Route /listings/:id fetches listing with populated reviews & owner
    → show.ejs renders full details, map, reviews
    → User submits a review (rating + comment)
    → POST /listings/:id/reviews creates Review document
    → Review is pushed into Listing.reviews array
    → Page redirects back with flash message "Review Added!"
```

### Journey 2: Creating a Listing (Property Owner)
```
User clicks "Airbnb your home" (navbar)
    → isLoggedIn middleware checks authentication
    → If not logged in → redirect to /login with flash error
    → If logged in → render new.ejs form
    → User fills form: title, description, image file, price, country, location, category
    → Form submits POST /listings (multipart/form-data)
    → Multer middleware processes image upload → sends to Cloudinary
    → Cloudinary returns image URL and filename
    → validateListing middleware validates body using Joi schema
    → geocodingClient converts location string to GeoJSON coordinates
    → New Listing document created with all fields + owner + geometry
    → Saved to MongoDB
    → Redirect to /listings with flash "New Listing Created!"
```

### Journey 3: Authentication Flow
```
New User clicks "Sign up"
    → GET /signup renders signup.ejs form
    → User enters username, email, password
    → POST /signup creates User document
    → passport-local-mongoose hashes password with PBKDF2 + salt
    → Auto-login after registration (req.login)
    → Redirect to /listings with flash "Welcome to Wanderlust"

Returning User clicks "Log in"
    → GET /login renders login.ejs form
    → User enters username + password
    → POST /login → saveRedirectUrl middleware saves original URL
    → passport.authenticate('local') verifies credentials
    → On success → redirect to saved URL or /listings
    → On failure → redirect to /login with flash error
```

## Step-by-Step Data Flow Diagram

```
┌─────────┐     HTTP Request      ┌──────────────┐
│  User's  │ ──────────────────►  │   Express.js  │
│ Browser  │                      │    Server     │
└─────────┘                       └──────┬───────┘
     ▲                                   │
     │                          ┌────────▼────────┐
     │                          │   Middleware     │
     │                          │  Chain:          │
     │                          │  1. Session      │
     │                          │  2. Flash        │
     │                          │  3. Passport     │
     │                          │  4. isLoggedIn   │
     │                          │  5. Multer       │
     │                          │  6. Joi Validate │
     │                          └────────┬────────┘
     │                                   │
     │                          ┌────────▼────────┐
     │                          │   Router        │
     │                          │  /listings      │
     │                          │  /reviews       │
     │                          │  /users         │
     │                          └────────┬────────┘
     │                                   │
     │                          ┌────────▼────────┐
     │                          │  Controller     │
     │                          │  Business Logic │
     │                          └───┬────────┬────┘
     │                              │        │
     │                    ┌─────────▼─┐  ┌───▼──────────┐
     │                    │  Mongoose  │  │  Cloudinary  │
     │                    │  (MongoDB) │  │  (Images)    │
     │                    └─────────┬──┘  └───┬──────────┘
     │                              │         │
     │                    ┌─────────▼─────────▼──┐
     │                    │   Mapbox Geocoding    │
     │                    │   (Coordinates)       │
     │                    └──────────┬────────────┘
     │                               │
     │                    ┌──────────▼────────────┐
     │   HTML Response    │   EJS Template        │
     │◄───────────────────│   Rendering           │
     │                    └───────────────────────┘
```

---

# SECTION 4: TECHNOLOGY STACK

## 4.1 Backend Runtime: Node.js (v18.x)

| Aspect | Details |
|--------|---------|
| **What it is** | A JavaScript runtime built on Chrome's V8 engine that allows running JavaScript on the server |
| **Why it's used** | Enables full-stack JavaScript (same language for frontend and backend), event-driven non-blocking I/O model makes it great for I/O-heavy web applications |
| **Why this project selected it** | Most popular runtime for web apps, huge npm ecosystem, excellent for building REST APIs, great community support |
| **Alternatives** | Python (Django/Flask), Java (Spring Boot), PHP (Laravel), Ruby (Rails), Go |

**Interview Questions:**
- *What is the event loop in Node.js?* — It's the mechanism that handles asynchronous operations. Node.js is single-threaded but uses the event loop to delegate I/O operations to the OS kernel, making it non-blocking.
- *What is the difference between `require()` and `import`?* — `require()` is CommonJS (used in this project), synchronous, works at runtime. `import` is ES Modules, can be async, evaluated at parse time.
- *Why is Node.js good for I/O-heavy applications?* — Its non-blocking event-driven architecture means it doesn't wait for database queries or file reads to complete before handling the next request.

---

## 4.2 Backend Framework: Express.js (v4.21.1)

| Aspect | Details |
|--------|---------|
| **What it is** | A minimal, unopinionated web framework for Node.js that provides routing, middleware, and HTTP utilities |
| **Why it's used** | Simplifies HTTP server creation, provides robust routing, middleware architecture enables modular request processing |
| **Why this project selected it** | De facto standard for Node.js web apps, lightweight, flexible, massive community, works perfectly with EJS and Mongoose |
| **Alternatives** | Fastify, Koa, Hapi, NestJS |

**Key Express concepts used in this project:**
- `app.use()` — Register middleware (session, flash, passport, static files)
- `app.set()` — Configure view engine (EJS) and views directory
- `express.Router()` — Modular routing (listing.js, review.js, user.js)
- `express.urlencoded()` — Parse form data from POST requests
- `express.static()` — Serve static files (CSS, JS) from `/public`

**Interview Questions:**
- *What is middleware in Express?* — Functions that have access to `req`, `res`, and `next`. They execute in order and can modify request/response, end the cycle, or call `next()`.
- *What is the difference between `app.use()` and `app.get()`?* — `app.use()` matches all HTTP methods and any path starting with the given prefix. `app.get()` only matches GET requests to the exact path.
- *How does `express.Router()` help?* — It creates modular route handlers. Each router handles a group of related routes and can be mounted on a path prefix (e.g., `/listings`).

---

## 4.3 Database: MongoDB with Mongoose ODM (v8.8.2)

| Aspect | Details |
|--------|---------|
| **What it is** | MongoDB is a NoSQL document database. Mongoose is an ODM (Object Data Modeling) library for MongoDB in Node.js |
| **Why it's used** | Document-oriented storage fits naturally with JavaScript objects, flexible schema, easy to store nested data (images, GeoJSON), horizontal scaling |
| **Why this project selected it** | Listings have varying attributes, reviews are nested arrays, GeoJSON requires flexible storage, Mongoose provides schema validation and population |
| **Alternatives** | PostgreSQL, MySQL, Firebase Firestore, DynamoDB |

**Why MongoDB over SQL for this project:**
- Listings have **nested objects** (image: {url, filename}, geometry: {type, coordinates})
- Reviews are stored as **arrays of ObjectId references** in listings — natural in MongoDB, requires join tables in SQL
- **Schema flexibility** — not every listing has the same fields
- **GeoJSON support** is built into MongoDB

**Interview Questions:**
- *What is the difference between SQL and NoSQL?* — SQL uses tables with fixed schemas and relationships (joins). NoSQL uses flexible documents (JSON-like) and embeds or references data.
- *What is Mongoose `populate()`?* — It replaces ObjectId references with actual documents from the referenced collection. Like a JOIN in SQL but done at the application level.
- *What is the difference between embedding and referencing in MongoDB?* — Embedding stores related data inside the same document (faster reads, data duplication). Referencing stores an ObjectId pointer to another collection (normalized, avoids duplication but requires populate).

---

## 4.4 Templating Engine: EJS with ejs-mate

| Aspect | Details |
|--------|---------|
| **What it is** | EJS (Embedded JavaScript) generates HTML with JavaScript. ejs-mate adds layout/partial support |
| **Why it's used** | Server-side rendering of dynamic HTML, supports layouts (boilerplate) and includes (navbar, footer, flash) |
| **Why this project selected it** | Simple syntax (`<%= %>`), no learning curve for JavaScript developers, built-in Express support |
| **Alternatives** | Pug (Jade), Handlebars, Nunjucks, React (SSR) |

**EJS Syntax used:**
- `<%= variable %>` — Output escaped HTML (prevents XSS)
- `<%- code %>` — Output unescaped HTML (used for includes and layouts)
- `<% code %>` — Execute JavaScript without output (loops, conditionals)
- `<% layout("./layouts/boilerplate") %>` — ejs-mate layout declaration

---

## 4.5 Authentication: Passport.js with passport-local-mongoose

| Aspect | Details |
|--------|---------|
| **What it is** | Passport.js is authentication middleware for Node.js. passport-local-mongoose is a plugin that simplifies username/password auth with Mongoose |
| **Why it's used** | Handles password hashing (PBKDF2), session serialization/deserialization, provides `authenticate()`, `register()`, `serializeUser()`, `deserializeUser()` methods |
| **Why this project selected it** | Industry standard for Node.js auth, eliminates need to manually handle password hashing, integrates seamlessly with Express sessions |
| **Alternatives** | bcrypt + manual JWT, Auth0, Firebase Auth, OAuth (Google/GitHub login) |

---

## 4.6 Image Upload: Cloudinary + Multer + multer-storage-cloudinary

| Aspect | Details |
|--------|---------|
| **What it is** | Cloudinary is a cloud-based image hosting CDN. Multer handles multipart form data (file uploads). multer-storage-cloudinary connects the two |
| **Why it's used** | Images are stored on Cloudinary's CDN (fast delivery worldwide), not on the server (which would consume disk space and bandwidth) |
| **Why this project selected it** | Free tier available, automatic format optimization, on-the-fly image transformations (resizing in edit page: `h_300,w_250`), CDN delivery |
| **Alternatives** | AWS S3, Firebase Storage, Imgur API, local storage |

---

## 4.7 Maps: Mapbox GL JS + Mapbox Geocoding SDK

| Aspect | Details |
|--------|---------|
| **What it is** | Mapbox GL JS renders interactive maps. The Geocoding SDK converts location names to coordinates (forward geocoding) |
| **Why it's used** | Shows property location on an interactive map, converts "Malibu" → `[-118.7, 34.03]` coordinates |
| **Why this project selected it** | Free tier generous, beautiful map styles, client-side GL rendering, comprehensive geocoding API |
| **Alternatives** | Google Maps API, Leaflet + OpenStreetMap, HERE Maps |

---

## 4.8 Validation: Joi (v17.13.3)

| Aspect | Details |
|--------|---------|
| **What it is** | A schema description language and data validator for JavaScript objects |
| **Why it's used** | Validates request body data before it reaches the database — ensures title is a string, price is a positive number, rating is 1–5 |
| **Why this project selected it** | Declarative validation, excellent error messages, works perfectly with Express middleware |
| **Alternatives** | express-validator, Yup, Zod, manual validation |

---

## 4.9 Session Management: express-session + connect-mongo

| Aspect | Details |
|--------|---------|
| **What it is** | express-session creates sessions. connect-mongo stores them in MongoDB instead of memory |
| **Why it's used** | Sessions persist user login state. Storing in MongoDB means sessions survive server restarts |
| **Why this project selected it** | Already using MongoDB, simple integration, `touchAfter: 24*3600` reduces unnecessary session writes |
| **Alternatives** | Redis (connect-redis), JWT tokens (stateless), PostgreSQL session store |

---

## 4.10 Styling: Bootstrap 5.3.3 + Custom CSS + Font Awesome 6.7.1

| Aspect | Details |
|--------|---------|
| **What it is** | Bootstrap provides responsive UI components. Font Awesome provides icons. Custom CSS adds project-specific styling |
| **Why it's used** | Rapid prototyping with responsive grid, pre-built components (cards, forms, navbar, alerts), icon library |
| **Alternatives** | Tailwind CSS, Material UI, Bulma, Foundation |

---

## 4.11 Other Key Libraries

| Library | Purpose | File Used In |
|---------|---------|-------------|
| `method-override` | Enables PUT/DELETE from HTML forms (forms only support GET/POST) | `app.js` |
| `connect-flash` | Flash messages ("New Listing Created!", "Review Deleted!") | `app.js` |
| `dotenv` | Loads environment variables from `.env` file | `app.js` |
| `cookie-parser` | Parses cookies from request headers | `package.json` (listed) |
| `serverless-http` | Wraps Express app for serverless deployment (Netlify) | `app.js` |

---

# SECTION 5: HIGH LEVEL ARCHITECTURE (HLD)

## System Architecture Diagram (ASCII)

```
                        ┌──────────────────────────────────────┐
                        │         CLIENT (Browser)              │
                        │  ┌─────────────────────────────────┐  │
                        │  │  EJS-Rendered HTML + Bootstrap   │  │
                        │  │  Mapbox GL JS (Interactive Map)  │  │
                        │  │  Client JS (Validation, Tax)     │  │
                        │  └─────────────────────────────────┘  │
                        └───────────────┬──────────────────────┘
                                        │ HTTP Requests
                                        │ (GET, POST, PUT, DELETE)
                                        ▼
┌───────────────────────────────────────────────────────────────────────┐
│                        EXPRESS.JS SERVER (app.js)                     │
│                                                                       │
│  ┌─────────────┐  ┌───────────┐  ┌────────────┐  ┌──────────────┐   │
│  │  Middleware  │  │  Routes   │  │ Controllers│  │    Utils     │   │
│  │  Pipeline   │→ │           │→ │            │  │              │   │
│  │             │  │ listing.js│  │ listings.js│  │ wrapAsync.js │   │
│  │ session     │  │ review.js │  │ reviews.js │  │ ExpressError │   │
│  │ flash       │  │ user.js   │  │ users.js   │  │   .js        │   │
│  │ passport    │  │           │  │            │  │              │   │
│  │ isLoggedIn  │  └───────────┘  └──────┬─────┘  └──────────────┘   │
│  │ isOwner     │                        │                            │
│  │ validateX   │                        ▼                            │
│  │ multer      │              ┌──────────────────┐                   │
│  └─────────────┘              │    Mongoose ODM   │                   │
│                               │    Models:        │                   │
│                               │    - Listing      │                   │
│                               │    - Review       │                   │
│                               │    - User         │                   │
│                               └────────┬─────────┘                   │
└────────────────────────────────────────┼─────────────────────────────┘
                                         │
           ┌─────────────────────────────┼──────────────────────┐
           │                             │                      │
           ▼                             ▼                      ▼
┌────────────────┐           ┌─────────────────┐    ┌─────────────────┐
│  MongoDB Atlas │           │   Cloudinary    │    │  Mapbox APIs    │
│  (Database)    │           │   (Image CDN)   │    │  (Geocoding +   │
│                │           │                 │    │   Map Tiles)    │
│  Collections:  │           │  Folder:        │    │                 │
│  - listings    │           │  Wanderlust_DEV │    │  Forward        │
│  - reviews     │           │                 │    │  Geocoding      │
│  - users       │           │  Formats:       │    │  "Malibu" →     │
│  - sessions    │           │  png, jpeg, jpg │    │  [-118.7, 34.0] │
│                │           │                 │    │                 │
└────────────────┘           └─────────────────┘    └─────────────────┘
```

## Component Interactions Explained

### 1. Client ↔ Server
- Browser sends HTTP requests (GET /listings, POST /listings, PUT /listings/:id, DELETE /listings/:id)
- Server responds with rendered HTML (EJS templates) or redirects
- All communication is synchronous (server-side rendering, not SPA)

### 2. Server ↔ MongoDB Atlas
- Mongoose connects via `ATLASDB_URL` environment variable
- All CRUD operations use Mongoose methods (`find()`, `findById()`, `save()`, `findByIdAndUpdate()`, `findByIdAndDelete()`)
- Sessions are also stored in MongoDB via `connect-mongo`

### 3. Server ↔ Cloudinary
- When user uploads an image, Multer intercepts the file from the multipart form
- `multer-storage-cloudinary` sends the file to Cloudinary's API
- Cloudinary returns a `url` and `filename` stored in the listing document
- Images are served from Cloudinary's CDN (not the server)

### 4. Server ↔ Mapbox
- When creating a listing, the server calls Mapbox Geocoding API with the location string
- Mapbox returns GeoJSON geometry (coordinates)
- Coordinates are stored in the listing's `geometry` field
- Client-side Mapbox GL JS uses these coordinates to render the map

### 5. Request Lifecycle

```
1. Browser sends request (e.g., GET /listings/abc123)
2. Express receives request
3. Session middleware loads/creates session from MongoDB
4. Flash middleware makes flash messages available
5. Passport middleware restores user from session (deserializeUser)
6. Global middleware sets res.locals (success, error, currUser)
7. Router matches /listings/:id → listingRouter
8. Route handler calls listingController.showListing
9. Controller queries MongoDB: Listing.findById(id).populate('reviews').populate('owner')
10. Mongoose returns the document with populated references
11. Controller renders show.ejs with the listing data
12. EJS processes template → generates HTML
13. ejs-mate wraps content in boilerplate.ejs layout
14. HTML sent back to browser
15. Browser renders HTML, loads Mapbox GL JS, renders map
```

---

# SECTION 6: LOW LEVEL DESIGN (LLD)

## 6.1 `app.js` — Application Entry Point
**File:** [app.js](file:///c:/Users/krish/Downloads/Major-Project-main/Major-Project-main/app.js)

| Aspect | Details |
|--------|---------|
| **Purpose** | Main server file — initializes everything, configures middleware, mounts routes, starts listening |
| **Responsibilities** | 1) Load environment variables 2) Connect to MongoDB 3) Configure Express settings 4) Set up session store 5) Initialize Passport 6) Mount route modules 7) Handle 404 and errors 8) Start HTTP server |
| **Dependencies** | express, mongoose, path, method-override, ejs-mate, express-session, connect-mongo, connect-flash, passport, passport-local |

**Line-by-line breakdown of critical sections:**

```javascript
// Lines 1-3: Load .env file only in development (not production where env vars are set by host)
if(process.env.NODE_ENV != "production"){
    require("dotenv").config();
}

// Lines 59-65: Session store in MongoDB (not in-memory)
const store = MongoStore.create({
    mongoUrl: dbUrl,           // Use same DB for sessions
    crypto: { secret: process.env.SECRET }, // Encrypt session data
    touchAfter: 24 * 3600,    // Only update session once per 24 hours (performance)
})

// Lines 71-82: Session configuration
const sessionOptions = {
    store,                     // Use MongoDB store (not default MemoryStore)
    secret: process.env.SECRET,// Sign the session cookie
    resave: false,             // Don't save session if not modified
    saveUninitialized: true,   // Save new sessions even if empty
    cookie: {
        expires: Date.now() + 7*24*60*60*1000,  // 7 days
        maxAge: 7*24*60*60*1000,                 // 7 days
        httpOnly: true,         // Cookie not accessible via client-side JS (XSS protection)
    }
}

// Lines 92-96: Passport setup
passport.use(new LocalStrategy(User.authenticate())); // Use passport-local-mongoose's authenticate
passport.serializeUser(User.serializeUser());    // Store user ID in session
passport.deserializeUser(User.deserializeUser()); // Retrieve user from session

// Lines 98-103: Make flash messages and user available to ALL templates
app.use((req, res, next) => {
    res.locals.success = req.flash("success");
    res.locals.error = req.flash("error");
    res.locals.currUser = req.user; // Passport attaches user to req
    next();
})

// Lines 135-137: Mount route modules with path prefixes
app.use("/listings", listingRouter);
app.use("/listings/:id/reviews", reviewsRouter);
app.use("/", userRouter)

// Lines 143-145: Catch-all 404 handler (runs if no route matched)
app.all("*", (req, res, next) => {
    next(new EpressErr(404, "page not Found"))
})

// Lines 148-152: Global error handler (4 parameters = error middleware)
app.use((err, req, res, next) => {
    let {statusCode=500, message="Something Error"} = err;
    res.status(statusCode).render("error.ejs", {err})
})
```

---

## 6.2 Models (Database Schemas)

### `models/listting.js` — Listing Model
**File:** [listting.js](file:///c:/Users/krish/Downloads/Major-Project-main/Major-Project-main/models/listting.js)

| Aspect | Details |
|--------|---------|
| **Purpose** | Defines the MongoDB schema for accommodation listings |
| **Fields** | title (String, required), description (String), image ({url, filename}), price (Number), location (String), country (String), reviews ([ObjectId → Review]), owner (ObjectId → User), geometry (GeoJSON Point), category (String enum) |
| **Key Design Decision** | Reviews are stored as an **array of ObjectId references** (not embedded) — allows reviews to exist independently and be populated |

**Critical code — the post-delete hook:**
```javascript
// Lines 57-61: Mongoose middleware — when a listing is deleted, also delete all its reviews
listingSchema.post("findOneAndDelete", async(listing) => {
    if (listing){
        await Review.deleteMany({_id: {$in: listing.reviews}});
    }
});
```
**Why this matters:** Without this hook, deleting a listing would leave orphaned review documents in the database. This is called **cascading delete** — similar to `ON DELETE CASCADE` in SQL.

### `models/review.js` — Review Model
**File:** [review.js](file:///c:/Users/krish/Downloads/Major-Project-main/Major-Project-main/models/review.js)

| Aspect | Details |
|--------|---------|
| **Purpose** | Defines the schema for user reviews on listings |
| **Fields** | comment (String), rating (Number, 1-5, required), createdAt (Date, default: now), author (ObjectId → User) |
| **Key Design Decision** | `author` references the User model — enables showing "@username" on reviews |

### `models/user.js` — User Model
**File:** [user.js](file:///c:/Users/krish/Downloads/Major-Project-main/Major-Project-main/models/user.js)

| Aspect | Details |
|--------|---------|
| **Purpose** | Defines user schema with Passport.js plugin |
| **Fields** | email (String, required). Username, hash, salt are auto-added by passport-local-mongoose |
| **Key Design Decision** | Using `passportLocalMongoose` plugin automatically adds: `username` (unique), `hash` (hashed password), `salt`, and methods like `register()`, `authenticate()`, `serializeUser()`, `deserializeUser()` |

---

## 6.3 Controllers (Business Logic)

### `controllers/listings.js`
**File:** [listings.js](file:///c:/Users/krish/Downloads/Major-Project-main/Major-Project-main/controllers/listings.js)

| Function | Purpose | Inputs | Outputs |
|----------|---------|--------|---------|
| `index` | Fetch all listings | None | Renders index.ejs with all listings |
| `renderNewForm` | Show create form | None | Renders new.ejs |
| `showListing` | Show single listing | `req.params.id` | Renders show.ejs with listing, reviews (populated with authors), owner |
| `createListing` | Create new listing | Form body + uploaded file | Geocodes location, saves to DB, redirects |
| `editListing` | Show edit form | `req.params.id` | Renders edit.ejs with listing data + resized image URL |
| `renderUpdateForm` | Update listing | `req.params.id` + form body + optional file | Updates DB, handles optional image replacement |
| `renderDelete` | Delete listing | `req.params.id` | Deletes from DB, triggers cascading review delete |

**Important pattern — nested populate:**
```javascript
// Line 28-33: Populate reviews AND each review's author (nested populate)
const listing = await Listing.findById(id).populate({
    path: "reviews",
    populate: { path: "author" }  // Within each review, also populate the author
}).populate("owner");
```

**Image transformation on edit page:**
```javascript
// Lines 80-81: Cloudinary on-the-fly image transformation
let orignalImageUrl = listing.image.url;
orignalImageUrl = orignalImageUrl.replace("/upload", "/upload/h_300,w_250")
// Transforms: .../upload/v123/image.jpg → .../upload/h_300,w_250/v123/image.jpg
// This tells Cloudinary to serve a 300x250 thumbnail without storing a new image
```

### `controllers/reviews.js`
**File:** [reviews.js](file:///c:/Users/krish/Downloads/Major-Project-main/Major-Project-main/controllers/reviews.js)

| Function | Purpose |
|----------|---------|
| `createReview` | Creates review, sets author to current user, pushes review ID into listing's reviews array, saves both |
| `reviewDelete` | Pulls review ID from listing's reviews array (`$pull` operator), deletes review document |

### `controllers/users.js`
**File:** [users.js](file:///c:/Users/krish/Downloads/Major-Project-main/Major-Project-main/controllers/users.js)

| Function | Purpose |
|----------|---------|
| `signup` | Creates user with `User.register()` (hashes password), auto-logs in with `req.login()` |
| `renderSignupForm` | Renders signup.ejs |
| `renderLoginForm` | Renders login.ejs |
| `login` | Sets flash message, redirects to saved URL or /listings |
| `logout` | Calls `req.logout()`, redirects to /listings |

---

## 6.4 Middleware
**File:** [middileware.js](file:///c:/Users/krish/Downloads/Major-Project-main/Major-Project-main/middileware.js)

| Middleware | Purpose | How It Works |
|-----------|---------|--------------|
| `isLoggedIn` | Ensures user is authenticated | Checks `req.isAuthenticated()` (Passport method). If not, saves original URL in session and redirects to login |
| `saveRedirectUrl` | Preserves redirect URL through Passport | Passport resets session on login, so this middleware copies `req.session.redirectUrl` to `res.locals.redirectUrl` before Passport processes login |
| `isOwner` | Ensures user owns the listing | Fetches listing, compares `listing.owner._id` with `req.user._id` using Mongoose `.equals()` |
| `validateListing` | Validates listing data | Runs `listingSchema.validate(req.body)` using Joi. If errors, throws ExpressError with concatenated messages |
| `validateReview` | Validates review data | Same pattern as validateListing but with reviewSchema |
| `isReviewAuthor` | Ensures user authored the review | Fetches review, compares `review.author` with `req.user._id` |

---

## 6.5 Utilities

### `utils/ExpressError.js`
**File:** [ExpressError.js](file:///c:/Users/krish/Downloads/Major-Project-main/Major-Project-main/utils/ExpressError.js)

```javascript
class EpressErr extends Error {
    constructor(statusCode, message){
        super();
        this.statusCode = statusCode;
        this.message = message;
    }
}
```
**Why it exists:** Native JavaScript errors don't have HTTP status codes. This custom class extends `Error` to include `statusCode`, which the global error handler in app.js uses to send the correct HTTP status.

### `utils/wrapAsync.js`
**File:** [wrapAsync.js](file:///c:/Users/krish/Downloads/Major-Project-main/Major-Project-main/utils/wrapAsync.js)

```javascript
module.exports = function wrapAsync(fn) {
    return function(req, res, next) {
        fn(req, res, next).catch(next);
    }
}
```
**Why it exists:** Async route handlers that reject (throw errors) won't be caught by Express's error handler unless you use try-catch in every handler. `wrapAsync` wraps the async function and automatically catches rejected promises, calling `next(error)` to pass them to the global error handler.

**Analogy:** Think of it as a safety net under a trapeze artist. Without it, if the async function "falls" (throws), the error crashes silently. With it, the error is caught and sent to the error handler.

---

## 6.6 Cloud Configuration
**File:** [cloudConfig.js](file:///c:/Users/krish/Downloads/Major-Project-main/Major-Project-main/cloudConfig.js)

```javascript
const cloudinary = require("cloudinary").v2;
const {CloudinaryStorage} = require("multer-storage-cloudinary")

cloudinary.config({ 
    cloud_name: "db9qk5kxj", 
    api_key: "781469131597199", 
    api_secret: "GJrpQjHHBQyIKm3xPP4LpEj8X-I"
});

const storage = new CloudinaryStorage({
    cloudinary: cloudinary,
    params: {
        folder: 'Wanderlust_DEV',           // All images go to this Cloudinary folder
        allowFormats: ["png", "jpeg", "jpg"] // Only allow these image formats
    },
});
```
**Note:** In production, these credentials should be in environment variables, not hardcoded.

---

## 6.7 Validation Schema
**File:** [schema.js](file:///c:/Users/krish/Downloads/Major-Project-main/Major-Project-main/schema.js)

Two Joi schemas:
- **listingSchema:** Validates title (string, required), description (string, required), location (string, required), country (string, required), price (number, min 0, required), image (string, optional), category (must be one of 10 predefined values)
- **reviewSchema:** Validates rating (number, 1-5, required), comment (string, required)

---

# SECTION 7: COMPLETE FOLDER STRUCTURE EXPLANATION

```
WanderNest/
│
├── 📄 app.js                    ← 🔴 ENTRY POINT: Server setup, middleware, routing
├── 📄 cloudConfig.js            ← ☁️ Cloudinary + Multer storage configuration
├── 📄 middileware.js            ← 🔒 Auth, authorization & validation middleware
├── 📄 schema.js                 ← ✅ Joi validation schemas (listing + review)
├── 📄 package.json              ← 📦 Dependencies and project metadata
├── 📄 package-lock.json         ← 🔐 Exact dependency version lock
├── 📄 netlify.toml              ← 🚀 Netlify deployment configuration
├── 📄 .gitignore                ← 🙈 Files excluded from Git (.env, node_modules)
├── 📄 README.md                 ← 📖 Project documentation
│
├── 📁 models/                   ← 📊 Mongoose schemas (Data Layer)
│   ├── 📄 listting.js           ← Listing schema (title, image, price, geo, reviews, owner)
│   ├── 📄 review.js             ← Review schema (comment, rating, author, createdAt)
│   └── 📄 user.js               ← User schema (email + passport-local-mongoose plugin)
│
├── 📁 controllers/              ← 🧠 Business Logic (Controller Layer)
│   ├── 📄 listings.js           ← CRUD + geocoding for listings
│   ├── 📄 reviews.js            ← Create + delete reviews
│   └── 📄 users.js              ← Signup, login, logout
│
├── 📁 routes/                   ← 🛣️ URL Routing (Route Layer)
│   ├── 📄 listing.js            ← /listings/* routes with multer upload
│   ├── 📄 review.js             ← /listings/:id/reviews/* routes
│   └── 📄 user.js               ← /signup, /login, /logout routes
│
├── 📁 views/                    ← 🎨 EJS Templates (View Layer)
│   ├── 📄 error.ejs             ← Error display page
│   ├── 📁 layouts/
│   │   └── 📄 boilerplate.ejs   ← Master layout (HTML head, body wrapper)
│   ├── 📁 includes/
│   │   ├── 📄 navbar.ejs        ← Navigation bar with search
│   │   ├── 📄 flash.ejs         ← Success/error flash messages
│   │   └── 📄 footer.ejs        ← Footer with social links
│   ├── 📁 listings/
│   │   ├── 📄 index.ejs         ← All listings grid with category filters
│   │   ├── 📄 show.ejs          ← Single listing detail + map + reviews
│   │   ├── 📄 new.ejs           ← Create listing form
│   │   ├── 📄 edit.ejs          ← Edit listing form
│   │   ├── 📄 search.ejs        ← Search results page
│   │   └── 📄 category.ejs      ← Category filter results page
│   └── 📁 users/
│       ├── 📄 login.ejs         ← Login form
│       └── 📄 signup.ejs        ← Signup form
│
├── 📁 public/                   ← 📂 Static Assets (served directly)
│   ├── 📁 css/
│   │   ├── 📄 style.css         ← Main stylesheet (layout, cards, navbar, footer)
│   │   └── 📄 rating.css        ← Star rating component (Starability library)
│   └── 📁 js/
│       ├── 📄 script.js         ← Bootstrap form validation
│       └── 📄 map.js            ← Mapbox map initialization + marker
│
├── 📁 utils/                    ← 🔧 Utility Functions
│   ├── 📄 ExpressError.js       ← Custom error class with statusCode
│   └── 📄 wrapAsync.js          ← Async error catching wrapper
│
├── 📁 init/                     ← 🌱 Database Seeding
│   ├── 📄 index.js              ← Seed script (deletes all, inserts sample data)
│   └── 📄 data.js               ← 28 sample listings with Unsplash images
│
└── 📁 screenshots/              ← 📸 Project screenshots for README
```

### Folder-by-Folder Interview Explanation

| Folder | Interview Explanation |
|--------|----------------------|
| `models/` | "Contains Mongoose schemas that define the structure of my MongoDB documents. I have three models: Listing for accommodations, Review for user feedback, and User for authentication." |
| `controllers/` | "Contains the business logic separated from routing. Following MVC, controllers handle what happens when a request reaches a specific route — querying the database, processing data, and rendering views." |
| `routes/` | "Contains modular Express routers. Each file handles a group of related routes. This keeps app.js clean and follows the Single Responsibility Principle." |
| `views/` | "Contains EJS templates organized by feature. The layouts folder has the master template, includes has reusable partials, listings has all listing-related pages, and users has auth pages." |
| `public/` | "Contains static files served directly to the browser — CSS stylesheets and client-side JavaScript. Express serves these without any processing." |
| `utils/` | "Contains utility functions used across the codebase — a custom error class for consistent error handling and an async wrapper to avoid repetitive try-catch blocks." |
| `init/` | "Contains database seeding scripts. The data.js file has 28 sample listings, and index.js clears the database and inserts them. This is used during development, not in production." |

---

# SECTION 8: CODE FLOW WALKTHROUGH

## Application Boot Sequence

When you run `node app.js`, here's exactly what happens in order:

### Step 1: Environment Variables (Line 1-3)
```
IF not in production → Load .env file into process.env
```
This gives us `ATLASDB_URL`, `SECRET`, `MAP_TOKEN`, etc.

### Step 2: Import Dependencies (Lines 7-32)
All `require()` calls execute synchronously, loading Express, Mongoose, Passport, etc.

### Step 3: MongoDB Connection (Lines 39-50)
```
main() → mongoose.connect(dbUrl) → "connection was successful" or error
```
This establishes a persistent connection pool to MongoDB Atlas. All subsequent Mongoose operations use this connection.

### Step 4: Express Configuration (Lines 52-57)
- Set EJS as the view engine
- Set views directory to `./views`
- Enable URL-encoded body parsing (for form submissions)
- Enable method override (to use PUT/DELETE from HTML forms via `?_method=PUT`)
- Register ejs-mate as the EJS engine (for layouts)
- Serve `/public` as static files

### Step 5: Session Store Setup (Lines 59-82)
MongoDB session store is created with:
- Same MongoDB connection as the app
- Encrypted session data
- Session cookie expires after 7 days
- `httpOnly: true` prevents client-side JavaScript from accessing the cookie

### Step 6: Middleware Registration (Lines 89-103)
In this exact order (order matters!):
1. `session()` — Creates/loads sessions
2. `flash()` — Enables flash messages
3. `passport.initialize()` — Sets up Passport
4. `passport.session()` — Enables persistent login sessions
5. `LocalStrategy` — Tells Passport to use username/password auth
6. Global middleware — Makes `success`, `error`, `currUser` available to all templates

### Step 7: Route Mounting (Lines 135-137)
- All `/listings/*` requests → `listingRouter`
- All `/listings/:id/reviews/*` requests → `reviewsRouter`
- All other requests → `userRouter` (handles `/signup`, `/login`, `/logout`)

### Step 8: Error Handling (Lines 143-152)
- 404 catch-all: Any unmatched route creates a 404 error
- Global error handler: Renders `error.ejs` with the error details

### Step 9: Start Listening (Lines 176-178)
```
app.listen(8080) → "server is listening to port 8080"
```
Server is now ready to accept connections.

---

## Request Handling Example: User Visits a Listing Page

```
GET /listings/65abc123def456
│
├─ 1. Express receives HTTP request
├─ 2. session middleware → loads session from MongoDB (finds cookie)
├─ 3. flash middleware → attaches flash methods to req
├─ 4. passport.initialize() → sets up req.login, req.logout, etc.
├─ 5. passport.session() → calls deserializeUser → loads User from DB
├─ 6. Global middleware → sets res.locals.currUser = loaded user
├─ 7. Router matching:
│     app.use("/listings", listingRouter) → matches "/listings"
│     Remaining path: "/65abc123def456"
│     router.get("/:id", ...) → matches "/:id" → id = "65abc123def456"
│
├─ 8. wrapAsync(listingController.showListing) executes:
│     ├─ Extract id from req.params
│     ├─ Listing.findById(id)
│     │   .populate({path:"reviews", populate:{path:"author"}})
│     │   .populate("owner")
│     ├─ MongoDB query: Find listing, join reviews with their authors, join owner
│     ├─ If listing is null → flash error, redirect to /listings
│     ├─ res.render("./listings/show.ejs", {listing})
│     │   ├─ EJS processes show.ejs
│     │   ├─ layout("./layouts/boilerplate") wraps content
│     │   ├─ Includes: navbar.ejs, flash.ejs, footer.ejs
│     │   ├─ Generates complete HTML with:
│     │   │   - Listing details (title, image, price, location)
│     │   │   - Owner username
│     │   │   - Edit/Delete buttons (if currUser is owner)
│     │   │   - Review form (if currUser exists)
│     │   │   - All reviews with star ratings
│     │   │   - Mapbox map div
│     │   │   - JavaScript: mapToken and listing JSON for map.js
│     │   └─ HTML sent to browser
│     └─ If async error → wrapAsync catches → calls next(error)
│
├─ 9. Browser receives HTML
│     ├─ Renders page
│     ├─ Loads Bootstrap CSS/JS
│     ├─ Loads Mapbox GL CSS/JS
│     ├─ Loads map.js
│     │   ├─ Sets mapboxgl.accessToken = mapToken
│     │   ├─ Creates Map with center = listing.geometry.coordinates
│     │   ├─ Adds red marker at coordinates
│     │   └─ Adds popup: "Exact Location will be Provided after Booking"
│     └─ Loads script.js (form validation)
│
└─ 10. User sees the fully rendered listing page with interactive map
```

---

# SECTION 9: DATABASE DESIGN

## Database: MongoDB Atlas (Cloud-hosted NoSQL)
## ODM: Mongoose v8.8.2
## Database Name: `wanderlust` (from init/index.js connection string)

## Collections Overview

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│    listings      │     │    reviews       │     │     users       │
├─────────────────┤     ├─────────────────┤     ├─────────────────┤
│ _id             │     │ _id             │     │ _id             │
│ title *         │     │ comment         │     │ username *      │
│ description     │◄────│ rating *  (1-5) │     │ email *         │
│ image.url       │refs │ createdAt       │     │ hash            │
│ image.filename  │     │ author ─────────│────►│ salt            │
│ price           │     └─────────────────┘     └─────────────────┘
│ location        │              ▲                       ▲
│ country         │              │ refs                   │ ref
│ reviews[] ──────│──────────────┘                       │
│ owner ──────────│──────────────────────────────────────┘
│ geometry.type   │
│ geometry.coords │
│ category        │
└─────────────────┘

  * = required field
```

## Collection: `listings`

| Field | Type | Required | Description | Example |
|-------|------|----------|-------------|---------|
| `_id` | ObjectId | Auto | MongoDB auto-generated unique identifier | `ObjectId("65abc...")` |
| `title` | String | Yes | Property name | "Cozy Beachfront Cottage" |
| `description` | String | No | Property description | "Escape to this charming..." |
| `image.url` | String | No | Cloudinary image URL | `https://res.cloudinary.com/...` |
| `image.filename` | String | No | Cloudinary filename | "Wanderlust_DEV/abc123" |
| `price` | Number | No | Price per night | 1500 |
| `location` | String | No | City/area name | "Malibu" |
| `country` | String | No | Country name | "United States" |
| `reviews` | [ObjectId] | No | Array of Review references | `[ObjectId("rev1"), ObjectId("rev2")]` |
| `owner` | ObjectId | No | Reference to User who created it | `ObjectId("user1")` |
| `geometry.type` | String | Yes (if geometry exists) | GeoJSON type, must be "Point" | "Point" |
| `geometry.coordinates` | [Number] | Yes (if geometry exists) | [longitude, latitude] | `[-118.7, 34.03]` |
| `category` | String (enum) | No | One of 10 predefined categories | "Mountains" |

**Category Enum Values:** "Trending", "Rooms", "Iconic Cities", "Mountains", "Castles", "Amazing Pools", "Camping", "Farms", "Arctic", "Domes"

## Collection: `reviews`

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `_id` | ObjectId | Auto | Unique identifier |
| `comment` | String | No | Review text |
| `rating` | Number | Yes | 1 to 5 stars |
| `createdAt` | Date | No (default: now) | Timestamp |
| `author` | ObjectId | No | Reference to User |

## Collection: `users`

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `_id` | ObjectId | Auto | Unique identifier |
| `email` | String | Yes | User's email |
| `username` | String | Yes (by plugin) | Unique username (added by passport-local-mongoose) |
| `hash` | String | Auto (by plugin) | PBKDF2 hashed password |
| `salt` | String | Auto (by plugin) | Random salt used in hashing |

## Collection: `sessions` (Auto-created by connect-mongo)

| Field | Type | Description |
|-------|------|-------------|
| `_id` | String | Session ID |
| `expires` | Date | Expiration timestamp |
| `session` | Object | Serialized session data (user ID, flash messages, redirectUrl) |

## Entity Relationship Diagram

```
┌──────────┐        owns         ┌──────────┐       writes       ┌──────────┐
│          │ ◄────────────────── │          │ ──────────────────► │          │
│ Listing  │    1 : many         │   User   │    1 : many        │  Review  │
│          │ ──────────────────► │          │ ◄────────────────── │          │
└──────────┘    has reviews      └──────────┘   author of        └──────────┘
     │                                                                 │
     │              contains (array of ObjectId refs)                  │
     └─────────────────────────────────────────────────────────────────┘
```

**Relationships:**
- **User → Listing:** One-to-Many (a user can own many listings, each listing has one owner)
- **User → Review:** One-to-Many (a user can write many reviews, each review has one author)
- **Listing → Review:** One-to-Many (a listing can have many reviews, stored as array of ObjectId references)

## Interview Questions on Database Design

**Q: Why did you use references instead of embedding reviews inside listings?**
> "I chose references because: (1) Reviews can grow unbounded — embedding could exceed MongoDB's 16MB document limit for popular listings. (2) Reviews need their own lifecycle — they can be independently created and deleted. (3) I need to populate author information from the User collection."

**Q: What happens when you delete a listing that has reviews?**
> "I implemented a Mongoose post-middleware hook on `findOneAndDelete`. After a listing is deleted, the hook automatically calls `Review.deleteMany()` with all the review IDs from the listing's reviews array. This prevents orphaned review documents."

**Q: Why MongoDB over PostgreSQL for this project?**
> "The data model has nested objects (image: {url, filename}), GeoJSON geometry, and variable-length arrays of references — all natural fits for MongoDB's document model. PostgreSQL would require multiple join tables and more complex queries for the same result."

---

# SECTION 10: API DOCUMENTATION

## Listing Routes (`/listings`)

| Method | Route | Purpose | Auth Required | Owner Required | Request Body | Response |
|--------|-------|---------|:-------------:|:--------------:|--------------|----------|
| GET | `/listings` | Show all listings | No | No | — | Renders index.ejs |
| GET | `/listings/new` | Show create form | ✅ | No | — | Renders new.ejs |
| POST | `/listings` | Create new listing | ✅ | No | `listing[title]`, `listing[description]`, `listing[image]` (file), `listing[price]`, `listing[country]`, `listing[location]`, `listing[category]` | Redirect to /listings |
| GET | `/listings/search?query=X` | Search listings by title | No | No | Query param: `query` | Renders search.ejs |
| GET | `/listings/category/:category` | Filter by category | No | No | Param: `category` | Renders category.ejs |
| GET | `/listings/:id` | Show single listing | No | No | Param: `id` | Renders show.ejs |
| GET | `/listings/:id/edit` | Show edit form | ✅ | ✅ | Param: `id` | Renders edit.ejs |
| PUT | `/listings/:id` | Update listing | ✅ | ✅ | Same as POST + optional image | Redirect to /listings/:id |
| DELETE | `/listings/:id` | Delete listing | ✅ | ✅ | Param: `id` | Redirect to /listings |

## Review Routes (`/listings/:id/reviews`)

| Method | Route | Purpose | Auth Required | Author Required | Request Body | Response |
|--------|-------|---------|:-------------:|:---------------:|--------------|----------|
| POST | `/listings/:id/reviews` | Create review | ✅ | No | `review[rating]`, `review[comment]` | Redirect to /listings/:id |
| DELETE | `/listings/:id/reviews/:reviewId` | Delete review | ✅ | ✅ | Params: `id`, `reviewId` | Redirect to /listings/:id |

## User Routes (`/`)

| Method | Route | Purpose | Auth Required | Request Body | Response |
|--------|-------|---------|:-------------:|--------------|----------|
| GET | `/signup` | Show signup form | No | — | Renders signup.ejs |
| POST | `/signup` | Register user | No | `username`, `email`, `password` | Redirect to /listings |
| GET | `/login` | Show login form | No | — | Renders login.ejs |
| POST | `/login` | Log in user | No | `username`, `password` | Redirect to saved URL or /listings |
| GET | `/logout` | Log out user | No | — | Redirect to /listings |

## How Frontend and Backend Communicate

This is a **server-side rendered** application (NOT a SPA/REST API). Communication flow:

1. **HTML Forms** submit data as `application/x-www-form-urlencoded` or `multipart/form-data` (for file uploads)
2. **GET requests** are triggered by links (`<a href="...">`) or form actions with `method="get"`
3. **POST requests** are triggered by form submissions with `method="post"`
4. **PUT/DELETE requests** are simulated using `method-override`:
   - Form has `method="post"` and `action="/listings/:id?_method=PUT"`
   - `method-override("_method")` middleware reads the query parameter and changes the HTTP method

```html
<!-- Example: DELETE request via form -->
<form action="/listings/<%= listing._id %>?_method=DELETE" method="post">
    <button class="btn btn-danger">Delete</button>
</form>
<!-- method-override intercepts this POST and converts it to DELETE -->
```

---

# SECTION 11: AUTHENTICATION & SECURITY

## Authentication Flow Detailed

### Signup Flow
```
1. User visits GET /signup → signup.ejs form rendered
2. User submits form with username, email, password
3. POST /signup → userController.signup:
   a. Destructure {username, email, password} from req.body
   b. Create User document with {email, username} (NO password stored directly)
   c. Call User.register(newUser, password):
      - passport-local-mongoose generates random salt
      - Hashes password with PBKDF2 (512 iterations, sha256)
      - Stores hash and salt in the User document
   d. Call req.login(registeredUser):
      - Passport serializes user ID into session
      - Session is stored in MongoDB
      - Session cookie is sent to browser
   e. Flash "Welcome to Wanderlust" → redirect /listings
4. On error (e.g., duplicate username):
   - Catch block flashes error message
   - Redirect back to /signup
```

### Login Flow
```
1. User visits GET /login → login.ejs form rendered
2. User submits form with username, password
3. POST /login:
   a. saveRedirectUrl middleware:
      - Copies req.session.redirectUrl → res.locals.redirectUrl
      - Why? Passport resets session on login, losing the saved URL
   b. passport.authenticate("local"):
      - Finds User by username in MongoDB
      - Calls User.authenticate(password):
        - Reads stored salt from User document
        - Hashes submitted password with same salt
        - Compares result with stored hash
      - If match → success (req.user = user, session updated)
      - If no match → failure (redirect to /login with flash error)
   c. userController.login:
      - Flash "Welcome back to Wanderlust!"
      - Redirect to res.locals.redirectUrl or /listings
```

### Session Management
```
Session lifecycle:
1. User logs in → Passport calls serializeUser → stores user._id in session
2. Every subsequent request:
   a. express-session reads session cookie from request
   b. Looks up session in MongoDB (connect-mongo)
   c. Passport calls deserializeUser → Mongoose findById(user._id)
   d. Attaches found user to req.user
3. Global middleware sets res.locals.currUser = req.user
4. EJS templates check currUser for conditional rendering
5. Session expires after 7 days (cookie maxAge)
6. connect-mongo touchAfter: 24*3600 → only updates session in DB once per 24 hours
```

### Logout Flow
```
1. User clicks "Log Out" (GET /logout)
2. req.logout() → Passport removes user from session
3. Flash "you are logged out"
4. Redirect to /listings
```

## Password Storage — How It Works

```
User enters: "mypassword123"
         ↓
passport-local-mongoose:
  1. Generate random salt: "a8f3c2..."
  2. Hash = PBKDF2("mypassword123", "a8f3c2...", 512 iterations, sha256)
  3. Store in MongoDB:
     {
       username: "john",
       email: "john@email.com",
       salt: "a8f3c2...",
       hash: "7b3d9e..."   ← NOT the original password
     }

On login:
  1. User enters: "mypassword123"
  2. Retrieve salt from DB: "a8f3c2..."
  3. Hash = PBKDF2("mypassword123", "a8f3c2...", 512 iterations, sha256)
  4. Compare computed hash with stored hash
  5. If match → authenticated!
```

**Key point:** The actual password is NEVER stored. Even if the database is compromised, attackers cannot reverse the hash to get the original password.

## Security Measures Implemented

| Threat | Protection | Implementation |
|--------|------------|----------------|
| **XSS (Cross-Site Scripting)** | EJS `<%= %>` auto-escapes HTML | All user-supplied data (titles, comments) is escaped before rendering |
| **Session Hijacking** | `httpOnly: true` cookie | Client-side JavaScript cannot access the session cookie |
| **CSRF** | (Partially) `method-override` only reads `_method` from query string | Forms use POST method, not directly exposed as API |
| **Password Theft** | PBKDF2 hashing with salt | Passwords are never stored in plaintext |
| **Broken Authentication** | `isLoggedIn` middleware | Protected routes check `req.isAuthenticated()` |
| **Broken Authorization** | `isOwner` / `isReviewAuthor` middleware | Only owners can edit/delete their listings/reviews |
| **Input Injection** | Joi validation | All inputs are validated against defined schemas before processing |
| **Session Fixation** | Passport regenerates session on login | New session created on authentication |
| **MongoDB Injection** | Mongoose parameterized queries | Mongoose sanitizes inputs in query methods |

## Security Threats & How They'd Be Handled

**SQL/NoSQL Injection:**
> This project uses Mongoose, which parameterizes queries internally. When you call `Listing.findById(id)`, Mongoose ensures `id` is treated as a value, not as executable code. However, the search feature uses `$regex` which could be a concern with crafted input.

**XSS (Cross-Site Scripting):**
> EJS's `<%= %>` tag automatically HTML-encodes output. If a user puts `<script>alert('xss')</script>` in a listing title, it renders as text, not executable code. The `<%- %>` tag (unescaped) is only used for trusted content like includes.

**CSRF (Cross-Site Request Forgery):**
> This project does NOT implement CSRF tokens. In a production app, you'd add the `csurf` middleware to generate and validate CSRF tokens in forms.

---

# SECTION 12: PROJECT FEATURES

## Feature 1: User Registration & Authentication

**User Perspective:** Sign up with username, email, and password. Log in to access protected features. Log out when done.

**Technical Perspective:**
- Passport.js Local Strategy with passport-local-mongoose plugin
- PBKDF2 password hashing with random salt
- Session-based authentication stored in MongoDB
- Flash messages for success/error feedback

**Backend Logic:**
- `User.register(newUser, password)` → hashes password, saves to DB
- `passport.authenticate('local')` → verifies credentials
- `req.login()` → creates session
- `req.logout()` → destroys session

**Database Impact:** Creates document in `users` collection with username, email, hash, salt.

---

## Feature 2: CRUD Listings

**User Perspective:** Create a new listing with title, description, image, price, location, country, and category. View listings. Edit your own listings. Delete your own listings.

**Technical Perspective:**
- Full CRUD with RESTful routing
- Image upload to Cloudinary via Multer
- Forward geocoding via Mapbox (location → coordinates)
- Authorization middleware (isOwner) for edit/delete

**Backend Logic:**
- **Create:** Multer processes file → Cloudinary stores it → Geocoding API resolves coordinates → Mongoose saves document
- **Read:** `Listing.find({})` for all, `Listing.findById(id).populate()` for single
- **Update:** `Listing.findByIdAndUpdate()` + optional image replacement
- **Delete:** `Listing.findByIdAndDelete()` → triggers post-hook to delete associated reviews

**Database Impact:** CRUD operations on `listings` collection. Delete cascades to `reviews`.

---

## Feature 3: Star-Rated Reviews

**User Perspective:** Logged-in users can leave 1–5 star ratings with comments on any listing. Reviews show the author's username.

**Technical Perspective:**
- Starability CSS library for star rating UI
- Joi validation ensures rating is 1–5
- Review references are pushed into listing's reviews array
- Nested population: listing → reviews → review.author

**Backend Logic:**
```javascript
// Create: Two saves required (review + listing)
let newReview = new Review(req.body.review);
newReview.author = req.user._id;
listings.reviews.push(newReview);
await newReview.save();
await listings.save();

// Delete: Pull from array + delete document
await Listing.findByIdAndUpdate(id, { $pull: { reviews: reviewId } });
await Review.findByIdAndDelete(reviewId);
```

---

## Feature 4: Category Filtering

**User Perspective:** Browse listings by 10 categories: Trending, Rooms, Iconic Cities, Mountains, Castles, Amazing Pools, Camping, Farms, Arctic, Domes.

**Technical Perspective:**
- Category icons rendered using Font Awesome in index.ejs
- GET `/listings/category/:category` queries `Listing.find({ category: category })`
- Category is an enum field in the Listing schema

---

## Feature 5: Search Functionality

**User Perspective:** Type a destination name in the search bar and see matching listings.

**Technical Perspective:**
- GET `/listings/search?query=X`
- MongoDB regex search: `Listing.find({ title: { $regex: query, $options: "i" } })`
- `$options: "i"` makes it case-insensitive

---

## Feature 6: Interactive Maps

**User Perspective:** Each listing shows a Mapbox map with a red pin showing the property location. Clicking the pin shows a popup.

**Technical Perspective:**
- On listing creation: Mapbox Geocoding SDK converts location string to GeoJSON coordinates
- Coordinates stored as `geometry: { type: "Point", coordinates: [lng, lat] }` in MongoDB
- Client-side: `map.js` initializes Mapbox GL map centered on coordinates, adds marker with popup

---

## Feature 7: Cloud Image Upload

**User Perspective:** Upload property images when creating or editing a listing.

**Technical Perspective:**
- HTML form uses `enctype="multipart/form-data"`
- Multer intercepts file, multer-storage-cloudinary sends it to Cloudinary
- Cloudinary returns URL and filename, stored in listing document
- Edit page shows resized preview using Cloudinary's on-the-fly transformation (`h_300,w_250`)

---

## Feature 8: Tax Toggle Display

**User Perspective:** Toggle switch on homepage to show/hide "+18% GST" next to prices.

**Technical Perspective:**
- Client-side JavaScript toggles `.tax-info` element visibility
- No API call — purely frontend toggle using `display: inline/none`

---

## Feature 9: Flash Messages

**User Perspective:** Green success alerts ("New Listing Created!") and red error alerts ("You are not the owner") appear after actions.

**Technical Perspective:**
- `connect-flash` middleware stores messages in session
- Global middleware in app.js passes `req.flash("success")` and `req.flash("error")` to all templates via `res.locals`
- `flash.ejs` include renders Bootstrap alerts with dismiss buttons

---

## Feature 10: Responsive Design

**User Perspective:** The application works on desktop, tablet, and mobile screens.

**Technical Perspective:**
- Bootstrap 5 responsive grid (`row-cols-lg-3 row-cols-md-2 row-cols-sm-1`)
- Custom media queries in navbar, search bar, and card layouts
- Navbar collapses to hamburger menu on mobile

---

# SECTION 13: INTERVIEW STORY

## 1. "Tell me about your project."

> "I built WanderNest, a full-stack travel accommodation platform similar to Airbnb. Users can discover unique stays like beachfront cottages, mountain cabins, and castles. Property owners can create listings with images, location maps, and pricing. I implemented user authentication with Passport.js, cloud image uploads with Cloudinary, interactive maps with Mapbox, and a review system with star ratings. The entire application follows the MVC architecture with Node.js, Express, MongoDB, and EJS."

## 2. "Why did you build it?"

> "I wanted to challenge myself with a real-world, full-stack project that covers all the concepts asked in interviews — REST APIs, authentication, database design, file uploads, error handling, and MVC architecture. Building an Airbnb-like platform gave me exposure to all these concepts in a single project. It also taught me how cloud services like Cloudinary and Mapbox integrate with backend applications."

## 3. "What problem does it solve?"

> "It connects travelers with unique accommodation options. Travelers can browse, search, and filter properties by category, view them on maps, and read reviews. Property owners can create listings with rich media. The review system builds trust between strangers. Without such a platform, travelers would have to search across multiple websites, and property owners would have limited reach."

## 4. "What was your biggest challenge?"

> "The biggest challenge was implementing the file upload pipeline with Cloudinary. I had to understand multipart form data, configure Multer as middleware to handle file streams, set up multer-storage-cloudinary as a custom storage engine, and handle cases where the image upload might fail. I also had to implement image replacement on edit — where a new upload should replace the old image's URL and filename in the database, but only if the user actually uploaded a new file."

## 5. "What was your biggest bug?"

> "I had a tricky bug with session management and Passport. After login, users weren't being redirected to the page they originally wanted to visit. The issue was that Passport.js resets the session during authentication, so the `redirectUrl` I stored in the session was being lost. I solved it by creating a `saveRedirectUrl` middleware that copies the URL from `req.session` to `res.locals` before Passport processes the login. Since `res.locals` persists through the request lifecycle, the URL was available after authentication."

## 6. "What did you learn?"

> "I learned five major things: (1) How the MVC pattern organizes code in real applications, (2) How session-based authentication actually works under the hood — cookies, serialization, hashing, (3) How middleware chains process requests in order and how each middleware can modify the request or short-circuit it, (4) How NoSQL databases model relationships using references and population, and (5) How to integrate third-party services (Cloudinary, Mapbox) into a Node.js application."

## 7. "What was your team size?"

> "I built this project individually. I was responsible for the entire development lifecycle — database design, backend development, frontend templating, authentication, cloud service integration, and testing."

## 8. "What was your contribution?"

> "As a solo developer, I handled everything: designed the database schema with three collections (Listings, Reviews, Users) and their relationships, built the Express server with modular routing and middleware, implemented Passport.js authentication with session management, integrated Cloudinary for image hosting and Mapbox for geocoding and maps, created responsive EJS templates with Bootstrap, and implemented custom error handling with ExpressError and wrapAsync utilities."

## 9. "Why this tech stack?"

> "I chose MongoDB because accommodation data has nested objects (images, GeoJSON coordinates) and variable-length arrays (reviews) — perfect for a document database. Express.js for its middleware architecture and routing simplicity. EJS for server-side rendering because this project doesn't need the complexity of a frontend framework. Passport.js because it's the standard Node.js authentication library. Cloudinary because images shouldn't be stored on the application server — CDN delivery is faster and more scalable. Mapbox because it provides both geocoding API and client-side map rendering in one service."

## 10. "What's the future scope?"

> "I'd add real-time features like a chat between hosts and guests using Socket.io, implement payment processing with Razorpay or Stripe, add an admin dashboard for managing listings and users, implement email notifications for bookings and reviews, add pagination and infinite scroll for better performance with large datasets, and migrate to React for the frontend to create a more dynamic single-page experience."

---

# SECTION 14: FREQUENTLY ASKED INTERVIEW QUESTIONS

## Easy Level (1–15)

**Q1: What is MVC architecture?**
> MVC stands for Model-View-Controller. **Model** handles data and business logic (Mongoose schemas). **View** handles the UI (EJS templates). **Controller** handles user input and coordinates between Model and View. In WanderNest, models/ has Mongoose schemas, views/ has EJS templates, and controllers/ has the business logic.

**Q2: What is REST API?**
> REST (Representational State Transfer) is an architectural style for APIs that uses standard HTTP methods. GET for reading, POST for creating, PUT for updating, DELETE for deleting. My routes follow REST conventions — e.g., `GET /listings` (index), `POST /listings` (create), `GET /listings/:id` (show), `PUT /listings/:id` (update), `DELETE /listings/:id` (destroy).

**Q3: What is middleware in Express?**
> Functions that execute between receiving a request and sending a response. They have access to `req`, `res`, and `next()`. In my project, `isLoggedIn` checks if user is authenticated, `isOwner` checks if user owns the listing, `validateListing` validates form data with Joi.

**Q4: What is Mongoose?**
> Mongoose is an ODM (Object Data Modeling) library for MongoDB in Node.js. It provides schema validation, type casting, query building, and methods like `find()`, `populate()`, `save()`. I used it to define schemas for Listing, Review, and User.

**Q5: What is the difference between `find()` and `findById()`?**
> `find({})` returns all documents matching a filter (array). `findById(id)` returns a single document by its `_id`. I use `find({})` on the index page and `findById(id)` on the show page.

**Q6: What is EJS?**
> EJS (Embedded JavaScript) is a templating engine that generates HTML with JavaScript. `<%= var %>` outputs escaped values, `<%- code %>` outputs unescaped HTML, `<% code %>` executes JS without output. I used it with ejs-mate for layout support.

**Q7: What is `method-override`?**
> HTML forms only support GET and POST methods. `method-override` allows using PUT and DELETE by adding `?_method=PUT` or `?_method=DELETE` to the form action URL. The middleware reads this query parameter and changes the request method.

**Q8: What is `.env` and `dotenv`?**
> `.env` is a file storing environment variables (database URL, API keys, secrets). `dotenv` loads these into `process.env`. This keeps sensitive data out of source code. `.env` is listed in `.gitignore` to prevent committing secrets.

**Q9: What is `connect-flash`?**
> A middleware that stores temporary messages in the session. After a redirect, the flash message is displayed once and then removed. I use it for "New Listing Created!", "Review Deleted!", etc.

**Q10: What does `express.urlencoded({extended: true})` do?**
> It parses incoming request bodies with URL-encoded payloads (from HTML forms). `extended: true` allows nested objects like `listing[title]`, which becomes `req.body.listing.title`.

**Q11: What is `express.static()`?**
> Middleware that serves static files (CSS, JS, images) from a directory. `app.use(express.static(path.join(__dirname, "/public")))` serves files from the `/public` folder.

**Q12: What is a session?**
> A way to persist user data across multiple HTTP requests. Express-session creates a unique session ID, stores it as a cookie in the browser, and keeps session data in a store (MongoDB in my case).

**Q13: What is the difference between cookie and session?**
> A **cookie** is stored in the browser and sent with every request. A **session** stores data on the server, linked to a cookie that contains only the session ID. Sessions are more secure because sensitive data stays on the server.

**Q14: What is `res.locals`?**
> An object that makes variables available to templates within a single request-response cycle. I use it to make `currUser`, `success`, and `error` available to all EJS templates.

**Q15: What does `module.exports` do?**
> It makes functions, objects, or classes available to other files via `require()`. Every model, controller, route, and utility in my project uses `module.exports`.

---

## Medium Level (16–35)

**Q16: Explain the Passport.js authentication flow in your project.**
> Passport uses the Local Strategy with passport-local-mongoose. On signup, `User.register()` hashes the password with PBKDF2 and salt. On login, `passport.authenticate('local')` retrieves the user, hashes the submitted password with the stored salt, and compares. `serializeUser` stores user ID in session. `deserializeUser` retrieves the full user on each request.

**Q17: How does file upload work in your project?**
> The form uses `enctype="multipart/form-data"`. Multer middleware intercepts the file from the request. `multer-storage-cloudinary` is configured as the storage engine, which streams the file to Cloudinary's API. Cloudinary returns a URL and filename, which I store in the listing's `image` field.

**Q18: What is `populate()` in Mongoose?**
> `populate()` replaces ObjectId references with actual documents from the referenced collection. In `Listing.findById(id).populate('reviews').populate('owner')`, MongoDB replaces the review ObjectIds with full review documents and the owner ObjectId with the full user document. It's like a JOIN in SQL.

**Q19: What is nested populate and when did you use it?**
> Nested populate populates a reference within an already populated document. I used `populate({path: "reviews", populate: {path: "author"}})` to first populate the reviews array, then within each review, populate the author field. This gives me the full user object for each reviewer.

**Q20: Explain the `wrapAsync` utility.**
> It's a higher-order function that wraps async route handlers. Without it, if an async function rejects (throws an error), Express won't catch it — the error is lost. `wrapAsync` wraps the function in a new function that catches rejected promises and passes the error to Express's global error handler via `next(error)`.

**Q21: How does the `isOwner` middleware work?**
> It extracts the listing ID from `req.params`, fetches the listing from MongoDB, and compares `listing.owner._id` with `res.locals.currUser._id` using Mongoose's `.equals()` method (not `===`, because ObjectIds are objects). If they don't match, it flashes an error and redirects.

**Q22: What is GeoJSON and how is it used?**
> GeoJSON is a format for encoding geographic data. In my listing schema, `geometry` follows the GeoJSON Point format: `{type: "Point", coordinates: [longitude, latitude]}`. Mapbox Geocoding API returns coordinates in this format, and Mapbox GL JS uses them to render markers on maps.

**Q23: How does category filtering work?**
> The Listing schema has a `category` field with an enum of 10 values. When a user clicks a category icon, it triggers `GET /listings/category/:category`. The route handler queries `Listing.find({ category: category })`, which returns only listings matching that category.

**Q24: How does search work?**
> The search form submits `GET /listings/search?query=X`. The handler extracts `req.query.query` and uses MongoDB regex: `Listing.find({ title: { $regex: query, $options: "i" } })`. `$regex` matches partial strings and `$options: "i"` makes it case-insensitive.

**Q25: What is `connect-mongo` and why is it needed?**
> `connect-mongo` stores Express sessions in MongoDB instead of the default in-memory store. The default `MemoryStore` loses all sessions on server restart and doesn't scale (each server instance has its own memory). MongoDB storage persists sessions across restarts and works with multiple server instances.

**Q26: Explain the `saveRedirectUrl` middleware.**
> When a non-authenticated user tries to access a protected route (like `/listings/new`), `isLoggedIn` saves `req.originalUrl` in `req.session.redirectUrl`. However, Passport.js resets the session during login. `saveRedirectUrl` runs before `passport.authenticate` and copies `req.session.redirectUrl` to `res.locals.redirectUrl`, which persists through the request lifecycle.

**Q27: What is `$pull` in MongoDB?**
> `$pull` is an update operator that removes all instances of a value from an array. When deleting a review, I use `Listing.findByIdAndUpdate(id, { $pull: { reviews: reviewId } })` to remove the review's ObjectId from the listing's reviews array.

**Q28: How does Cloudinary image transformation work in the edit page?**
> I use Cloudinary's URL-based transformations. By inserting `h_300,w_250` after `/upload/` in the URL: `url.replace("/upload", "/upload/h_300,w_250")`. Cloudinary dynamically generates a 300x250 resized version without creating a new image. The original image remains unchanged.

**Q29: What is the `touchAfter` option in connect-mongo?**
> `touchAfter: 24 * 3600` tells connect-mongo to only update the session in MongoDB once every 24 hours, even if the user makes many requests. This reduces database write operations and improves performance. The session is only "touched" (updated) when its data actually changes.

**Q30: Explain Express error handling in your project.**
> Three layers: (1) `wrapAsync` catches rejected promises in async handlers and calls `next(error)`. (2) `EpressErr` custom error class adds `statusCode` to errors. (3) The global error handler `app.use((err, req, res, next) => {...})` catches all errors, extracts status code (default 500), and renders `error.ejs`.

**Q31: What are the differences between `app.use()`, `app.get()`, `app.post()`?**
> `app.use()` matches ALL HTTP methods and any path starting with the given prefix. `app.get()` matches only GET requests to the exact path. `app.post()` matches only POST requests. `app.use()` is for middleware; `app.get()/post()` are for route handlers.

**Q32: Why use `router.route()` in your routes?**
> `router.route("/")` chains multiple HTTP method handlers for the same path. Instead of writing `router.get("/", ...)` and `router.post("/", ...)` separately, I write `router.route("/").get(...).post(...)`. This is cleaner and clearly shows all operations available on a single path.

**Q33: What is `mergeParams: true` in Express Router?**
> When a router is mounted with a path that has parameters (like `/listings/:id/reviews`), the inner router doesn't have access to `:id` by default. `mergeParams: true` merges the parent's `req.params` with the child router's params, making `req.params.id` accessible in the review router.

**Q34: What is `enctype="multipart/form-data"`?**
> It's an encoding type for HTML forms that tells the browser to send form data in multiple parts. Regular forms use `application/x-www-form-urlencoded` which can't handle file uploads. `multipart/form-data` splits the form data into separate parts, allowing file binary data to be sent alongside text fields.

**Q35: How does the cascading delete work for listings and reviews?**
> I used a Mongoose `post` middleware on `findOneAndDelete`. When `Listing.findByIdAndDelete(id)` is called, after the listing is deleted, the middleware triggers `Review.deleteMany({_id: {$in: listing.reviews}})`. This deletes all reviews whose IDs are in the listing's reviews array, preventing orphaned documents.

---

## Advanced Fresher Level (36–55)

**Q36: How would you implement pagination in this project?**
> Use MongoDB's `skip()` and `limit()`: `Listing.find({}).skip((page-1)*perPage).limit(perPage)`. Pass page number as query param. Add "Previous" and "Next" buttons in the EJS template. Also add a count query for total pages.

**Q37: Why did you store image URLs instead of image files in MongoDB?**
> MongoDB has a 16MB document size limit. Storing binary images would quickly exceed this. Instead, images are stored on Cloudinary (a CDN optimized for media delivery), and only the URL is stored in MongoDB. This also gives better performance since images are served from edge servers closest to the user.

**Q38: What's the difference between `findByIdAndUpdate()` and `save()`?**
> `findByIdAndUpdate()` sends an update query directly to MongoDB (one database call). `save()` first loads the document into memory, modifies it, then saves back (two database calls). `findByIdAndUpdate()` is more efficient but doesn't trigger all Mongoose middleware. `save()` triggers pre/post save hooks.

**Q39: How would you handle concurrent edit conflicts?**
> Implement optimistic concurrency control using a version field. Mongoose has built-in `__v` version field. On update, check if `__v` matches what the user loaded. If it's changed (someone else edited), reject the update and ask the user to reload.

**Q40: What happens if Cloudinary is down when a user creates a listing?**
> The Multer middleware would throw an error when trying to upload. The `wrapAsync` wrapper would catch this error and pass it to the global error handler, which renders `error.ejs` with a "Something Error" message. The listing would NOT be created (the Cloudinary upload happens before the listing is saved to MongoDB).

**Q41: How would you add real-time notifications?**
> Implement Socket.io alongside Express. When a user creates a review, emit an event to the listing owner's socket room. The frontend listens for this event and shows a notification toast. Store notification history in a separate MongoDB collection.

**Q42: Explain the differences between authentication and authorization.**
> **Authentication** = verifying WHO the user is (login/signup). **Authorization** = verifying WHAT the user can do (isOwner, isReviewAuthor). In my project, Passport handles authentication, and custom middleware handles authorization.

**Q43: How would you implement rate limiting?**
> Use `express-rate-limit` middleware. Example: `{ windowMs: 15*60*1000, max: 100 }` limits each IP to 100 requests per 15 minutes. Apply globally or to specific routes (like login to prevent brute-force).

**Q44: What are the drawbacks of session-based authentication?**
> (1) Sessions are stored server-side — memory/DB overhead. (2) Harder to scale across multiple servers without shared session store. (3) Vulnerable to CSRF attacks. (4) Session cookie must be sent with every request. Alternative: JWT tokens (stateless, no server storage) but have their own tradeoffs (can't revoke, larger request size).

**Q45: How does Mongoose schema validation differ from Joi validation?**
> Mongoose validation runs when saving/updating documents (database level). Joi validation runs on incoming request data (application level). I use BOTH — Joi validates request body before it reaches the controller, Mongoose validates before data is saved to MongoDB. This provides defense in depth.

**Q46: What is the N+1 query problem and does your project have it?**
> N+1 problem: Loading a list of items, then making N additional queries to load related data. My index page could have this — loading all listings, then individually loading reviews for each. I avoid it on the index page by not showing reviews there. On the show page, I use `populate()` which does at most 2 queries (1 for the listing, 1 for reviews).

**Q47: How would you add image compression before upload?**
> Use the `sharp` library as middleware before Multer. Process the file stream: resize, compress, convert format. Or configure Cloudinary transformations in the upload params (e.g., `transformation: [{quality: 'auto', fetch_format: 'auto'}]`).

**Q48: What is the significance of `express.Router()` over putting routes in app.js?**
> Separation of concerns. Without Router, all routes would be in app.js (hundreds of lines). With Router, each feature area (listings, reviews, users) has its own file. This makes the code maintainable, testable, and easier for multiple developers to work on simultaneously.

**Q49: How does the form validation in script.js work?**
> It uses Bootstrap's custom validation. On form submit, it checks `form.checkValidity()` which validates HTML5 attributes (required, min, max, type). If invalid, it prevents submission and adds `was-validated` class, which triggers Bootstrap's CSS to show invalid-feedback messages.

**Q50: What happens if two users try to delete the same listing simultaneously?**
> The first `findByIdAndDelete` succeeds and triggers the post middleware (deleting reviews). The second call returns `null` (listing already deleted). Since the show page checks `if(!listing)` and redirects, the second user would be redirected with an error flash.

**Q51: How would you implement image upload progress tracking?**
> On the frontend, use XMLHttpRequest or Fetch with a progress event listener. Display a progress bar. On the backend, Multer streams the file — you could use a progress stream to emit percentage. For Cloudinary, their SDK has upload progress callbacks.

**Q52: What is the purpose of `express-session`'s `resave: false`?**
> `resave: false` prevents the session from being saved to the store on every request if it wasn't modified. This reduces unnecessary database writes. `saveUninitialized: true` saves new (but empty) sessions.

**Q53: How would you handle MongoDB connection failures gracefully?**
> Add retry logic with exponential backoff: try connecting, wait 1s, try again, wait 2s, etc. Set `serverSelectionTimeoutMS` in Mongoose options. Implement a health check endpoint that returns connection status. Use Mongoose connection events (`mongoose.connection.on('error', ...)`) to log and alert.

**Q54: What is the security risk in cloudConfig.js?**
> API credentials are hardcoded instead of being in environment variables. If this file is committed to a public Git repository, anyone can access the Cloudinary account, upload/delete images, or run up charges. These should be `process.env.CLOUDINARY_CLOUD_NAME`, etc.

**Q55: How would you implement role-based access control (RBAC)?**
> Add a `role` field to the User schema with enum values like 'user', 'host', 'admin'. Create middleware for each role: `isAdmin` checks `req.user.role === 'admin'`. Apply to routes: admin routes for managing all listings, host routes for managing own listings.

---

# SECTION 15: DEEP DIVE FOLLOW-UP QUESTIONS

These are the "What if?" questions a product company interviewer would ask:

**1. "Why did you choose MongoDB over PostgreSQL?"**
> MongoDB fits because listings have nested objects (image: {url, filename}), GeoJSON geometry (coordinates), and variable-length review arrays. This maps naturally to documents. PostgreSQL would need separate tables for images, coordinates, and reviews with JOIN queries.

**2. "What if a user uploads a 50MB image?"**
> Currently there's no size limit. I'd add `limits: { fileSize: 5 * 1024 * 1024 }` to Multer config to reject files over 5MB. Show a user-friendly error. Cloudinary also has plan-based limits.

**3. "How would you handle 1000 reviews on a single listing?"**
> Implement pagination for reviews. Use `Listing.findById(id).populate({path: 'reviews', options: {skip: (page-1)*10, limit: 10}})`. Add "Load More" button or infinite scroll.

**4. "What if someone submits a review with malicious HTML?"**
> EJS's `<%= %>` tag auto-escapes HTML, so `<script>` tags render as text. For extra safety, I could use a sanitization library like `sanitize-html` before saving to the database.

**5. "Why server-side rendering? Why not React?"**
> SSR with EJS is simpler for a CRUD app with few interactive elements. It provides better SEO (search engines can crawl rendered HTML), faster first paint, and doesn't require a build step. React would be better if the app had complex client-side state or real-time features.

**6. "How would you implement booking/payment?"**
> Add a Booking model with fields: listingId, userId, startDate, endDate, totalPrice, status. Integrate Razorpay/Stripe for payment processing. Create a checkout flow: select dates → calculate price → payment → confirm booking.

**7. "What if your Mapbox API key gets exposed?"**
> Restrict the API key by domain in the Mapbox dashboard. Use environment variables instead of hardcoding. For public map rendering (client-side), use a separate restricted token with only Map Tiles scope.

**8. "How would you test this application?"**
> Unit tests for middleware and utility functions using Jest/Mocha. Integration tests for routes using Supertest. E2E tests using Cypress. Test database using a separate MongoDB instance.

**9. "What is the biggest performance bottleneck?"**
> The index page loads ALL listings from MongoDB without pagination. With thousands of listings, this would be slow. Solution: pagination, lazy loading, and caching.

**10. "How would you deploy this to production?"**
> Deploy to a platform like Render, Railway, or AWS EC2. Use environment variables for secrets. Enable MongoDB Atlas VPC peering. Set up a reverse proxy with Nginx. Add SSL/TLS certificates. Implement CI/CD with GitHub Actions.

**11. "What if two users create listings with the same title?"**
> Currently allowed. Titles aren't unique in the schema. If uniqueness is needed, add `unique: true` to the title field or add a slug field.

**12. "How do you handle form validation on the client and server?"**
> Client-side: Bootstrap's `checkValidity()` in script.js checks HTML5 attributes. Server-side: Joi schemas validate the request body. Both are needed — client-side for UX, server-side for security (client-side can be bypassed).

**13. "What happens to reviews when a listing is deleted?"**
> The Mongoose `post('findOneAndDelete')` middleware triggers `Review.deleteMany()` for all reviews referenced in the listing. This is cascading delete — similar to SQL's ON DELETE CASCADE.

**14. "How would you add sorting (by price, by date)?"**
> Add `sort` query parameter: `GET /listings?sort=price_asc`. In the controller: `Listing.find({}).sort({price: 1})` for ascending or `{price: -1}` for descending. Add sort buttons/dropdown in the UI.

**15. "What's the difference between `req.user` and `res.locals.currUser`?"**
> `req.user` is set by Passport after deserializing the user from session. `res.locals.currUser` is set by my global middleware — it's the same user object but made available to EJS templates. Templates can't access `req.user` directly; they access `res.locals`.

**16. "How would you implement email verification?"**
> On signup, send a verification email with a unique token (using `crypto.randomBytes()`). Store the token and expiry in the User document. Create a verification route `GET /verify/:token`. On access, mark the user as verified. Don't allow login until verified.

**17. "What if MongoDB Atlas goes down?"**
> Mongoose would throw connection errors. Add retry logic and a health check endpoint. For high availability, use MongoDB Atlas with multi-region replication. Implement a circuit breaker pattern to gracefully degrade.

**18. "Why use `passport-local-mongoose` instead of bcrypt directly?"**
> `passport-local-mongoose` provides a complete solution: `register()`, `authenticate()`, `serializeUser()`, `deserializeUser()`, automatic username uniqueness. Using bcrypt directly would require writing all this manually. The tradeoff is less control but faster development.

**19. "How would you add an admin dashboard?"**
> Add `role: {type: String, enum: ['user', 'admin'], default: 'user'}` to User schema. Create `isAdmin` middleware. Build admin routes: `GET /admin/listings` (all listings with edit/delete), `GET /admin/users` (manage users), `GET /admin/reviews` (moderate reviews).

**20. "What happens if the geocoding API returns no results?"**
> Currently, `response.body.features[0]` would throw `TypeError: Cannot read property of undefined`. I should add a check: `if (!response.body.features.length) { throw new ExpressError(400, 'Location not found') }`.

**21. "How would you implement user profiles?"**
> Add fields to User schema: `profileImage`, `bio`, `listings` (array of owned listing IDs). Create profile routes: `GET /users/:id`, `PUT /users/:id/edit`. Show user's listings and reviews on their profile page.

**22. "What is the Express middleware execution order in your app?"**
> session → flash → passport.initialize() → passport.session() → global res.locals → route-specific middleware (isLoggedIn → multer → validateListing → isOwner) → controller → EJS render → global error handler.

**23. "How do you prevent brute-force login attacks?"**
> I don't currently. I would add `express-rate-limit` to the login route, limiting to 5 attempts per 15 minutes per IP. Passport-local-mongoose also has a `maxAttempts` option that locks accounts after N failed attempts.

**24. "What is the `__v` field in MongoDB documents?"**
> Mongoose's version key. It's incremented on each `save()` call. Used for optimistic concurrency control. If two users load the same document, the second `save()` would detect a version mismatch and throw a VersionError.

**25. "How would you make the search more powerful?"**
> Currently searches only titles with regex. I would: (1) Search across title, description, location, country using `$or`. (2) Implement MongoDB Atlas Search (full-text search with relevance scoring). (3) Add autocomplete suggestions using debounced API calls.

**26. "Why is `httpOnly: true` important for cookies?"**
> It prevents client-side JavaScript from accessing the session cookie via `document.cookie`. Without it, an XSS attack could steal the session cookie and impersonate the user. With it, the cookie is only sent with HTTP requests, not accessible to scripts.

**27. "How would you handle image deletion from Cloudinary?"**
> When a listing is deleted or its image is replaced, call `cloudinary.uploader.destroy(filename)` to remove the old image. Currently, old images remain in Cloudinary (orphaned). This would be important for storage management.

**28. "What is the difference between `app.all('*')` and `app.use()`?"**
> `app.all('*')` matches all HTTP methods for all paths but only for unmatched routes (placed after all route definitions). `app.use()` runs for every request regardless. My 404 handler uses `app.all('*')` after all routes to catch unmatched URLs.

**29. "How would you implement wishlist/favorites?"**
> Add `wishlist: [{ type: Schema.Types.ObjectId, ref: 'Listing' }]` to User schema. Create routes: `POST /wishlist/:listingId` (add), `DELETE /wishlist/:listingId` (remove), `GET /wishlist` (view all). Toggle a heart icon on listing cards.

**30. "What are the CORS implications if you convert this to an API?"**
> Currently no CORS issues because the server renders HTML directly. If I build a separate React frontend, the API would need `cors` middleware: `app.use(cors({origin: 'https://frontend.com', credentials: true}))` to allow cross-origin requests with cookies.

---

# SECTION 16: FAILURE SCENARIOS

| Scenario | What Happens | Expected Behavior | How to Improve |
|----------|-------------|-------------------|----------------|
| **Database goes down** | `mongoose.connect()` rejects, app crashes on startup. During runtime, queries hang/timeout | Error logged to console, no requests processed | Add retry logic, health check endpoint, graceful degradation page |
| **Cloudinary API fails** | Image upload fails, Multer throws error, `wrapAsync` catches it | User sees error page ("Something Error") | Add specific error message for upload failures, allow listing creation without image |
| **Mapbox API fails** | Geocoding returns error, `response.body.features[0]` throws TypeError | Error page displayed, listing NOT created | Add try-catch around geocoding, allow creation with default coordinates |
| **Network disconnects (user)** | Form submission fails, browser shows connection error | No server impact | Add service worker for offline indicator, retry logic on forms |
| **Invalid input arrives** | Joi validation catches it, throws `EpressErr(404, errMsg)` | User sees error page with specific validation message | Better: redirect back to form with errors displayed inline |
| **Authentication expires** | Session expires after 7 days, `req.isAuthenticated()` returns false | `isLoggedIn` redirects to login with flash "You must be Logged in" | Add "Remember Me" option, refresh session on activity |
| **Cloudinary rate limit** | Upload requests fail with 429 status | Error page displayed | Implement queue for uploads, show user-friendly rate limit message |
| **Large traffic spike** | Node.js event loop saturates, response times increase | Slow page loads, potential timeouts | Add caching (Redis), load balancer, horizontal scaling, pagination |
| **Duplicate username signup** | `passport-local-mongoose` throws error | Caught by try-catch, flash error message, redirect to /signup | Show specific "Username already taken" message |
| **Invalid listing ID format** | Mongoose throws CastError (invalid ObjectId) | `wrapAsync` catches, error page displayed | Add middleware to validate ObjectId format before query |

---

# SECTION 17: SCALABILITY DISCUSSION

## Current Architecture (Single Server)

```
Browser → Express.js Server → MongoDB Atlas
                ↓
           Cloudinary CDN
           Mapbox API
```

## Scaling Discussion (Fresher-Friendly)

### 100 Users — Current Architecture Works Fine
- Single Node.js process handles requests
- MongoDB Atlas free tier sufficient
- Cloudinary free tier for images
- No optimization needed

### 1,000 Users — Minor Optimizations
- **Add pagination:** Don't load ALL listings at once. Load 20 per page.
- **Add database indexes:** `db.listings.createIndex({category: 1})` for faster category queries, `db.listings.createIndex({title: "text"})` for full-text search
- **Optimize images:** Use Cloudinary's auto-format and auto-quality transformations

### 10,000 Users — Significant Changes Needed
- **Add caching:** Use Redis to cache frequently accessed listings (reduces DB load)
- **Session store:** Switch from MongoDB to Redis for sessions (faster reads)
- **CDN for static files:** Serve CSS/JS from a CDN instead of Express
- **Connection pooling:** Increase Mongoose connection pool size
- **Monitoring:** Add APM (Application Performance Monitoring) with tools like PM2 or New Relic

### 100,000 Users — Architecture Overhaul
- **Horizontal scaling:** Run multiple Node.js instances behind a load balancer (Nginx/AWS ALB)
- **Database sharding:** Shard MongoDB by geographic region
- **Microservices:** Split into services: Auth Service, Listing Service, Review Service, Image Service
- **Message queues:** Use RabbitMQ/Kafka for async operations (email notifications, image processing)
- **Search engine:** Replace MongoDB regex with Elasticsearch for full-text search
- **Serverless functions:** Move geocoding to a serverless function (AWS Lambda)

### Key Bottlenecks to Address

| Bottleneck | Impact | Solution |
|-----------|--------|----------|
| No pagination | Index page loads ALL listings | `skip()` + `limit()` |
| No caching | Every request hits MongoDB | Redis cache with TTL |
| No indexes | Slow queries on category/search | MongoDB indexes |
| Session in MongoDB | Slow session lookups | Redis session store |
| Single process | CPU-bound operations block everything | PM2 cluster mode |
| Synchronous geocoding | Blocks request during listing creation | Background job queue |

---

# SECTION 18: OPTIMIZATIONS

## Current Limitations

| Area | Limitation |
|------|-----------|
| Performance | No pagination, no caching, no database indexes |
| Security | Cloudinary credentials hardcoded, no CSRF protection, no rate limiting |
| Code | Some typos in filenames (listting.js, middileware.js), no input sanitization |
| UX | No loading indicators, no optimistic updates, basic error pages |
| Testing | No unit tests, integration tests, or E2E tests |

## Recommended Improvements

### Code Improvements
1. Fix filename typos (`listting.js` → `listing.js`, `middileware.js` → `middleware.js`)
2. Move Cloudinary credentials to environment variables
3. Add input sanitization with `express-mongo-sanitize`
4. Add CSRF protection with `csurf` middleware
5. Add rate limiting with `express-rate-limit`
6. Add request logging with `morgan`
7. Add unit tests with Jest/Mocha
8. Add TypeScript for type safety

### Database Improvements
1. Add indexes on frequently queried fields (`category`, `title`, `owner`)
2. Implement pagination (`skip/limit`)
3. Add soft delete instead of hard delete (add `isDeleted` field)
4. Validate `ObjectId` format before queries
5. Add compound indexes for search optimization

### UI Improvements
1. Add loading spinners/skeletons during page loads
2. Implement infinite scroll or pagination buttons
3. Add image carousel for multiple listing images
4. Add date picker for booking
5. Improve mobile responsiveness
6. Add dark mode toggle
7. Add price range filter slider

### Security Improvements
1. Move all secrets to environment variables
2. Add CSRF tokens to all forms
3. Implement rate limiting on auth routes
4. Add `helmet` middleware for security headers
5. Implement Content Security Policy (CSP)
6. Add account lockout after failed login attempts
7. Implement HTTPS enforcement

### Performance Improvements
1. Add Redis caching for popular listings
2. Implement lazy loading for images
3. Minify and bundle CSS/JS
4. Use CDN for static assets
5. Enable gzip compression with `compression` middleware
6. Implement database connection pooling
7. Add service worker for offline support

---

# SECTION 19: FUTURE SCOPE

| # | Enhancement | Business Value | Technical Implementation |
|---|------------|----------------|------------------------|
| 1 | **Booking System** | Core business functionality — monetization | Booking model, date picker, availability calendar, payment gateway |
| 2 | **Payment Integration** | Revenue generation | Razorpay/Stripe API, webhook handlers, transaction logging |
| 3 | **Real-time Chat** | Host-guest communication | Socket.io, message model, chat UI component |
| 4 | **Email Notifications** | User engagement & trust | Nodemailer, email templates, booking confirmations |
| 5 | **Admin Dashboard** | Content moderation & analytics | Admin role, management routes, dashboard UI with charts |
| 6 | **Multiple Images per Listing** | Better property showcase | Image array in schema, carousel UI, multi-file upload |
| 7 | **Advanced Search & Filters** | Better user experience | Elasticsearch, price range, date availability, amenity filters |
| 8 | **User Profiles** | Community building | Profile model, avatar upload, review history, hosted listing count |
| 9 | **Wishlist/Favorites** | User retention | Wishlist array in User model, heart icon toggle, saved listings page |
| 10 | **Mobile App** | Wider reach | React Native consuming REST API |
| 11 | **Social Login** | Faster signup | Passport OAuth strategies (Google, Facebook, GitHub) |
| 12 | **Price Recommendations** | Smart pricing | ML model based on location, season, amenities |
| 13 | **Review Moderation** | Quality control | Profanity filter, admin review queue, report button |
| 14 | **Multi-language Support** | International users | i18n library, language toggle, translated templates |
| 15 | **Host Verification** | Trust & safety | ID verification, phone number, email verification |
| 16 | **Analytics Dashboard for Hosts** | Business insights | View counts, booking stats, revenue reports |
| 17 | **Cancellation Policy** | Business rules | Policy model, refund logic, cancellation workflow |
| 18 | **Accessibility (a11y)** | Inclusivity | ARIA labels, keyboard navigation, screen reader support |
| 19 | **Progressive Web App (PWA)** | Mobile experience | Service worker, manifest.json, offline support |
| 20 | **CI/CD Pipeline** | Development efficiency | GitHub Actions, automated testing, staging deployment |

---

# SECTION 20: RESUME & PLACEMENT PREPARATION

## 1. Resume Project Description (3 bullet points)
- Developed **WanderNest**, a full-stack travel platform (Airbnb clone) with Node.js, Express, MongoDB, EJS, and Bootstrap following MVC architecture
- Implemented user authentication (Passport.js), cloud image uploads (Cloudinary), interactive maps (Mapbox), star-rated reviews, category filtering, and text search with RESTful APIs
- Built authorization middleware (isOwner, isReviewAuthor), Joi validation, custom error handling, cascading deletes, and MongoDB session management with 7-day persistence

## 2. ATS-Friendly Description
```
WanderNest - Full-Stack Travel Accommodation Platform
Technologies: Node.js, Express.js, MongoDB, Mongoose, EJS, Bootstrap 5, Passport.js, Cloudinary, Mapbox, Joi, Multer
- Designed and implemented MVC architecture with 3 Mongoose models, 6 controllers, and 12+ RESTful API endpoints
- Built session-based authentication using Passport.js with PBKDF2 hashing and MongoDB session store
- Integrated Cloudinary CDN for image uploads and Mapbox Geocoding API for location-based map rendering
- Implemented server-side validation (Joi), custom error middleware, and authorization (owner-only CRUD operations)
- Developed responsive UI with Bootstrap 5, EJS templating, category filtering, and search with MongoDB regex
```

## 3. LinkedIn Project Description
> 🏠 **WanderNest — Full-Stack Airbnb Clone**
>
> Built a production-ready travel accommodation platform using **Node.js, Express.js, MongoDB, EJS, and Bootstrap 5**.
>
> 🔑 **Key Technical Highlights:**
> • MVC architecture with modular Express routing and Mongoose ODM
> • Passport.js authentication with session management in MongoDB
> • Cloudinary integration for cloud image hosting with on-the-fly transformations
> • Mapbox Geocoding API for converting locations to interactive map coordinates
> • Joi schema validation, custom error handling, and authorization middleware
>
> 🎯 **Features:** CRUD listings, star-rated reviews, 10 category filters, destination search, tax toggle, responsive design
>
> #nodejs #expressjs #mongodb #fullstack #webdevelopment

## 4. HR Round Explanation
> "I built a travel website called WanderNest where people can list their properties for travelers to find. Think of it like a smaller version of Airbnb. Users can create an account, log in, list their property with photos and location, and other users can browse these listings, filter them by type like Mountains or Castles, and leave reviews. I used popular industry technologies like Node.js, MongoDB, and Bootstrap. The project taught me about building secure web applications, managing databases, and integrating cloud services."

## 5. Technical Round Explanation
> "WanderNest follows the MVC architecture pattern with Express.js for routing, Mongoose ODM with MongoDB Atlas for data persistence, and EJS with ejs-mate for server-side rendering. Authentication uses Passport.js Local Strategy with passport-local-mongoose for PBKDF2 password hashing and express-session with connect-mongo for session persistence. I integrated Cloudinary via Multer middleware for image uploads and Mapbox Geocoding SDK for forward geocoding, storing coordinates as GeoJSON Points for map rendering with Mapbox GL JS. Authorization is handled through custom middleware — isLoggedIn, isOwner, isReviewAuthor — and input validation uses Joi schemas. Error handling uses a custom ExpressError class with a wrapAsync utility for catching rejected promises in async handlers."

---

# SECTION 21: PROJECT REVISION SHEET

## ⚡ One-Night-Before-Interview Quick Revision (3 Pages)

### PAGE 1: Architecture & Tech Stack

**Project:** WanderNest — Airbnb-like travel platform
**Pattern:** MVC (Model-View-Controller)
**Backend:** Node.js + Express.js
**Database:** MongoDB Atlas + Mongoose ODM
**Views:** EJS + ejs-mate (layouts) + Bootstrap 5
**Auth:** Passport.js + passport-local-mongoose (PBKDF2 hashing)
**Images:** Cloudinary + Multer + multer-storage-cloudinary
**Maps:** Mapbox GL JS + Mapbox Geocoding SDK
**Validation:** Joi schemas
**Sessions:** express-session + connect-mongo (MongoDB store)

**3 Models:**
- **Listing:** title, description, image, price, location, country, reviews[], owner, geometry, category
- **Review:** comment, rating (1-5), author, createdAt
- **User:** email, username, hash, salt (via passport-local-mongoose)

**Relationships:** User →owns→ Listing, User →writes→ Review, Listing →has→ Reviews[]

### PAGE 2: Key Routes & Features

**Listing CRUD:**
- GET /listings → Index (all listings)
- POST /listings → Create (auth + multer + validate + geocode)
- GET /listings/:id → Show (populate reviews+authors+owner)
- PUT /listings/:id → Update (auth + owner + validate)
- DELETE /listings/:id → Delete (auth + owner + cascade reviews)

**Search & Filter:**
- GET /listings/search?query=X → MongoDB regex, case-insensitive
- GET /listings/category/:category → Filter by enum

**Auth:**
- POST /signup → User.register() + req.login() (auto-login)
- POST /login → passport.authenticate('local') + saveRedirectUrl
- GET /logout → req.logout()

**5 Key Middleware:**
1. `isLoggedIn` — checks `req.isAuthenticated()`
2. `isOwner` — compares `listing.owner._id` with `req.user._id`
3. `isReviewAuthor` — compares `review.author` with `req.user._id`
4. `validateListing` — Joi validation
5. `validateReview` — Joi validation

**2 Key Utilities:**
1. `wrapAsync(fn)` — catches async errors, calls `next(error)`
2. `EpressErr(statusCode, message)` — custom error class extending Error

### PAGE 3: Interview-Ready Answers

**"Tell me about your project"** → WanderNest, full-stack Airbnb clone, MVC, CRUD, auth, Cloudinary, Mapbox, reviews, categories

**"Biggest challenge"** → File upload pipeline (Multer → Cloudinary → URL storage), and session redirect URL being lost during Passport login

**"Why MongoDB?"** → Document model fits nested data (images, GeoJSON, review arrays)

**"Why server-side rendering?"** → Simpler for CRUD app, better SEO, no build step needed

**"Security measures"** → PBKDF2 hashing, httpOnly cookies, Joi validation, isOwner authorization, EJS auto-escaping

**"What would you improve?"** → Pagination, Redis caching, CSRF protection, real-time chat, payment integration

**"How does auth work?"** → Passport.js Local Strategy → PBKDF2 hash with salt → session stored in MongoDB → deserialized on each request → req.user available

**"How does file upload work?"** → Form (multipart) → Multer (parse file) → CloudinaryStorage (upload) → URL returned → stored in listing.image

---

# SECTION 22: MOCK INTERVIEW

## Round 1: HR Questions

**Q: Tell me about yourself.**
> "I'm a computer science graduate with a strong foundation in full-stack web development. I recently built WanderNest, an Airbnb-inspired travel platform using Node.js, Express, and MongoDB. I'm passionate about building web applications that solve real-world problems and I'm eager to apply my skills in a professional environment."

**Q: Why should we hire you?**
> "I have hands-on experience building a production-level full-stack application from scratch. I understand MVC architecture, database design, authentication, cloud service integration, and error handling — all skills directly applicable to your projects. I'm a quick learner and I take ownership of my work."

**Q: Where do you see yourself in 5 years?**
> "I see myself as a senior full-stack developer, mentoring junior developers and architecting scalable systems. I want to grow with a company where I can contribute to meaningful products while continuously learning new technologies."

**Q: Tell me about a time you faced a difficult problem.**
> "While building WanderNest, I struggled with a bug where login redirects weren't working. Users would try to access a protected page, get redirected to login, but after logging in, they'd go to the homepage instead of the original page. After debugging, I discovered that Passport.js resets the session during authentication, losing my stored redirect URL. I solved it by creating middleware that saves the URL to `res.locals` before Passport processes the login."

## Round 2: Technical Questions

**Q: What is the event loop in Node.js?**
> "The event loop is Node.js's mechanism for handling asynchronous operations on a single thread. When Node encounters an async operation (like a database query), it delegates it to the system kernel or a thread pool and continues processing the next task. When the async operation completes, its callback is pushed to a queue. The event loop continuously checks this queue and executes callbacks when the call stack is empty. This is why Node.js is non-blocking and efficient for I/O-heavy applications."

**Q: Explain the difference between SQL and NoSQL databases.**
> "SQL databases (PostgreSQL, MySQL) use structured tables with fixed schemas, support ACID transactions, and use SQL for queries. They're great for structured data with clear relationships.

> NoSQL databases (MongoDB) use flexible documents (JSON-like), can have varying fields per document, and scale horizontally more easily. They're great for data that doesn't fit neatly into tables — like my listing data with nested image objects, GeoJSON coordinates, and variable-length review arrays.

> I chose MongoDB because it naturally models the listing data without requiring join tables for images, coordinates, or reviews."

**Q: What is middleware in Express.js?**
> "Middleware functions have access to the request object, response object, and the `next` function. They execute in the order they're registered. Each middleware can: (1) Execute code, (2) Modify req/res, (3) End the request-response cycle, or (4) Call next() to pass control to the next middleware.

> In my project, I use 5 custom middleware functions: `isLoggedIn` checks authentication, `isOwner` checks authorization, `validateListing` validates input with Joi, `saveRedirectUrl` preserves redirect URLs, and `isReviewAuthor` protects review deletion."

**Q: How does indexing work in MongoDB?**
> "Indexes are special data structures that store a small portion of a document's fields in an ordered format, allowing MongoDB to efficiently locate documents without scanning every document in the collection.

> Without an index on `category`, a query like `Listing.find({category: 'Mountains'})` scans all documents. With an index, MongoDB uses a B-tree structure to jump directly to matching documents.

> I should add indexes on `category`, `title` (for search), and `owner` (for user's listings) to improve query performance."

**Q: Explain RESTful routing with examples from your project.**
> "REST maps HTTP methods to CRUD operations:

> | HTTP Method | URL | Action | Example |
> |-------------|-----|--------|---------|
> | GET | /listings | Read all | Index page |
> | POST | /listings | Create | New listing |
> | GET | /listings/:id | Read one | Show page |
> | PUT | /listings/:id | Update | Edit listing |
> | DELETE | /listings/:id | Delete | Remove listing |

> HTML forms only support GET and POST, so I use `method-override` to simulate PUT and DELETE by adding `?_method=PUT` to the form action."

## Round 3: Project-Specific Questions

**Q: Walk me through what happens when a user creates a new listing.**
> "When the user fills the form and clicks Submit:
> 1. The form sends a POST request to `/listings` with `multipart/form-data` encoding (because of the file upload)
> 2. `isLoggedIn` middleware checks if the user is authenticated
> 3. Multer middleware parses the multipart data, extracts the file, and sends it to Cloudinary via the CloudinaryStorage engine
> 4. Cloudinary stores the image and returns a URL and filename
> 5. `validateListing` middleware validates the form body using the Joi listing schema
> 6. The controller calls Mapbox Geocoding API with the location string, which returns GeoJSON coordinates
> 7. A new Listing document is created with all form data, the Cloudinary image URL, the geocoded geometry, and `req.user._id` as the owner
> 8. The listing is saved to MongoDB
> 9. A flash message 'New Listing Created!' is set
> 10. The user is redirected to `/listings`"

**Q: How do you handle errors in your application?**
> "I have a three-layer error handling strategy:
> 1. **wrapAsync**: A utility that wraps async route handlers. If the handler's promise rejects, it catches the error and calls `next(error)`. This eliminates repetitive try-catch blocks.
> 2. **Custom Error Class**: `EpressErr` extends JavaScript's `Error` class to include an HTTP `statusCode`. This allows me to throw errors with specific status codes.
> 3. **Global Error Handler**: A middleware with 4 parameters `(err, req, res, next)` that catches all errors, extracts the status code (defaulting to 500), and renders an error page.

> Additionally, Joi validation throws errors before data reaches the database, and Passport.js handles authentication errors with its own redirect strategy."

**Q: How did you implement authorization in your project?**
> "Authorization is separate from authentication. Authentication verifies WHO you are; authorization verifies WHAT you can do.
>
> I have three authorization checks:
> 1. `isLoggedIn` — uses Passport's `req.isAuthenticated()` to ensure the user is logged in
> 2. `isOwner` — fetches the listing from the database and compares `listing.owner._id` with the current user's ID using Mongoose's `.equals()` method (can't use `===` because ObjectIds are objects)
> 3. `isReviewAuthor` — same pattern but compares `review.author` with the current user
>
> These are applied as middleware in the route definitions, so they run before the controller function. If authorization fails, the middleware flashes an error and redirects — the controller never executes."

---

# SECTION 23: CHEAT SHEET

## ⚡ 30-Minute Pre-Interview Quick Reference

### 🎯 THE PROBLEM
> Travelers need to discover unique accommodations. Owners need to list properties. No centralized platform with images, maps, and reviews.

### 💡 THE SOLUTION
> WanderNest — Full-stack Airbnb clone. Browse listings, filter by category, search, view on maps, leave reviews. Owners can CRUD their listings with cloud-hosted images.

### 🏗️ ARCHITECTURE
```
MVC Pattern:
  Models     → Mongoose schemas (Listing, Review, User)
  Views      → EJS templates with ejs-mate layouts
  Controllers → Business logic (CRUD, geocoding, auth)
  Routes     → Express routers (listing.js, review.js, user.js)
  Middleware → Auth, authorization, validation, file upload
```

### 📊 DATABASE (MongoDB)
```
Listing: title*, description, image{url,filename}, price, location, country,
         reviews[→Review], owner→User, geometry{Point}, category(enum)
Review:  comment, rating*(1-5), author→User, createdAt
User:    email*, username*, hash, salt (passport-local-mongoose)
```

### 🛣️ KEY APIs
```
GET  /listings          → All listings
POST /listings          → Create (auth + upload + validate + geocode)
GET  /listings/:id      → Show + map + reviews
PUT  /listings/:id      → Update (auth + owner)
DELETE /listings/:id    → Delete + cascade reviews (auth + owner)
POST /listings/:id/reviews     → Add review (auth)
DELETE /listings/:id/reviews/:reviewId → Delete review (auth + author)
GET/POST /signup, /login, GET /logout
GET /listings/search?query=X
GET /listings/category/:category
```

### 🔐 AUTHENTICATION
```
Signup: User.register(user, password) → PBKDF2 hash + salt → req.login()
Login:  passport.authenticate('local') → verify hash → session → req.user
Session: express-session + connect-mongo, 7-day cookie, httpOnly
```

### 🔒 AUTHORIZATION MIDDLEWARE
```
isLoggedIn    → req.isAuthenticated() check
isOwner       → listing.owner._id.equals(req.user._id)
isReviewAuthor → review.author.equals(req.user._id)
```

### ☁️ CLOUD SERVICES
```
Cloudinary: Image upload via Multer → CDN URL stored in DB
Mapbox:     Forward geocoding (location→coordinates) + GL JS map rendering
```

### 🧪 VALIDATION & ERROR HANDLING
```
Joi schemas → validate request body before controller
wrapAsync   → catch async errors, call next(error)
EpressErr   → custom Error class with statusCode
Global handler → renders error.ejs
```

### 🔥 FEATURES
1. User Registration & Login (Passport.js)
2. CRUD Listings (with image upload + geocoding)
3. Star-Rated Reviews (1–5, with author)
4. 10 Category Filters (Icons + DB query)
5. Search (MongoDB regex, case-insensitive)
6. Interactive Maps (Mapbox GL JS + marker)
7. Tax Toggle Display (client-side JS)
8. Flash Messages (success/error alerts)
9. Responsive Design (Bootstrap 5 grid)
10. Authorization (owner-only edit/delete)

### 🚧 BIGGEST CHALLENGE
> File upload pipeline (Multer → Cloudinary → URL storage) and session redirect URL being lost during Passport login (solved with saveRedirectUrl middleware).

### 🔮 FUTURE SCOPE
> Booking system, payment integration (Razorpay), real-time chat (Socket.io), email notifications, admin dashboard, multiple images, advanced search, pagination, Redis caching.

### 🛡️ SECURITY
> PBKDF2 password hashing, httpOnly session cookies, Joi input validation, EJS auto-escaping (XSS prevention), authorization middleware, Mongoose parameterized queries (injection prevention).

### 📈 SCALABILITY IMPROVEMENTS
> Pagination, database indexes, Redis caching, CDN for static files, horizontal scaling with load balancer, microservices architecture.

---

> **Confidence Level:** This guide is based on a thorough analysis of every source file in the project. All code references, function names, file paths, and technical explanations are directly derived from the actual codebase. No assumptions were made about features not present in the code.

> **Assumptions:**
> 1. The `.env` file contains `ATLASDB_URL`, `SECRET`, `MAP_TOKEN` variables (referenced in code but not available for review)
> 2. The project uses a local MongoDB instance for seeding (`mongodb://127.0.0.1:27017/wanderlust`) and MongoDB Atlas for production (`ATLASDB_URL`)
> 3. The project name is referred to as both "WanderNest" (in navbar/README) and "Wanderlust" (in flash messages/seed script) — likely an evolution during development

---

*Generated by comprehensive analysis of the complete WanderNest codebase — every file, every function, every route, every model.*
