# Walmart Midterm

## 1. Goals

Agree on these out loud before you assign anything. Write each answer in a sentence or two.

### What does a good result look like for our team?

"A notebook we are proud of and can explain" is a goal. "Do well" is not.

In terms of successful execution of each data exercise, a good result looks like successfully cleaning the data and calling the OpenMateo API.

In terms of mastering good data analysis practices, a good result looks like being able to explain every step we did and why, contextualizing each line/chunk of code with clear descriptions to ensure everyone is on the same page, being able to reference past labs to help us understand the task at hand, and creating good dictionaries that are clear and easy to reference.

### What does each person want to get out of this?

One person might want API practice, another wants a strong presentation, another just wants to get through it. All of that is fine, and it is easier to plan around when everyone says it.

**Mark Chernov:**

1. Successfully referencing past labs and making good future proof templates that save time for future projects.
2. Be more experienced with data cleaning in a real world example on a larger scale
3. Have more API practice with OpenMeteo since calling the data was difficult due to mistakes in renaming dates, which ties into data cleaning
4. Making structured code that is successful long term and can take on data added to the data set without needing updates to account for the specific changes to the records
5. Efficiently and professionally analyzing the data using a marketing and business perspective, answering each management position's questions effectively.
6. Being able to communicate my code correctly through comments to others.

**Michael Tague:**

1. Being able successfully clean a dataset, as this is very applicable to real-world responsibilities post-grad
2. Being able to contextualize aspects of the labs that I was previously confused on (for example: I struggled with the API lab and it took me a lot of tries to successfully call data)
3. Being able to make an actionable recommendation that is informed by reliable data that has been successfully cleaned
4. Using this as a case study for understanding how companies analyze supply trends and consumer needs during natural disasters/other disruptive events.
5. Being able to explain each step of the process, and back up why we made certain decisions (understanding terminology and the logic behind data analysis)

**Jun Seo (J.S.) Lee:**

1. Being able to analyze cleaned data and create strong visualizations that clearly inform future business decisions.
2. Working together with a team to create a repository and notebook with the necessary code that will be edited, pulled, pushed, and worked on together.
3. Looking at the limits or issues with the current data and code we have and being able to improve upon what we have or suggest improvements for the future.
4. Being able to reference the exact work every member did for future reference in a real professional setting to execute on tasks in a job.
5. Having good quality code as ownership and responsibility of code will be important for code that will be referenced and relied on for future work

### What limits our time?

Work shifts, other classes, travel. Put the real ones on the table now.

**Michael**

1. TA office hours,
2. architecture studio 4 hours per day
3. Mo. Wed. Fri.,
4. internship applications + interviews,
5. conducting interviews for club over the next week
6. usually good at night
7. afternoons are the worst

**Mark Chernov:**

1. Jobs: Weekend morning job willing
2. Thursday: After 7pm
3. Usually good everyday at night
4. Second Data Science project for DS 2003

**Jun Seo (J.S.) Lee:**

1. Job interviews
2. TA office hours
3. Saturdays: Club commitments
4. Class commitments

### What would make this a bad experience, and what will we do to avoid it?

A bad experience can be divided into two categories: failure to properly carry out the conditions of the assignment, and creating an unorganized workflow across each person. A bad result could look like improper data cleaning, leaking the API, and presenting weak visualizations that don’t properly display our recommendations. A bad result could also look like members missing committed work times, forgetting to post updates regularly, and duplicating tasks due to lack of communication.

## 2. Who does what

There are four jobs, matched to where the points are. Each job has an owner, who makes sure it gets done, and a backup, who can step in if the owner is sick or swamped. The owner does not have to do all of it alone, but the owner is the person we ask

| Role | The owner is responsible for | Done when | Grade area |
| --- | --- | --- | --- |
| **Cleaning + decision log**<br>Michael<br>Mark, JS | The cleaning functions, the SKU mapping table, and the decision log | One function takes the raw files and returns clean tables. The log lists every decision and how many rows it touched | Data cleaning, 25% |
| **Weather API + join**<br>Mark<br>JS, Michael | The API call with retries, parsing it into a DataFrame, timezone alignment, and the checked join to sales | One function returns daily weather. The join passes a row-count check. The API base URL is cited | Weather API, 15% |
| **Analysis Q1–Q5**<br>JS<br>Mark | The five answers: code, a number, and a written finding for each. Question 5 uses the SQLite database | Every question has code, a number, and a paragraph | Questions, 30% |
| **Charts + presentation**<br>Mark<br>JS | At least six plots with message titles, the six-minute talk, and the README | Six labeled plots, and the talk has been rehearsed once | Visualizations, 15% |

Two more things sit on top of the four jobs:

- **Notebook owner.** One person per week puts the team's finished work together in the shared notebook and runs Restart and Run All. This one rotates.
- **Everyone.** Everybody reads all five questions, reviews at least one other person's code, and writes their own part of the reflection, which is 15% of the grade.

**Fewer than four people? Combine the jobs:**

| Team of | How to split it |
| --- | --- |
| 2 | One person owns cleaning and the decision log. The other owns the weather API and the join. Split the five questions between you, and both build charts. |
| 3 | One person each for cleaning, weather, and analysis. Charts and the presentation are shared. |
| 4 | One owner per job. Everyone takes at least one question. |

## 3. Who owns each question

Every question gets an owner, who writes it, and a reviewer, who is someone else and tries to break it. Is the baseline stated? Does the row count check out? Does the chart say what its title says?

Q1 and Q3 are the critical path, as covered above. Q1 needs clean dates and Q3 needs the weather API. Everything else can run in parallel, so get those two moving first.

## 4. How we work together

Answer these in the charter. Vague answers cause trouble later, so be specific.

### Where do we talk, and how fast do we answer?

Name the channel. "Within a day" is a real answer. "Whenever" is not.

Text messaging our iMessage group chat is the best form of communication

1. Michael: Within 2 hours
2. J.S.: I will respond within the hour
3. Mark: I will respond within an hour unless I am in class. At worst, by EOD

### When do we meet outside class?

A real recurring slot that is on everyone's calendar. "We'll text" is not a meeting time.

7:00 - 8:30 PM on Tuesdays is our initial recurring slot, though we have collectively found that any weeknight around this time is ideal as a fallback.

### How do we decide when we disagree?

A reasonable rule: look at the data first. If you are still stuck, the owner of that part decides, and the decision goes in the decision log with the reason.

Understanding first what’s breaking the code, how can it be streamlined. Try multiple avenues for the most efficient route.

What will be the most efficient visualization for the data in question?

### What do we do if someone goes quiet?

A reasonable rule: Don't wait until the week it is due.

We decided that checking in after a day is appropriate. If there is still nothing after two, bring it to the whole team. If it is still not resolved, emailing Professor Williamson is our course of action.
