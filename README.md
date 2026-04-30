# Streamlit-Inventory-Dashboard
### `app.py` - Streamlit Inventory OS Dashboard

This `app.py` file contains a Streamlit application designed for an Inventory OS dashboard, using a Google Sheet as its backend. When hosted on GitHub, this file would be the core of your web application, accessible to others or deployable to services like Streamlit Cloud.

#### Core Functionality (as explained previously):
-   **Google Sheets Integration**: Uses `gspread` to read and write data to specific worksheets ('List Inventaris' and 'Categories') in a Google Sheet, which acts as the database.
-   **Data Loading & Caching**: Efficiently loads data from Google Sheets into pandas DataFrames, using `st.cache_data` for performance.
-   **UI & Interaction**: Provides a user-friendly interface with filters, KPIs, data tables, and forms for adding/editing assets.
-   **Utility Functions**: Includes logic for generating unique asset codes and QR codes.
-   **Data Visualization & Analytics**: Features interactive charts (`plotly.express`) and "Smart Analytics" sections for insights, alerts, and summaries.

#### GitHub-Specific Considerations:
When placing this `app.py` file in a GitHub repository, you'll typically need a few additional files and configurations to make it runnable and deployable by others:

1.  **`requirements.txt`**: This file lists all the Python libraries your application depends on. Deployment platforms (like Streamlit Cloud) or other developers will use this file to install the necessary dependencies. For this `app.py`, it would look something like this:

    ```
    streamlit
    pandas
    plotly
    gspread
    google-auth
    oauth2client # If any legacy oauth2client code is still used for auth
    pyngrok # If you still plan to use ngrok for local tunneling
    qrcode[pil]
    openpyxl
    ```

2.  **Authentication Management (`.streamlit/secrets.toml` or similar)**:
    -   In a Colab environment, authentication is handled via `google.auth.default()`, which relies on the Colab runtime's authenticated user.  
    -   For a deployed Streamlit app (e.g., on Streamlit Cloud), you would typically use a `secrets.toml` file (located in a `.streamlit` folder at your repository's root) to store sensitive information like Google Service Account credentials (if using a service account) or API keys.  
    -   The `gspread` authentication would then be adapted to read credentials from these secrets, rather than relying on Colab's default authentication context.

3.  **Deployment**: Platforms like [Streamlit Cloud](https://streamlit.io/cloud) can directly deploy applications from a GitHub repository. You would point Streamlit Cloud to your repository, specify `app.py` as the main file, and it would handle the environment setup based on your `requirements.txt`.

4.  **README.md**: A good `README.md` file in your GitHub repository is essential. It should include:
    -   A description of the project.
    -   Instructions on how to set up and run the app locally.
    -   Details on required Google Sheet permissions and setup.
    -   Information about authentication (e.g., how to set up `secrets.toml`).
    -   A link to the live deployed application (if applicable).

By including these elements in your GitHub repository, you make your Streamlit Inventory OS dashboard easily shareable, reproducible, and deployable.
