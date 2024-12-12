# Web App Workshop: Build and Integrate Dash Applications

### Introduction ⭐⭐⭐

#### What is a Web App? 🌐
- **Definition**: A web app is a program that runs in a browser, enabling users to perform tasks or access interactive content. 🌐
- **Examples**:
  - Google Docs 📝 for online document editing.
  - Airbnb 🏠 for interactive rental searches.
  - Weather apps ☀️ for real-time visualizations.
  - Seismic Viewer: View seismic cubes interactively.
  - ReSim: Analyze flow simulation results interactively.
  - ESP Pumps Monitoring: Observe alarms interactively.

#### Why Use Plotly Dash? 📊
- **Key Features**:
  - 🐍 Python-based: Write all logic in Python.
  - 🖼️ Interactive visualizations: Create stunning dashboards.
  - 🚀 Quick deployment: Share locally or on cloud platforms.
  - 🖌️ Customization: Use HTML and CSS for personalized layouts.
- **Advantages**:
  - 🤝 Seamless integration with pipelines (e.g., Dataiku).
  - 🎨 Minimal coding for professional visuals.

#### Workshop Outcomes 🎯
- Understand what a web app is and why Plotly Dash is powerful. 💪
- See a demonstration of building a web app for SCAL sample categorization. 🛠️
- Learn to integrate a web app with Dataiku for real-world use. 🌍

---

### Setting Up the Environment 🖥️
- **Install Required Libraries**:
  ```bash
  pip install dash plotly pandas
  ```
- **Prepare the Environment**:
  Open your favorite code editor (e.g., VS Code). 💻

---

### Understanding Dash Basics 📘
- **Core Components**:
  - `html`: 🖍️ Create layout elements (headers, divs).
  - `dcc`: 🛠️ Add graphs, dropdowns, and sliders.
- **Example Layout**:
  ```python
  import dash
  from dash import html, dcc

  app = dash.Dash(__name__)

  app.layout = html.Div([
      html.H1("Hello Dash!"),
      dcc.Graph()
  ])

  if __name__ == "__main__":
      app.run_server(debug=True)
  ```

---

### Demonstrating the SCAL Sample Categorization App 🔬

#### Step 1: Data Preparation 📊
- Create a sample dataset:
  ```python
  import pandas as pd

  data = {
      "Sample_ID": [1, 2, 3, 4],
      "CV": [0.1, 0.5, 1.2, 0.9],
      "HI": [0.3, 0.7, 1.5, 0.8],
      "RQI": [15, 12, 8, 10],
      "FZI": [3.5, 2.5, 1.0, 1.8],
  }
  df = pd.DataFrame(data)
  ```

#### Step 2: Categorizing the Data 🎛️
- Define a function to categorize samples:
  ```python
  def categorize_samples(row):
      if row["CV"] < 0.3 and row["HI"] < 0.5 and row["RQI"] > 10 and row["FZI"] > 2.0:
          return "Suitable"
      elif (row["CV"] < 0.5 and row["HI"] < 0.7) or (row["RQI"] > 8 and row["FZI"] > 1.5):
          return "Mostly Homogeneous"
      else:
          return "Heterogeneous"

  df["Category"] = df.apply(categorize_samples, axis=1)
  ```

#### Step 3: App Layout 🖌️
- Create an interactive layout:
  ```python
  app.layout = html.Div([
      html.H1("SCAL Sample Categorization"),
      dcc.Graph(id="scatter-plot"),
      dcc.Dropdown(
          id="variable-x",
          options=[{"label": var, "value": var} for var in ["CV", "HI", "RQI", "FZI"]],
          value="CV",
          clearable=False,
      ),
      dcc.Dropdown(
          id="variable-y",
          options=[{"label": var, "value": var} for var in ["CV", "HI", "RQI", "FZI"]],
          value="HI",
          clearable=False,
      )
  ])
  ```

#### Step 4: Adding Interactivity 🎛️
- Add callbacks for interactivity:
  ```python
  from dash import Input, Output

  @app.callback(
      Output("scatter-plot", "figure"),
      Input("variable-x", "value"),
      Input("variable-y", "value"),
  )
  def update_scatter_plot(x_var, y_var):
      import plotly.express as px
      fig = px.scatter(
          df, x=x_var, y=y_var, color="Category",
          title=f"Scatter Plot of {x_var} vs {y_var}",
          hover_data=["Sample_ID"],
      )
      return fig
  ```

#### Step 5: Running the App 🚀
- Run the script:
  ```bash
  python app.py
  ```
- Open the app at `http://127.0.0.1:8050/`.

---

### Integrating the Web App with Dataiku 🤝

#### Why Integrate?
- 📂 Seamless data management and collaboration.
- 🔄 Dynamic dataset updates using workflows.

#### Steps to Integrate:
1. Fetch datasets using the Dataiku Python API:
   ```python
   import dataiku

   client = dataiku.api_client()
   project = client.get_project("YOUR_PROJECT_KEY")
   dataset = project.get_dataset("YOUR_DATASET_NAME")
   df = dataset.get_dataframe()
   ```
2. Replace static datasets with Dataiku’s `df`.
3. Demonstrate a workflow to reflect changes in the app.

#### Creating the Requirements File 📋
1. **Why?** Ensures all libraries are available in the environment. ✅
2. **Steps**:
   - Create `requirements.txt`:
     ```bash
     dash
     plotly
     pandas
     dataiku-api-client
     ```
   - Upload to Dataiku:
     - Navigate to "Code Envs" → "Manage Python Envs" → "Create New Environment."
     - Choose "Use a requirements file."
   - Test setup with Python recipes or notebooks. 🧪

---

### Q&A and Next Steps 💬

- 🤔 Address questions.
- 📚 Suggested resources:
  - [Dash Documentation](https://dash.plotly.com/)
  - [Python Basics](https://docs.python.org/3/tutorial/)
  - [Pandas Documentation](https://pandas.pydata.org/docs/)
  - [Dataiku Python API Documentation](https://doc.dataiku.com/)

---

### Deliverables 📝

- ✅ Demonstrated Dash web app.
- 📄 Workshop materials for offline review.

**End of Workshop** 🛑

