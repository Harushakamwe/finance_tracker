# **Strategic, Architectural, and Commercial Blueprint for an Active Personal Finance Tracking Platform**

The transition from embedded systems programming, characterized by C++ memory management and hardware interfacing, to full-stack application development requires a profound paradigm shift. Building a personal finance tracking and dashboard application demands a transition from deterministic hardware loops to asynchronous, event-driven software architectures, distributed data synchronization, and user-centric behavioral design. The provided dataset outlines a highly manual but sophisticated financial tracking mechanism. It incorporates granular transaction logging, dynamic budget tracking against specific categories, and complex net-worth calculations involving multiple asset classes, such as equities, cash, and receivables, alongside liabilities like student loans and consumer credit.  
Transforming this two-dimensional, pivoted spreadsheet matrix into a robust, scalable software application represents a significant architectural leap. This report provides a comprehensive blueprint for developing a personal finance application built upon the philosophy of "Active Tracking." The subsequent analysis bridges academic foundations with enterprise-grade system architecture and modern business strategy, charting a clear course from a local-first native desktop utility to a fully monetized, cloud-enabled platform.

## **Academic Alignment: Translating University Syllabi into Application Features**

The theoretical and practical frameworks provided by the computer science curriculum at Linköping University directly map to the architectural requirements of building a modern financial platform. The concepts taught across the three specified courses serve as the technical foundation for the application's phased rollout, providing the necessary theoretical rigor to handle sensitive financial data.

### **Database Technology (TDDD37): Schema Design and Data Integrity**

The primary challenge in migrating the provided financial data is the transformation of a pivoted, human-readable spreadsheet into a machine-readable, normalized relational database structure. The TDDD37 syllabus emphasizes the Enhanced Entity-Relationship (EER) model, normalization up to the Third Normal Form (3NF), and transaction concurrency. In the existing spreadsheet, data is pivoted cross-sectionally: rows represent expense categories, and columns represent temporal periods. While optimal for visual scanning by a human, this structure is fundamentally incompatible with a transactional software backend.  
By applying the normalization principles taught in TDDD37, the application will eliminate data redundancy and insertion anomalies. The academic curriculum mandates that students design relational databases for different types of domains by first creating a conceptual schema using the EER model and then translating this schema into a corresponding logical schema captured in the relational data model. This exact methodology is required to parse the complex financial life depicted in the CSV files. For instance, the course's "BrianAir" laboratory project requires students to manage flights, passenger bookings, and payment states. This directly parallels the financial application's need to manage accounts, transaction ledgers, and budget thresholds.  
Furthermore, the application of TDDD37 concepts includes a deep focus on concurrency control and the ACID (Atomicity, Consistency, Isolation, Durability) properties of database transactions. As the application scales to cloud synchronization in later phases, ensuring that concurrent writes do not corrupt the financial ledger becomes paramount. The principles of transaction serialization taught in the course will dictate how the backend handles overlapping database commits from multiple devices. Finally, the application will require rapid filtering of transactions by date range and category to populate dashboard metrics dynamically. TDDD37's focus on file structures and indexing will inform the creation of B-Tree indexes on fields such as the transaction date and category identifier to ensure real-time rendering of the graphical user interface without lag.

### **Web Programming (TDDD97): API Development and Server-Side Logic**

While the initial phase of the project dictates a local-native architecture, subsequent phases necessitate a decoupled client-server model. TDDD97 covers the foundational elements of server-side development, specifically Python, Flask, SQL, and REST API construction, alongside data serialization using JSON. This curriculum bridges the gap between the underlying database and the user-facing application.  
The course structure, which includes laboratory assignments such as developing the backend for a "Phonebook" application, provides the exact template needed to build a financial REST API. Moving from basic client-side HTML to server-side Python development allows for the creation of robust API endpoints. The Python fundamentals applied in the local native application will eventually be wrapped into a Flask-based REST API. The course's focus on stateless communication will guide the creation of secure endpoints for retrieving transactions, updating budgets, and managing user accounts.  
Financial data fetched from the SQLite database must be serialized into JSON to be transmitted to web or mobile clients. TDDD97 teaches the exact data-handling paradigms required to format floating-point currency arrays and timestamped ledgers for network transmission over HTTP. Moreover, handling financial data requires rigorous input validation and security. The backend development concepts taught in TDDD97 will be critical for implementing server-side checks to prevent SQL injection attacks and ensure that malformed data payloads are rejected before reaching the database execution layer.

