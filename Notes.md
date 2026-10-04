# Fix notes

## Bugs found and fixed

1. **Status filtering could show tasks with a different status.** I traced the selected status from the dropdown to the API and then read the backend SQL. The query searched the title *or* description, but without parentheses SQL could treat a title match as sufficient even when the status did not match. I grouped the title and description conditions so the status and archived filters apply to every result. I updated the matching H2 query and SQL reference.

2. **An old search or filter response could replace newer results.** I inspected how the React hook fetches tasks. Each request updated the list whenever it finished, so responses completing out of order could display results for an earlier selection. I passed an `AbortController` signal to `fetch`, abort the previous request when inputs change, and ignore callbacks from stale requests.

3. **The backend added an artificial response delay.** While investigating the out-of-order responses, I found a query “complexity” calculation followed by `Thread.sleep` in the controller. It made requests slower without doing useful work. I removed the calculation, sleep, and related log field.

4. **Changing filters on a later page could hide matching tasks.** I followed the search and status input handlers in the app and saw they left the current page unchanged. A narrower result set might not have that page. Both handlers now reset pagination to page 1.



I used code inspection and the frontend build, with Copilot AI assistance. 
Also I used 5 bug detection ChatGpt prompts to find these bugs.
