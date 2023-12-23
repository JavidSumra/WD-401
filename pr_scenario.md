### Handling Code Review Feedback

Imagine you're working on a login function. Here's the initial code:

```javascript
// Original code
function login(username, password) {
  // Some authentication logic
  if (username === "admin" && password === "password123") {
    return "Login successful";
  } else {
    return "Login failed";
  }
}
```

Feedback: "Add input validation to ensure non-empty username and password."

Code Improvement:

```javascript
// Improved code based on feedback
function login(username, password) {
  if (!username || !password) {
    return "Invalid input. Please provide both username and password.";
  }

  // Authentication logic remains unchanged
  if (username === "admin" && password === "password123") {
    return "Login successful";
  } else {
    return "Login failed";
  }
}
```

### Iterative Development Process

![Flow Chart Diagram](<WD-401 Iterative Development.png>)

1. **Initial Implementation:** Develop the initial feature on a feature branch.
2. **Code Review:** Submit a pull request for code review.
3. **Feedback Received:** If feedback is received, go back to the development phase with improvements.
4. **Iterative Development:** Repeat steps 2-3 until the code is approved.
5. **Merge to Develop:** Merge the feature branch into the `develop` branch.

### Resolving Merge Conflicts

Imagine a scenario where two developers modify the same function in a file. Developer A's change:

```javascript
// Developer A's code
function calculateTotal(price, quantity) {
  return price * quantity;
}
```

Developer B's change:

```javascript
// Developer B's code
function calculateTotal(price, quantity) {
  // Add a discount
  return price * quantity * 0.9;
}
```

When merging Developer A's branch into `develop`, a conflict occurs. To resolve:

1. **Identify Conflicts:**
   ```bash
   git pull origin develop
   # Conflicts occur, Git marks conflicted files
   ```

2. **Open Conflicted File:**
   ```javascript
   // Conflicted code
   function calculateTotal(price, quantity) {
     <<<<<<< HEAD
       return price * quantity;
     =======
       // Add a discount
       return price * quantity * 0.9;
     >>>>>>> feature-branch
   }
   ```
   
### Resolving Merge Conflicts in a Signup Process

Imagine the original signup function:

```javascript
// Original code
function signUp(username, email, password) {
  // Some signup logic
  if (validateEmail(email) && validatePassword(password)) {
    // User registration logic
    return `User ${username} successfully registered`;
  } else {
    return "Invalid email or password";
  }
}

function validateEmail(email) {
  // Email validation logic
  // Assume a simple validation for demonstration
  return email.includes("@");
}

function validatePassword(password) {
  // Password validation logic
  // Assume a minimum length for demonstration
  return password.length >= 8;
}
```

Now, Developer A decides to add an additional check for a strong password:

```javascript
// Developer A's code
function signUp(username, email, password) {
  if (validateEmail(email) && validatePassword(password) && hasUpperCase(password)) {
    return `User ${username} successfully registered`;
  } else {
    return "Invalid email or password";
  }
}

function hasUpperCase(str) {
  // Check if the password has at least one uppercase letter
  return /[A-Z]/.test(str);
}
```

Simultaneously, Developer B enhances the email validation logic:

```javascript
// Developer B's code
function signUp(username, email, password) {
  if (isBusinessEmail(email) && validatePassword(password)) {
    return `User ${username} successfully registered`;
  } else {
    return "Invalid email or password";
  }
}

function isBusinessEmail(email) {
  // Check if the email is a business email
  // Assume a simple check for demonstration
  return email.endsWith(".com");
}
```

When merging Developer A's and Developer B's branches into the `develop` branch, a conflict occurs in the `signUp` function.

1. **Identify Conflicts:**
   ```bash
   git pull origin develop
   # Conflicts occur, Git marks conflicted files
   ```

2. **Open Conflicted File (`signup.js`):**
   ```javascript
   // Conflicted code
   function signUp(username, email, password) {
     <<<<<<< HEAD
       if (validateEmail(email) && validatePassword(password) && hasUpperCase(password)) {
     =======
       if (isBusinessEmail(email) && validatePassword(password)) {
     >>>>>>> feature-branch
         return `User ${username} successfully registered`;
       } else {
         return "Invalid email or password";
       }
   ```

3. **Manually Resolve Conflict:**
   ```javascript
   // Resolved code
   function signUp(username, email, password) {
     if (isBusinessEmail(email) && validatePassword(password) && hasUpperCase(password)) {
       return `User ${username} successfully registered`;
     } else {
       return "Invalid email or password";
     }
   }
   ```

4. **Mark as Resolved:**
   ```bash
   git add signup.js
   ```

5. **Continue with Merge:**
   ```bash
   git merge --continue
   ```

### CI/CD Integration


```javascript
test('login function', () => {
  const result = login("admin", "password123");
  expect(result).toBe("Login successful");
});
```


```yaml
on:
  push:
    branches:
      - develop

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v2

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '14'

      - name: Install Dependencies
        run: npm install

      - name: Lint Code
        run: npm run lint

      - name: Run Tests
        run: npm test

```
