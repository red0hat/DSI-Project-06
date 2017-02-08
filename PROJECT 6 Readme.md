# PROJECT 6 Readme
These are my working notes for the various piplines.  
 
## Project 6- Pipeline 1 - Download

This is a mostly working version of my download pipeline.  I prefer to use the commandline tool, Download.py

The documentation for this tool is non-existant, except for these notes.

Download.py accepts 3 types of arguments on the commandline.  First, it will check if any arguments are files, and attempt to read those files like yaml files containing a dictionary.  Second, it will look for a arugument like "location=...".  This arugment will be used to change the location of the database used to store the downloaded information.  The locations are defined in a file called 'credentials.yml'.  Third, any strings that don't match either of other special types will be used as a categories to be searched.

## Database Design. 

Plase see doc/database_design.pdf for a ERD of the this database.

The Database went through many iterations.  The database team was composed of 3 people, and had at least 2 contradictory points of view at all times.  Suketu Kothari consistently argued for consistency.  My point of view was for to relax rigor in favor of speed.  I planned for the integrity of the database to be maintained programatically and strucutrally.  Suketu preferred to handle all consistency with the structure of the database.  The final desgin is a fair compromise.  

The main tables are the Category table and the page table.  Because of the nature of the data source, a page could belong to multiple categories.  In fact, this is the case for categories we have queries.  To support a many to many relationship, there is a junction table called "page-category".  

The last 2 tables support the vectors for the search function.  These tables link back to the page and caegory tables, respectively.  The other field is an array of double precision numbers or floats.  Filling these tables has been delegated to the encoding team.

The database module and function calls have been developed in an ad hoc manner.  The functions are developed to fulfill an immedate need and will need to be rewritten to reduce redundant code and expand functionality.

### Further discussion: 

I've chosen to use my on Download function, from the command line.  There are few difference between my process and process from the wiki team.

1. The wiki team didn't handle unicode well, at least in the first versions.  I tried to build a system that would tolerate unicode characters.  If there is a problem encoding those characters for the vectorization, I would choose to extract words with unicode characters, strip the unicode, and append the stripped words to the text before vectorization.

1. Inclusion of the summary as a return doesn't make sense to store in the database.  Putting redundant information in the database violates my database design principles.  When needed, the snippet can be recovered from the stored text of the page.

1. The wiki team's module doesn't handle categories with more than 500 pages.  The api has a limited return of 500 pages per request.  When the call exceeds 500 pages, an additional key/value pair is returned with information to continue the query.  To call all pages for large categories, the "pages for a category" query needs to be recurrsive in cases of categories with many pages.



## Project 6- Pipeline 2 

This is where I'm stuck at the moment.  I've been exploring corner cases, and trying to make the code work for these uncommon situations.  This includes categories with more than 500 pages, pages that don't return a text block and unicode.  Accordingly, I need to rebuild the transformer to accomdate this situation.  I am folowing the encoding team's code, but I expect to modify to support my situation.
