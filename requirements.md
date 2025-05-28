# Arc Requirements Document

## Overview

This document defines the full scope of Arc, the real-life side quest app. It outlines the user features, data structures, backend logic, and rules needed to support the vision laid out in the README.

---

## Key Modules

### 1. **User Profiles**

* Username
* Avatar
* Short Bio
* Active quests
* Completed quests
* Bucket list
* Followers / Following

### 2. **Quest Generation (GPT)**

**Input fields:**

* Timeframe (day, weekend, week, month)
* Budget (broke, cheap, mid, baller)
* Distance (local, national, global)
* Vibe (creative, physical, spiritual, chaotic, funny, weird)

**Output:**

* Title
* Description
* Estimated cost
* Suggested location
* Duration
* Hook tagline
* Optional: link to gear/templates

### 3. **Quest Management**

* Create manually or via GPT
* Log updates (images, text, video)
* Set public/private visibility
* Add tags (theme, location, gear, goal)
* Quest status: Not Started / In Progress / Completed / Failed

### 4. **Quest Feed & Discovery**

* Public quests with filters
* Trending quests
* Quests near me
* Quests by vibe or theme
* Quest detail page (with lore log, creator info, and interaction options)

### 5. **Map Integration**

* Optional live check-in / delayed check-in / static pin
* Filter map by quest status, vibe, budget, etc.

### 6. **Lore Logs**

* Per-quest logs
* Users can react & comment
* Final log = “Chapter Close” with reflections or learnings

### 7. **Bucket List System**

* Save quests created by others
* Organize with tags or folders (optional in v2)

### 8. **Social**

* Follow/unfollow users
* See their current & past quests
* Get updates on their new arcs

### 9. **Monetization Layer**

* AMA system (pay to ask questers questions)
* Behind-the-Quest subscriptions
* One-off template sales (budget, gear, tips)
* Arc+ Premium (GPT boost, map unlocks, enhanced visibility)

---

## Database Structure (Core Tables)

### `users`

| Field       | Type      |
| ----------- | --------- |
| id          | UUID (PK) |
| username    | text      |
| email       | text      |
| avatar\_url | text      |
| bio         | text      |
| created\_at | timestamp |

### `quests`

| Field          | Type          |
| -------------- | ------------- |
| id             | UUID (PK)     |
| user\_id       | FK (users.id) |
| title          | text          |
| description    | text          |
| vibe           | text\[]       |
| budget         | enum          |
| timeframe      | text          |
| status         | enum          |
| visibility     | boolean       |
| location\_text | text          |
| created\_at    | timestamp     |

### `quest_logs`

| Field      | Type           |
| ---------- | -------------- |
| id         | UUID (PK)      |
| quest\_id  | FK (quests.id) |
| text       | text           |
| media\_url | text           |
| timestamp  | timestamp      |

### `bucket_list`

\| Field | Type |
\| id | UUID (PK) |
\| user\_id | FK (users.id) |
\| quest\_id | FK (quests.id) |
\| added\_at | timestamp |

### `followers`

\| Field | Type |
\| id | UUID (PK) |
\| follower\_id | FK (users.id) |
\| following\_id | FK (users.id) |

### `ama_questions`

\| Field | Type |
\| id | UUID (PK) |
\| quest\_id | FK (quests.id) |
\| asker\_id | FK (users.id) |
\| question | text |
\| answer | text |
\| price\_paid | decimal |
\| timestamp | timestamp |

### `bts_templates`

\| Field | Type |
\| id | UUID (PK) |
\| user\_id | FK (users.id) |
\| title | text |
\| file\_url | text |
\| price | decimal |
\| is\_included\_in\_subscription | boolean |

---

## API Endpoints (via Bolt)

* `POST /generate-quest`
* `POST /create-quest`
* `GET /quests/feed`
* `GET /quests/map`
* `POST /quests/:id/log`
* `POST /bucketlist/add`
* `POST /follow`
* `POST /ask-question`
* `POST /upload-template`

---

## Roles & Permissions

* Users can only edit/delete their own quests and logs
* AMA answers are optional and only paid if answered
* Bucket list is private by default
* Arc+ users can toggle visibility on templates & advanced stats

---

Next step: UI wireframes + user journey flows.
