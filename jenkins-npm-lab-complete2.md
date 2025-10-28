# Jenkins Pipeline with Package Manager Integration (npm)

## Aim
To create a Declarative Jenkins Pipeline using a Jenkinsfile that:
- Checks out source code from a Git repository
- Installs dependencies using npm
- Builds and tests the project automatically
- Archives the build artifact

## Requirements
- Jenkins installed and running
- Node.js and npm installed on Jenkins server
- A sample Node.js project in a Git repository

---

## Sample Node.js Project

### 1. Create Project Structure

```
my-node-app/
├── package.json
├── index.js
├── test/
│   └── test.js
└── Jenkinsfile
```

### 2. Sample Code Files

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

#### `Jenkinsfile`
```groovy
pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Build and Test') {
            steps {
                sh 'npm test'
            }
        }
        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: '**/build/**', allowEmptyArchive: true
            }
        }
    }
}
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
  git remote add origin <your-repo-url>
  git push -u origin master
  ```

### Step 2: Create Jenkins Pipeline Job
- Open Jenkins dashboard.
- Click **New Item** > Enter name > Select **Pipeline** > **OK**.
- In job config, set your repo URL under **Pipeline**.

### Step 3: Run the Pipeline
- Click **Build Now** in Jenkins job.
- Watch stages execute: Checkout, Install Dependencies, Build and Test, Archive Artifact.

### Step 4: Verify Results
- All stages should succeed.
- Console output should show npm install and test logs.
- Artifact (if any) archived in Jenkins.

---

## Expected Outcome
- Jenkins pipeline runs all stages successfully.
- npm dependencies are installed.
- Tests are executed and pass.
- Build artifacts are archived.

---

## Notes
- This is a minimal Node.js project for lab demonstration.
- You can expand with more files, dependencies, or advanced tests as needed.
