# Streamlit-Inventory-Dashboard
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
