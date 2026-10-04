# Full-Stack Patch Exercise - Notes

## Summary of Changes

1. **Fixed SQL Operator Precedence:** Added parentheses in `TaskRepository.java` `@Query` so search `OR` conditions cannot bypass the `archived` and `status` filters.

2. **Removed Artificial Latency:** Removed the `Thread.sleep()` logic in `TaskController.java` that unnecessarily delayed API responses.

3. **Implemented Debounce & Race Condition Fix:** Added a 300ms debounce for search input and stale-request protection in `useTasks.js` to prevent older responses from overwriting newer results.

4. **Fixed Pagination State:** Reset the page to 1 whenever the search query or status filter changes.

## What I chose not to change and why

I noticed missing input validation in `TaskController.java`, such as negative page numbers and invalid status values. I also found the same SQL precedence issue in the `db/oracle/task_search_package.sql` reference artifact. I chose not to change these because I wanted to stay within the timebox and focus on the highest-value user-facing issues.

## The biggest remaining risk

**In-Memory Pagination:** `TaskController` currently loads all matching rows into memory and then uses `subList()` for pagination. As the dataset grows, this could cause excessive memory usage and potentially lead to `OutOfMemoryError`. Database-level pagination using `LIMIT/OFFSET` or Spring Data `Pageable` would scale better.

## Tools/AI used

I used Gemini AI to explore the codebase faster, investigate observed UI behavior, identify root causes, and draft parts of the debounce and request-handling logic. I reviewed and understood the final changes before applying them.