### **Advanced Web Programming (TDDD27): Reactive Dashboards and Complex State**

As the application matures, the requirement for an interactive, dynamic front-end dashboard will emerge to fulfill the "Active Tracking" philosophy. TDDD27 focuses on advanced client-side frameworks, asynchronous communication, and large-scale application deployment. The course requires students to plan, implement, and present a mid-sized multi-user application on the web, utilizing modern micro-frameworks and external REST API integrations.  
The "Active Tracking" philosophy requires a frictionless user experience where engagement does not feel burdensome. When a user approves an auto-suggested transaction generated by the rules engine, the interface must update immediately without a full page reload. Concepts of asynchronous JavaScript, REST technologies, and state management taught in TDDD27 will be used to build a highly reactive web dashboard. The financial dashboard will feature distinct modules: a net-worth line chart, a monthly budget progress bar, and a pending transactions list. TDDD27 provides the knowledge to architect these as modular components using frameworks like React or Vue, ensuring maintainable and testable code.  
Additionally, TDDD27 mandates the use of Gitlab for version control, continuous integration, and project management, explicitly requiring that all code and version history be strictly maintained. This academic requirement aligns perfectly with establishing a professional repository structure, automating testing pipelines, and deploying the application backend safely to the cloud, bridging the gap between a solo passion project and a production-ready software ecosystem. Students are also encouraged to utilize serverless backends and GraphQL, which present highly scalable options for managing the complex state of a financial application across multiple clients.

## **The Technical Project Plan: From Local-Native to Cloud**

The development roadmap must be strictly phased to manage architectural complexity. It begins with a tightly scoped, fully local desktop application to establish core logic and user experience before expanding into distributed cloud infrastructure.

### **Phase 1: The Local-First Native App**

The paramount constraint for the initial phase is the creation of a purely native desktop application relying on Python, PyQt6, and SQLite. This explicitly avoids browser wrappers, Electron bloat, or localized web servers, ensuring maximum performance, minimal resource consumption, and absolute data privacy. For a developer with a strong C++ background, transitioning to PyQt6 offers a highly familiar paradigm. PyQt6 is a set of Python bindings for the Qt application framework, which is natively written in C++. The signal and slot mechanism used in Qt for communication between objects will translate seamlessly, allowing for robust event-driven programming.  
**Course Reference for Architecture & ORM:** To understand how to structure this application, you will implement the Model-View-Controller (MVC) design pattern. You can learn the architectural separation of concerns (separating the UI View from the Data Model and Business Controller) by reviewing the architectural and server-side lectures in **TDDD97**.1 For mapping your Python data classes to the SQLite database (Object-Relational Mapping), you should reference **TDDD37**. Specifically, the topic *"Mapping of EER diagrams to relations"* teaches the exact rules for translating conceptual entities (like your accounts and categories) into a logical SQLite relational schema with proper primary and foreign keys.

#### **Normalized Database Schema Design**

An exhaustive analysis of the user's uploaded CSV documents reveals a highly complex financial ecosystem involving diverse asset classes, liabilities, recurring expenses, and granular monthly budget categories. The existing spreadsheet calculates net worth dynamically by pulling from various pivoted summary tables. To replicate and enhance this capability in software, the data must be fundamentally restructured into a normalized relational SQLite schema.  
The normalization process eliminates the brittle structure of the spreadsheet, where new columns must be added for every new month. Instead, the schema relies on immutable transaction ledgers and relational mapping. The following table outlines the Phase 1 normalized database schema required to capture the full breadth of the user's financial data.

