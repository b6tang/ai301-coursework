# Fixing facet counts and links across pages in Datasette

## Overview

I fixed a pagination bug in Datasette, an open-source tool for exploring data. Facets count the values in a column, and clicking one of those values filters the table to it. On page 2 of a paginated table, the counts covered only the rows after the `_next` cursor, and the facet links carried that cursor too, so a click left out rows from page 1. I reproduced it on the official demo. The cause came down to two things: the facet count used the wrong SQL string, and the links copied the cursor. I changed both in [PR #3026](https://github.com/simonw/datasette/pull/3026). Simon Willison merged the three-commit PR on November 8, 2026, and confirmed that the fix was live on `latest.datasette.io`.

## What happened

### Reproducing the bug on the demo

The bug was already in the tracker as [#3021](https://github.com/simonw/datasette/issues/3021), opened on October 30, 2026. I reproduced it on the official demo the next day, which was running a 1.0 alpha build. I opened the demo's `sortable` table and added a facet on `pk1`:

```text
page 1: heading "201 rows"   pk1: a=26, b=26, c=26, d=26, e=26, f=26, g=26, h=19
page 2: heading "201 rows"   pk1: e=26, f=26, g=26, h=19, d=4
click d: heading "26 rows where pk1 = 'd'"   table: 4 rows   url keeps _next=d%2Cv
```

The table heading still showed the full count on page 2, but the facet count dropped and clicking `d` skipped matching rows. I saw the same count problem with `pk2`.

### Two SQL strings

The total row count was right because the view had already saved the filtered query before it added the pagination conditions. The facet query was built later, from the same clause those conditions had just changed, so it counted only the rows after the cursor. The fix sends the saved query to the facets, with the matching parameters, instead of rebuilding it. My first plan was to rebuild that SQL from scratch. Then I saw the saved version was already there, so I reused it.

### The link fix and the order that mattered

Facet links copied the current URL's query arguments, including `_next` when it was present. I changed the links to drop `_next` when selecting a facet value. 

The order mattered when clearing a selected value. If the value is the only query argument and I remove it first, the URL helper returns a path with no `?`. A second call then falls back to the original query string and brings the value back. Removing the cursor first avoids that.

In my second commit, I added a comment to the test explaining why the order mattered. The same two steps still appeared in three places in the facet code, one for each facet type. In my third commit, I moved them into a shared helper so all three would use the same order.

### Tests and what landed

The new tests are in `tests/test_facets.py`. Ten of them failed before the fix. The count test asks the database directly for `select pk1, count(*) as count from sortable {where_sql} group by pk1` instead of taking the numbers from the view. The link test parses the URL and checks that its only query parameters are `_size=100`, `_facet=pk1`, and `pk1=d`. It also compares the number of rows returned with a direct count from the database. All eleven focused tests passed, and both the serial and non-serial test suites completed without failures.

I ran the Test workflow on my fork, and all five Python versions (3.11–3.15) passed.

On November 8, Simon reproduced the problem on a different table, `facetable`, a few minutes before he merged:

> Here's a demo of what this fixes: ... Those state facet numbers should not have changed.

## Limitations and what's next

Next time, I would check how the code builds the queries and links before writing a plan. The existing query already had what I needed, and reading the link helpers earlier would have shown me why the order mattered. If someone reports another facet pagination bug, I would first try to reproduce it. Then I would compare it with the cases I already tested and add a regression test if it exposes a gap.
