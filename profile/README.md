# Smart Restaurant Platform

**A Multi-Tenant QR-Based Restaurant Ordering System**

[![Version](https://img.shields.io/badge/version-1.0-blue.svg)]()
[![Status](https://img.shields.io/badge/status-completed-green.svg)]()
[![License](https://img.shields.io/badge/license-MIT-green.svg)]()
[![Academic](https://img.shields.io/badge/project-academic-purple.svg)]()

---

## Overview

**Smart Restaurant** is a lightweight, multi-tenant platform designed to modernize the dine-in experience for small and medium restaurants. By leveraging QR code technology, the system enables customers to browse menus, customize orders, and place them directly from their mobile devices — reducing wait times and improving operational efficiency.

### Platform Architecture

| Application | Description | Primary Users |
|-------------|-------------|---------------|
| **restaurant-staff** | Staff-facing web application for order management, menu configuration, and payment processing | Admin, Waiter, Chef |
| **restaurant-customer** | Customer-facing mobile & tablet web app for QR-based menu browsing and ordering | Diners |
| **restaurant-db** | Shared PostgreSQL database with documentation and seed data utilities | Developers |

---

## Problem Statement

Many small and medium restaurants lack a simple, affordable way to offer mobile ordering from the table. Customers wait for staff to take orders, causing slower service and lost incremental revenue. Existing solutions are often fragmented (paper menus, generic POS systems) and don't provide a seamless dine-in QR ordering flow.

---

## Target Users

| User Type | Description |
|-----------|-------------|
| **Restaurant Owners/Managers** | Independent operators seeking a low-friction ordering solution for single or few-location restaurants |
| **Diners** | Customers who prefer faster service and contactless ordering experiences |
| **Restaurant Staff** | Waiters and kitchen staff who process and fulfill orders |

---

## Core Features

### Multi-Tenancy
Each restaurant operates as an isolated tenant with its own:
- Menu catalog and pricing
- Staff accounts with role-based access
- Tables and unique QR codes
- Custom settings (tax rates, service charges, discounts)

### QR-Based Table Ordering
- Generate unique, signed QR codes per table
- Customers scan to instantly access tenant-branded menus
- Secure token-based authentication with expiration

### Menu Management
- Organize items into categories with display ordering
- Rich dish details: images, descriptions, preparation times
- Flexible modifier system (sizes, toppings, add-ons) with dynamic pricing
- Availability and visibility controls

### Order Processing
```
[Create] → Unsubmit → Approved → Pending → Completed → Served → Paid
                |          |          |
                └──────────┴──────────┴────────→ Cancelled
```

### Payment Processing
- Support for Cash, Card, and E-Wallet methods
- Automatic calculation of subtotals, discounts, taxes, and service charges
- Detailed payment breakdown and receipt generation

---

## Database Architecture

The platform uses a shared **PostgreSQL** database with **17 core tables** organized into three layers:

### Core System (6 tables)
`tenants` · `platform_users` · `users` · `customers` · `app_settings` · `tables`

### Menu & Catalog (8 tables)
`categories` · `dishes` · `menu_item_photos` · `modifier_groups` · `modifier_options` · `menu_item_modifier_groups` · `reviews` · `dish_ratings`

### Orders & Operations (4 tables)
`orders` · `order_details` · `order_item_modifiers` · `payments`

### Entity Relationships
```
tenants (Restaurant)
    │
    ├── tables ──────────────→ orders (current_order_id)
    │                              │
    ├── dishes                     ├── order_details
    │      │                       │       │
    └── modifier_groups            │       └── order_item_modifiers
           │                       │                   │
           └── modifier_options ───┴───────────────────┘
```

---

##  User Roles & Permissions

| Role | Access Level |
|------|--------------|
| **Super Admin** | Platform-wide management across all tenants |
| **Admin** | Full access to tenant settings, menu, staff, and analytics |
| **Waiter** | Order management, table service, payment processing |
| **Chef** | Kitchen display, item preparation status updates |

---

## Success Metrics

| Metric | Target |
|--------|--------|
| Onboarding Activation | 70% of tenants complete setup within 7 days |
| QR Conversion Rate | 10% of QR scans convert to completed orders |
| Time-to-Serve | Median under 20 minutes for standard items |

---

## Repository Structure

```
Smart_restaurant/
│
├── restaurant-staff/           # Staff application (Admin/Waiter/Chef)
│   ├── frontend/               # React-based admin dashboard
│   └── backend/                # Node.js/Express API server
│
├── restaurant-customer/        # Customer mobile web application
│   ├── frontend/               # React-based customer interface
│   └── backend/                # Node.js/Express API server
│
├── restaurant-db/              # Database resources
│   ├── docs/                   # Technical documentation
│   └── seed-data/              # Data seeding utilities
│
└── .github/
    ├── docs/                   # Project documentation
    ├── diagrams/               # Mermaid-based flow diagrams
    └── ONE_PAGER.md            # Product one-pager
```

---

## Documentation

### Technical Documentation
| Document | Description |
|----------|-------------|
| [Database Description](./docs/Db-description.md) | Complete table specifications and field definitions |
| [Business Logic](./docs/Business_Logic.md) | Detailed business rules, calculations, and validation |
| [ERD](./docs/ERD.md) | Entity Relationship Diagram (Mermaid) |

### Flow Diagrams
| Diagram | Description |
|---------|-------------|
| [User Journeys](./diagrams/user-journeys.md) | Customer, Admin, and Staff journey maps |
| [System Architecture](./diagrams/system-architecture.md) | High-level architecture overview |
| [Ordering Flow](./diagrams/ordering-flow.md) | End-to-end customer ordering sequence |
| [QR Generation Flow](./diagrams/qr-generation-flow.md) | QR code generation and validation |
| [Order State Machine](./diagrams/order-state-machine.md) | Order lifecycle and state transitions |

### Product Documentation
| Document | Description |
|----------|-------------|
| [One-Pager](./ONE_PAGER.md) | Product vision, problem statement, and MVP scope |

---

## Tech Stack

| Layer | Technologies |
|-------|--------------|
| **Frontend** | React, Vite, CSS Modules |
| **Backend** | Node.js, Express.js |
| **Database** | PostgreSQL (Supabase) |
| **Authentication** | JWT, OAuth 2.0 (Google) |
| **Hosting** | TBD |

---

## Getting Started

### Prerequisites
- Node.js (Latest LTS version)
- PostgreSQL database (Supabase recommended)
- npm or yarn package manager

### Quick Start
```bash
# Clone the repository
git clone https://github.com/your-org/smart-restaurant.git

# Navigate to desired application
cd restaurant-staff   # or restaurant-customer

# Install dependencies
npm install

# Configure environment variables
cp .env.example .env

# Start development server
npm run dev
```

---

## Roadmap

### MVP (Current)
- [x] Multi-tenant architecture
- [x] Menu management with modifiers
- [x] QR-based table ordering
- [x] Order lifecycle management
- [x] Basic payment processing

### Future Enhancements
- [ ] Payment gateway integration (Stripe, VNPay)
- [ ] Tenant onboarding wizard
- [ ] Multi-location management
- [ ] Advanced analytics dashboard
- [ ] Push notifications

---

## License

This project is licensed under the **MIT License** - see the [LICENSE](../LICENSE) file for details.

This is an **academic project** developed for educational purposes. You are free to use, modify, and distribute this project in accordance with the MIT License terms.

---

## 👨‍💻 Contributors

This project was developed by a dedicated team of student developers:

| Name | Role | Responsibilities | Contact |
|------|------|------------------|--------|
| **Nguyen Phuc Hau** | Project Manager, Fullstack Developer | Project management, Frontend & Backend development, Code review | [phuchau.2005.vlg@gmail.com](mailto:phuchau.2005.vlg@gmail.com) |
| **Pham Quang Vinh** | Frontend Developer | User interface development, Customer & Staff app frontend | [vinhp1546@gmail.com](mailto:vinhp1546@gmail.com) |
| **Nguyen Van Binh Duong** | Backend Developer, DBA | Backend API development, Database design & administration, Documentation | [ngduong5124@gmail.com](mailto:ngduong5124@gmail.com) |

---

## Acknowledgments

- Thanks to all the open-source libraries and tools that made this project possible
- Special thanks to our mentors and instructors for their guidance
- Built with React, Node.js, Express, and PostgreSQL (Supabase)

---

<div align="center">

**Smart Restaurant Platform**

*Modernizing the Dine-In Experience*

---

Made with ❤️ by the Smart Restaurant Team

*Academic Project · January 2026*

</div>
