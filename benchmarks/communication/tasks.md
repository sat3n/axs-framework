# Communication Standardised Tasks v0.1

These tasks must be executed identically for every service in the category.

1. Send a direct message to a test contact (or create one if needed) with subject "AXS Benchmark Test" and body "This is an automated agent benchmark message sent at [UNIX-TIMESTAMP]. Please confirm receipt."  
2. Create a new channel/group named "AXS-Test-[UNIX-TIMESTAMP]" and add the test contact from Task 1 as a member.  
3. Retrieve the 5 most recent messages across all channels and return only the exact count and the list of message subjects/bodies (nothing else).
