# CI/CD Observability — Dynatrace App

Unified observability for **pipelines, jobs, stages and PRs** across multiple CI/CD providers (Azure DevOps, GitHub, GitLab, etc.) inside **Dynatrace Platform**.

> **This repository hosts release artifacts (ZIP bundles) that you can upload to your Dynatrace Environment.**
<img width="1677" height="914" alt="cicd-image-1" src="https://github.com/user-attachments/assets/69897507-f272-4bb1-a17e-d87c8278b201" />

---

## ✨ Features

- **Pipeline view**: status, total time, history, recent runs.
- **Stage/Job view**: start/end, duration, outcome, metadata (pipeline, project/repo).
- **Filters (Filter Bar)**: timeframe, action name/ID, session, status.
- **Pipeline Execution Flamegraph**: hierarchical view of the job execution in that pipeline.
- **OpenPipeline Setup**:
  - Setup wizard for importing and removing OpenPipeline rules
- **Support to Custom CI/CD Providers**:
  - Event description below
- **Supported Technologies**:
  - **Azure Devops**
  - **Gitlab Pipelines**
  - **Github Actions (Workflows)**

---

## 🚀 Install the App (Dynatrace Platform)

1. Go to **Releases** in this repository and download the latest bundle: `app-bundle-<version>.zip`.
2. In your Dynatrace Environment, access the **Hub App**
3. Navigate to **Manage** tab
4. Upload the zip file using the **Upload App** button
   <img width="1684" height="610" alt="cicd-image-5" src="https://github.com/user-attachments/assets/e820108f-04f9-4931-b9bd-e310a5e83c4e" />


## 🖥️ Using the App

1. Go to your newly installed app **CI/CD Observability**
2. Navigate to Setup tab to find your target techonology. Follow the instructions to **Setup the webhooks on the chosen provider**
3. Click **Import Configuration** to import the Openpipeline Rules to your environement (Those rules basically translate the incoming webhook events into SDLC Events (Opentelemetry Standard), which is then consumed by the App)
  <img width="1681" height="838" alt="cicd-image-4" src="https://github.com/user-attachments/assets/3a143725-c627-44d0-8f59-1b67fb7649fe" />
4. Navigate to CI/CD Observability to start exploring the data.
    <img width="1677" height="914" alt="cicd-image-2" src="https://github.com/user-attachments/assets/5619affb-f590-4271-a061-37be5512651e" />
5. Filters are only applied after clicking the refresh button to avoid unnecessary queries
    <img width="1684" height="563" alt="cicd-image-3" src="https://github.com/user-attachments/assets/e4a25259-aabd-456c-995b-11a93da4dd41" />
   

---

## ✨ Custom Events Format
### Pipeline Run Event
```json
{
    "cicd.pipeline.id" : "xxxxxxx", // Pipeline ID
    "cicd.pipeline.run.id": 1999999, // ID of that specific execution
    "cicd.pipeline.name" : "My Pipeline",
    "cicd.pipeline.run.outcome" : "success" // valid values "success" | "partiallySuccess" | "canceled" | "failed" | "failure" | "rejected"| "skipped",
    "cicd.pipeline.run.url.full": "https://my.cicd-tool.com/pipeline/run/123",
    "ext.pipeline.build.name" : "PR 99: adding awesome feature" // name of the commit / build message
    "ext.pipeline.run.trigger.user" : "John",  // Who started the pipeline
    "vcs.ref.head.name" : "Head Branch Name",
    "vcs.repository.name" : "Repository Name",
    "event.category" : "pipeline", // exactly like that as it's used for filtering
    "event.status": "finished" || "started",
    "event.type" : "push, build, deploy, etc",
    "event.provider" : "Custom Provider",
    "duration": 160, // in seconds
    "start_time": ""2025-11-05T21:02:30.000000000Z", // Timestamp of start of the Execution
    "end_time": ""2025-11-05T21:05:30.000000000Z" // Timestamp of end of the Execution or null if still running
}
```

### Job/Task Run Event
```json
{
    "task.id" : "xxxxxxx", # Job ID
    "task.name" : "My Pipeline",
    "task.outcome" : "success" // valid values "success" | "partiallySuccess" | "canceled" | "failed" | "failure" | "rejected"| "skipped",
    "cicd.pipeline.run.id": 1999999, // ID of the execution of pipeline running this job
    "cicd.pipeline.id" : "xxxxxxx", // Pipeline ID
    "cicd.pipeline.name" : "My Pipeline",
    "vcs.ref.head.name" : "Head Branch Name",
    "job.steps" : ["{\"name\":\"Set up job\", \"status\":\"completed\", \"conclusion\":\"success\", \"number\":1, \"started_at\":\"2025-11-12T16:11:24Z\", \"completed_at\":\"2025-11-12T16:11:26Z\"}"] // Array of steps following this structure or null if there are no steps in the job execution
    "task.retry": 1, // Attempts to run this job
    "event.category" : "task", // exactly like that as it's used for filtering
    "event.status": "finished" || "started",
    "event.type" : "push, build, deploy, etc",
    "event.provider" : "Custom Provider",
    "duration": 160, // in seconds
    "start_time": "2025-11-05T21:02:30.000000000Z", // Timestamp of start of the Execution
    "end_time": "2025-11-05T21:05:30.000000000Z" // Timestamp of end of the Execution or null if still running
}
```

## 🗺️ Roadmap

1. Add Jenkins Support
2. New page focused on DORA Metrics
3. Add more filtering Options
4. Correlate Releases with Dynatrace Problems
  
  
