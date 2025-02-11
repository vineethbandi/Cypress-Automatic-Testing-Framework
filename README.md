# 🚀 Cypress Automation Framework

A comprehensive testing framework built with Cypress for UI, API, and performance testing. This framework provides a structured approach to automated testing with built-in utilities, reporting, and best practices.

## ✨ Features

- **UI Testing**: Page Object Model implementation for maintainable UI tests
- **API Testing**: Reusable utilities for API automation
- **Performance Metrics**: Basic performance monitoring capabilities
- **Reporting**: Integrated test reporting with screenshots
- **Data-Driven**: Support for external test data
- **Cross-browser Testing**: Run tests across multiple browsers

## 🛠️ Prerequisites

- Node.js (v14 or higher)
- npm (v6 or higher)
- Git

## 📦 Installation

1. Clone the repository:
```bash
git clone https://github.com/yourusername/cypress-automation-framework.git
cd cypress-automation-framework
```

2. Install dependencies:
```bash
npm install
```

3. Install required plugins:
```bash
npm install -D cypress-xpath
npm install -D @cypress/xpath
npm install -D cypress-mochawesome-reporter
```

## 🏗️ Project Structure

```
cypress/
├── e2e/
│   ├── ui/
│   │   ├── login/
│   │   │   └── TestLogin.cy.js
│   │   └── products/
│   │       └── TestProducts.cy.js
│   └── api/
│       └── TestAPI.cy.js
├── fixtures/
│   ├── testData.json
│   └── apiTestData.json
├── pageObjects/
│   ├── LoginPage.js
│   └── ProductsPage.js
├── support/
│   ├── commands.js
│   ├── e2e.js
│   └── apiUtils.js
├── downloads/
├── screenshots/
└── videos/
```

## ⚙️ Configuration

### Base Configuration (cypress.config.js)

```javascript
const { defineConfig } = require('cypress')

module.exports = defineConfig({
  e2e: {
    baseUrl: 'https://your-application-url.com',
    defaultCommandTimeout: 10000,
    pageLoadTimeout: 60000,
    viewportWidth: 1280,
    viewportHeight: 720,
    video: true,
    screenshotOnRunFailure: true,
    reporter: 'cypress-mochawesome-reporter',
    reporterOptions: {
      charts: true,
      reportPageTitle: 'Cypress Automation Report',
      embeddedScreenshots: true
    }
  }
})
```

### API Configuration (cypress/fixtures/apiConfig.json)

```json
{
  "baseUrl": "https://mobilestore-c8yg.onrender.com",
  "endpoints": {
    "login": "/api/login",
    "products": "/api/products",
    "orders": "/api/orders"
  }
}
```

## 📝 Code Examples

### Page Object Example (cypress/pageObjects/LoginPage.js)

```javascript
class LoginPage {
    elements = {
        usernameInput: () => cy.get('#username'),
        passwordInput: () => cy.get('#password'),
        loginButton: () => cy.get('#login-button'),
        errorMessage: () => cy.get('.error-message')
    }

    visit() {
        cy.visit('/login')
    }

    login(username, password) {
        this.elements.usernameInput().type(username)
        this.elements.passwordInput().type(password)
        this.elements.loginButton().click()
    }

    verifyErrorMessage(message) {
        this.elements.errorMessage().should('have.text', message)
    }
}

export default new LoginPage()
```

### UI Test Example (cypress/e2e/ui/login/TestLogin.cy.js)

```javascript
import LoginPage from '../../../pageObjects/LoginPage'

describe('Login Functionality', () => {
    beforeEach(() => {
        LoginPage.visit()
    })

    it('should login successfully with valid credentials', () => {
        LoginPage.login('validUser', 'validPassword')
        cy.url().should('include', '/dashboard')
    })

    it('should show error message with invalid credentials', () => {
        LoginPage.login('invalidUser', 'invalidPassword')
        LoginPage.verifyErrorMessage('Invalid credentials')
    })
})
```

### API Test Example (cypress/e2e/api/TestAPI.cy.js)

```javascript
import apiConfig from '../../fixtures/apiConfig.json'
import { generateAuthToken } from '../../support/apiUtils'

describe('API Tests', () => {
    let authToken

    before(() => {
        authToken = generateAuthToken()
    })

    it('should retrieve products successfully', () => {
        cy.request({
            method: 'GET',
            url: `${apiConfig.baseUrl}${apiConfig.endpoints.products}`,
            headers: {
                'Authorization': `Bearer ${authToken}`
            }
        }).then((response) => {
            expect(response.status).to.eq(200)
            expect(response.body).to.have.property('products')
        })
    })
})
```

## 🚀 Running Tests

### Run All Tests

```bash
npm run test
```

### Run UI Tests Only

```bash
npm run uitest
```

### Run API Tests Only

```bash
npm run apitest
```

### Run Tests in Specific Browser

```bash
npm run test:chrome
npm run test:firefox
```

### Run Tests in Headless Mode

```bash
npm run test:headless
```

## 📊 Reports

Test reports are generated after each test run and can be found in the `cypress/reports` directory. To open the latest report:

```bash
npm run open:report
```

## 🔄 CI/CD Integration

### GitHub Actions Workflow Example (.github/workflows/cypress.yml)

```yaml
name: Cypress Tests

on: [push, pull_request]

jobs:
  cypress-run:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v2
      
      - name: Cypress run
        uses: cypress-io/github-action@v2
        with:
          browser: chrome
          headless: true
```

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch: `git checkout -b feature/amazing-feature`
3. Commit your changes: `git commit -m 'Add amazing feature'`
4. Push to the branch: `git push origin feature/amazing-feature`
5. Open a pull request

## 📝 Best Practices

- Use page objects for UI element locators and common actions
- Implement proper waiting strategies instead of hard delays
- Use data-testid attributes for element selection when possible
- Keep tests independent and atomic
- Use before/beforeEach hooks for test setup
- Implement proper error handling and logging
- Use meaningful test descriptions

## 🚨 Known Issues

- API endpoints hosted on render.com may experience intermittent availability issues
- Alternative API endpoint: https://mobilestore-c8yg.onrender.com

## 📄 License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## 👥 Support

For support and questions, please open an issue in the repository or contact the maintainers.
