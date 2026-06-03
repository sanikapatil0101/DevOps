# CI/CD with Repository Branches

CI/CD often works using **Git branches**.

Common flow:

```text id="9h5h3s"
feature → dev → main → production
```

---

## Branch Flow

### 1. Feature Branch

Developer works on new feature.

```bash id="sldwj0"
git checkout -b feature/login
```

Push code:

```bash id="cqz4e8"
git push origin feature/login
```

---

### 2. Dev Branch (Testing)

Merge feature into `dev`.

```text id="x6j5j8"
feature/login → dev
```

**CI runs automatically:**

* Install dependencies
* Run tests
* Build project

Example:

```bash id="8f8vdq"
npm install
npm test
npm run build
```

---

### 3. Main Branch (Production Ready)

After testing success:

```text id="v5vr2h"
dev → main
```

CD pipeline deploys app.

Example:

```text id="cvmq02"
main push → AWS/Server → Live Website
```

---

## Typical CI/CD Rules

| Branch            | Purpose                 |
| ----------------- | ----------------------- |
| `feature/*`       | New feature development |
| `dev`             | Testing/Staging         |
| `main` / `master` | Production              |

---

## Example Workflow

```text id="3n0nhs"
Developer Code
      ↓
feature branch
      ↓
Merge → dev
      ↓
CI: Test + Build
      ↓
Merge → main
      ↓
CD: Deploy to Production
```

---

## CI (Continuous Integration)

Flow:

```text
Developer → Git Push → Build → Test
```

Example:

```bash
npm install
npm test
npm run build
```

---

## CD (Continuous Deployment)

After CI success:

Deploy app automatically to server/cloud.

Flow:

```text
Build Success → Deploy → Live App
```

Example:

```text
GitHub → CI/CD → AWS EC2 → Website Live
```

---

## Example: GitHub Actions

`.github/workflows/main.yml`

```yaml
name: CI-CD

on: push

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4
      - run: npm install
      - run: npm test
```

---
# Process Management & PM2

## Process Management

Process management = **Managing running programs in Linux/server**.

Tasks:

* Start process
* Stop process
* Monitor process
* Restart process
* Run in background

### Basic Commands

See running processes:

```bash id="w3tqwo"
ps aux
```

Live monitoring:

```bash id="36v8gd"
top
```

Find process:

```bash id="u2qh7f"
ps aux | grep node
```

Kill process:

```bash id="j93g5v"
kill PID
```

Force kill:

```bash id="edby29"
kill -9 PID
```

---

## Problem with `node index.js`

If you run:

```bash id="bd1m18"
node index.js
```

Problems:

* Stops when terminal closes
* No auto restart on crash
* Hard to manage multiple apps

Solution → **PM2**

---

## PM2

PM2 = **Node.js Process Manager**

Used to:

* Run apps in background
* Auto restart on crash
* Monitor apps
* Manage multiple Node apps

---

## Install PM2

```bash id="ekz7ei"
npm install -g pm2
```

---

## Start Application

```bash id="1z1uz7"
pm2 start index.js
```

Custom name:

```bash id="svk7m8"
pm2 start index.js --name backend
```

---

## Useful PM2 Commands

See apps:

```bash id="u7v7qq"
pm2 list
```

Logs:

```bash id="zsy2lu"
pm2 logs
```

Stop app:

```bash id="wzj0rx"
pm2 stop backend
```

Restart app:

```bash id="mjlwm8"
pm2 restart backend
```

Delete app:

```bash id="b4a2ra"
pm2 delete backend
```

---

## Auto Start on Server Reboot

Save processes:

```bash id="8xg6nl"
pm2 save
```

Enable startup:

```bash id="7tpg20"
pm2 startup
```

---

## Quick Definition

> **Process Management = Managing running programs.**
> **PM2 = Process manager for Node.js apps with background running + auto restart.**

