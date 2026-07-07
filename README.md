# 🌿 Paradise Nursery Shopping Cart Application

> *Where Green Meets Serenity*

A fully functional e-commerce shopping cart application built with React and Redux Toolkit, allowing users to browse and purchase a wide variety of plants across multiple categories.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [How It Works](#how-it-works)
- [Components](#components)
- [Redux State Management](#redux-state-management)
- [Deployment](#deployment)
- [Screenshots](#screenshots)

---

## Overview

Paradise Nursery is a React-based plant shopping application where users can browse plants organized into five categories, add them to a cart, and manage their selections before checkout. The application uses Redux Toolkit to manage global cart state, ensuring real-time updates across all components.

---

## Features

- 🌱 Browse **30 plants** across 5 curated categories
- 🛒 Add plants to a global shopping cart
- ➕ Increment or decrement item quantities in the cart
- 🗑️ Remove individual items from the cart
- 💰 Real-time per-item subtotal and overall cart total calculation
- 🔢 Live cart item count badge on the cart icon
- ✅ "Added to Cart" button state — disables and grays out after a plant is added
- 🔄 Continue Shopping navigation between the plant listing and cart pages
- 📱 Responsive card-based layout for plant listings

---

## Tech Stack

| Technology | Purpose |
|---|---|
| React 18 | UI component library |
| Redux Toolkit | Global state management |
| React-Redux | Connecting React components to the Redux store |
| CSS | Component-level styling |
| GitHub Pages | Deployment |

---

## Project Structure

```
src/
├── main.jsx               # App entry point; wraps <App> in Redux <Provider>
├── App.jsx                # Root component; manages page-level routing
├── store.js               # Redux store configuration
├── CartSlice.jsx          # Redux slice: cart state + reducers (addItem, removeItem, updateQuantity)
├── ProductList.jsx        # Plant listing page with Add to Cart functionality
├── ProductList.css        # Styles for the product listing page
├── CartItem.jsx           # Shopping cart page with quantity controls and totals
└── CartItem.css           # Styles for the cart page
```

---

## Getting Started

### Prerequisites

- Node.js (v16 or higher)
- npm or yarn

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/<your-username>/<your-repo-name>.git
   cd <your-repo-name>
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Start the development server**
   ```bash
   npm run dev
   ```

4. **Open your browser** and navigate to `http://localhost:5173`

---

## How It Works

### Adding a Plant to the Cart

1. User browses the plant listing page (`ProductList.jsx`).
2. User clicks **Add to Cart** on a plant card.
3. `handleAddToCart(plant)` is called, which:
   - Dispatches `addItem(plant)` to the Redux store.
   - Updates local `addedToCart` state so the button becomes disabled and changes label to "Added to Cart".
4. The cart icon badge updates immediately with the new total quantity.

### Managing Cart Items

Inside the cart page (`CartItem.jsx`):

- **`+` button** → dispatches `updateQuantity` with `quantity + 1`
- **`-` button** → dispatches `updateQuantity` with `quantity - 1`, or `removeItem` if quantity would reach 0
- **Delete button** → dispatches `removeItem` to remove the plant entirely
- All totals (per-item and overall) recalculate automatically on every state change

---

## Components

### `ProductList.jsx`

The main shopping page. Responsibilities:
- Renders the navbar with the Paradise Nursery branding and cart icon
- Maps over `plantsArray` to render a card for every plant in every category
- Handles adding plants to the Redux cart via `handleAddToCart`
- Tracks which plants have been added locally (`addedToCart` state) to disable buttons
- Reads live cart item count from Redux and displays it as a badge on the cart icon
- Conditionally renders either the plant listing or the `<CartItem />` cart page

### `CartItem.jsx`

The shopping cart page. Responsibilities:
- Reads `state.cart.items` from Redux via `useSelector`
- Renders a card for each cart item with its image, name, unit cost, quantity controls, and subtotal
- Calculates and displays the overall cart total
- Handles increment, decrement, and remove actions by dispatching Redux actions
- Provides "Continue Shopping" (returns to plant listing) and "Checkout" (placeholder alert) buttons

### `CartSlice.jsx`

The Redux slice that owns all cart state. Contains three reducers:

| Reducer | Payload | Behaviour |
|---|---|---|
| `addItem` | plant object `{ name, image, cost }` | Adds plant with `quantity: 1`, or increments quantity if already in cart |
| `removeItem` | plant name string | Filters out the matching item from `state.items` |
| `updateQuantity` | `{ name, quantity }` | Finds the matching item and sets its quantity to the new value |

### `store.js`

Configures the Redux store using `configureStore` from Redux Toolkit. Registers `cartReducer` under the `cart` key, making `state.cart.items` available globally.

---

## Redux State Management

### State Shape

```js
{
  cart: {
    items: [
      {
        name: "Aloe Vera",
        image: "https://...",
        cost: "$14",
        quantity: 2
      },
      // ...more items
    ]
  }
}
```

### Data Flow

```
User clicks "Add to Cart"
        ↓
handleAddToCart(plant) in ProductList.jsx
        ↓
dispatch(addItem(plant))
        ↓
CartSlice.jsx addItem reducer runs
        ↓
state.cart.items updated
        ↓
All useSelector subscribers re-render:
  → Cart icon badge count updates
  → CartItem totals update
```

### Provider Setup (`main.jsx`)

```jsx
import { Provider } from 'react-redux';
import store from './store.js';

<Provider store={store}>
  <App />
</Provider>
```

The `<Provider>` wrapper makes the Redux store accessible to every component in the tree via `useSelector` and `useDispatch` — no prop drilling required.

---

## Plant Categories

| Category | Plants |
|---|---|
| 🌬️ Air Purifying Plants | Snake Plant, Spider Plant, Peace Lily, Boston Fern, Rubber Plant, Aloe Vera |
| 🌸 Aromatic Fragrant Plants | Lavender, Jasmine, Rosemary, Mint, Lemon Balm, Hyacinth |
| 🦟 Insect Repellent Plants | Oregano, Marigold, Geraniums, Basil, Lavender, Catnip |
| 💊 Medicinal Plants | Aloe Vera, Echinacea, Peppermint, Lemon Balm, Chamomile, Calendula |
| 🪴 Low Maintenance Plants | ZZ Plant, Pothos, Snake Plant, Cast Iron Plant, Succulents, Aglaonema |

---

## Deployment

This application is deployed using **GitHub Pages**.

### Deploy Steps

1. **Install the GitHub Pages package**
   ```bash
   npm install gh-pages --save-dev
   ```

2. **Add to `package.json`**
   ```json
   "homepage": "https://<your-username>.github.io/<your-repo-name>",
   "scripts": {
     "predeploy": "npm run build",
     "deploy": "gh-pages -d dist"
   }
   ```

3. **Deploy**
   ```bash
   npm run deploy
   ```

4. Visit `https://<your-username>.github.io/<your-repo-name>` — note it may take a few minutes for images and content to fully load on first deploy.

---

## Key Concepts Learned

- **React Hooks**: `useState` for local UI state, `useEffect` for side effects
- **Redux Toolkit**: `createSlice`, `configureStore`, action creators, and reducers using Immer for safe state mutation
- **React-Redux Hooks**: `useSelector` to read from the store, `useDispatch` to send actions
- **Array Methods**: `.map()` for rendering lists, `.filter()` for removing items, `.reduce()` for computing totals, `.find()` for locating items
- **Component Architecture**: Lifting state to Redux so sibling components (ProductList and CartItem) share the same source of truth

---

## License

This project was created as part of a front-end development learning exercise.

---

*Built with 💚 by Paradise Nursery*