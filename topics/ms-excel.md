# Absolute/relative Cell referencing
Reference the same cell regardless of where you are using it.

Say if the percentage for each row is calculated by `total of that row(eg: E4,E5,E6...)/grand total (eg: E10)`.

Then use `E4/$E$10` to always keep `E10` constant as you drag the formula to lower cells.

# Other functions
`SUM`, `MIN`, `MAX`, `AVERAGE`, `COUNT`.
'Count' only counts numeric data.

# Stop `copy` mode
The circling ants means copy mode is enabled.

Stop it by hitting the `esc` key.

# Auto fit column/row
Select all the rows/columns and double-click any row/column divider line

# Hide/Unhide
Hide: Right click the row/column and click hide.

Unhide: Select range of row or column which includes a hidden row/column and right click-> Unhide.

# Sheets
Deleting a sheet is permanent. There is no undo button. 

Copy a sheet by using `Ctrl`+ drag the sheet tab.

Rename a sheet by double clicking it.

# Formatting
On the home tab-> Numbers-> choose percentage/currency, comma separated places, decrease/increase decimal places, etc

Format painter: Allows to copy the exact format from one cell to another. 

Styles: If you have a particular style you like, select the cell with applied style, and choose `Home`->`Styles`->`Cell Styles`->`New Style`. and save this style with a name. Now if you right click the saved style and choose `Modify` and modify any value, it modifies all cells using this style.

To make a heading to a sheet which is right in the middle at the top, choose the cells you wanna merge, Go to `Home`->`Alignment`->`Merge and center`.

Conditional formatting: It can be merged. So multiple conditions on one cell can occur.

# shortcuts 
* `ctrl` + `shift` + `+` or `ctrl` + `+`: add a column or row before the row/column you selected. If you want to enter multiple rows/columns, select that many rows/columns and hit the shortcut to add that mamny new rpws/coulmns before the selected row/column
* `ctrl` + `shift` + `-`: Remove what you selected.

# Templates
Erase all the variable data and save the file as `template`. Then on the screen to choose a new document, etc, choose Personal>Your template. This will create an instance of your template for use.

# Sorting
Works on contiguous groups of items. Such group is called list.

## Single level sort
click a cell in the column. Go to Data>Choose and ascending or descending sorting method. 

## Multi level sort
Sorting by multiple columns.
we need to select any cell in the list anf choose the big sort button under data>sort & filter. Then choose the primary sorting column. Then 2nd, and so on. We can choose upto 64 levels deep.

## Custom sort months/days
Select a cell in the list and choose the big sort button. Now choose the method of sorting in the 3rd column.

## Filtering
Click in a list and choose the Filter icon on Data tab. Now Filtering will allow you to see only those results you want from the column. After done using, hit the Clear button.

# Subtotal
For large data with totals, Sort the column you want to group subtotaling by. Then choose Data>Subtotal and for every change in, choose the column you sorted. And for the formula, apply it to the appropriate column.

# Format as table
Home>Format as table. Clicking this will give filter button for all columns, add alternate row colouring, Allow to perform calculations on current number of visible rows (filtering reduces total visible rows so count needs to be updated, math, sum to be updated as well.)

# Using conditional formatting to find duplicates
**Identify** the column that should be unique. Select the top cell. Then select `ctrl+shift+down arrow` to select full column. Home>styles>Conditional format>Highlight cell rows>Duplicate values.

**Remove** Duplicates by choosing Data>Data tools> Remove duplicates.

# Database sum (DSUM)
`=DSUM(A1:E43,E1,G7:G8)`: A1:E43 is full database range, E1 is column to evaluate, G7:G8 looks like Column header where condition is checked and the condition which can be found in that column, written below it.

# Database Average (DAVERAGE)
same as above

# DCOUNT
same as above

# Subtotal as smart summing for filtered columns
11 functions are listed in alphabetical order, and each function has two numbers: One in the range of 1 to 11, and another in the range of 101 to 111
Function	Numbers
AVERAGE	1 or 101
COUNT	2 or 102
COUNTA	3 or 103
MAX	4 or 104
MIN	5 or 105
PRODUCT	6 or 106
STDV	7 or 107
STDEVP	8 or 108
SUM	9 or 109
VAR	10 or 110
VARP	11 or 111

Function Numbers Ranges
There is one key difference between the two sets of numbers.

If rows are hidden by a filter, the functions in both number ranges will exclude the filtered cells.

If rows are manually hidden:
functions in the lower number range (1-11) will include those cells
functions in the upper number range (101-111) will exclude those cells.

Note: Rows that you format with zero height WILL NOT be included in either type of subtotal.



