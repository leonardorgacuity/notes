- **employee_id**: A unique string identifier for each employee.  
   _Note_: It is possible for an `employee_id` to be shared across employers, as it is only unique within a given employer’s records. Therefore, `employer_id + employee_id` ensures uniqueness for a particular person.

  Since `employee_id` and `email` may change over time (e.g., due to company acquisition or employee name change), we create an immutable unique identifier for each person using the following:  
   `TO_HEX(SHA256(CONCAT(COALESCE(stg.employee_id), COALESCE(stg.email))))`.  
   This hashed ID is used to populate `employee_id` and `manager_id` columns downstream from staging tables for a common reference. The history of `employee_id` and `email` changes is tracked at the worker level in `people.dim_worker_ids_scd`.

---

!QUESTION
Q: What exactly is the employee_id for this? is it the unique identifier for each employee or `TO_HEX(SHA256(CONCAT(COALESCE(stg.employee_id), COALESCE(stg.email))))`?  
A: The `employee_id` is a unique string identifier for each employee within a specific employer's records, while the hashed ID is used for a common reference across systems.

!QUESTION
Q: what is people.dim_worker_ids_scd? is it a table name?  
A: Yes, `people.dim_worker_ids_scd` is a table that tracks the history of employee identifiers and their changes over time.

!TASK
Q: Note: Do not build the Influencers >> Teams page that exists in our NiceGUI app, as this is being deprecated. (Should I remove it?)

!TASK
Q: The global Manager filter in NiceGUI (left hand pane) is somewhat janky (e.g., when searching, the tree doesn’t expand to the searched value), so we would love to see a new design for this.

!TASK
Q: The Survey Name filter should always default to the survey with an Experience survey type that has the most recent survey_start_datetime (surveys.def_survey_parameters table) that is on or before the current date. Surveys with a survey_start_date after the current date should not show in the dashboard at all. The Survey Name options in the dropdown list should be sorted from most recent (top) to earliest (bottom).

!TASK
Q: Our current dashboard is mostly reading pre-aggregated metrics. Going forward, we would like the following dashboard pages to read row-level data, with the calculations and aggregations that are currently performed in GBQ being performed in the UI. This will allow dynamic date filtering for “always on” Lifecycle survey types, which function differently from the Experience and Pulse survey types that have defined start and end dates.
Questions >> Ratings
Questions >> Comments
Questions >> Multiple Choice (new page… requirements are below)
Analytics >> Heatmap (page is currently named Distributions)

!TASK
Q: The front end should have an OrgAcuity-branded URL.

- URL?

!TASK
Q: We need a user login page where users can authenticate to the dashboard (with the proper level of access… Super User vs. Manager). Shown below is the design we want to achieve for the first and second login screens. OrgAcuity’s logo (svg file) is here. You can ignore “Good Morning!” and include “Welcome to OrgAcuity.” beneath the logo.

!QUESTION
Q: Are we using an authentication library for our auth?
