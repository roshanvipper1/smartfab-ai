# 🏭 SmartFab AI

**Manufacturing Planning & Scheduling Platform**

SmartFab AI is a manufacturing planning prototype that transforms production demand data into constraint-aware production schedules through an automated planning pipeline.

The system generates production jobs, creates process routings, assigns products to compatible factories, schedules operations across production lines, accounts for changeovers and downtime, and visualizes the resulting production plan using interactive Gantt charts.

> **Project Status:** Portfolio and educational prototype. The application is designed to run locally and is not currently deployed as a production service.

---

## 🚀 Overview

Manufacturing planning requires coordinating demand, product routings, factory capabilities, production lines, changeovers, downtime, and delivery requirements.

SmartFab AI demonstrates how these planning steps can be integrated into a single automated workflow.

Given production demand data, the platform:

* Generates manufacturing jobs based on demand and lot sizes
* Builds process routings for individual jobs
* Assigns products to factories based on manufacturing requirements
* Validates factory and process compatibility
* Schedules operations across eligible production lines
* Accounts for changeover times and planned downtime
* Checks production schedules against job deadlines
* Generates interactive Gantt charts for schedule visualization

The project is intended to explore manufacturing planning, production scheduling, and data-driven decision support in a multi-factory environment.

---

## 🎯 Key Features

### 📁 Demand Input

Accept production demand data in CSV format for automated planning.

### ⚙️ Job Generation

Convert product demand into manufacturing jobs based on product-specific lot sizes and delivery deadlines.

### 🔄 Process Routing

Generate the required manufacturing process sequence for each production job.

### 🏭 Factory Assignment

Assign products to factories according to process requirements and factory capabilities.

The current implementation uses rule-based assignment logic rather than a machine learning model.

### 📅 Production Scheduling

Generate production schedules while considering:

* Factory assignment
* Process sequence
* Production-line availability
* Processing time
* Product changeover time
* Planned downtime
* Process compatibility
* Job deadlines

For each operation, the scheduler evaluates eligible production lines within the assigned factory and selects a feasible line based on completion time.

### 🛠 Planned Downtime Scheduling

Create planned downtime windows for production lines while accounting for process-specific scheduling preferences and spacing requirements.

### ⚠️ Deadline Validation

Evaluate completed schedules against job deadlines and identify late jobs and their associated lateness.

### 📊 Interactive Gantt Visualization

Visualize production schedules and manufacturing timelines using Plotly-based Gantt charts.

### 🌐 REST API

Use a FastAPI backend to trigger the planning pipeline and interact with the application locally.

---

## 🏗 System Architecture

```text
Production Demand
       │
       ▼
┌──────────────────┐
│  Job Generator   │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│ Routing Generator│
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│Factory Assignment│
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│Feasibility Check │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│Production        │
│Scheduler         │
└────────┬─────────┘
         │
         ├──────────────► Deadline Validation
         │
         ▼
┌──────────────────┐
│ Gantt Generator  │
└────────┬─────────┘
         │
         ▼
 Production Plan
```

---

## 🧠 Scheduling Logic

The current version of SmartFab AI uses deterministic rules and scheduling heuristics rather than machine learning or mathematical programming.

### Factory Assignment

Products are assigned to factories according to their required manufacturing processes and available factory capabilities.

### Production-Line Selection

For each operation, the scheduler:

1. Identifies production lines capable of performing the required process.
2. Restricts candidate lines to the job's assigned factory.
3. Calculates applicable product changeover time.
4. Accounts for blocked periods and planned downtime.
5. Determines the earliest feasible start and completion time.
6. Selects the candidate line with the earliest completion time.

### Schedule Constraints

The scheduling workflow accounts for:

* Process precedence
* Factory compatibility
* Production-line availability
* Processing duration
* Product-family changeovers
* Planned downtime
* Fixed blocked time windows
* Delivery deadlines

Additional validation ensures that a job remains within its assigned factory throughout its production routing.

---

## ⚡ Technology Stack

### Backend

* Python
* FastAPI
* Uvicorn

### Data Processing

* Pandas

### Visualization

* Plotly

### Data Storage

* CSV-based input and intermediate datasets

### API

* REST API
* File upload support
* Automated planning workflow

---

## 📂 Project Structure

