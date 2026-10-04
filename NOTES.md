# Patch Notes

## Summary of changes
- Fixed task-search SQL precedence so archived tasks are always excluded and the optional status filter applies to both title and description matches. Updated both the H2 query reference and Oracle artifact.
- Removed the artificial per-request `Thread.sleep` delay from the search endpoint. It added up to roughly one second of avoidable latency to normal requests.
- Added API validation for invalid page/pageSize values and unknown status values so malformed requests return 400 instead of producing incorrect results or server errors.
- Made frontend searches cancel stale requests and clear loading/error state correctly, preventing an older response from overwriting a newer search.

## Not changed
I left logging cleanup, broader pagination/database optimization, UI redesign, and missing automated tests out of the patch. They are worthwhile, but they are not as directly tied to the current user-visible correctness/performance problems and would expand the diff beyond the exercise timebox.

## Biggest remaining risk
The API loads the complete matching result set into memory and paginates with `subList`. That will become inefficient as the task table grows. A database-level paginated query with a count query would be the next improvement.

## Tools / AI
I used ChatGPT to inspect the codebase, reason about SQL operator precedence and request-state behavior, and review the patch. I verified the changes by inspecting the resulting source and frontend build configuration.
