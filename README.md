# Jupyter Surveys for Teaching

This respository was developed as a means of embedding survey questions inside Jupyter notebooks. We use these surveys for teaching, as a means of facilitating data collection and analysis in live classroom environments. These tools are very definitely in *alpha*. We hope to develop them further in the future.

## 1 Basic Strategy

Each survey we develop consists of two parts: 

- a centralized Google sheet where all data from the survey are recorded. This Google sheet houses data from everyone who uses the survey no matter which class they are in. It also is attached to an Apps Script (accessible in the Extensions menu of the Google sheet.
- an html file that renders the survey in the Jupyter notebook and posts data to the Google sheet.

<div style="font-size: 18px; line-height: 1.4; border: 2px solid black; padding: 10px;"><b>Important Note:</b> Everyone using these survey tools has edit-level access to the centralized Google sheet. Please be careful not to make any changes in the basic structure or Apps Script as it will affect everyone using the survey tool.</div>

Some columns in the Google sheet will be defined by the specific survey questions asked. Others will be defined inside the Jupyter notebook, generally by the teacher, and used to filter data on access. This is what makes it possible for all classes data to be housed in a single Google sheet. We used this approach because it keeps the teacher from having to set up their own Google sheet for collecting data.

## 2 The Surveys (So Far)

We have developed a few specific surveys, each with an html file (in this repository) and a Google sheet. At some point we make this into a general tool, but for now, if you want a new survey you can work from one of these by example. 

### 2.1 The Jupyter notebook survey

This survey could be embedded inside any Jupyter notebook for the purpose of collecting feedback on the notebook.

**File:** survey-v1.html

**Google sheet:** <a href="https://docs.google.com/spreadsheets/d/11x_zyspHQ4nL-ucyaxy7eDUgYiFTar4FsP16kb6gGXk/edit?gid=0#gid=0">jupyter-survey-data</a>

Variables that should be defined inside the notebook:

- `class_id <- "arthur"` # put any unique id here (e.g., teacher email address)
- `user_id <- Sys.getenv("JUPYTERHUB_USER")` # this gets the users Jupyter ID
- `notebook_id <- "id"` # may be set to identify specific notebook

**Additional variables in the Google sheet:**

- `enjoy_rating`
- `learn_rating`
- `suggestions`
- `date`

**Example embed code to place in Jupyter markdown cell:**

```r
IRdisplay::display_html(sprintf(
  '<iframe src="https://uclatall.github.io/jupyter-survey/survey-v1.html?class_id=%s&notebook_id=%s&user_id=%s" width="950" height="540" 
    sandbox="allow-scripts allow-same-origin" 
    style="border: none; box-shadow: none; background: white;"></iframe>',
  class_id, user_id, notebook_id
))
```

**Example code for reading your class's data into R:**

```r
sheet_url <- "https://docs.google.com/spreadsheets/d/e/2PACX-1vT47ZPJ-19035LkwvRIubWry-liqNapq7Eyj_XhkWtqD8c01fHP62HZ6WNOCKipZNVfk-pZt7lf2w4W/pub?gid=0&single=true&output=csv"
data <- suppressWarnings(utils::read.csv(sheet_url, header = TRUE, stringsAsFactors = FALSE)) %>%
  mutate(date = as.POSIXct(date, format = "%Y-%m-%dT%H:%M:%OSZ", tz = "UTC")) %>%
  filter(class_id == !!class_id) %>%
  select(date, user_id, class_id, enjoy_rating, learn_rating, suggestions)
```

### 2.2 Cats, Dogs, and Happiness Survey

This simple survey could be embedded inside any Jupyter notebook. It asks students what kind of pet they have and how happy they feel. (Spoiler alert: in general, people with dogs are known to be happier than people with cats.)

**File:** survey-v1.html

**Google sheet:** <a href="https://docs.google.com/spreadsheets/d/1_QNzpIZp3H3PG1SFirx5LpCvsoOyNjJdA7NfbckHDHw/edit?gid=0#gid=0">cats-dogs-master-data</a>

Variables that should be defined inside the notebook:

- `class_id <- "arthur"` # put any unique id here (e.g., teacher email address)
- `user_id <- Sys.getenv("JUPYTERHUB_USER")` # this gets the users Jupyter ID

**Additional variables in the Google sheet:**

- `pet_type` (`Cat`, `Dog`, `Both`, `Neither`)
- `happiness`
- `date`

**Example embed code to place in Jupyter markdown cell:**

```r
IRdisplay::display_html(sprintf(
  '<iframe src="https://uclatall.github.io/jupyter-survey/cats-n-dogs-v3.html?class_id=%s&user_id=%s" width="100%%" height="520" 
    sandbox="allow-scripts allow-same-origin" 
    style="border: none; box-shadow: none; background: white; "></iframe>',
  class_id, user_id
))
```

**Example code for reading your class's data into R:**

```r
sheet_url <- "https://docs.google.com/spreadsheets/d/e/2PACX-1vTm5IafEMmLJBMdaGLiDzAsFu0lEQYXQeKJDNlPVSm33FwdoWdUjYgki1RlDQ-gVfVVH78MfEeNzozm/pub?gid=0&single=true&output=csv"
data <- suppressWarnings(utils::read.csv(sheet_url, header = TRUE, stringsAsFactors = FALSE)) %>%
  mutate(date = as.POSIXct(date, format = "%Y-%m-%dT%H:%M:%OSZ", tz = "UTC")) %>%
  filter(class_id == !!class_id) %>%
  select(date, user_id, class_id, pet_type, happiness)
```





