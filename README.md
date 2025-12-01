# Home Energy Footprint Optimizer

**Track:** Agents for Good – Sustainability
**Category:** Concierge-style personal agent focused on reducing home energy usage and electricity costs.

## 1. Problem

Most households receive an electricity bill once a month, with a single big number and maybe a usage chart.What’s missing is the answer to the questions:

- *Which appliances are actually driving my bill?*
- *What should I change first to meaningfully reduce cost and emissions?*
- *How much money could I save if I tweak my habits just a little?*

Utility dashboards show raw kWh, but they do not translate that into concrete, ranked actions. As a result, most people keep wasting money and energy even though small, targeted changes (like adjusting AC runtime or laundry timings) could make a big difference.

## 2. Solution – Home Energy Footprint Optimizer

The **Home Energy Footprint Optimizer** is a multi-agent system built with the Agent Development Kit (ADK) and Gemini that:

1. Ingests a simple table (CSV/JSON) of home appliances and their usage.
2. Computes monthly energy consumption (kWh) and cost per appliance.
3. Identifies the biggest “energy hogs” in the home.
4. Generates a personalized action plan with estimated savings:
   - “Reduce AC usage by 25% → save ~$X/month”
   - “Shift dishwasher usage to off-peak hours → save ~$Y/month”
   - “Replacing this old appliance with a more efficient model could save ~$Z/year.”

The goal is to turn raw usage data into *actionable, prioritized decisions* for more sustainable and cost-effective living.

## 3. Value

- **Quantifiable personal value:** The agent estimates how much money and energy the user can save by adopting its recommendations.
- **Sustainability impact:** Reducing home energy usage directly lowers carbon footprint.
- **Product potential:** This agent could be integrated into:
  - Utility company portals (to drive engagement and sustainability)
  - Smart home dashboards
  - Personal finance / carbon footprint tracking apps

This demonstrates how AI agents can move beyond chat and become real-world sustainability tools.

---

## 4. System Overview

### 4.1 High-Level Flow

1. User provides a CSV or dataset of home appliance usage.
2. **Data Ingestion Agent** parses and normalizes the data, computing kWh and cost per appliance.
3. **Optimizer Agent** analyzes the normalized data, identifies top contributors, and proposes behavior changes.
4. **Report Agent** generates a friendly, human-readable summary with a ranked list of actions and estimated savings.
5. Optionally, a **Judge/Evaluation Agent** scores the quality of the recommendations.

See `energy_architecture.png` for the architecture diagram.

---

## 5. Agents and Responsibilities

### 5.1 Data Ingestion Agent

**Role:**
Load and normalize energy usage data.

**Input:**

- Path to a CSV file, or JSON data string.

**Responsibilities:**

- Read CSV with columns like:
  - `appliance`
  - `wattage`
  - `hours_per_day`
  - `days_per_month`
  - `cost_per_kwh`
- Compute:
  - `kwh_per_month = wattage * hours_per_day * days_per_month / 1000`
  - `cost_per_month = kwh_per_month * cost_per_kwh`
- Return a structured JSON list of appliances with kWh and cost fields.

### 5.2 Optimizer Agent

**Role:**
Find the largest energy contributors and suggest optimized usage patterns.

**Input:**

- Normalized JSON data from the Data Ingestion Agent.

**Responsibilities:**

- Rank appliances by `cost_per_month` and `kwh_per_month`.
- Identify the “top N” energy hogs (e.g., top 3–5).
- Suggest:
  - Usage reductions (e.g. “reduce AC from 6h/day to 4.5h/day”)
  - Behaviour changes (e.g. “run dishwasher at night instead of peak hours”)
  - Optional upgrade suggestions (“consider replacing very old equipment”).

### 5.3 Report Agent

**Role:**
Generate a clear, friendly report for the user.

**Input:**

- Analysis and suggestions from the Optimizer Agent.

**Responsibilities:**

- Explain:
  - Which appliances contribute most to energy and cost.
  - What specific actions the user should take.
  - Estimated monthly savings (in cost and percentage).
- Use accessible language and avoid technical jargon where possible.

### 5.4 (Optional) Judge / Evaluation Agent

**Role:**
Evaluate the quality and consistency of the suggestions.

**Responsibilities:**

- Check if the advice is consistent with the input data.
- Rate how actionable the recommendations are.
- Provide feedback that could be used to refine prompts or logic.

---

## 6. ADK Features Demonstrated

This project intentionally showcases several key ADK concepts:

1. **Multi-Agent System**

   - Data Ingestion Agent
   - Optimizer Agent
   - Report Agent
   - (Optional) Judge Agent
2. **Tools**

   - Custom Python tools:
     - `load_energy_data(path)`
     - `compute_energy_stats(data_json)`
   - Optional: `google_search` tool for typical appliance consumption lookups.
3. **Sessions & Memory**

   - Session-level state to track a single analysis flow.
   - Optional long-term memory to save previous months’ analyses and compare:
     - “This month vs last month energy usage.”
4. **Observability: Logs & Traces**

   - Logging of:
     - Loaded data
     - Top energy hogs
     - Generated recommendations
   - Traces to show the path:
     - Raw data → Normalized data → Optimized suggestions → Final report.
5. **Agent Evaluation**

   - Judge Agent scoring:
     - “Are these recommendations actionable?”
     - “Do they match the data provided?”

---

## 7. Data Format

### 7.1 Example CSV

```csv
appliance,wattage,hours_per_day,days_per_month,cost_per_kwh
Fridge,150,24,30,0.15
Air Conditioner,1200,6,25,0.15
Washing Machine,500,1,12,0.15
TV,100,4,25,0.15
Laptop,60,6,25,0.15
```
