# Cloud Infrastructure Standardised Tasks v0.1

These tasks must be executed identically for every service in the category (use free tier or test resources that can be safely deleted).

1. Create a new compute/resource group named "axs-test-[UNIX-TIMESTAMP]" with a minimal instance (e.g. t2.micro or equivalent).  
2. Attach a tag/label "benchmark=test" and a second tag "owner=agent" to the resource created in Task 1.  
3. List all resources tagged with "benchmark=test" and return only the exact count and the list of resource names/IDs (nothing else).
