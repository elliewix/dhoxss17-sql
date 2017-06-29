# 00-Setup

We'll be using the Firefox [SQLite manager plugin](https://addons.mozilla.org/en-US/firefox/addon/sqlite-manager/) that should be installed on the lab computers or you can install it on your own computer.

Once the plugin is installed and Firefox is restarted, you can go up to Tools and select SQLite Manager to open up the wizard.

Time estimate:  20-30 minutes to get everyone on the same page.

## Directions for importing the data

Inside this repository is the `pettigrew.csv` file.  Be sure that you have access to that somewhere.  

1.  Click the blank paper icon in the upper left with the tooltip of "New Database"
2.  Type in `pettigrew` for the name.
3.  Click OK
4.  It'll take you to a window to show you where to save the database, just click "Open".
5.  You should now be back at the main SQLite Manager screen, and a new option is available at the top left.
6.  Click the icon of the blue disk with the arrow going into it with the tooltip of "Import"  (this near the New Database button).
7.  This should take you to an import wizard tool.  By default, CSV should be selected.
8.  Click "Select file" and naviate to the `pettigrew.csv` file on your computer. 
9.  Check the box that says "First row contains column names."
10. Ensure that the "Comma" option is selected for the "Fields separated by" section
11. Click to select the "Like MSExcel" for "Fields enclosed by"
12. Click ok.  
13. It will prompt you that a new table is being made.  Click OK to that.
14. A new window will appear where you can declare properties about the specific columns.
15. Change all the "Data Type" drop down options to "text" and leave everything else alone.
16. Click ok.
17. It will confirm with you again if you are sure and confirm that there are 601 rows to import. Click ok again.
18. Over on the left you should see that you now have an option of "pettigrew" under Tables.
19. You can now also use more features in the right hand section where there are tabs about Structure, Browse.
20. Click on Browse & Search, and you should see your data in tabular format.

Phew! Now you can actually start doing stuff.