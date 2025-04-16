# OrgAcuity

Authenticate via SSO
SSO?
SSO is a single sign-on authentication method that allows users to access multiple applications with one set of credentials. It simplifies the login process and enhances security by reducing the number of passwords users need to remember.

Use AG Grid enterprise for tables and grid
Note:
Do not build Influencers page (depracated)

NiceGUI?

---

## Data Processing Groups

Beyond the users of our platform, it is important to understand three groups of people for which we process and store data:

1. **Active Workers**: Employers provide data on their entire population of workers.
2. **Survey Recipients**: Employers invite a sample (or all) of these workers to participate in surveys.
3. **Survey Respondents**: A subset (or all) of the invited workers respond to surveys and submit survey responses.

_Notes_:

- We use the terms worker, employee, and person/people interchangeably
- We use the terms employer, company, and organization interchangeably

### Key Fields

Here are several key fields that are critical to our data processing and application functionality:

- **employer_id**: A unique numeric identifier for each employer (customer). This column exists in every GBQ table and is a partitioned column to support performant queries.  
   _Note_: This ensures that each employer’s data is presented only to that employer and no one else. It is a **very important** field for our application.

- **snapshot_date**: The effective date of the worker data provided by the employer. For each survey, we retrieve and integrate the most recent worker data with a `snapshot_date` that is on or before `survey_start_datetime`.

- **survey_id**: A unique string identifier for each survey.

- **employee_id**: A unique string identifier for each employee.  
   _Note_: It is possible for an `employee_id` to be shared across employers, as it is only unique within a given employer’s records. Therefore, `employer_id + employee_id` ensures uniqueness for a particular person.

  Since `employee_id` and `email` may change over time (e.g., due to company acquisition or employee name change), we create an immutable unique identifier for each person using the following:  
   `TO_HEX(SHA256(CONCAT(COALESCE(stg.employee_id), COALESCE(stg.email))))`.  
   This hashed ID is used to populate `employee_id` and `manager_id` columns downstream from staging tables for a common reference. The history of `employee_id` and `email` changes is tracked at the worker level in `people.dim_worker_ids_scd`.

- **manager_id**: The `employee_id` for an employee’s manager.

### Notes on Survey Items and Factors

- Survey items are grouped under **factors**, which represent high-level categorizations (e.g., Engagement, Manager Effectiveness).
- Customers can choose between **standard survey items** or **custom survey items**:
  - **Standard survey items**: These have fixed `reference_id` values starting with "s" (stored in `reference.ref_survey_config`) and remain consistent across employers and surveys.
  - **Custom survey items**: These do not have fixed `reference_id` values and are specific to the employer or survey.

#### Keys for Trending and Overall Scores:

- **Overall multi-item factors**: When `item` is `null`, join on `factor` + `reference_id`.
- **Standard items**: When `category` ≠ 'Custom', join on `reference_id`.
- **Custom items**: When `category` = 'Custom', join on `item`.

---

### Notes on Global Manager Filtering

A critical table for global Manager filtering is `curated.viz_mgr_vw_setup`. This table contains the following key details for each `snapshot_date`:

- **user_id**: Represents every manager.
- **manager_id**: Represents each manager’s manager.
- **user_layer**: Indicates how many layers a manager is from the senior-most leader (e.g., CEO).
- **manager_layer**: Indicates the relative layer of a manager to the respective `user_id`.

#### Key Characteristics:

- This table is designed to build the global manager hierarchy for filtering results.
- It includes only people managers (those with one or more direct reports) and excludes employees who are not managers.
- To capture all employees (managers and individual contributors), this table must be joined with the `manager_id` column in our `viz` tables.

#### Handling Missing Matches:

- Some dashboard tables may not match certain `manager_id` values in `viz_mgr_vw_setup`. This is expected due to data suppression for small samples (e.g., Analytics >> Drivers).
- The global Manager filter must function like a **LEFT JOIN**, displaying all values regardless of matches to base data sets.