| Table Name | Column Name | Data Type | Constraints & Description |
| :---- | :---- | :---- | :---- |
| **Accounts** | account\_id | UUID | Primary Key. |
|  | name | VARCHAR | e.g., "Swedbank", "CSN", "Avanza LF Global". |
|  | account\_type | ENUM | 'ASSET' or 'LIABILITY'. |
|  | currency | VARCHAR | Defaults to 'SEK'. |
|  | is\_active | BOOLEAN | Allows hiding closed accounts without losing historical data. |
| **Categories** | category\_id | UUID | Primary Key. |
|  | name | VARCHAR | e.g., "Mat", "Hyra/El/Försäkring", "Lön". |
|  | category\_type | ENUM | 'INCOME', 'EXPENSE', 'TRANSFER'. |
|  | parent\_id | UUID | Foreign Key to Categories (Self-referencing for sub-categories). |
| **Transactions** | transaction\_id | UUID | Primary Key. The immutable ledger entry. |
|  | account\_id | UUID | Foreign Key to Accounts. |
|  | transaction\_date | DATE | The date the transaction cleared. |
|  | payee\_name | VARCHAR | The raw text of the merchant or sender. |
|  | amount | DECIMAL | Positive for inflow, negative for outflow. |
|  | category\_id | UUID | Foreign Key to Categories. Nullable if awaiting user approval. |
|  | status | ENUM | 'CLEARED', 'PENDING', 'NEEDS\_APPROVAL' (Crucial for Active Tracking). |
|  | parent\_txn\_id | UUID | Foreign Key to Transactions. Used exclusively for split receipts. |
| **Budgets** | budget\_id | UUID | Primary Key. |
|  | category\_id | UUID | Foreign Key to Categories. |
|  | target\_amount | DECIMAL | User-defined threshold, e.g., 2000.00 for "Mat". |
|  | period\_month | INTEGER | The specific month the budget applies to. |
|  | period\_year | INTEGER | The specific year the budget applies to. |
| **Recurring\_Rules** | rule\_id | UUID | Primary Key. Powers the cash-flow forecasting view. |
|  | payee\_pattern | VARCHAR | String matching pattern for automated expense recognition. |
|  | expected\_amount | DECIMAL | e.g., Spotify 129kr, CSN 2000kr. |
|  | category\_id | UUID | Foreign Key to Categories. |
|  | day\_of\_month | INTEGER | The expected deduction date for forecasting. |
| **Holdings** | holding\_id | UUID | Primary Key. Represents specific equity or fund positions. |
|  | account\_id | UUID | Foreign Key to Accounts (e.g., Avanza). |
|  | ticker\_symbol | VARCHAR | e.g., "LF Global", "Spiltan Invest". |
|  | target\_allocation | DECIMAL | Percentage allocation target. |

This schema entirely replaces the spreadsheet's running totals. Current account balances and total dynamic net worth are calculated on the fly by executing SQL aggregation queries (e.g., SUM(amount)) against the Transactions table, filtered by account\_id and grouped by account\_type. This ensures that the application's state is always derived from a single source of truth: the transaction ledger.

#### **Git Repository Structure and Data Protection**

To open-source the application infrastructure without leaking the user's highly sensitive financial matrix—such as net worth, loan balances, and transactional history—the Git repository must strictly partition application code from user data and environment configurations. The project will utilize a .gitignore file to ensure that local database files and execution artifacts are explicitly excluded from version control.  
Ignoring SQLite database files is a critical security practice; storing binary database files in Git not only bloats the repository size but inevitably leads to data loss when remote branches are pulled, overwriting local production data. Furthermore, placing the database in the exact same directory as the application binaries is considered poor practice; instead, an instance/ or data/ directory should be utilized.  
The repository structure should resemble the following layout, ensuring that external collaborators can clone the application, install dependencies, and run an empty, localized instance without ever interacting with the original creator's personal ledger.

