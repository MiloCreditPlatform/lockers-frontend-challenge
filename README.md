# Rappi Lockers – Frontend & Architecture Challenge

Imagine you are already part of the **Rappi Colombia** engineering team and we want to launch a new module: **Rappi Lockers**.

The idea: a user can choose to receive their order in a **locker near their location** (or where they will be) instead of their usual address. The order is delivered into a locker slot and the user receives a **dynamic access code** and the **slot number** to collect their order.

This challenge focuses on building an **MVP** of:

- An **admin Lockers CMS module**
- A minimal **client experience** to select a locker as delivery address

---

## Scope & expected time

This challenge is intended to be completed in **3–5 effective working days**, as an MVP:

- You do **not** need to cover all edge cases
- The key goal is to see **how you structure a realistic and professional proposal** with limited time

---

## High-level requirements

### Tech expectations

- Frontend built with **Next.js** (TypeScript)
- Public repository in your **personal GitHub account**

Optional but nice to have:

- **Docker** + `docker-compose` to run API + database
- Storybook
- Tests (unit / integration / E2E)
- Any architectural pattern you want to demonstrate (e.g. feature-sliced, clean architecture, etc.)

---

### 1. Locker management (Admin CMS)

**Scenario: Admin creates a new locker**

```gherkin
Scenario: Admin creates a new locker
  Given I am logged in as an admin
  And I am on the "Lockers" management page
  When I create a locker with name "Locker Unicentro"
    And address "Calle 123 #45-67, Bogotá"
    And coordinates latitude "4.123" and longitude "-74.123"
  Then I should see "Locker Unicentro" in the lockers list
    And its address and coordinates should be visible
```

**Scenario: Admin views the lockers list**

```gherkin
Scenario: Admin views the lockers list
  Given at least one locker exists
  When I open the "Lockers" management page
  Then I should see a list of lockers
    And each locker should show its name and address
```

### 2. Selecting a locker as delivery address (Client)

**Expected behavior (high level)**

- The system shows a list of lockers ordered from nearest to farthest based on the user’s location

- The user can select a locker for their order

**Scenario: User sees nearby lockers ordered by distance**

```gherkin
Scenario: User sees nearby lockers ordered by distance
  Given there are multiple lockers registered with different coordinates
  And the user location is set to latitude "4.100" and longitude "-74.100"
  When the user opens the "Deliver to locker" screen
  Then the user should see a list of lockers
    And the list should be ordered from the closest to the farthest
```

**Scenario: User selects a locker for the order**

```gherkin
Scenario: User selects a locker for the order
  Given the user sees a list of nearby lockers
  When the user selects a locker from the list
  Then a new order should be created associated with that locker
    And the user should see a confirmation showing the selected locker
```

### 3. Dynamic code & slot assignment (Locker delivery)

When an order is delivered to a locker:

- A slot number is assigned

- A dynamic access code is generated

You do not need to build a full courier UI; you can simulate the courier action via an API endpoint or a simple admin action.

**Scenario: Courier finishes the delivery in a locker**

```gherkin
Scenario: Courier finishes the delivery in a locker
  Given there is an order associated with a locker
    And the order status is "in progress"
  When the courier marks the order as "delivered in locker"
  Then the system should assign a slot number to the order
    And the system should generate a dynamic access code
    And the order status should change to "completed"
```

You can decide how to generate the dynamic access code (random string, numeric code, etc.).

### 4. User views locker details for a completed order

**Scenario: User views locker details for a completed order**

```gherkin
Scenario: User views locker details for a completed order
  Given there is a completed order associated with a locker
    And the order has a slot number and a dynamic access code
  When the user opens the "My orders" section
    And selects that order
  Then the user should see:
      The locker name
      The locker address
      The slot number
      The dynamic access code
```

You can expose this via a simple “My orders” page or a minimal order details view.

---

## What we will evaluate

We are primarily interested in:

- Architecture & structure
  - How you organize your Next.js app (pages, components, modules)
  - How you structure your backend/API and models

- Domain modeling
  - How you represent lockers, orders, slots, and states

- Code quality
  - Readability, naming, separation of concerns
  - Use of TypeScript types and interfaces

- Developer experience
  - How easy it is to run the project with Docker
  - Clarity of the README and scripts

- UX (basic)

- Clear flows for:
  - Admin managing lockers

  - User selecting a locker and seeing their locker details

You do not need to implement every possible edge case. We value clear, well-structured, and thoughtful solutions.

---

## Deliverables

Create a public GitHub repository in your personal account and include:

- A README with:
  - Short description of the solution

  - High-level architecture (frontend + backend) and key decisions

  - How to run the environment with Docker (docker-compose)

  - How to run the frontend

  - (Optional) How to run tests / Storybook if you include them

- Any additional documentation you consider relevant

**When you are done, please send us the link to your repository.**
