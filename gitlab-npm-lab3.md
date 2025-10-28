# Package Manager Integration in GitLab CI/CD Pipeline (npm)

## Aim
To demonstrate the use of a package manager (npm) in a CI/CD pipeline by:
- Automatically installing project dependencies during build execution in GitLab CI

## Requirements
- GitLab account and repository
- GitLab Runner configured
- Node.js and npm installed on the runner
- A sample Node.js project (see below)

---

## Sample Node.js Project

### Project Structure
```
my-node-app/
├── package.json
├── index.js
├── test/
│   └── test.js
└── .gitlab-ci.yml
```

### Sample Code Files

#### `package.json`
```json
{
  "name": "my-node-app",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "test": "node test/test.js"
  },
  "dependencies": {},
  "devDependencies": {}
}
```

#### `index.js`
```js
function greet(name) {
  return `Hello, ${name}!`;
}

module.exports = greet;
```

#### `test/test.js`
```js
const greet = require('../index');
const assert = require('assert');

assert.strictEqual(greet('World'), 'Hello, World!');
console.log('Test passed!');
```

#### `.gitlab-ci.yml`
```yaml
stages:
  - install
  - test

install_dependencies:
  stage: install
  script:
    - npm install

run_tests:
  stage: test
  script:
    - npm test
```

---

## Procedure

### Step 1: Create and Push the Project
- Create the above files in a folder named `my-node-app`.
- Initialize a git repository:
  ```sh
  git init
  git add .
  git commit -m "Initial commit"
  git remote add origin <your-gitlab-repo-url>
  git push -u origin master
  ```

### Step 2: Set Up GitLab CI/CD
- Ensure your repository is on GitLab.
- Make sure a GitLab Runner is available (shared or specific).

### Step 3: Pipeline Execution
- On push, GitLab CI will automatically run the pipeline defined in `.gitlab-ci.yml`:
  - **install_dependencies**: Installs npm packages
  - **run_tests**: Runs tests

### Step 4: Verify Results
- Go to your GitLab project > **CI/CD > Pipelines**.
- Check that all stages succeed.
- View logs for npm install and test output.

---

## Expected Outcome
- GitLab CI pipeline runs all stages successfully.
- npm dependencies are installed automatically.
- Tests are executed and pass.

---

## Notes
- This is a minimal Node.js project for demonstration.
- You can expand with more files, dependencies, or advanced tests as needed.
