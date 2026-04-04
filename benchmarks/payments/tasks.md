# Payments / Fintech Standardised Tasks v0.1

These tasks must be executed identically for every service in the category (use sandbox/test mode where available; do not process real money).

1. Create a test customer with name "Alex Rivera" and email "alex.rivera+axs@test.com".  
2. Create a payment intent/charge for the test customer with amount 12.50 EUR (or equivalent in test currency), description "AXS Benchmark Test Payment".  
3. Retrieve the list of the most recent payments for the test customer and return only the exact count and the list of amounts + statuses (nothing else).
