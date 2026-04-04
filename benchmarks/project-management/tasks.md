# Project Management Standardised Tasks v0.1

These tasks must be executed identically for every service in the category.

1. Create a project named "AXS-Test-[UNIX-TIMESTAMP]" (replace [UNIX-TIMESTAMP] with current Unix timestamp, e.g. 1743690000) containing exactly 5 tasks. Each task must have: unique title, one-sentence description, due date exactly 7 days from today in ISO format (YYYY-MM-DD).

2. Take the first task from the project created in Task 1, change its status to "In Progress", and add the comment: "Agent benchmark test - step 2".

3. Query all tasks due within the next 7 days assigned to the authenticated user. Return only the exact count and the list of task titles (nothing else).