| Directory / File | Purpose and Version Control Status |
| :---- | :---- |
| src/main.py | Application entry point. (Tracked) |
| src/models/ | Python data classes and SQL ORM definitions. (Tracked) |
| src/views/ | PyQt6 graphical user interface definitions. (Tracked) |
| src/controllers/ | Business logic, rules engine, and data routing. (Tracked) |
| requirements.txt | Python dependency list. (Tracked) |
| .gitignore | Configuration specifying untracked files. (Tracked) |
| instance/ | Local directory for application state. (**Untracked**) |
| instance/data.db | The user's actual SQLite financial database. (**Untracked**) |
| .env | Local environment variables and API keys. (**Untracked**) |

### **Phase 2: Cloud Transition via Local-First Architecture**

To enable multi-device access, such as combining desktop data entry with on-the-go mobile checking, the architecture must evolve. However, transitioning away from SQLite to a traditional cloud-hosted PostgreSQL database accessed entirely via REST APIs fundamentally breaks the offline, highly responsive nature of a native desktop application.  
**Course Reference for Cloud Transition:** When you are ready to build the cloud synchronization server, refer to **TDDD97**'s "Server-side development" modules.3 This is where you will learn how to set up the Flask backend and create the REST API endpoints necessary to receive background sync payloads from your desktop client.  
To preserve the zero-latency speed and offline capabilities of the PyQt6 application while enabling cloud synchronization, a "Local-First Software Architecture" will be implemented. In the local-first paradigm, the application code continues to work directly with the client-side embedded database. This ensures that all operations can be handled by reading and writing files on the local disk, allowing the application to respond near-instantaneously to user input without waiting for network requests or displaying loading spinners.  
Instead of treating the cloud database as the primary source of truth for every read and write, a background synchronization engine is introduced to sync the local SQLite file with a remote PostgreSQL backend. Technologies such as PowerSync or standard logical replication queues enable local-first change-tracking and bidirectional synchronization.  
The architecture operates through dynamic partial replication. When the user records a manual transaction on the desktop app, the change is instantly persisted in the local SQLite file. The sync layer places the mutation into a local upload queue. The client does not update its state to the authoritative state of the server as long as there are pending writes present in the client's upload queue, preventing local conflict resolution issues. Once network connectivity is established, the queue is pushed to the central PostgreSQL server. Conversely, the cloud database pushes state changes—such as transactions entered via a companion mobile app—down to the local SQLite database. By maintaining a timestamped, append-only log of transactions, the system ensures that offline edits from multiple distributed devices merge deterministically without data loss.  
This approach minimizes the need for extensive custom REST routes for each client operation on the backend. A cloud-hosted server will run a Python Flask backend to serve as the authoritative synchronization hub for the distributed SQLite clients, while also providing standard API endpoints for user authentication and provisioning.

## **Business Case & Monetization Strategy**

The intersection of behavioral economics and financial software presents a unique market opportunity. While incumbent financial technology focuses on extreme automation and invisibility, a growing demographic recognizes that total automation leads to financial complacency and lifestyle creep.

### **The Market for "Active Tracking"**

The modern financial technology landscape is dominated by "Passive Tracking" applications. These platforms utilize open banking infrastructure to automatically categorize expenses and present the user with retrospective pie charts at the end of the month, requiring zero user intervention. However, behavioral economics research indicates that reducing friction in financial transactions inherently correlates with increased and unmindful spending.  
Passive trackers suffer from the "set and forget" paradox: by abstracting away the pain of tracking, they remove the psychological friction required to foster behavioral change. An "Active Tracking" application deliberately reintroduces beneficial friction into the user's financial workflow. Academic studies on financial mindfulness—defined as the awareness and acceptance of one's financial reality—demonstrate that putting "some space" between a stimulus and a response, even delaying a decision process by a fraction of a second, enables the brain to focus attention on relevant information and block out impulses. By forcing the user to interact with their data, such as manually categorizing imported CSV transactions, approving rule suggestions, and visually confronting the impact of a transaction on their remaining monthly budget, the software acts as a psychological intervention rather than merely a reporting tool.  
The market demand for this philosophy is highly robust. This is evidenced by the dedicated following of zero-based budgeting platforms where users actively prefer manual entry or manual approval of imported transactions because it forces accountability and ensures the budget accurately reflects reality in real-time. In a multi-person household, active tracking guarantees that the budget is always up to date, eliminating the risk of accidental overdrafts caused by pending, un-synced transactions that a passive aggregator might delay importing by several days.

