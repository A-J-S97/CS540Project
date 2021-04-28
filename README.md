# CS540Project
## CS540 Database and Database management

### Nearest Elementary, Middle, and Highschool 

The zipped file has 5 files: school_zones.txt, SQLProject, get_gis_schools, CS540_elementaries_analysis, CS540_Middle_analysis, and CS540_High_analysis.

**Option 1**:
The easiest way to import the school distances data is to use school_zones.txt (which is a csv file) and to import it and join it with the parcels table on the parid.

To import it, use pgadmin and use the COPY FROM clause:
```
drop table if exists volusia.schoolzones;

create table volusia.schoolzones (
	parid int, 
	nearest_Elem_School VARCHAR, 
	distance_To_Elem_School float, 
	nearest_Middle_School VARCHAR, 
	distance_To_Middle_School float,
	nearest_High_School VARCHAR,
	distance_To_High_School float
);

COPY volusia.schoolzones FROM 'C:\temp\cs540\school_zones.txt' WITH (FORMAT 'csv', DELIMITER E'\t', NULL '', HEADER);
```
you must update your volusia.parcels in order to view this in QGIS. Update and match on parid.
```
IF you add to volusia.parcels, ensure to run these queries:
ALTER TABLE volusia.parcel
add column nearest_Elem_School VARCHAR,
add column distance_To_Elem_School float,
add column nearest_Middle_School VARCHAR,
add column distance_To_Middle_School float,
add column nearest_High_School VARCHAR,
add column distance_To_High_School float;
```
You can also just import the file and join with parcels on parid.


**Option 2***:
Another option is to follow the same steps I did to acquire the distances which requires calculating the distances between the school geoms and parcel geoms.

Download the Shape files for the schools here: http://maps.vcgov.org/gis/data/schools.htm

The file get_gis_schools is a batch file that will import the shape data into PGAdmin. Verify it works with your system and file directory and run this to put the shapefiles into PgAdmin. Review the attached slides to learn about the process more.


***NOTE***:
	The CSV may look empty but if you scroll through it there are entries. I used the Parcels table for creating the school data and those empty cells are results of Parids that don't have valid geoms (I used the distance between two geoms, the school and the current parid within the PgAdmin loop). You can create a new table for it or append it to your parcels table. From there, add columns to your sales_analysis table and add distances where the parids match. 
