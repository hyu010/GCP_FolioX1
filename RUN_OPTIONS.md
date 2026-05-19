# Running Cosmic Guestbook: 3 Options

## Prerequisites

Open the Terminal tab at the bottom of PyCharm and clone the project if you haven't already:

```bash
git clone https://github.com/kweinmeister/cosmic-guestbook.git
cd cosmic-guestbook
```

---

## Option 1: Localhost (Fast Progress)

Runs entirely on your laptop — no Google Cloud setup required. Launch the backend and frontend separately to get hot reloading.

### Step 1: Start the Backend API

```bash
cd backend
npm install
npm start
```

The backend API starts on `localhost:3000`.

### Step 2: Start the Frontend UI

Open a **new terminal tab** in PyCharm (click the `+` icon on the terminal toolbar):

```bash
cd frontend
npm install
npm run dev
```

PyCharm will print a local link (usually `http://localhost:5173`). Ctrl-click (Windows) or Cmd-click (Mac) it to open the app.

---

## Option 2: Inner Loop (Local Files → Cloud Run)

Bypasses GitHub entirely. Uses your local PyCharm files, bundles them, and deploys directly to Google Cloud Run via the Gemini DevOps agent.

### Step 1: Authenticate with Google Cloud

```bash
gcloud auth login
gcloud auth application-default login
gcloud config set project YOUR_PROJECT_ID
```

### Step 2: Deploy via the AI Agent

```bash
gemini "Deploy this application to Google Cloud using the google-cicd-deploy skill"
```

The agent will run a local secret scan, set up the project structure, then pause to ask about your preferred region and whether the service should be public. Answer the prompts and it will return a live `run.app` URL.

---

## Option 3: Outer Loop (GitHub → Production Automation)

Sets up a full CI/CD pipeline. Every push from PyCharm to GitHub automatically triggers Cloud Build, which builds and deploys your latest code.

### Step 1: Generate the Pipeline Design

```bash
gemini "Design a CI/CD pipeline using the google-cicd-pipeline-design skill"
```

### Step 2: Review and Approve

The agent generates a `cloudbuild.yaml` plan in the terminal. Review it, then type `yes` or `approve` when prompted. It will configure Artifact Registry and link your GitHub account via Developer Connect.

### Step 3: Test the Automation

Make a visible change to any file (e.g., a title in `frontend/src/App.jsx`), then push:

```bash
git add .
git commit -m "Testing the outer loop automation"
git push origin main
```

Open **Cloud Build** or **Cloud Deploy** in the Google Cloud Console — a build pipeline will spin up automatically. Once it finishes, refresh your public URL to see the changes live.
