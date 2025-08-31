# Contributing Guide

Welcome to ShopSphere! We appreciate your interest in contributing to our e-commerce platform. This guide will help you get started with contributing to the project, from setting up your development environment to submitting your changes.

## Table of Contents

- [Getting Started](#getting-started)
- [Development Environment Setup](#development-environment-setup)
- [Project Structure](#project-structure)
- [Coding Standards](#coding-standards)
- [Development Workflow](#development-workflow)
- [Testing Guidelines](#testing-guidelines)
- [Commit Guidelines](#commit-guidelines)
- [Pull Request Process](#pull-request-process)
- [Code Review Guidelines](#code-review-guidelines)
- [Getting Help](#getting-help)

## Getting Started

### Prerequisites

Before you begin, ensure you have the following installed on your system:

- **Node.js** (>= 16.x) - [Download here](https://nodejs.org/)
- **npm** (comes with Node.js) or **yarn**
- **MongoDB** - Local installation or MongoDB Atlas account
- **Git** - [Download here](https://git-scm.com/)
- **Code Editor** - We recommend VS Code with the following extensions:
  - ES7+ React/Redux/React-Native snippets
  - Prettier - Code formatter
  - ESLint
  - Tailwind CSS IntelliSense
  - GitLens

### Fork and Clone the Repository

1. Fork the repository on GitHub
2. Clone your fork locally:
   ```bash
   git clone https://github.com/your-username/ShopSphere-ecommerce.git
   cd ShopSphere-ecommerce
   ```
3. Add the original repository as an upstream remote:
   ```bash
   git remote add upstream https://github.com/DaveSimoes/ShopSphere-ecommerce.git
   ```

## Development Environment Setup

### 1. Install Dependencies

Install dependencies for both frontend and backend:

```bash
# Install backend dependencies
cd server
npm install

# Install frontend dependencies
cd ../client
npm install
```

### 2. Environment Configuration

Create a `.env` file in the `server` directory with the following variables:

```env
# Database
MONGO_URI=your-mongodb-connection-string

# Authentication
JWT_SECRET=your-jwt-secret-key

# Payment Processing
STRIPE_SECRET_KEY=your-stripe-secret-key
STRIPE_PUBLISHABLE_KEY=your-stripe-publishable-key

# Server Configuration
PORT=5000
NODE_ENV=development

# Client URL (for CORS)
CLIENT_URL=http://localhost:5173
```

### 3. Database Setup

If using local MongoDB:
```bash
# Start MongoDB service
sudo systemctl start mongod  # Linux
brew services start mongodb-community@6.0  # macOS
```

If using MongoDB Atlas, ensure your connection string is correctly set in the `.env` file.

### 4. Start Development Servers

Run both frontend and backend in development mode:

```bash
# Terminal 1 - Backend
cd server
npm run dev

# Terminal 2 - Frontend
cd client
npm run dev
```

The application will be available at:
- Frontend: http://localhost:5173
- Backend API: http://localhost:5000

## Project Structure

Understanding the project structure will help you navigate and contribute effectively:

```
ShopSphere-ecommerce/
├── client/                 # React frontend application
│   ├── src/
│   │   ├── components/     # Reusable UI components
│   │   ├── pages/         # Page components
│   │   ├── hooks/         # Custom React hooks
│   │   ├── services/      # API service functions
│   │   ├── utils/         # Utility functions
│   │   ├── context/       # React context providers
│   │   └── styles/        # Global styles
│   ├── public/            # Static assets
│   └── package.json
├── server/                # Node.js backend application
│   ├── routes/            # Express route definitions
│   ├── controllers/       # Request handlers
│   ├── models/           # Mongoose schemas
│   ├── middleware/       # Custom middleware
│   ├── config/           # Configuration files
│   ├── utils/            # Utility functions
│   └── package.json
├── docs/                 # Project documentation
└── README.md
```

## Coding Standards

### General Guidelines

- Use English for all code, comments, and documentation
- Write self-documenting code with clear variable and function names
- Keep functions small and focused on a single responsibility
- Use consistent indentation (2 spaces for JavaScript/React, 2 spaces for CSS)
- Remove unused imports and variables
- Use meaningful commit messages

### JavaScript/React Standards

- Use ES6+ features (arrow functions, destructuring, template literals)
- Prefer functional components over class components
- Use hooks for state management and side effects
- Follow the principle of composition over inheritance
- Use PropTypes or TypeScript for type checking (when applicable)

#### Example Component Structure:
```jsx
import React, { useState, useEffect } from 'react';
import PropTypes from 'prop-types';

const ProductCard = ({ product, onAddToCart }) => {
  const [isLoading, setIsLoading] = useState(false);

  const handleAddToCart = async () => {
    setIsLoading(true);
    try {
      await onAddToCart(product.id);
    } catch (error) {
      console.error('Failed to add to cart:', error);
    } finally {
      setIsLoading(false);
    }
  };

  return (
    <div className="bg-white rounded-lg shadow-md p-4">
      <h3 className="text-lg font-semibold">{product.name}</h3>
      <p className="text-gray-600">${product.price}</p>
      <button 
        onClick={handleAddToCart}
        disabled={isLoading}
        className="mt-2 bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600 disabled:opacity-50"
      >
        {isLoading ? 'Adding...' : 'Add to Cart'}
      </button>
    </div>
  );
};

ProductCard.propTypes = {
  product: PropTypes.shape({
    id: PropTypes.string.isRequired,
    name: PropTypes.string.isRequired,
    price: PropTypes.number.isRequired
  }).isRequired,
  onAddToCart: PropTypes.func.isRequired
};

export default ProductCard;
```

### CSS/Styling Standards

- Use TailwindCSS utility classes for styling
- Group related classes together
- Use responsive design principles (mobile-first)
- Create custom components for repeated patterns
- Follow TailwindCSS best practices for maintainability

### Node.js/Express Standards

- Use async/await instead of callbacks
- Implement proper error handling with try-catch blocks
- Use middleware for cross-cutting concerns
- Follow RESTful API conventions
- Validate and sanitize all input data

#### Example API Route:
```javascript
const express = require('express');
const { body, validationResult } = require('express-validator');
const Product = require('../models/Product');
const auth = require('../middleware/auth');

const router = express.Router();

// GET /api/products
router.get('/', async (req, res) => {
  try {
    const { page = 1, limit = 10, category } = req.query;
    const query = category ? { category } : {};
    
    const products = await Product.find(query)
      .limit(limit * 1)
      .skip((page - 1) * limit)
      .sort({ createdAt: -1 });

    res.json({
      success: true,
      data: products,
      pagination: {
        page: parseInt(page),
        limit: parseInt(limit)
      }
    });
  } catch (error) {
    console.error('Error fetching products:', error);
    res.status(500).json({
      success: false,
      message: 'Internal server error'
    });
  }
});

module.exports = router;
```

## Development Workflow

### 1. Create a Feature Branch

```bash
git checkout -b feature/your-feature-name
```

Use descriptive branch names:
- `feature/add-user-profile`
- `fix/cart-calculation-bug`
- `docs/update-contributing-guide`

### 2. Make Your Changes

- Follow the coding standards outlined above
- Write tests for new functionality
- Update documentation if necessary
- Test your changes thoroughly

### 3. Run Quality Checks

Before committing, ensure your code passes all quality checks:

```bash
# Run linting
npm run lint

# Run tests
npm run test

# Check formatting
npm run format:check
```

### 4. Commit Your Changes

Follow the commit message guidelines (see below) and commit your changes:

```bash
git add .
git commit -m "feat: add user profile page with avatar upload"
```

## Testing Guidelines

### Frontend Testing

- Write unit tests for utility functions
- Write component tests using React Testing Library
- Focus on testing user interactions and behavior
- Mock external API calls

Example component test:
```javascript
import { render, screen, fireEvent } from '@testing-library/react';
import ProductCard from '../ProductCard';

const mockProduct = {
  id: '1',
  name: 'Test Product',
  price: 29.99
};

test('calls onAddToCart when button is clicked', () => {
  const mockOnAddToCart = jest.fn();
  
  render(
    <ProductCard product={mockProduct} onAddToCart={mockOnAddToCart} />
  );
  
  const addButton = screen.getByText('Add to Cart');
  fireEvent.click(addButton);
  
  expect(mockOnAddToCart).toHaveBeenCalledWith('1');
});
```

### Backend Testing

- Write unit tests for controllers and utility functions
- Write integration tests for API endpoints
- Test error handling scenarios
- Use meaningful test descriptions

Example API test:
```javascript
const request = require('supertest');
const app = require('../app');

describe('GET /api/products', () => {
  test('should return products with pagination', async () => {
    const response = await request(app)
      .get('/api/products?page=1&limit=5')
      .expect(200);

    expect(response.body.success).toBe(true);
    expect(response.body.data).toBeInstanceOf(Array);
    expect(response.body.pagination).toEqual({
      page: 1,
      limit: 5
    });
  });
});
```

## Commit Guidelines

We follow the [Conventional Commits](https://www.conventionalcommits.org/) specification:

### Commit Message Format

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

### Types

- `feat`: A new feature
- `fix`: A bug fix
- `docs`: Documentation only changes
- `style`: Changes that do not affect the meaning of the code
- `refactor`: A code change that neither fixes a bug nor adds a feature
- `perf`: A code change that improves performance
- `test`: Adding missing tests or correcting existing tests
- `chore`: Changes to the build process or auxiliary tools

### Examples

```bash
feat: add product search functionality
fix: resolve cart total calculation error
docs: update API documentation for user endpoints
style: format code according to prettier rules
refactor: extract payment logic into separate service
```

## Pull Request Process

### 1. Update Your Branch

Before creating a pull request, ensure your branch is up to date:

```bash
git fetch upstream
git rebase upstream/main
```

### 2. Push Your Changes

```bash
git push origin feature/your-feature-name
```

### 3. Create a Pull Request

1. Go to the repository on GitHub
2. Click "New Pull Request"
3. Select your branch
4. Fill out the pull request template

### Pull Request Template

```markdown
## Description
Brief description of the changes made.

## Type of Change
- [ ] Bug fix (non-breaking change which fixes an issue)
- [ ] New feature (non-breaking change which adds functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to not work as expected)
- [ ] Documentation update

## Testing
- [ ] Unit tests pass
- [ ] Integration tests pass
- [ ] Manual testing completed

## Screenshots (if applicable)
Add screenshots to help explain your changes.

## Checklist
- [ ] Code follows the style guidelines
- [ ] Self-review of code completed
- [ ] Code is commented, particularly in hard-to-understand areas
- [ ] Documentation has been updated
- [ ] Tests have been added/updated
```

## Code Review Guidelines

### For Authors

- Provide clear descriptions of changes
- Respond promptly to feedback
- Be open to suggestions and criticism
- Update code based on review comments

### For Reviewers

- Be constructive and specific in feedback
- Focus on code quality, performance, and maintainability
- Check for adherence to coding standards
- Verify that tests cover the changes
- Approve when satisfied with the quality

## Getting Help

If you need help or have questions:

1. **Documentation**: Check the existing documentation in the `docs/` folder
2. **Issues**: Search existing issues on GitHub
3. **Discussions**: Start a discussion on GitHub Discussions
4. **Code Comments**: Look for inline comments in the codebase
5. **README**: Review the main README.md file

## Additional Resources

- [React Documentation](https://reactjs.org/docs)
- [Express.js Guide](https://expressjs.com/en/guide/routing.html)
- [MongoDB Documentation](https://docs.mongodb.com/)
- [TailwindCSS Documentation](https://tailwindcss.com/docs)
- [Jest Testing Framework](https://jestjs.io/docs/en/getting-started)

Thank you for contributing to ShopSphere! Your efforts help make this platform better for everyone.