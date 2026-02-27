# Gruv Knowledge Base Search

This project implements a high-performance, locally-hosted search engine for a massive dataset of JSON articles. It utilizes **Meilisearch** for lightning-fast keyword and semantic search (using built-in HuggingFace vector embeddings) and provides a polished **React UI** for easy exploration and faceted filtering.

## Project Structure

The repository is organized into three main areas:

*   `/content-v2` - The directory containing all the raw JSON blog articles.
*   `/backend` - The Python scripts and setup files necessary to launch Meilisearch and index the JSON data.
*   `/meili-ui` - The React application (built with Vite and Tailwind CSS) that provides the graphical search interface.

---

## Prerequisites

Before you can run this project locally, ensure you have the following installed on your machine:

1.  **Python 3.8+** (for running the indexing script)
    *   Verify installation: `python --version`
2.  **Node.js (v18+) and npm** (for running the React frontend)
    *   Verify installation: `node -v` and `npm -v`
3.  **PowerShell** (Windows users: for running the Meilisearch setup script)

---

## Step-by-Step Local Setup Guide

Follow these steps in order to start the database, index the data, and launch the user interface.

### Step 1: Start the Meilisearch Database

First, we need to download and run the Meilisearch engine.

1.  Open your terminal or command prompt.
2.  Navigate to the `backend` directory:
    ```bash
    cd backend
    ```
3.  Run the setup script. This will download the Meilisearch binary executable (if it isn't already there) and start the server:
    *   **On Windows (PowerShell):**
        ```powershell
        .\setup_meilisearch.ps1
        ```
    *   *(Alternatively, you can manually run the downloaded `meilisearch.exe` with the master key: `$env:MEILI_MASTER_KEY="masterKey123"; .\meilisearch.exe`)*
4.  Keep this terminal window open. The database is now running locally on `http://127.0.0.1:7700`.

### Step 2: Index the Data

Now that the database is running, we must parse the thousands of JSON articles in `/content-v2` and upload them to Meilisearch.

1.  Open a **new** terminal window (leave the Meilisearch server running in the first one).
2.  Navigate to the `backend` directory:
    ```bash
    cd backend
    ```
3.  Install the required Python packages (it is recommended to use a virtual environment):
    ```bash
    pip install meilisearch sentence-transformers requests tqdm
    ```
4.  Run the indexing script. This script automatically enables the vector store, updates the index settings (for filtering and custom embeddings), and uploads the articles in batches.
    ```bash
    python index_meilisearch.py
    ```
5.  Wait for the progress bar to complete. You will see logs confirming the successful enqueuing of the batch jobs.

### Step 3: Start the React Frontend UI

With the data fully indexed, we can now launch the visual search interface.

1.  Open a **third** terminal window.
2.  Navigate to the `meili-ui` directory:
    ```bash
    cd meili-ui
    ```
3.  Install the Node JS dependencies:
    ```bash
    npm install
    ```
4.  Start the Vite development server:
    ```bash
    npm run dev
    ```
5.  Open your web browser and navigate to the local URL provided by Vite (usually **http://localhost:5173/**).

---

## Features Built-in

*   **Hybrid Semantic Search:** Try searching using natural language sentences (e.g., *"how to deal with US sales tax as an expat"*). Meilisearch will automatically convert your query into a vector and compare it against the embedded vectors of the articles to find conceptual matches, even if the exact words aren't present.
*   **Faceted Category Filtering:** On the left sidebar, you can filter articles by their associated "Tags" (e.g., clicking "agency workflow" will dynamically narrow down the results).
*   **Lightning Fast:** Leveraging React InstantSearch, the UI updates synchronously as you type, providing an instant feedback loop without full page reloads.
