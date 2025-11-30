# School Funding & Poverty Analysis Dashboard

An interactive data visualization project exploring the relationship between educational funding, poverty rates, and resource allocation across U.S. school districts.

## Project Overview

This project examines the complex relationship between school funding and student poverty levels across the United States. The analysis introduces the **Poverty Sensitivity Index (PSI)**, a novel metric that measures how well school funding responds to student poverty levels.

**PSI Formula**: `Revenue per Student ÷ Poverty Rate`

Higher PSI values indicate districts that receive more funding per student relative to their poverty rates, suggesting better resource allocation for high-need populations. For example, a PSI of 400,000 means the district allocates $400,000 per student for each percentage point of poverty.

## Interactive Visualization (index.html)

The `index.html` file contains a fully interactive D3.js-powered dashboard featuring:

### Visualizations
- **State-Level Map**: Displays PSI scores for all U.S. states using a color-coded choropleth map
- **District-Level Maps**: Click on any state to view detailed district-level data within that state
- **Comparison Charts**: Shows selected district metrics compared against state and national averages for:
  - Revenue per Student
  - PSI Score
  - Share of school-age population in poverty
- **Scatter Plot**: Visualizes the relationship between revenue per student and poverty rates, with points colored by PSI score

### Key Features
- Interactive tooltips showing detailed statistics for each district
- Dynamic color scales based on PSI values
- Responsive design with centered layout
- Project overview section explaining methodology and insights

## Data Sources

All data is sourced from the **Urban Institute's Education Data API** and the **U.S. Census Bureau**, as detailed in the `Urban_Institute_API.ipynb` notebook.

### 1. School Finance Data
**Source**: Urban Institute Education Data API - School Districts Finance (2020)
- API Endpoint: `https://educationdata.urban.org/api/v1/school-districts/ccd/finance/2020/`
- **Data Collected**:
  - Total revenue (federal, state, and local)
  - Total expenditures
  - Current expenditures for elementary and secondary education
  - Student enrollment numbers

### 2. Poverty Estimates
**Source**: Urban Institute Education Data API - SAIPE (Small Area Income and Poverty Estimates, 2020)
- API Endpoint: `https://educationdata.urban.org/api/v1/school-districts/saipe/2020/`
- **Data Collected**:
  - Total population estimates
  - School-age population (ages 5-17)
  - School-age population in poverty
  - Poverty rates

### 3. Geographic Boundaries
**Source**: U.S. Census Bureau TIGER/Line Shapefiles (2020)
- **File Location**: `https://www2.census.gov/geo/tiger/TIGER2020/UNSD/`
- **Data Collected**:
  - Unified School District boundaries
  - Elementary School District boundaries
  - Secondary School District boundaries
  - State boundaries

All three school district types were combined to ensure complete geographic coverage across the United States.

## Urban_Institute_API.ipynb

This notebook handles all data acquisition and initial processing:

### What It Does
1. **Fetches Financial Data**: Retrieves 2020 school district finance data from the Urban Institute API, including revenue sources, expenditures, and enrollment figures
2. **Fetches Poverty Data**: Downloads SAIPE poverty estimates for school-age populations by district
3. **Downloads Shapefiles**: Combines three types of school district shapefiles (Unified, Elementary, Secondary) from the Census Bureau to ensure complete geographic coverage
4. **Merges Data**: Joins financial data, poverty data, and geographic boundaries into a single comprehensive dataset
5. **Spatial Join**: Links districts with their corresponding states using geographic analysis
6. **Cleans Coordinates**: Fixes geometry issues (particularly for Virginia) to ensure proper visualization
7. **Aggregates by State**: Creates state-level summaries by aggregating district-level data
8. **Exports Data**: Saves final datasets as GeoJSON files for visualization

### Key Outputs
- `finance_poverty_school_districts_FINAL.geojson`: District-level data with all metrics
- `updated_states.geojson`: State-level aggregated data

## Clean and prep data.ipynb

This notebook performs final data cleaning and creates the PSI metric:

### What It Does

#### State-Level Processing
1. **Data Cleaning**:
   - Removes unnecessary columns (RESA expenditures, density, IDs)
   - Renames columns for clarity
   - Removes Puerto Rico from the dataset
   - Rounds numerical values for consistency
2. **PSI Calculation**: Creates the Poverty Sensitivity Index using the formula:
   ```
   PSI = (Total Revenue / Number of Students) / (Estimated School Age Poverty / Estimated School Age Population)
   ```
3. **Additional Metrics**: Calculates Revenue per Student and Poverty Share for easier interpretation
4. **Export**: Saves cleaned state data as `state_FINAL.geojson`

#### District-Level Processing
1. **Column Management**: Renames columns, drops duplicates, and removes unnecessary fields
2. **Poverty Adjustment**: Handles zero-poverty cases by setting them to NaN to avoid division errors
3. **PSI Calculation**: Computes district-level PSI only for valid cases (districts with students and poverty data)
4. **Revenue per Student**: Calculates per-student funding for each district
5. **Data Validation**: Identifies and handles outliers and edge cases
6. **Export**: Saves final district data as `districts.geojson`

### Key Transformations
- Replaces underscores with spaces in column names for readability
- Converts -1, -2, -3 values to NaN (Urban Institute's missing data codes)
- Masks invalid calculations where student enrollment is zero
- Removes the Aleutian Region School District (data quality issues)

## Technologies Used

- **Python**: Data processing and API requests
- **GeoPandas**: Spatial data manipulation
- **Pandas**: Data cleaning and transformation
- **D3.js**: Interactive visualizations
- **GeoJSON**: Geographic data format
- **HTML/CSS/JavaScript**: Web-based dashboard

## Files in This Repository

- `index.html`: Interactive visualization dashboard
- `Urban_Institute_API.ipynb`: Data acquisition notebook
- `Clean and prep data.ipynb`: Data cleaning and PSI calculation notebook
- `state_FINAL.geojson`: Cleaned state-level data
- `districts.geojson`: Cleaned district-level data
- `README.md`: This file

## How to Use

1. Open `index.html` in a web browser to view the interactive dashboard
2. Click on any state in the map to explore district-level data
3. Hover over districts to view detailed statistics
4. Click on districts to see comparison charts

## Key Insights

- The analysis reveals significant regional variations in funding equity
- Some states show consistently higher PSI values, suggesting more equitable funding formulas
- The findings highlight districts that may benefit from funding formula adjustments
- The PSI metric helps identify areas where funding may not adequately address student poverty levels

## License

Data sources:
- Urban Institute Education Data API
- U.S. Census Bureau TIGER/Line Shapefiles