### **Handling UX Edge Cases**

A critical factor in the commercial success of a financial application is how gracefully it handles real-world complexity that breaks simple algorithms. The application must address several user experience edge cases derived from the user's actual spending behavior.  
**Combined Receipts (The "Supermarket Problem")** A single transaction from a major retailer frequently contains items belonging to drastically different budget categories. An analysis of the user's data shows transactions from retailers like IKEA, where a single large purchase may consist of both furniture and cafeteria food. If the application forces a single category per transaction, the budget tracking becomes inherently inaccurate. The user interface must support "Split Transactions." When a user reviews a pending transaction, they can invoke a split modifier. The interface dynamically generates child transaction rows that must mathematically sum to the exact total of the parent transaction. The database handles this by creating multiple records in the Transactions table, all sharing a common parent\_txn\_id, allowing them to be categorized independently while reconciling cleanly with the total bank statement outflow.  
**Un-fetchable Cash Transactions** Despite the Nordic region's push toward a cashless society, peer-to-peer transactions or international cash spending still occur. The application handles this by treating cash identically to any other financial entity in the Accounts table. When a user withdraws cash from an ATM, they log a transfer within the application. The system records an outflow from the primary bank account and a corresponding inflow to the cash account. Subsequent cash purchases are logged manually as outflows from the cash account, ensuring the total dynamic net worth remains mathematically accurate without relying on API connections.  
**Predictive Friction (The Rules Engine)** To prevent active tracking from becoming an overwhelming administrative burden, the "Rules Engine" learns from the user's behavior. If the user consistently categorizes a transaction containing the payee string "Systembolaget" as *Utekäk, krök, bio, events*, the system records a deterministic rule or utilizes a Bayesian filter linking that specific text to that specific category. When a user imports a CSV or the system processes a recurring forecast matching that pattern, the user interface highlights the category in a distinct color, signaling an auto-suggestion. The user simply clicks a single "Approve" button to clear the transaction. This mechanism speeds up manual entry without removing the user's final approval, perfectly balancing interface velocity with psychological accountability.  
**Forecasting and Dynamic Accounts** The application includes a forecasting view powered by the Recurring\_Rules table. By recognizing dates associated with predictable cash flows, such as rent, subscriptions, and salary inflows, the dashboard projects how these upcoming expenses will impact the user's liquidity for the remainder of the month. Furthermore, the interface includes dynamic options to add new accounts, allowing the user to instantiate new asset or debt sources (e.g., a new credit card or a vehicle loan) at any time. These accounts immediately integrate into the dynamic net-worth calculation, adjusting the overarching financial dashboard in real-time.

### **Monetization Strategy & Cost-to-Reach Analysis**

Software architecture fundamentally dictates commercial viability. By intentionally avoiding high-cost automated bank integrations, the product can operate with highly favorable profit margins. The phased rollout enables a tiered monetization strategy that mitigates risk while maximizing reach.

