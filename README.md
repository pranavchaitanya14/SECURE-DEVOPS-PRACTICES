
## 🔐 Secure DevOps – CI/CD Pipeline with OWASP ZAP

This project demonstrates **Secure DevOps practices** by integrating an automated **security scanning step** into a **CI/CD pipeline** using **GitHub Actions** and **OWASP ZAP**.

The pipeline runs on every push to the `main` branch and performs:

- Build & run of a simple web application
- Automatic **OWASP ZAP Baseline Scan**
- Generation of a **vulnerability report** as a GitHub Actions artifact

---

## 🧩 Objective

> **Integrate security scanning tools like OWASP ZAP into a CI/CD pipeline and produce:**
> - Pipeline configuration
> - Report on identified vulnerabilities

---

## ⚙️ Technologies Used

- **Node.js + Express**
- **OWASP ZAP Baseline Scan**
- **GitHub Actions (CI/CD)**
- **GitHub Artifacts**

---

## 🌐 Application Overview

A very simple **Node.js + Express** web app is used as the scan target.

### `index.html`

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Secure DevOps Demo</title>
  </head>
  <body>
    <h1>Secure DevOps Demo Application</h1>
    <p>This app is used to demonstrate OWASP ZAP security scanning in CI/CD.</p>
  </body>
</html>
````

### `server.js`

```js
const express = require('express');
const app = express();
const port = 3000;

app.use(express.static('.'));

app.listen(port, () => {
  console.log(`App running at http://localhost:${port}`);
});
```

### `package.json`

```json
{
  "name": "secure-devops-task4",
  "version": "1.0.0",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.18.2"
  }
}
```

> The app runs at `http://localhost:3000` inside the GitHub Actions runner and becomes the target for OWASP ZAP.

---

## 🚀 CI/CD Pipeline – GitHub Actions

This CI/CD pipeline is defined in:

```
.github/workflows/security-scan.yml
```

### 🔧 Workflow Configuration

```yaml
name: OWASP ZAP Security Scan

on:
  push:
    branches: [ "main" ]
  workflow_dispatch:

jobs:
  zap_scan:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: "18"

      - name: Install dependencies
        run: npm install

      - name: Start application in background
        run: |
          npm start &
          sleep 15

      - name: Run OWASP ZAP Baseline Scan
        uses: zaproxy/action-baseline@v0.15.0
        with:
          target: "http://localhost:3000"
          cmd_options: "-a"
          allow_issue_writing: false
          artifact_name: "zapreport"
```

---



---

## ▶️ How to Trigger the Pipeline

### ✔ Automatic

Runs automatically on every push to the `main` branch.

### ✔ Manual Trigger

1. Go to **Actions** tab
2. Select **OWASP ZAP Security Scan**
3. Click **Run workflow**
4. Select `main` branch → **Run**

---

## 📥 Viewing the OWASP ZAP Vulnerability Report

1. Go to the **Actions** tab
2. Open the latest **OWASP ZAP Security Scan** run
3. Scroll to the **Artifacts** section
4. Download the artifact **`zapreport`**
5. Extract ZIP → open `zap_report.html`

The report contains:

* Number of alerts (High / Medium / Low / Informational)
* URLs spidered
* Vulnerability description with recommended fixes


---

## ✅ Conclusion

This project successfully implements a **Secure DevOps CI/CD pipeline**, where:

* Security scanning is **automated**
* OWASP ZAP detects vulnerabilities **early**
* Results are published as **artifacts**
* Dev teams can review security issues after every deployment

This approach aligns with modern **DevSecOps practices**, adding security as a core part of the CI/CD lifecycle.


````
