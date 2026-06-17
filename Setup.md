# Setup Guide: Singapore Bus Boarding Analysis

This guide will walk you through setting up the Singapore Bus Boarding Analysis project on your local machine.

## Prerequisites

Before starting, ensure you have:
* A computer with Windows, macOS, or Linux
* Administrator/sudo access for installations
* Internet connection for downloading packages and API access

## Step 1: Install Python 3.10

### Windows
1. Download Python 3.10 from https://www.python.org/downloads/
2. Run the installer
3. **Important**: Check "Add Python to PATH" during installation
4. Click "Install Now"
5. Verify installation by opening Command Prompt and running:
   ```
   python --version
   ```

### macOS
1. Using Homebrew (recommended):
   ```
   brew install python@3.10
   ```
2. Verify:
   ```
   python3 --version
   ```

### Linux (Ubuntu/Debian)
```
sudo apt update
sudo apt install python3.10 python3.10-venv
```

## Step 2: Install PostgreSQL

This project uses PostgreSQL to store the bus arrival data.

### Windows
1. Download PostgreSQL from https://www.postgresql.org/download/windows/
2. Run the installer
3. Set a password for the postgres user (remember this!)
4. Keep default settings for port (5432)
5. Complete installation

### macOS
Using Homebrew:
```
brew install postgresql
brew services start postgresql
```

### Linux (Ubuntu/Debian)
```
sudo apt install postgresql postgresql-contrib
sudo service postgresql start
```

## Step 3: Create PostgreSQL Database

1. Open PostgreSQL command line (psql)
2. Create a new database for this project:
   ```
   CREATE DATABASE singapore_bus_analysis;
   ```
3. Exit psql by typing `\q`

Note: The pandas.to_sql() function will automatically create the necessary tables (phase1_data, phase2_data) when you run the notebooks. You don't need to create them manually.

## Step 4: Set Up LTA API Access

1. Visit Singapore LTA DataMall: https://datamall.lta.gov.sg/
2. Click "Sign Up" and create an account
3. After account verification, request API access
4. In your application, specify your use case (e.g., research on passenger boarding behavior)
5. LTA will verify and send you an API key via email
6. Save this API key securely

**Important**: Never share your API key or commit it to GitHub. Store it safely.

## Step 5: Install Project Dependencies

1. Create a project directory:
   ```
   mkdir Singapore-Bus-Boarding-Analysis
   cd Singapore-Bus-Boarding-Analysis
   ```

2. Create a Python virtual environment:
   ```
   python -m venv venv
   ```

3. Activate the virtual environment:
   * **Windows**: `venv\Scripts\activate`
   * **macOS/Linux**: `source venv/bin/activate`

4. Install required Python libraries:
   ```
   pip install pandas==1.5.3
   pip install psycopg2==2.9.6
   pip install requests==2.31.0
   pip install jupyter==1.0.0
   pip install numpy==1.24.3
   pip install scipy==1.10.1
   ```

Alternatively, create a requirements.txt file with the above versions and run:
```
pip install -r requirements.txt
```

## Step 6: Configure Database Connection

In your Jupyter notebooks, you'll need to establish a connection to PostgreSQL. Here's how to set up your connection string:

```python
import psycopg2
from sqlalchemy import create_engine

# Database configuration
db_connection_string = 'postgresql://postgres:YOUR_PASSWORD@localhost:5432/singapore_bus_analysis'

# Create SQLAlchemy engine
engine = create_engine(db_connection_string)
```

Replace `YOUR_PASSWORD` with the PostgreSQL password you set during installation.

**Recommended**: Use environment variables to store your password instead of hardcoding it:

```python
import os
from dotenv import load_dotenv

load_dotenv()
password = os.getenv('DB_PASSWORD')
db_connection_string = f'postgresql://postgres:{password}@localhost:5432/singapore_bus_analysis'
engine = create_engine(db_connection_string)
```

## Step 7: Configure LTA API Key

Store your API key as an environment variable:

```python
import os
from dotenv import load_dotenv

load_dotenv()
api_key = os.getenv('LTA_API_KEY')

# Use in your API requests
headers = {
    'AccountKey': api_key
}
```

Create a .env file in your project directory:
```
DB_PASSWORD=your_postgres_password
LTA_API_KEY=your_lta_api_key
```

**Important**: Add .env to your .gitignore file so credentials are never committed to GitHub.

## Step 8: Run the Notebooks

1. Start Jupyter Notebook:
   ```
   jupyter notebook
   ```

2. Run notebooks in this order:
   * Data extraction notebook (API calls and data collection)
   * Data preprocessing notebook (if applicable)
   * Phase 1 analysis notebook
   * Phase 2 analysis notebook

3. Each notebook will:
   * Fetch bus arrival data from LTA API using requests library
   * Process data with pandas
   * Store results in PostgreSQL using pandas.to_sql()
   * Generate analysis results

## Data Structure

The project uses the following data fields from LTA Bus Arrival API:

* **ServiceNo**: Bus service number
* **Operator**: Bus operator (e.g., SBST, SBS)
* **Load**: Passenger load percentage
* **ArrivalTime**: Bus arrival timestamp (Singapore Time)

Two separate tables are created:
* **phase1_data**: Counterfactual simulation results
* **phase2_data**: A/B test results

## Verifying Your Setup

To verify everything is working:

1. Test Python and libraries:
   ```
   python -c "import pandas, psycopg2, requests; print('All imports successful')"
   ```

2. Test PostgreSQL connection:
   ```
   psql -U postgres -d singapore_bus_analysis
   ```

3. Test LTA API key (in a notebook cell):
   ```python
   import requests
   url = "http://datamall2.mytransport.sg/api/LTADataMall/BusArrival"
   params = {'BusStopCode': '01012'}
   headers = {'AccountKey': api_key}
   response = requests.get(url, params=params, headers=headers)
   print(response.status_code)
   ```

If status code is 200, your API key is working correctly.

## Troubleshooting

**PostgreSQL connection error**: 
* Ensure PostgreSQL service is running
* Check password is correct
* Verify database name matches

**API returns 401 error**:
* Check your API key is correct
* Ensure AccountKey header is properly formatted
* Verify LTA DataMall account is active

**Pandas to_sql() timeout**:
* May occur with large datasets
* Increase timeout in connection string:
  ```python
  engine = create_engine(db_connection_string, 
                        connect_args={'connect_timeout': 30})
  ```

## Next Steps

Once setup is complete:
1. Review phase2.md for methodology details on the sigmoid function and utility function
2. Run the data extraction notebook to collect bus arrival data
3. Execute Phase 1 analysis notebooks for counterfactual analysis
4. Execute Phase 2 analysis notebooks for A/B test results
5. Check PostgreSQL tables to verify data storage

For questions or issues, refer to the project README or create an issue on GitHub.