#### Survey Type Considerations:

- **Experience Surveys**: Use `curated.viz_mgr_vw_setup` records for the `snapshot_date` defined in `surveys.def_survey_parameters`.
- **Lifecycle Surveys**: Use `curated.viz_mgr_vw_setup` records for the `max(snapshot_date)` since no `snapshot_date` is defined in `surveys.def_survey_parameters`.

This table is essential for ensuring accurate filtering and hierarchy representation across dashboards and reports.

---

### Notes on Surveys

- **Experience Surveys**: These are point-in-time surveys with a defined start and end datetime, typically conducted over a two-week window. They are designed to capture employee sentiment or feedback at a specific moment in time.
- **Lifecycle Surveys**: These are “always on” surveys triggered by specific events in an employee's lifecycle, such as onboarding (hire) or exit (termination). They provide insights into key moments in the employee journey.

_Important_: Every customer will have Experience surveys, but Lifecycle surveys are optional and may not be implemented by all customers.

---

## General Design Notes and Tasks

### General Design Overview

The updated NiceGUI dashboard, combined with the changes outlined in this document, will define the success of the new frontend. The design will leverage AG Grid Enterprise for tables and charts, ensuring a consistent and modern user experience.

### Key Design Elements

1. **AG Grid Enterprise**:

   - Use for all data tables and charts.
   - Data tables must include sorting, filtering, search, and Excel export capabilities.
   - Advanced features like pivots and aggregate functions should be disabled.
   - Hierarchical data (e.g., Participation >> Teams) and Influencers pages should format names with darker font and job titles in lighter font.

