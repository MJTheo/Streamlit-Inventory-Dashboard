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

### Code Explanation: `app.py` - Streamlit Inventory OS Dashboard

This `app.py` file contains the full Streamlit application code for an Inventory OS dashboard. It's designed to manage assets by interacting with a Google Sheet as its backend database.

#### 1. Configuration & Setup
-   **`st.set_page_config`**: Sets up the Streamlit page title and layout.
-   **`SHEET_URL`**: Defines the URL of the Google Sheet where inventory data is stored.
-   **`QR_FOLDER`**: Specifies a folder in Google Drive (`/content/drive/MyDrive/QR_CODES`) for storing generated QR codes, ensuring it exists.

#### 2. Google Sheets Authentication
-   **`gspread` and `google.auth.default`**: Authenticates with Google Sheets using credentials established by `auth.authenticate_user()` (from the Colab environment).
-   **`sheet.worksheet`**: Opens specific worksheets: `"List Inventaris"` for asset data and `"Categories"` for category/subcategory definitions.

#### 3. Data Loading
-   **`@st.cache_data(ttl=20)`**: Decorator used to cache data for 20 seconds, improving performance by avoiding repeated fetches from Google Sheets.
-   **`load_data()`**: Fetches all records from the `"List Inventaris"` worksheet into a pandas DataFrame (`df`).
-   **`load_subcat()`**: Fetches all records from the `"Categories"` worksheet into a pandas DataFrame (`sub_df`).

#### 4. Utility Functions
-   **`generate_code(cat)`**: Generates a unique asset code based on the category (e.g., `AST-Category-0001`).
-   **`generate_qr(code)`**: Creates a QR code image for a given asset code, converts it to base64, and returns the string representation for embedding in Streamlit.

#### 5. UI Styling
-   **`st.markdown`**: Applies custom CSS to the Streamlit app for a modern, dark-themed look.

#### 6. Header and Global Search
-   **`st.columns`**: Divides the header into columns for the app title, a global search input, and a 'New Asset' button.

#### 7. Sidebar Filters
-   **`st.sidebar.multiselect`**: Provides interactive filters for `Category`, `Status`, and `Location`, allowing users to narrow down the displayed assets.

#### 8. Data Filtering
-   Applies `global_search` and sidebar filters to the main `df` to create a `filtered` DataFrame that is used for display and analysis.

#### 9. Key Performance Indicators (KPIs)
-   Displays cards showing `Total` assets, `Active` assets, and assets `Maintenance` status based on the `filtered` data.

#### 10. Insights (Charts)
-   **`plotly.express`**: Generates interactive charts to visualize asset distribution by `Location`, `Condition`, and `Status`.

#### 11. Assets Table
-   **`st.selectbox` and `st.number_input`**: Implements pagination for the asset table, allowing users to view assets in manageable chunks.
-   **`st.dataframe`**: Displays the paginated and filtered asset data.

#### 12. Quick Edit and Delete
-   Allows users to select an asset by `Asset Code`.
-   **`st.text_input` and `st.selectbox`**: Provides fields to update the `Item Name` and `Status`.
-   **`assets_ws.update()` and `assets_ws.delete_rows()`**: Directly modifies or deletes rows in the Google Sheet based on user actions.

#### 13. Add Asset
-   **`st.selectbox` and `st.text_input`**: Collects details for a new asset (Category, Sub Category, Item Name, Location).
-   When 'Create Asset' is clicked, it generates an `Asset Code` and `QR code`.
-   **`assets_ws.append_row()`**: Adds the new asset's data to the `"List Inventaris"` worksheet.

#### 14. Smart Analytics
-   **Asset Growth Trend**: Visualizes the growth of assets over time using a line chart.
-   **Value Distribution**: Calculates and displays the total asset value and a histogram of purchase prices.
-   **Health Alerts**: Provides warnings if a high percentage of assets are under maintenance or in poor condition, and highlights the most common location.
-   **Smart Summary**: Offers textual summaries of key inventory metrics, such as active asset percentage and most common categories.

This application provides a comprehensive interface for managing and analyzing inventory data stored in Google Sheets directly from a Streamlit dashboard.
