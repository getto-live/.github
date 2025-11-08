# 🚀 Serverless URL Shortener — **GETTO.LIVE**

### Real-Time Analytics • Scalable • Built on Alibaba Cloud

---

## 🧠 Project Overview

This repository contains the source code and documentation for **GETTO.LIVE**, a **serverless URL shortener** with **near real-time analytics**, developed as part of the **Cloud Systems (AWSB)** course in the **Computer Science** program.

**Authors:**

* Mateusz Tyszczak
* Fabian Waszkiewicz
* Kamil Ochociński
* Sebastian Szwajnoch
* Sebastian Pogoda

**Supervisor:**

> mgr inż. Dawid Jurczyński

---

## 🎯 Project Goal

The goal of this project is to build a **simple, scalable, serverless URL shortener** that can **track clicks** (count, top links, sources) in near real-time.

Our MVP is designed to:

* Fit entirely within Alibaba Cloud’s **Free Tier**,
* Deliver a functional **proof of concept** quickly,
* Be later extended with a **visual analytics dashboard**.

---

## 🧩 MVP Scope

### HTTP Endpoints

1. **`/create?url=...`** — Creates a short link and stores a `token -> URL` mapping.
2. **`/{token}`** — Redirects (`302`) to the original URL and logs the click event.

### Storage & Logging

* **OSS (Object Storage Service)** — JSON files for token mappings (`u/{token}.json`).
* **Log Service (SLS)** — Event logging with SQL-based aggregations.

### Frontend

* **static HTML/JS** page hosted in OSS.
* Styled using **Tailwind CSS**.
* Allows users to paste a long URL and get a short one instantly.

### Testing

* Manual via `curl` or browser.
* Quick analytical queries directly in **Log Service**.

---

## ☁️ Alibaba Cloud Services Used

| Service                             | Purpose                                                             |
| ----------------------------------- | ------------------------------------------------------------------- |
| **Function Compute (Node.js)**      | Single HTTP function handling both `/create` and `/{token}` routes. |
| **OSS (Object Storage Service)**    | JSON mapping files and static frontend hosting.                     |
| **Log Service (SLS)**               | Event logging (IP, UA, timestamp, token) and SQL analytics.         |
| **API Gateway (optional)**          | Custom domain, rate-limiting, or authentication (for later stages). |
| **DataV Portal / Grafana (future)** | Public dashboard with live analytics visualization.                 |

---

## ⚙️ How It Works

1. The user opens the static frontend (hosted in OSS).
2. They paste a long URL and click **Generate**.
3. Frontend sends:

   ```
   GET /create?url=https://example.com
   ```
4. The Function Compute handler:

   * Generates a **6-character token**,
   * Saves a JSON mapping to OSS (`u/{token}.json`),
   * Returns a shortened link:

     ```
     BASE_URL/{token}
     ```
5. When the shortened link is accessed:

   * The function reads the mapping from OSS,
   * Logs the event to **Log Service** (IP, User Agent, timestamp),
   * Returns a **302 Redirect** to the original URL.
6. In **Log Service**, we can query:

   * Total number of clicks,
   * Top tokens,
   * Clicks over time.
7. *(Post-MVP)* Scheduled SQL tasks write aggregated results to OSS → displayed via **DataV Portal** or **Grafana** dashboard.

---

## 🧱 Tech Stack

| Layer                  | Technology                             |
| ---------------------- | -------------------------------------- |
| **Backend**            | Node.js (Alibaba Function Compute)     |
| **Frontend**           | Static HTML + Fetch API + Tailwind CSS |
| **Data Storage**       | JSON files in OSS                      |
| **Analytics**          | Alibaba Cloud Log Service (SLS)        |
| **Version Control**    | GitHub                                 |
| **Project Management** | GitHub Projects (Kanban workflow)      |

---

## 📋 Project Management — *Kanban Methodology*

We manage development through a **Kanban board** in **GitHub Projects**, providing a clear view of the workflow:

### Core Principles:

* No fixed roles or sprints.
* Visualization of all tasks: “To Do”, “In Progress”, “Done”.
* **WIP limits** (Work In Progress) to maintain flow efficiency.
* Quick adaptation to changing requirements.
* Focus on eliminating waste and increasing quality.
* Empowered, motivated, and self-organized team.

---

## ✅ MVP Success Criteria

* Short links redirect correctly (`302 → target URL`) with latency around **0–100 ms**.
* Click logs visible in **SLS** with working SQL aggregations (top N, counters).
* Frontend allows a **non-technical user** to create short links easily.
* All costs remain within **Alibaba Cloud Free Tier**.
* No **5xx errors** under normal usage.

---

## 🤝 Contributing

We use GitHub Issues and Pull Requests for collaboration.
Each feature or fix should be linked to a Kanban card in **GitHub Projects**.
Please follow good commit messages naming conventions and use short, descriptive branch names.

---

## 📄 License

All the projects under the [getto-live](https://github.com/getto-live) organization are developed for **educational purposes** within the Cloud Systems (AWSB) course.
All rights reserved © 2025 — *Mateusz Tyszczak, Fabian Waszkiewicz, Kamil Ochociński, Sebastian Szwajnoch, Sebastian Pogoda*.
