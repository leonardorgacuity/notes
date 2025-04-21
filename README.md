# Task checklist

## Pending Tasks

- [x] fix build bug with AG Grid versions
- [x] Check Home pages adherance to requirements document
- [ ] `Home/Alerts`, Add filter function top 10, 25, 50, 100; add client side sorting.
- [ ] Fix Table Components column to have minimum width to see the contents.
- [ ] Global specific Filter, add nested number for manager hierarchy. i.e (1, 2, 3, 4)
  - groups users base on their manager_layer number, from smallest number to biggest (ascending)
  - The Survey Name filter should always default to the survey with an Experience survey type that has the most recent survey_start_datetime (surveys.def_survey_parameters table) that is on or before the current date. Surveys with a survey_start_date after the current date should not show in the dashboard at all. The Survey Name options in the dropdown list should be sorted from most recent (top) to earliest (bottom).
  - example queries:
    - `select \* from curated.viz_mgr_vw_setup`
    - `select \* from curated.viz_mgr_vw_setup where employer_id = 1 and user_layer = 0`
    - `select \* from curated.viz_mgr_vw_setup where employer_id = 1 and user_layer = 0 and - user_id = '' order by snapshot_data, manager_layer`
- [ ] `Questions/Rating`, (Enforce thresholds with filter) make sure it complies to threshold. If it does not comply to minimum. Do not display the data and display placeholder.

  - the query for threshold is `select * from surveys.def_survey_parameters`
  - quantitative_threshold scores for numeric scores, qualitative_threshold scores for text data.

- [ ] `Analytics/Heatmap`, also enforce threshold like the `Questions/Ratings` representation

- [ ] `Analytics/Heatmap` Make updates to any query of the deprecated tables

  - `admin.adm_display_names` to `people.def_display_names`
  - `admin.adm_survey_parameters` to `surveys.def_survey_parameters`
  - `admin.adm_attributes` to `reference.ref_attributes`
  - when looking at changes, just fetch the comparison survey id and get the record of that survey, if it is null, that means it is recently created and no previous survey to compare with.
  - to simplify things, just check comparison_survey_id.
  - Lifecycle surveys do not have snapshot_start_date and snapshot_end_date.

- [ ] `Analytics/Heartbeat Analysis` - create page, refer to requirements document.
- [ ] `Analytics/Heatmap` - default to category, attribute type = Tenure; attribute name = Org Tenure
- [ ] Add help question mark icon for pages that should have help text and show help texts; reference requirements doc.
- [ ] `Participation/Teams` - We should display managers (teams) in a drillable tree structure (like this example) rather than displaying the Layer value before the manager name. Note that these layer values are dynamic based on global Manager filter selections. For example, Layer 0 in this Team field represents people who report into the specific Manager selected via the global filter.
  - the reference is: https://www.ag-grid.com/example-hr/
- [ ] `Participation/Representation (NEW PAGE)` Requirements are in the orgacuity requirement doc.
- [ ] `Home/Action` (rename… currently named "Narratives"). check if requirements is applied correctly.
- [ ] `Questions/Comments` check if requirements is applied correctly.
- [ ] `Questions/Comments` compare new mockup page
- [ ] `Questions/Multiple Choice` check if requirements is applied correctly.
- [ ] `Analytics/Drivers` check if requirements is applied correctly.
- [ ] `Every data visualization should be exportable (e.g., .png file).`

