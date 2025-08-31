# ShopSphere

Welcome to **ShopSphere**! This project aims to revolutionize the online shopping experience by offering an intuitive and user-friendly platform for both customers and sellers.

---

## 🚀 Project Description

**ShopSphere** is a modern e-commerce application designed to meet the needs of small and medium-sized businesses looking to sell products online. The platform combines cutting-edge technology with a simplified design to make the buying and selling experience more efficient.

- **For Customers**: Browse, compare, and purchase products with ease and security.
- **For Sellers**: Manage inventory, process orders, and track sales in real time.

---

## 🛠️ Tech Stack

This project was built using the following technologies:

- **Front-end**: React.js with Vite
- **Back-end**: Node.js with Express.js
- **Database**: MongoDB
- **Authentication**: JSON Web Token (JWT)
- **Styling**: TailwindCSS
- **Testing**: Jest and React Testing Library
- **Deployment**: Vercel (Front-end) and Render (Back-end)

---

## 📦 Key Features

- **Intuitive Interface**: UX/UI designed for simplicity and efficiency.
- **Secure Login System**: Login and registration with JWT authentication.
- **Product Management**: Easily add, edit, and remove products.
- **Shopping Cart**: Add items to the cart and proceed to checkout.
- **Secure Payments**: Integration with Stripe for online payments.
- **Customer Feedback**: Ratings and reviews for each product.
- **Responsiveness**: Optimized design for mobile and desktop devices.

---

## 🛠️ How to Run the Project

Follow the steps below to run the project locally:

### Prerequisites

Ensure you have the following installed:
- **Node.js** (>= 16.x)
- **MongoDB** (local or cloud)
- **Git**

### Clone the Repository

```bash
git clone https://github.com/DaveSimoes/ShopSphere.git
cd ShopSphere
```

### Install Dependencies

```bash
# For the back-end
cd server
npm install

# For the front-end
cd client
npm install
```

### Configure Environment Variables

Create a `.env` file in the `server` folder with the following variables:

```env
MONGO_URI=your-mongodb-uri
JWT_SECRET=your-secret-key
STRIPE_KEY=your-stripe-key
```

### Run the Project

```bash
# Run the back-end
cd server
npm run dev

# Run the front-end
cd client
npm run dev
```

The front-end will be available at [http://localhost:5173](http://localhost:5173) and the back-end at [http://localhost:5000](http://localhost:5000).

---

## 📸 Screenshots

### Homepage
![Homepage](https://via.placeholder.com/800x400.png?text=Homepage+Preview)

### Product Page
![Product Page](https://via.placeholder.com/800x400.png?text=Product+Page+Preview)

### Shopping Cart
![Cart](https://via.placeholder.com/800x400.png?text=Cart+Preview)

---

## 🤝 Contributing

Contributions are welcome! Follow the steps below to contribute to the project:

1. Fork the repository.
2. Create a branch for your feature (`git checkout -b my-feature`).
3. Commit your changes (`git commit -m 'Add my feature'`).
4. Push to the branch (`git push origin my-feature`).
5. Open a Pull Request.

---

## 📝 License

This project is licensed under the [MIT License](LICENSE).

---

## 🌟 Acknowledgments

Thank you for checking out **ShopSphere**! If you enjoyed the project, consider leaving a ⭐ on the repository.