2. **Charts**:

   - Bubble Charts: [Example](https://www.ag-grid.com/charts/gallery/multiple-bubble-series/)
   - Heatmap: [Example](https://www.ag-grid.com/charts/gallery/heatmap-with-labels/)
   - Line Chart: [Example](https://www.ag-grid.com/charts/gallery/simple-line/)
   - Normalized Bar Chart: [Example](https://www.ag-grid.com/charts/javascript/bar-series/)

3. **Styling**:

   - Retain NiceGUI dashboard aesthetics but replace NiceGUI grays with the provided grayscale.
   - Use consistent font types, colors, sizes, tooltips, dialog boxes, animations, and transitions.

4. **Responsiveness**:

   - Transition from fixed/static dimensions to a flexible UI that adapts to screen sizes and content.

5. **Filters**:

   - Redesign the global Manager filter for improved usability (e.g., better tree expansion during search).
   - Survey Name filter should default to the most recent Experience survey and exclude future-dated surveys.

6. **Authentication**:

   - Add a user login page with OrgAcuity branding.
   - Include a second login screen with "Welcome to OrgAcuity" beneath the logo.

7. **Dashboard Enhancements**:

   - Transition specific pages to read row-level data for dynamic date filtering:
     - Questions >> Ratings
     - Questions >> Comments
     - Questions >> Multiple Choice (new page)
     - Analytics >> Heatmap (currently named Distributions)

8. **URL Branding**:
   - Ensure the frontend has an OrgAcuity-branded URL.

### Tasks to Be Completed

1. **UI Development**:

   - Implement AG Grid Enterprise for tables and charts.
   - Apply custom CSS for styling and aesthetics.

2. **Chart Integration**:

   - Add Bubble Charts, Heatmaps, Line Charts, and Normalized Bar Charts as per examples.

3. **Filter Redesign**:

   - Improve the global Manager filter functionality.
   - Implement Survey Name filter logic.

4. **Authentication**:

   - Design and implement the login page with OrgAcuity branding.

5. **Dynamic Data Handling**:

   - Update specified dashboard pages to read row-level data and perform calculations in the UI.

6. **Responsiveness**:

   - Refactor the UI for flexible layouts and adaptive design.

7. **Deprecations**:

   - Exclude the Influencers >> Teams page from the new design.

8. **URL Setup**:
   - Configure the frontend with an OrgAcuity-branded URL.

### Notes

- Ensure all changes align with the provided design examples and requirements.
- Focus on improving usability and performance while maintaining brand consistency.
- Lifecycle survey-specific differences should be implemented as outlined.

### Notes on Handling Minimum N-Count Threshold

When the minimum n-count threshold is not met for a particular manager, the following adjustments must be implemented to ensure accurate representation:

1. **Engagement Score**:

   - Display “--” instead of `0` to avoid misinterpretation of a highly disengaged team.
   - _Note_: This ensures that the absence of data is not misinterpreted as a negative outcome.

2. **Scores Increased/Decreased**:

   - Replace “None” with “--” in the sentences under the **Questions** section.
   - _Note_: This change highlights the lack of sufficient data rather than implying no change occurred.

3. **Retention Risk**:

   - Display “--” instead of “None” to align with the updated handling logic.
   - _Note_: This avoids confusion by clearly indicating that data is unavailable rather than suggesting no risk exists.

_Note_: These changes are essential for maintaining consistency and avoiding confusion in scenarios where data thresholds are not met.

---

### Notes on Strengths and Opportunities Identification

OrgAcuity's Focus Agent employs a sophisticated methodology to identify strengths and opportunities, ensuring actions have the greatest impact on organizational effectiveness. This process involves a **heartbeat analysis** to determine items that respondents care about most, both positively and negatively.

#### Key Definitions:

- **Strengths**: Items rated significantly higher than a respondent's average rating.  
   _Note_: If there is a tie, the item with the highest average score is selected.

- **Opportunities**: Items rated significantly lower than a respondent's average rating.  
   _Note_: If there is a tie, the item with the highest percentage of neutral respondents is selected.

#### Methodology:

1. **Individual Baseline**: Each respondent's average rating across all items is calculated.
2. **Deviation Analysis**: Items are flagged as strengths or opportunities based on significant deviations from the respondent's baseline.
3. **Tie-Breaking**: Ties are resolved using predefined rules to ensure consistency.

This approach minimizes bias and ensures that actionable insights are derived from the data.

---

### Notes on Collapsed and Expanded Table Views

#### Collapsed Table View:

When the table’s records are collapsed, display only the following three columns:

1. **Workforce Segment**:

   - Format: `Attribute Name > Attribute Value`
   - Display `Attribute Type` in smaller, lighter font beneath.

2. **Favorable Alerts**:

   - Count of level 2 records where `Alert Type` is either `Increases` or `High Scores`.

3. **Unfavorable Alerts**:
   - Count of level 2 records where `Alert Type` is either `Decreases` or `Low Scores`.

#### Expanded Table View:

For level 2 rows, display the following metrics:

- **Score**
- **Comparison**
- **% Difference**
- **n**
- **Response Rate**

---

### Page-Specific Filters

1. **Alert Type**:

   - Multi-select filter to allow selection of specific alert types.

2. **Show**:
   - Single-select filter to display top X alerts based on the `Rank` field.
   - Options:
     - **All**
     - **Top 10** (default)
     - **Top 25**
     - **Top 50**
     - **Top 100**

_Note_: Ensure the default filter settings are applied when the page loads.

---

- **employee_id**: A unique string identifier for each employee.  
   _Note_: It is possible for an `employee_id` to be shared across employers, as it is only unique within a given employer’s records. Therefore, `employer_id + employee_id` ensures uniqueness for a particular person.

  Since `employee_id` and `email` may change over time (e.g., due to company acquisition or employee name change), we create an immutable unique identifier for each person using the following:  
   `TO_HEX(SHA256(CONCAT(COALESCE(stg.employee_id), COALESCE(stg.email))))`.  
   This hashed ID is used to populate `employee_id` and `manager_id` columns downstream from staging tables for a common reference. The history of `employee_id` and `email` changes is tracked at the worker level in `people.dim_worker_ids_scd`.

---
