# Questions and Tasks for OrgAcuity Dashboard Development

!QUESTION
What exactly is the employee_id for this? is it the unique identifier for each employee or `TO_HEX(SHA256(CONCAT(COALESCE(stg.employee_id), COALESCE(stg.email))))`?

---

!QUESTION
What is people.dim_worker_ids_scd? is it a table name?

---

!TASK
Do not build the Influencers >> Teams page that exists in our NiceGUI app, as this is being deprecated. (Should I remove it?)

---

!TASK
The global Manager filter in NiceGUI (left hand pane) is somewhat janky (e.g., when searching, the tree doesn’t expand to the searched value), so we would love to see a new design for this. Is it the upper left corner filter component?

---

!TASK
The Survey Name filter should always default to the survey with an Experience survey type that has the most recent survey_start_datetime (surveys.def_survey_parameters table) that is on or before the current date. Surveys with a survey_start_date after the current date should not show in the dashboard at all. The Survey Name options in the dropdown list should be sorted from most recent (top) to earliest (bottom).

---

!TASK
Our current dashboard is mostly reading pre-aggregated metrics. Going forward, we would like the following dashboard pages to read row-level data, with the calculations and aggregations that are currently performed in GBQ being performed in the UI. This will allow dynamic date filtering for “always on” Lifecycle survey types, which function differently from the Experience and Pulse survey types that have defined start and end dates.
Questions >> Ratings
Questions >> Comments
Questions >> Multiple Choice (new page… requirements are below)
Analytics >> Heatmap (page is currently named Distributions)

---

!TASK
The front end should have an OrgAcuity-branded URL.

- URL?

---

!TASK
Q: We need a user login page where users can authenticate to the dashboard (with the proper level of access… Super User vs. Manager). Shown below is the design we want to achieve for the first and second login screens. OrgAcuity’s logo (svg file) is here. You can ignore “Good Morning!” and include “Welcome to OrgAcuity.” beneath the logo.

---

!QUESTION
Q: Are we using an authentication library for our auth?

---

!QUESTION AND TASK AFTER ANSWERED
Q: Video reference for Filter in Representation page for Participation, Is this collapsible dropdown within dropdown? Or is it the same in Analytics > Heatmap?
T: Check if the filter in Representation page is collapsible dropdown within dropdown or not and apply if needed changes.

---

!TASK
Check if HOME Page > Summary is the requirements is applied correctly.

---

!QUESTION AND TASK AFTER ANSWERED
Q: Currently the Home Alerts table representation is much more verbose than the 3 column represented in the documentation. Need to identify which source should I follow. Then do it as task
T: Check if the representation in the documentation is correct or not. If it is correct, then apply the changes to the current representation.

---

!QUESTION
We need two page-specific filters:
Alert Type (multi-select)
Show (single-select… display top X alerts based on Rank field)
Options
All
Top 10 (default)
Top 25
Top 50
Top 100

- currently no image on how it should look like.
- is it the same as video reference in representation page?

---

!QUESTION AND TASK AFTER ANSWERED:
Page: Participation >> Teams

Q: "Replace the existing help text available via the info icon with this text". Is help icon a question mark icon?.
T: Check if the help icon is a question mark icon or not. If it is, then apply the changes.

!TASK
We should display managers (teams) in a drillable tree structure (like this example) rather than displaying the Layer value before the manager name. Note that these layer values are dynamic based on global Manager filter selections. For example, Layer 0 in this Team field represents people who report into the specific Manager selected via the global filter.

- the reference is: https://www.ag-grid.com/example-hr/

---

!TASK
Page: Participation >> Representation (NEW PAGE)

- Requirements are in the orgacuity requirement doc.

---

!QUESTION
Does the filter on upper right going to be in the same location for every page? But will replace how if functions and replace the selection depending on where on the page they are currently are.

---

!QUESTION
Page: Questions >> Ratings

- It does not have image representation in the documentation that I could reference as guide for the design. I need input from orgacuity partners.

---

!TASK
Page: Analytics >> Heatmap (rename… currently named “Distributions”)

- check if the requirements is applied correctly.

---

!TASK
We need to replace the table on the Distributions page with a dynamic heatmap containing every Factor + Item down the y-axis and each value of the selected Attribute across the x-axis (as shown in the image below). By default, only factor-level rows/metrics should display in the heatmap, and users should be able to drill into the factors to see rows/metrics for the individual survey items related to each factor. Both horizontal and vertical scrolling will need to be enabled given the number of display values is not static across surveys and dimensions. We need to display the following metrics within tooltips when users hover over each cell/metric:
Favorable
Neutral
Unfavorable
Response Rate

- Currently has no tooltip in the app.

---

!QUESTION
There should be a toggle with two options: “Scores” and “Changes”...

When a user selects “Scores” (the default option), the metrics in the heatmap should represent average scores: AVG(CAST(response AS INT64)). We should display the metric rounded to the nearest whole number.
When a user selects “Changes”, the metrics in the heatmap should display the difference between the average score for the user-selected survey and the average score for the prior survey. Use `this logic` to identify the survey_id for the prior survey.

- link of `this logic` is not working. https://drive.google.com/file/d/1rsjAbp3SdZOhcRP9KCgGY5N7zbKBCqWd/view

---

!QUESTION
Page: Analytics Heatmap

- Sorting UI and function, currently it has built-in table sorting. But the docs has difference reference for sort in a separate component. Which one should I follow?

---

!TASK
Page: Home >> Action (rename… currently named “Narratives”)

- check if requirements is applied correctly.

---

!TASK
Page: Questions >> Comments

- check if requirements is applied correctly.

---

!TASK
Page: Questions >> Comments

- Compare new mockup page

---

!TASK
Page: Questions >> Multiple Choice

- check if requirements is applied correctly.

---

!TASK
Page: Anayltics >> Drivers

- check if requirements is applied correctly.
- currently no image representation in the documentation for reference.

---

!QUESTION
Page: Analytics >> Heartbeat Analysis (NEW)

- Is it the same with Analytics >> Heartbeat? For comparison. Should I remove the current heartbeat route?
- Instruction in here `https://docs.google.com/document/d/1OHVsTNeGxsF2_veoIU5adEBmYWOl1Ano04fySBbOOgU/edit?tab=t.0` does not include Heartbeat but Heartbeat Analysis. Should I remove the current heartbeat route?

---

!TASK
Page: Admin (NEW)

- It does not exist at the moment.

---

!TASK
Page: User Management on left pane (NEW)

- It does not exist at the moment.

---

!TASK
Page: Users (NEW)

- It does not exist at the moment.

---

!TASK
Page: Group (NEW)

- It does not exist at the moment.

---

!TASK
Page: Roles (NEW)

- It does not exist at the moment.

---

### OTHERS

!QUESTION AND TASK AFTER ANSWERED
Need more clarifying info for this.
There are several pages in which data dimensions are concatenated. We need to break these concatenated values into separate filters.

- Page: Influencers >> Networks
- Page: Analytics >> Heatmap
- Page: Analytics >> Drivers

---

!TASK
There should be two icons in the upper right that look like those provided below. The lighter grey color assigned to the existing icons in NiceGUI should be applied to these so that they do not detract from the data on which users should be focused.

When the user clicks the question mark icon, there should be two options:
Product Feedback (link: https://www.orgacuity.com/product-feedback)
Technical Support (link: https://www.orgacuity.com/technical-support)

---

!TASK
Every data visualization should be exportable (e.g., .png file).

---

!TASK
Add filter functionality per page, reference the orgacuity requirement docs.
