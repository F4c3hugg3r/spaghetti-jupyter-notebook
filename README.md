# spaghetti-jupyter-notebook

## Workshop: Refactoring a Spaghetti-Notebook

Welcome to the hands-on session! Your task is to take a broken, poorly structured Jupyter Notebook (`messy_analysis.ipynb`) and transform it into a robust, reproducible data pipeline. 

Please use the integrated Terminal in GitHub Codespaces for all CLI commands.

---

### Environment Setup 
Before fixing any code, we must set up a safe execution environment. Modern Linux containers restrict global package installations to prevent system corruption (PEP 668). Therefore, we need an isolated virtual environment.

1. Open the Terminal in your Codespace.
2. Create a virtual environment:
   ```bash
   python3 -m venv .venv
3. Activate it:
    ```bash
    source .venv/bin/activate
4. Install the absolute minimum required to view the notebook:
    ```bash
    pip install ipykernel pandas matplotlib
5. Open `messy_analysis.ipynb`. In the top right corner, select **Select Kernel -> Python Environments ->** `.venv.`

---

### Task 1: Fix the Foundation (Root Cause Analysis)
If you click "Restart Kernel & Run All Cells" in `messy_analysis.ipynb`, it crashes immediately.

- **The Root Cause:** The notebook suffers from "Hidden States". Variables are used before they are defined because the original author executed the cells out of order.

- **Your Job:** Reorder the cells so the notebook executes strictly linearly from top to bottom without any errors.

---

### Task 2: Modularization & Documentation
A single monolithic notebook is hard to maintain.

- Split your repaired notebook into two separate sub-notebooks:

    1. `01_clean.ipynb` (Loads the CSV, filters the data, and saves a `cleaned_data`.csv).

    2. `02_plot.ipynb` (Loads the `cleaned_data.csv` and generates the plot).

Add a **Markdown cell** at the very top of *both* notebooks explaining the *intent* of the notebook in 2-3 sentences. (Explain the process, not just the code).

**Data Transfer:** Variables are not shared between different notebooks. You must write the filtered data to your hard drive at the end of notebook 01 (`df_clean.to_csv('cleaned_data.csv', index=False)`) and load it at the beginning of notebook 02 (`pd.read_csv('cleaned_data.csv')`). 

---

### Task 3: Pipeline Preparation (Papermill)
We want to automate the cleaning process so we don't have to change values manually in the code.

- Open `01_clean.ipynb`.

- Ensure your `THRESHOLD` variable is isolated in its own code cell. <details>
Papermill looks for the parameters tag to know where to inject new variables. Ensure that the cell you are tagging contains only the variable definitions (e.g., THRESHOLD = 40.0) and no other logic like imports or data processing.
</details>

- Using the Jupyter interface, assign the tag `parameters` to this specific cell. 
*(Hint: Click the '...' in the cell toolbar -> Add Cell Tag).*

---

### Task 4: Dependencies & Automation
Now we bundle everything into a reproducible pipeline.

1. Create a `requirements.txt` file. Explicitly list the libraries we need for our pipeline to run on any other machine. Include at least: `pandas`, `matplotlib`, `papermill`, `jupyterlab`.

2. Additionally create a `runtime.txt` file containing `python-3.12` so that tools like **Binder** can create a working environment automatically.

3. Install the newly added tools in your terminal: `pip install -r requirements.txt`.

4. Create a file named `Makefile` (no file extension) to automate the execution. Write a `run` target that uses `papermill` to:

    - Execute `01_clean.ipynb` with a new threshold value (e.g., 25.0) and save the output.

    - Execute `02_plot.ipynb` and save the output.

For more information on `Makefile` and `papermill` syntax take a look at the [example repo](https://github.com/F4c3hugg3r/reproducable-jupyter-notebook.git).

5. Test your pipeline by typing `make run` in the terminal.

---

### Addtitional task

If everything runs fine you can download your finished project and push it into your own repository. To test the reproducability add a link to **Binder** in your `README` and test if it is correctly building and running the project.

Binder link for your `README`: 

`[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/YOUR-GITHUB-NAME/YOUR-REPO-NAME/BRANCH-NAME)`

*(Note: Remember to replace the details in the link above with your own repository details!)*
