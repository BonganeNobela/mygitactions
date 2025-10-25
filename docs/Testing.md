# Testing Documentation

## Setup

1. **Install dependencies**

npm install --save-dev jest @testing-library/react @testing-library/jest-dom supertest

2. **Add test scripts to `package.json`**
```
"scripts": {

  "test": "jest --watchAll", 

  "test:ui": "jest --watchAll --testPathPattern=frontend", 

  "test:api": "jest --watchAll --testPathPattern=backend" 
}
```

3. **Create `jest.config.js`**
```
export default { 

    testEnvironment: "jsdom", 

    moduleFileExtensions: ["js", "jsx", "json"], 

    transform: { 

        "^.+\\.(js|jsx)$": "babel-jest", 

    }, 
    setupFilesAfterEnv: ["@testing-library/jest-dom"], 
};
```
---

## Running Tests

- Run all tests: `npm test`  
- Run UI tests only: `npm run test:ui`  
- Run API tests only: `npm run test:api`  
- Run a single test file: `npx jest filename.js`  

---

## UI Testing

- Validate components, forms, and user flows.  
- Frontend uses **React Testing Library** with Jest to test UI components.  
- Tests include checking:  
  - Dropdowns render correctly  
  - Users can select values  
  - Form flows work correctly  

---

## API Testing

- Use **Jest** to test API endpoints.  
- Example: Matchmaking API tests verify:  
  - API responds with matches correctly  
  - Errors are handled properly
