# 02 Aggregation

Aggregating data is at the heart of where SQL's utility will shine.  Crafting aggregation and calculation queries in SQL allows you to also combine in the robust filtering and selection into a single query.  This sounds like a lot of information going into a single query, but the compartmentalized structure of SQL queries maintains a aood amount of readability.

Having all your selection, filtering, aggretation, and other transformation code all in one place helps you document you analysis with more certainty than working in just a spreadsheet.  Most of the time, each query will presume that you are working from the same base set of raw data and transforming it each time to what you need.

At the core of aggregation is to tell you more than just your raw data, and is where you can start answering a lot of questions that you may have.

## `MIN()` and `MAX()`

This will be a new type of item to work with.  We've used `()` before to group queries, but in this context they will have a different meaning. You use these functions inside of your SELECT statement items 

## `COUNT()`

`SELECT COUNT(*) FROM pettigrew;`

This returns a single result:  `601`.  Knowing the rest of our data, can we take a guess as to what happened?  `COUNT()` is a function that will count the number of rows returned.  In this case, we've just passed it the `*` wildcard because it doesn't matter which columns we select.

Most of the aggregation functions will want to work with continuous data, such as measurements.  But if what you have is mostly text codes or categories, you might be sticking with `COUNT()`.

You can combine this with other keywords.  For example, how many BoxNumbers are there?  We know there are 601 total records, but we want to count the number of BoxNumbers mentioned.

This query involves two actions:

1. Construct a query that displays the unique names for BoxNumber.
2. Add `COUNT()` in there to count how many there are.

For step 1:

`SELECT DISTINCT BoxNumber FROM pettigrew;`

For step 2:

The question here is where should COUNT() go.  In our previous example, we had COUNT(*) so our first instinct might be to put it just around BoxNumber.  Let's go ahead and try that:

`SELECT DISTINCT COUNT(BoxNumber) FROM pettigrew;`

This gives us a result of 601, which we know to be the total count of all our rows.  What can this tell us?

We know that something went wrong but SQL isn't being noisy about it.  We only know that something has gone wrong because we know our data very well.

Recall that DISTINCT operates on the row level. As in, it only returns the unique rows.  We also know that COUNT returns back however many rows were returned.

So putting this all together, we can take a guess that COUNT() operated first and returned a count of all of BoxNumber.  Since we had no other filters in place, it spat back all 601 rows.  So then DISTINCT had really nothing to operate on.

Let's try moving DISTINCT in with BoxNumber.

`SELECT COUNT(DISTINCT BoxNumber) FROM pettigrew;`

Good, we have a count of 13 in there, and we know that there are 13 box numbers.

## AS keyword

Take another look at the results that we just got.  The column label for the result is `COUNT(DISTINCT BoxNumber)`.  This may be fine if we're just investigating things, but isn't great if we end up with a larger tool.  By default, whatever piece of SQL code that creates that column in the SELECT statement area will end up as the column name in the table view.  There may also be situations where you might want to change an exising column name to something more descriptive.  

Either way, you can control the header name with the `AS` keyword.  Place this after the column decleration in the `SELECT` area.  Example:  `BoxNumber as box`.

`SELECT DISTINCT BoxNumber as box FROM pettigrew;`

When we run this, we get the same contents as we have before, but now the column header is `box`.  Let's add our COUNT() back in there.

`SELECT COUNT(DISTINCT BoxNumber) as numberOfBoxes FROM pettigrew;`

We can also make these aggregation column names much more specific, so we can remember what they stand for.  Sometimes aggregations or transformations can get lengthy, so you'll want to change them to make your results more readable.

## `GROUP BY`

You won't always want to count the number of all results coming up from a single column.  The `GROUP BY` keyword allows us to refine our `COUNT()` or other aggregation statements to create groups or subsets that are further counted.  This allows us some similar tools like a PivotTable does.

Whereas `COUNT()` can exist without `GROUP BY` and acts on the entire set of results, `GROUP BY` cannot exist without some aggregation function.  Well, it will technically work, but do silly things.

`SELECT * FROM pettigrew GROUP BY BoxNumber;`

The results of this query are composed of the last result of each row in each of the BoxNumber categories.  You can visualize this in your head, of imagine running a `WHERE` clause for each of the BoxNumbers.  Then take the last row of each of those results.  That's what we're seeing here.

SQL is a very forgiving language, where it will desperately attempt to execute what you tell it, even if the results are nonsense.  This means that you need to know your data very well so you can keep an eye out for these kinds of issues.

So let's get back to making an actual aggregation statement that works.  Remember that it first performs the grouping, and then it performs the aggregation on each of those groups.

`SELECT COUNT(*) as NumLettersInEachBox FROM pettigrew GROUP BY BoxNumber;`

This does a few actions:

1. It selects all the columns, which is fine.
2. It groups all the data by the BoxNumber value.  So knowing the data, you should have 13 groups.
3. It counts how many results are inside of each group and gives you that number for each value.

When performing these types of actions, it can be helpful to also report back the value of the column you've grouped by. Remember that it'll take the last value for each group, but the contents should all be the same, so that's fine.

Here's our final statement for this:

`SELECT BoxNumber, COUNT(*) as NumLettersInEachBox FROM pettigrew GROUP BY BoxNumber;`

Again, remember that you can do silly things here.  

`SELECT FolderNumber, COUNT(*) as NumLettersInEachBox FROM pettigrew GROUP BY BoxNumber;` 

This results in the last folder number for each group.  Maybe that's what you want, but likely not.

Our basic formula is:

1. Pick the column you want to do math to.  This will likely the name of the column you're going to group by if you're just counting instances.
2. Pick the aggregation function you want to do.  Be sure to read the documentation and understand how that function will operate on the data type of the column you're going to give it.
2. Pick the column that you want to aggregate by (this is usually some categorical variable).  Do remember that this will group by unique content, so if you have typos or uncontrolled values, each unique value will be treated as a unique instance.

Thus our format is `SELECT AGGREGATION_FUNCTION(math_column)




select avg(boxCount) from (SELECT count(BoxNumber) as boxCount FROM pettigrew group by BoxNumber);