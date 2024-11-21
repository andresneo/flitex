# How to use PGAdmin 4 

Make sure every connection port in `db_select.py` (database selection) file which is inside `A02_Input_Output` folder has all the correct connections.

Download latest version of `PGAdmin - PostgreSQL Tools` from [pgadmin website](https://www.pgadmin.org/).

Once downloaded, open the program (better to execute as an administrator) and create a server (in my case `Andres Test`) filling the needed credentials.

On PGAdmin, some useful commands Sid showed me and I have been using inside `ibmclouddb>schemas>public>tables>flightplan_info` to filter flights of interest:

```SQL
SELECT * FROM public.flightplan_info where aircraft_id = 'DAL701'
ORDER BY source_timestamp DESC LIMIT 10

SELECT aircraft_id, departure, arrival, etd ,legacy_format FROM public.flightplan_info where aircraft_id = 'DAL701'
ORDER BY source_timestamp DESC LIMIT 10
```
the star means selecting everything, the aircraft_id can be found listing the first 100 rows and picking one of interest, althought the ones I've been using are ones that Sid already has recommended trying (FDX383, DAL701,..., see the [README file](./README.md) for others)
the second line means that we are ordering by the time in descending order and that we only want 10 rows.

The first example command looks like this:
![image](https://github.com/user-attachments/assets/7d6c0e61-6a01-4678-822b-71be8eb79e24)

And the second will look something like this:
![image](https://github.com/user-attachments/assets/5c05882d-2bad-40b4-8bc6-271484fbcfe3)

> Dark theme was used and this can be chosen in `File>Preferences>Miscellaneous>Themes` (Only newer versions of PGAdmin have this feature)

---
