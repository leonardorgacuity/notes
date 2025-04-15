# OrgAcuity Dashboard Tables Reference Guide

## Table Organization in Database

### Core Database Schemas
- **curated**: Contains pre-aggregated visualization data
- **surveys**: Contains survey configurations and parameters
- **reference**: Contains reference data, display names, and configurations
- **people**: Contains user management and worker data

## Key Tables by Page

### Global Data Tables

| Table Name | Schema | Purpose | Related Pages |
|------------|--------|---------|--------------|
| `viz_mgr_vw_setup` | curated | Drives global Manager filtering | All pages |
| `ref_survey_parameters` | reference | Survey configurations and thresholds | All pages |
| `ref_survey_config` | reference | Standard survey item definitions | Questions pages |
| `ref_attributes` | reference | Display names and derived flags for attributes | Multiple pages |
| `ref_display_names` | reference | Dashboard display names for custom dimensions | Multiple pages |
| `ref_favorability_ranges` | reference | Color range definitions for heatmaps | Analytics » Heatmap |
| `viz_survey_responses` | curated | Individual-level response data | Questions » Ratings, Multiple Choice |
| `viz_survey_recipients` | curated | Survey recipient data | Analytics » Heatmap |

### Home Pages

| Table Name | Schema | Purpose | Related Pages |
|------------|--------|---------|--------------|
| `viz_summary_metrics` | curated | Summary metrics for dashboard | Home » Summary |
| `viz_strengths_opportunities` | curated | Top strengths and opportunities | Home » Summary |
| `viz_alerts` | curated | Alert data for dashboard | Home » Alerts |
| `viz_narratives` | curated | Narratives for action page | Home » Action |

### Participation Pages

| Table Name | Schema | Purpose | Related Pages |
|------------|--------|---------|--------------|
| `viz_participation_trends` | curated | Participation trend data | Participation » Trends |
| `viz_participation_teams` | curated | Team participation data | Participation » Teams |
| `viz_participation_representation` | curated | Attribute representation | Participation » Representation |
| `viz_participation_distributions` | curated | Segment distribution data | Participation » Representation |

### Questions Pages

| Table Name | Schema | Purpose | Related Pages |
|------------|--------|---------|--------------|
| `viz_ratings_summary` | curated | Summary of question ratings | Questions » Ratings |
| `viz_comment_summary` | curated | Comment sentiment summary | Questions » Comments |
| `viz_comment_detail` | curated | Detailed comment data | Questions » Comments |

### Influencers Pages

| Table Name | Schema | Purpose | Related Pages |
|------------|--------|---------|--------------|
| `viz_influencers` | curated | Influencer metrics | Influencers » Influencers |
| `viz_networks` | curated | Network connection data | Influencers » Networks |
| `viz_leaders` | curated | Leader effectiveness data | Influencers » Leaders |

### Analytics Pages

| Table Name | Schema | Purpose | Related Pages |
|------------|--------|---------|--------------|
| `viz_drivers` | curated | Driver analysis data | Analytics » Drivers |
| `viz_heartbeat_analysis` | curated | Time-based heartbeat data | Analytics » Heartbeat Analysis |
| `viz_heatmap` | curated | Heatmap data | Analytics » Heatmap |

## User Management Tables

| Table Name | Schema | Purpose |
|------------|--------|---------|
| `dim_users` | people | Complete list of all users |
| `dim_worker_ids_scd` | people | History of employee_id and email changes |
| `user_list` | *UI-driven* | Dynamic list of every person from worker files |
| `group_list` | *UI-driven* | Employer-created user groups |
| `role_list` | *UI-driven* | Access roles definitions |
| `page_list` | *UI-driven* | List of dashboard pages |
| `attribute_list` | *UI-driven* | Dynamic list of employer attributes |
| `results_list` | *UI-driven* | Access scope options ("All" or "Team") |

## Assignment Tables

| Table Name | Purpose |
|------------|---------|
| `user_assignments` | Maps users to roles |
| `group_assignments` | Maps users to groups |
| `survey_assignments` | Maps roles to accessible surveys |
| `page_assignments` | Maps roles to accessible dashboard pages |
| `attribute_assignments` | Maps roles to accessible attributes |
| `result_assignments` | Maps roles to results scope ("All" or "Team") |

## Common Table Relationships

- **Employee Data**: Join tables using `employer_id` + `employee_id` for uniqueness
- **Survey Data**: Link using `survey_id` and `reference_id` 
- **Manager Hierarchy**: `user_id` and `manager_id` columns in `viz_mgr_vw_setup`
- **Attribute Data**: Access via `attribute_type`, `attribute_name`, and `attribute_value` fields

## Key Identifiers

| Field | Description | Notes |
|-------|------------|-------|
| `employer_id` | Customer identifier | Exists in all tables, critical for data security |
| `employee_id` | Worker identifier | Unique within employer, may change over time |
| `survey_id` | Survey identifier | Unique string for each survey |
| `snapshot_date` | Worker data effective date | Used to match worker data to surveys |
| `manager_id` | Manager identifier | Used for hierarchical filtering |
| `reference_id` | Item identifier | Used to link standard survey items across surveys |

## Thresholds and Ranges

| Metric Type | Threshold Source |
|------------|------------------|
| Quantitative metrics | `reference.ref_survey_parameters.quantitative_threshold` |
| Qualitative metrics | `reference.ref_survey_parameters.qualitative_threshold` |
| Color ranges | `reference.ref_favorability_ranges` |