- [ ] Admin Page - reference requirements document

  - Can assign roles in the application for users
  - User Groups
  - 3 types of roles
    - admin (can have config capabilities can assign roles
    - super user (can see survey results for entire company)
    - manager (can only see results to their immediate team)

---

# Questions and Tasks for OrgAcuity Dashboard Development

# Questions

### Q

What exactly is the employee_id for this? is it the unique identifier for each employee or `TO_HEX(SHA256(CONCAT(COALESCE(stg.employee_id), COALESCE(stg.email))))`?

---

### Q

What is people.dim_worker_ids_scd? is it a table name?

---

### Q

Are we using an authentication library for our auth?

---

### Q

`Page: Home >> Alerts`

document reference:

```
We need two page-specific filters:
Alert Type (multi-select)
Show (single-select… display top X alerts based on Rank field)
Options
All
Top 10 (default)
Top 25
Top 50
Top 100
```

Q:

- currently no image on how it should look like.
- is it the same as video reference in representation page?

---

### Q

Does the filter on upper right going to be in the same location for every page? But will replace how if functions and replace the selection depending on where on the page they are currently are.

---

### Q

`Page: Questions >> Ratings`

- It does not have image representation in the documentation that I could reference as guide for the design. I need input from orgacuity partners.

---

### Q

```
There should be a toggle with two options: "Scores" and "Changes"...

When a user selects "Scores" (the default option), the metrics in the heatmap should represent average scores: AVG(CAST(response AS INT64)). We should display the metric rounded to the nearest whole number.
When a user selects "Changes", the metrics in the heatmap should display the difference between the average score for the user-selected survey and the average score for the prior survey. Use `this logic` to identify the survey_id for the prior survey.
```

- link of `this logic` is not working. https://drive.google.com/file/d/1rsjAbp3SdZOhcRP9KCgGY5N7zbKBCqWd/view

---

### Q

`Page: Analytics Heatmap`

- About sorting UI and function, currently it has built-in table sorting. But the docs has different reference for sort in a separate component. Which one should I follow?

---

### Q

`Page: Analytics >> Heartbeat Analysis (NEW)`

- Is it the same with Analytics >> Heartbeat? For comparison. Should I remove the current heartbeat route?
- Instruction in here `https://docs.google.com/document/d/1OHVsTNeGxsF2_veoIU5adEBmYWOl1Ano04fySBbOOgU/edit?tab=t.0` does not include Heartbeat but Heartbeat Analysis. Should I remove the current heartbeat route?

---

<br><br><br>

# Questions and Tasks if Answered

#### Q

Video reference for Filter in Representation page for Participation, Is this collapsible dropdown within dropdown? Or is it the same in Analytics > Heatmap?

#### T

Check if the filter in Representation page is collapsible dropdown within dropdown or not and apply if needed changes.

---

#### Q

Currently the Home Alerts table representation is much more verbose than the 3 column represented in the documentation. Need to identify which source should I follow. Then do it as task.

#### T

Check if the representation in the documentation is correct or not. If it is correct, then apply the changes to the current representation.

---

#### Q

`Page: Participation >> Teams` <br>
"Replace the existing help text available via the info icon with this text". Is help icon a question mark icon?.

#### T

Check if the help icon is a question mark icon or not. If it is, then apply the changes.

---

#### Q

```
There are several pages in which data dimensions are concatenated. We need to break these concatenated values into separate filters.

- Page: Influencers >> Networks
- Page: Analytics >> Heatmap
- Page: Analytics >> Drivers
```

#### T

Need more clarifying info for this

---

<br><br><br>

# Tasks

### T

Do not build the Influencers >> Teams page that exists in our NiceGUI app, as this is being deprecated. (Should I remove it?)

---

### T

The global Manager filter in NiceGUI (left hand pane) is somewhat janky (e.g., when searching, the tree doesn't expand to the searched value), so we would love to see a new design for this. Is it the upper left corner filter component?

---

### T

The Survey Name filter should always default to the survey with an Experience survey type that has the most recent survey_start_datetime (surveys.def_survey_parameters table) that is on or before the current date. Surveys with a survey_start_date after the current date should not show in the dashboard at all. The Survey Name options in the dropdown list should be sorted from most recent (top) to earliest (bottom).

---

### T

Our current dashboard is mostly reading pre-aggregated metrics. Going forward, we would like the following dashboard pages to read row-level data, with the calculations and aggregations that are currently performed in GBQ being performed in the UI. This will allow dynamic date filtering for "always on" Lifecycle survey types, which function differently from the Experience and Pulse survey types that have defined start and end dates.
Questions >> Ratings
Questions >> Comments
Questions >> Multiple Choice (new page… requirements are below)
Analytics >> Heatmap (page is currently named Distributions)

---

### T

The front end should have an OrgAcuity-branded URL.

- URL?

---

### T

Q: We need a user login page where users can authenticate to the dashboard (with the proper level of access… Super User vs. Manager). Shown below is the design we want to achieve for the first and second login screens. OrgAcuity's logo (svg file) is here. You can ignore "Good Morning!" and include "Welcome to OrgAcuity." beneath the logo.

---

### T

Check if HOME Page > Summary is the requirements is applied correctly.

---

### T

We should display managers (teams) in a drillable tree structure (like this example) rather than displaying the Layer value before the manager name. Note that these layer values are dynamic based on global Manager filter selections. For example, Layer 0 in this Team field represents people who report into the specific Manager selected via the global filter.

- the reference is: https://www.ag-grid.com/example-hr/

---

### T

`Page: Participation >> Representation (NEW PAGE)`

- Requirements are in the orgacuity requirement doc.

---

### T

`Page: Analytics >> Heatmap (rename… currently named "Distributions")`

- check if the requirements is applied correctly.

---

### T

```
We need to replace the table on the Distributions page with a dynamic heatmap containing every Factor + Item down the y-axis and each value of the selected Attribute across the x-axis (as shown in the image below). By default, only factor-level rows/metrics should display in the heatmap, and users should be able to drill into the factors to see rows/metrics for the individual survey items related to each factor. Both horizontal and vertical scrolling will need to be enabled given the number of display values is not static across surveys and dimensions. We need to display the following metrics within tooltips when users hover over each cell/metric:
Favorable
Neutral
Unfavorable
Response Rate
```

- Currently has no tooltip in the app.

### T

Check if the requirements is applied correctly.

---

### T

`Page: Home >> Action (rename… currently named "Narratives")`

- check if requirements is applied correctly.

---

### T

`Page: Questions >> Comments`

- check if requirements is applied correctly.

---

### T

`Page: Questions >> Comments`

- Compare new mockup page

---

### T

`Page: Questions >> Multiple Choice`

- check if requirements is applied correctly.

---

### T

`Page: Analytics >> Drivers`

- check if requirements is applied correctly.
- currently no image representation in the documentation for reference.

---

### T

`Page: Admin (NEW)`

- It does not exist at the moment.

---

### T

`Page: User Management on left pane (NEW)`

- It does not exist at the moment.

---

### T

`Page: Users (NEW)`

- It does not exist at the moment.

---

### T

`Page: Group (NEW)`

- It does not exist at the moment.

---

### T

`Page: Roles (NEW)`

- It does not exist at the moment.

---

### T

```
There should be two icons in the upper right that look like those provided below. The lighter grey color assigned to the existing icons in NiceGUI should be applied to these so that they do not detract from the data on which users should be focused.

When the user clicks the question mark icon, there should be two options:
Product Feedback (link: https://www.orgacuity.com/product-feedback)
Technical Support (link: https://www.orgacuity.com/technical-support)
```

- reference the requirements documentation

---

### T

Every data visualization should be exportable (e.g., .png file).

---

### T

Add filter functionality per page, reference the orgacuity requirement docs.
