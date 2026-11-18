I fixed a bug in Datasette, an open-source tool for exploring data. I corrected the counts and links in facets, a feature that shows users how many records fall into each category and lets them click a category to see those records.

The problem appeared when a user selected a facet and then moved to the second page. The count for the same category changed, and the filter left out matching records from the first page when the user clicked that category. I fixed both problems and checked that users could see all the matching records. I also added tests for the counts and links, and the maintainer merged my changes.

One thing I learned was to keep repeated code in one place. My first fix worked, but I had written the same two steps in three different places. Their order also mattered, so I moved them into one function. Now, if someone changes that code, they only have to update it once. This also makes it less likely that someone will accidentally change the order.

https://github.com/simonw/datasette/pull/3026