```text
smartfab-ai/
│
├── app.py
├── requirements.txt
├── index.html
│
├── Engine/
│   ├── job_generator.py
│   ├── routing_generator.py
│   ├── factory_assignment.py
│   ├── scheduler.py
│   └── gantt_chart.py
│
├── data/
│   ├── demand.csv
│   ├── factories_capacities.csv
│   ├── changeover.csv
│   └── downtime.csv
│
├── database/
│   ├── generated_jobs.csv
│   ├── job_process_flow.csv
│   ├── product_factory_assignment.csv
│   ├── schedule_baseline.csv
│   ├── schedule_final.csv
│   ├── planned_downtime_schedule.csv
│   └── deadline_violations.csv
│
└── gantt_chart.html
```

---

## 🔄 Planning Workflow

### 1. Load Production Demand

The workflow begins with product demand and required delivery dates.

### 2. Generate Production Jobs

Demand quantities are converted into individual manufacturing jobs according to product lot sizes.

### 3. Generate Process Routings

Each job receives the manufacturing process sequence required for its product.

### 4. Assign Factories

Products are assigned to compatible factories based on manufacturing requirements.

### 5. Validate Factory Feasibility

Before scheduling begins, the system verifies that every process required by a job can be performed within its assigned factory.

### 6. Schedule Production

Operations are assigned to eligible production lines while accounting for process sequence, line availability, changeovers, blocked periods, and planned downtime.

### 7. Validate Deadlines

The final completion time of each job is compared with its required deadline.

### 8. Visualize the Schedule

The generated production schedule is displayed through an interactive Gantt chart.

---

## 📡 API Endpoints

### Health Check

```http
GET /
```

Example response:

```json
{
  "message": "SmartFab API is running"
}
```

### Run Planning Pipeline

```http
POST /run
```

Input:

```text
multipart/form-data
demand.csv
```

Example response:

```json
{
  "status": "success",
  "message": "Pipeline completed"
}
```

### Web Interface

```http
GET /ui
```

Opens the local SmartFab user interface.

---

## 🛠 Running Locally

### Clone the Repository

```bash
git clone https://github.com/jasontruong11513/smartfab-ai.git
cd smartfab-ai
```

### Create a Virtual Environment

```bash
python -m venv venv
```

Activate the environment on Windows:

```bash
venv\Scripts\activate
```

On macOS/Linux:

```bash
source venv/bin/activate
```

### Install Dependencies

```bash
pip install -r requirements.txt
```

### Start the Application

```bash
uvicorn app:app --reload
```

Local application:

```text
http://127.0.0.1:8000
```

Swagger API documentation:

```text
http://127.0.0.1:8000/docs
```

> SmartFab AI is currently tested as a local application. Public cloud deployment is not part of the current project implementation.

---

## 📈 Generated Outputs

The planning pipeline produces intermediate and final datasets such as:

```text
generated_jobs.csv
job_process_flow.csv
product_factory_assignment.csv
schedule_baseline.csv
schedule_final.csv
planned_downtime_schedule.csv
deadline_violations.csv
gantt_chart.html
```

These outputs make the planning process traceable from demand input through factory assignment and production scheduling.

---

## 🎓 Applications

SmartFab AI explores concepts related to:

* Manufacturing Planning
* Production Scheduling
* Multi-Factory Production Planning
* Capacity Planning
* Manufacturing Analytics
* Operations Management
* Supply Chain Decision Support
* Smart Manufacturing

---

## ⚠️ Current Limitations

SmartFab AI is a portfolio prototype and does not represent a production-grade manufacturing planning system.

The current implementation:

* Uses rule-based factory assignment
* Uses heuristic production scheduling
* Uses CSV files rather than a production database
* Does not currently use machine learning or deep learning models
* Does not guarantee a globally optimal production schedule
* Does not currently integrate with ERP or MES systems
* Is designed and tested primarily for local execution

These limitations provide opportunities for future development.

---

## 🔮 Future Improvements

Potential extensions include:

* Mathematical optimization using linear or mixed-integer programming
* Machine learning-based demand forecasting
* Reinforcement learning for production scheduling
* Dynamic multi-factory allocation
* More detailed capacity and labor constraints
* Scenario and what-if analysis
* ERP/MES integration
* Real-time production monitoring
* Predictive maintenance
* Cloud deployment
* AI-assisted production planning and decision support

---

## 👨‍💻 Author

**Jason Truong**

M.S. Information Systems Candidate  
California State University, Long Beach

---

## 📄 License

This project is provided for educational and portfolio purposes.