| Tier / Phase | Product Offering | Revenue Model | Target Audience & Reach Maximization | Marketing Strategy & Cost Analysis |
| :---- | :---- | :---- | :---- | :---- |
| **1\. The Origin** *(FOSS Desktop)* | A standalone, open-source PyQt6 desktop app. Users own their data purely locally via SQLite. | **Free / Donationware** (GitHub Sponsors, Patreon). Serves as top-of-funnel marketing. | Privacy-centric power users, software developers, and open-source advocates. | **Cost: Zero.** Marketed entirely via organic grassroots engagement on platforms like Reddit (r/selfhosted, r/personalfinance) and Hacker News. Reaches highly technical early adopters who will report bugs and suggest features. |
| **2\. Sync License** *(Bring Your Own Cloud)* | The desktop app with the local-first sync engine activated. Users point the sync engine to their own basic cloud storage. | **One-time perpetual license fee** (e.g., €49). The user buys the compiled software binaries. | Technical users who desire multi-device support but refuse to store plain-text financial data on third-party SaaS servers. | **Cost: Low.** Marketed through content marketing (SEO blogs comparing active vs. passive tracking) and offering free licenses to YouTube tech reviewers who focus on privacy-first software setups. |
| **3\. Standard SaaS** *(Hosted Sync)* | Fully managed local-first synchronization. The backend PostgreSQL server handles real-time data sync across all devices. | **Standard Subscription** (e.g., €4.99/month or €49/year). Provides recurring revenue to stabilize operations. | Mainstream budgeters who desire privacy and "Active Tracking" but lack the technical skills to manage their own synchronization. | **Cost: Moderate/High.** As LTV (Lifetime Value) increases due to subscriptions, CAC (Customer Acquisition Cost) can be higher. Marketed via TikTok/Instagram micro-influencers in the personal finance niche, and targeted ads aimed at users seeking "YNAB alternatives." |

### **Project Timeline (15 Hours / Week)**

Assuming a commitment of 15 hours per week (roughly 60 hours per month), the transition from Phase 1 through to the initial launch of Phase 2 monetization will take approximately 4.5 months. By focusing strictly on PyQt6 and Python backends (and avoiding intensive web frontend development), the timeline remains lean.

* **Month 1: Foundation & Data Modeling (Hours 1–60)**  
  * *Week 1-2:* Apply TDDD37 concepts to design the SQLite relational schema. Map Python dataclasses to the schema using an ORM or native SQL \`\`.  
  * *Week 3-4:* Initialize the PyQt6 repository with a strict .gitignore. Construct the basic MVC architecture, ensuring the View (GUI) accurately reads from the Model (SQLite).  
* **Month 2: The Core "Active Tracking" Engine (Hours 61–120)**  
  * *Week 5-6:* Build the core UI ledgers, form inputs for manual transactions, and the dynamic net-worth calculation algorithms.  
  * *Week 7-8:* Implement the CSV parsing logic and the predictive "Rules Engine" to automate category suggestions.  
* **Month 3: Refinement & FOSS Launch (Hours 121–180)**  
  * *Week 9-10:* Build the UI components for budget tracking (progress bars) and upcoming cash-flow forecasting.  
  * *Week 11-12:* Polish the PyQt6 UI, thoroughly test edge cases (split transactions), and officially release **Tier 1 (FOSS Desktop)** to open-source communities.  
* **Month 4: The Local-First Sync Backend (Hours 181–240)**  
  * *Week 13-14:* Apply TDDD97 concepts to build the Flask REST API backend 3. Define the synchronization endpoints.  
  * *Week 15-16:* Implement the client-side local-first sync logic in PyQt6 to queue offline changes and push to the Flask server upon connection.  
* **Month 5 (First Half): Cloud Deployment & Monetization (Hours 241–270)**  
  * *Week 17-18:* Deploy the PostgreSQL and Flask backend to a hosted environment (e.g., Heroku, DigitalOcean). Implement authentication. Launch **Tier 2 (Sync License)** and **Tier 3 (Hosted SaaS)**. Begin marketing efforts to convert FOSS users to paid tiers.

#### **Citerade verk**

1. TDDD97 \> Lectures \- ida.liu.se, hämtad juni 2, 2026, [https://www.ida.liu.se/\~TDDD97/lectures/index.en.shtml](https://www.ida.liu.se/~TDDD97/lectures/index.en.shtml)  
2. TDDD97 Web Programming \- Syllabus \- Studieinfo, hämtad juni 2, 2026, [https://studieinfo.liu.se/pdf/en/kursplan/TDDD97/vt-2018](https://studieinfo.liu.se/pdf/en/kursplan/TDDD97/vt-2018)  
3. TDDD97 Web Programming \- Studieinfo \- Linköpings universitet, hämtad juni 2, 2026, [https://studieinfo.liu.se/en/kurs/TDDD97/vt-2017](https://studieinfo.liu.se/en/kurs/TDDD97/vt-2017)