# How to run the single flight path optimization notebook

- get the repo from github.
- Make sure to change all the paths to point to the correct locations in your machine.
  > Note: Here is good to use either e.g. `'users/andres'` or `r'users\andres'` as the characters \ or / can cause trouble otherwise.
- make virtual enviroment with the instructions given on the notebook (venv) and run with `requirements.txt` loaded there (which is just some libraries). And run the code with the kernel from the virtual enviroment
- Inside the A02_input_output_folder, which is working with databases, the file `db_select.py` had to be changed, specifically the function *select_table*, the *mode* 'flightpath' and 'flightpath info' had to be updated with the newest connection host-ports to cloud databases.

Once everything seems ok, then we can use the terminal to run *shaify* script which is called `tfms_dbquery12.py` to run it we have to use the terminal (I have been using git bash inside the vscode)

to do this we move to `shaify2\FDX` so the path in the terminal should look like `Andres@LAPTOP-AJLEGION MINGW64 ~/OneDrive/Escritorio/flitex/Python_4DOptimization_Rewrite_JAN2021/shaify2/FDX (test_sid)` with the last part being the branch we are working on.

Once there (using ls and cd commands) we write 

`python tfms_dbquery12.py --airline FDX`

This will get all the FDX flights and make an output like this 

```bash
Airline : FDX
Flights : []
Out-Dir : .
exiting
**4643 new Flights found.**
Writing new data for flight: FDX5260 at 2022-02-07 18:05:00, with 14 waypoints.
Writing new data for flight: FDX7 at 2022-02-07 19:00:00, with 17 waypoints.
.
.
.
Writing new data for flight: FDX9016 at 2022-02-08 02:28:00, with 25 waypoints.
.
.
.
```

After this, pick one of the given flights listed in the output (let's say the last one shown here) and write 
`python tfms_dbquery12.py --idents FDX9016 -d 20220208`

Doing these steps will create a folder with the waypoints file in it and a file with two lines (which contains origin and destination airports, arrival time, aircraft type...)
in our example case would be

`shaify2\FDX\20220208\FDX9016_20220208.csv` (wayponts)

which seems like this:

```
Name,Latitude,Longitude,Outbound_Course,Distance_this_Leg,Distance_Remaining,Distance_Flown,Type
ANC,61.17,-150.0,103,0,0,0,WPT
YAK,59.51,-139.65,102,324,0,324,WPT
.
.
.
WANOB,39.7,-86.47,82,7,0,2652,WPT
IND,39.72,-86.29,-1,8,0,2660,WPT
```

and `shaify2\FDX\GEN_20220208.csv` (flight info) which seems like

```
origin_airport,destination_airport,arrival_time,dept_time,aircraft_type,flight_names,identity_num,route,filed_altitude,filed_airspeed_kts,flight_ref
PANC,KIND,Tue 07.54,Tue 02.35,MD11,FDX9016_20220208.csv,FDX9016,PANC./.YOUNG..YBR..HML..KP09G..KP90I..KG81K..BRBIE.JAKKS2.KIND,,,14825991
```

Now we go into the notebook and run all cells (restarting the kernel when necessary) and in the **loading data** part we change the date to the flight we want to look at (in this case `date=20220208`)

We keep running the next cells until we reach the **path optimization** part. This contains a cell

```python
#Load the planned flight path and create the optimized one
data_planned_optim = get_waypoint_info(data, analysis_params, shortstrpath, option= 'optim_altitude') #time astar algorithm
```

and this part is the one we are interested in as it **takes a lot of time to run** because it is basically the optimizer part.

Currently, I got to this point where the code is running but there seems to be a graph problem which is maybe related with the mix of tuples and numbers inside a `ret` file...(?)

```python
Connecting to the PostgreSQL database...
from db_BADA_query.py -> aircraft_info_query
waypoints_optim(row=
 origin_airport                                                      PANC
destination_airport                                                 KIND
arrival_time                                                   Tue 07.54
dept_time                                                      Tue 02.35
aircraft_type                                                       MD11
flight_names                                        FDX9016_20220208.csv
identity_num                                                     FDX9016
route                  PANC./.YOUNG..YBR..HML..KP09G..KP90I..KG81K..B...
filed_altitude                                                       NaN
filed_airspeed_kts                                                   NaN
flight_ref                                                      14825991
head_time                                                       2.583333
flight_time                                                     5.316667
schedule_route_list    [PANC, YOUNG, YBR, HML, KP09G, KP90I, KG81K, B...
schedule_route_time                                                     
ap_from                            [-149.998138427734, 61.1740875244141]
ap_from_alt                                                        151.0
ap_to                              [-86.2946395874024, 39.7173042297363]
ap_to_alt                                                          796.0
Name: 0, dtype: object 
 folderpath= c:\Users\Andres\OneDrive\Escritorio\flitex\Python_4DOptimization_Rewrite_JAN2021\shaify2\FDX\20220208 
 aircraft= <B04_AC_Performance.Aircraft.Aircraft object at 0x0000021D1EEAB910> 
...
GraphProblem( 27934 211445 )
53243.767127805164
Graph optimizer error.
Using unoptimized route instead.
Output is truncated. View as a scrollable element or open in a text editor. Adjust cell output settings...
c:\Users\Andres\OneDrive\Escritorio\flitex\Python_4DOptimization_Rewrite_JAN2021\env\lib\site-packages\numpy\lib\npyio.py:1508: VisibleDeprecationWarning: Creating an ndarray from ragged nested sequences (which is a list-or-tuple of lists-or-tuples-or ndarrays with different lengths or shapes) is deprecated. If you meant to do this, you must specify 'dtype=object' when creating the ndarray.
  X = np.asarray(X)
Traceback (most recent call last):
  File "c:\Users\Andres\OneDrive\Escritorio\flitex\Python_4DOptimization_Rewrite_JAN2021\env\lib\site-packages\numpy\lib\npyio.py", line 1565, in savetxt
    v = format % tuple(row) + newline
TypeError: must be real number, not tuple

The above exception was the direct cause of the following exception:

Traceback (most recent call last):
  File "c:\Users\Andres\OneDrive\Escritorio\flitex\Python_4DOptimization_Rewrite_JAN2021\B05_Flight_Path\get_waypoint_info.py", line 108, in waypoints_optim
    run_flight_path_optimization(min_degree,max_degree,alt,ap_from,ap_to,forecast,forecast_interval,aircraft,analysis_params,name_from,name_to)
  File "c:\Users\Andres\OneDrive\Escritorio\flitex\Python_4DOptimization_Rewrite_JAN2021\B05_Flight_Path\horizontal_optim.py", line 335, in run_flight_path_optimization
    np.savetxt('C:/Users/Andres/OneDrive/Escritorio/flitex/Python_4DOptimization_Rewrite_JAN2021/notebooks/ret.txt',ret)
  File "<__array_function__ internals>", line 180, in savetxt
  File "c:\Users\Andres\OneDrive\Escritorio\flitex\Python_4DOptimization_Rewrite_JAN2021\env\lib\site-packages\numpy\lib\npyio.py", line 1567, in savetxt
    raise TypeError("Mismatch between array dtype ('%s') and "
TypeError: Mismatch between array dtype ('object') and format specifier ('%.18e')
```
---
ideas to use custom flight:

change `schedule_filepath = f'c:/Users/Andres/OneDrive/Escritorio/flitex/Python_4DOptimization_Rewrite_JAN2021/shaify2/FDX/GEN_{date}.csv'` to a convenient csv file with custom info:

currently is usign this file for example:

| origin_airport | destination_airport | arrival_time | dept_time | aircraft_type | flight_names           | identity_num | route                                                                      | filed_altitude | filed_airspeed_kts | flight_ref |
|----------------|---------------------|--------------|-----------|---------------|------------------------|--------------|----------------------------------------------------------------------------|----------------|--------------------|------------|
| PANC           | KIND                | Tue 07.54    | Tue 02.35 | MD11          | FDX9016_20220208.csv    | FDX9016      | PANC./.YOUNG..YBR..HML..KP09G..KP90I..KG81K..BRBIE.JAKKS2.KIND              |                |                    | 14825991   |

but I want to use something like this:

|origin|destination|date|time|aircraft|arrivalwindowmin|arrivalwindowmax|
|---|---|---|---|---|---|---|
|KJFK|KLAX|20191005|2130|A320|01:00|02:00|

but in order to do this seems like I would have to know the route column (how would I get that??) also times of departure and arrival and custom everything!

In the notebook `data` variable is created using
```python
# Load the flight path data using the updated filepath
data = load_flight_path(schedule_filepath)
```
---
## 23 Oct 2024
Yesterday Sid helped me setup the correct connection to databases cause that was causing trouble as I was not getting the latest data. I was able to try the following flight inside the notebook:

```bash
python tfms_dbquery12.py --idents DAL1743 -d 20241022
```

but I'm getting still the: Graph optimizer error.
Using unoptimized route instead.

In order to see if this is an error inside the code or an error with the specific flight I need to be able to try different flights. This is only possible if I'm able to look directly into the latest flights.
As Sid recommended this must be done via pgadmin with the help of arnau. 

> as a note Sid recommended using flights with aircraft type `B763` although interestingly the DAL1743 flight used a `B739`.
> Also use the website [flightaware](https://www.flightaware.com/) which is very useful.

## 31 Oct 2024

As of today I'm able to run the optimization notebook :D with 3 flights:

|name|date|origin|destination|number_waypoints|number_arcs|
|---|---|---|---|---|---|
|DAL1743|2024-10-22|KATL(Atlanta)|KTYS(Knoxville-Tennessee)|8|128772|
|FDX383|2024-09-28|KMEM(Memphis)|KORD(Chicago)|17|482218|
|DAL960|2024-10-20|KLAX(Los Angeles)|KJFK(NY)|44|4.5million|

## November 5

---
### Meeting with Sid and Bernard. 

About output csv file, at first it seemed like it was right under `/notebooks>testDL` but at closer inspection that path pointed me to in the meeting is not really connected to anything. So yes, the variable name is outpath, but it seems like it is not really being used as an output anywhere (i noticed this since I run the code for DAL701 and also other couple but that path and inside the folder is not changing anything, just some fixed fdx old flight)

looking at `bada>public>tables>aircrafts` by right clicking and choosing view/edit data and list first 100 rows, the
performance table in bada files for some aircraft types are strings instead of floats (to open it just double click . As an example aircraft type "A21N" has almost everrything with strings as opposed to -another example- aircraft "B763" which has float type data.

![BADA-A21N](https://github.com/user-attachments/assets/bb6c45fc-1483-4c85-a05a-4ddab7ce49a4)
![BADA-B763](https://github.com/user-attachments/assets/51987791-82e3-485c-981a-483794fc68f6)

this is causing trouble...

![A21N_BUG](https://github.com/user-attachments/assets/617c4bf6-f760-4602-aa45-2ca72f4a348a)

About Flights Sid remembered not working or taking too long, he suggested me to run `DAL701` as he recalled that one was taking sometimes undefinite time-runs. He did not remember any other specifics so he told me I should try randomly some other flights to find other non-working and taking too long flights...

At first test `DAL701` didn't run, it truncated... In other run it did compile, but took almost 10 minutes... I  

![DAL701](https://github.com/user-attachments/assets/9e74a8ee-73c9-45a5-aced-7f8749991fdf)

On PGAdmin, some useful commands Sid showed me and I have been using inside `ibmclouddb>schemas>public>tables>flightplan_info` to filter flights of interest:

```SQL
SELECT * FROM public.flightplan_info where aircraft_id = 'DAL701'
ORDER BY source_timestamp DESC LIMIT 10

SELECT aircraft_id, departure, arrival, etd ,legacy_format FROM public.flightplan_info where aircraft_id = 'DAL701'
ORDER BY source_timestamp DESC LIMIT 10
```
the star means selecting everything, the aircraft_id can be found listing the first 100 rows and picking one of interest, althought the ones I've been using are ones that Sid already has recommended trying (FDX383, DAL701,..., see this notes for others)
the second line means that we are ordering by the time in descending order and that we only want 10 rows.

---

### Gantt chart
It took some time and is pending to upload but I made a great gantt chart to keep track of progress :)

## November 6
To be able to get the csv file in the outpath I had to run vertical analysis part inside the notebook as well (previously I had only used horizontal optimization) but inside the vertical analysis the csv output is created and put into `C:\Users\Andres\OneDrive\Escritorio\flitex\Python_4DOptimization_Rewrite_JAN2021\notebooks\TestDL\Result_shaify\20241022\DAL1743\optim_flight_detailed.csv`

Running the vertical part was a challenge since it had connection port issues... but Priyanka helped me solve this issue.

Now I got the csv file (two of them)
![image](https://github.com/user-attachments/assets/d2d33ab5-2bc1-4caf-9c2f-f34cf7e72f6f)

This does not look good once I open them in excel, 
![image](https://github.com/user-attachments/assets/a916fa3a-6a87-4d50-91f6-84ab64fce2e3)

But I found two ways to "solve" this issue, one is to open a blank file in excel and use data tab to import from csv and then choosing the generated csv files. The other one is using an extension inside vscode which can help visualizing csv files as ascii tables (I think this one is a good solution in the meantime). To do this I installed the extension and use the search bar on top of vscode, show and run commands option and then convert to table from csv... I think this works for both.
![image](https://github.com/user-attachments/assets/5ced86aa-83c6-4f42-b091-522adcc4aa6f)

I do not know yet how much of the horizontal or vertical analysis is involved in this outputs... it is also a question how is the cost of the opt route compared to the non-opt.

---
I have been reading the united airlines methodology and I'm preparing a small document in latex to summarize it, at first it does not seem to be that much different from the caguya research... pretty much the same idea with backtracking.

## November 7
Inside the notebook I'm trying to understand in more depth this function `get_waypoint_info`(which is the kernel of the optimization)
![image](https://github.com/user-attachments/assets/99a50af4-e663-44f3-b135-c30c959c7cab)

right now is using the `optim_altitude` which returns the `waypoints_optim` function 
![image](https://github.com/user-attachments/assets/8deca2a5-67ab-4df1-a596-b1be568087f9)

That function is located in `get_waypoint_info.py` script and among other things, does the following:

- Get the coordinates of the departure and arrival airports
- Sets the altitude to be used (if non given assumes 36000 ft)

  ```python
  if not np.isnan(row.filed_altitude):
    alt=row.filed_altitude
  else:
    alt=360
  ```
- **Sets the angles used for arc purging ** *it says (This is deprecated)*
  ```python
  min_degree=60
  max_degree=100
  ```
### Remarks until this point
- The angle definition could be potentially a great point of improvement.
- From what I understand (but I have to take a better look at this point) the altitude to be used is fixed here, and then the algorithm runs only horizontal optimization (which means a planar graph set at that fixed altitude). This could also be a good improvement point: If changing flight level is not allowed (as UAL said customer hate changes in altitude) then horizontal optimization could be run for several altitudes (i.e. if I have possible altitudes a,b,c then running horizontal optim for a, then for b and then for c and finally making a comparison could be interesting(?)). If changing flight level is allowed between waypoints then a different approach would be needed... In any case I don't fully understand how the calculation/optimization is possible not being live. What I mean by this is: they say they have the data for the wind along the path but that is made at time $t_0$ at that moment is possible that certain calculated path is the best, but this is only the case if that wind is fixed in time (and I don't think this is the case). How can one know if at some point the wind changed dramatically from what was expected beforehand and wind is against the path...

### Heart of the optimizer
`waypoints_optim` tries running the optimizer 

```python
# Run the optimizer
print("Running run_flight_path_optimization(", min_degree,max_degree,alt,ap_from,ap_to,forecast,forecast_interval,aircraft,analysis_params,name_from,name_to, ")")
node_names, node_lon, node_lat, number_of_arcs, node_ugrid, node_vgrid, node_temp = \
run_flight_path_optimization(min_degree,max_degree,alt,ap_from,ap_to,forecast,forecast_interval,aircraft,analysis_params,name_from,name_to)`
```
so naturally we go to `run_flight_path_optimization` function, which is inside `horizontal_optim.py` script.

What I first notice is that **`min_degree` and `max_degree` are parameters in the function not being used!**- What it does first is the following:

```python
# Set the bounds for the arc request, lon_from should be xmin, lon_to is xmax, lat_from is ymin, lat_to is ymax
```
it does so by adding/substracting some degrees to the actual origin/destination coordinates (to not be so on the edge)

```python
if ap_from[0] > ap_to[0]: # Longitude
  lon_from_bound = ap_to[0]   - 0.1 # ap_from[0] + 0.1 
  lon_to_bound   = ap_from[0] + 0.1 # ap_to[0]   - 0.1 
else:
  lon_from_bound = ap_from[0] - 0.1 
  lon_to_bound   = ap_to[0]   + 0.1
if ap_from[1] > ap_to[1]: # Latitude
  lat_from_bound = ap_to[1]   - 0.1 # ap_from[1] + 0.1 
  lat_to_bound   = ap_from[1] + 0.1 # ap_to[1]   - 0.1
else:
  lat_from_bound = ap_from[1] - 0.1
  lat_to_bound   = ap_to[1]   + 0.1
```
All this makes me think that when they said (min_degree and max_degree deprecated) it probably means that some time ago they used to do a cone search but know using the postresql is not needed anymore (at least in the function those variables are not in use)

Then the arcs are imported using the function `arc_query` inside `db_flightpath_query` script.
![image](https://github.com/user-attachments/assets/40455ac6-793b-4348-a696-4fd284f80fc4)

this function (as well as others inside is a postgreSQL function that can be found in Confluence
> DAIR_AWS Server & PostgreSQL Arc Preparation (Used by Flight Path Optimizer) under "Define Function To Request Arcs"

for now I won't go deeply into it, but one thing I got is that what they define as *bounding box* and the search area for arcs is a rectangle between origin and destination airports, this could be improved since probably this is a very big area.

The arcs are imported (taking restrictions into consideration) and time is measured for this task.

```python
Querying arcs for optimization.
Connecting to the PostgreSQL database...
from arc_query
Calling get_arcs function with  33.54 , -84.53 , 35.910000000000004 , -83.89 , 1200 , 27 , 125
output received and connection closed in the arc_query in 3.5746734142303467 seconds
Arcs imported.
Number of arcs: 128772.0
```

After this, geometric distances from each node to destination airport are calculated (using the haversine formula)
    # Add the nodes to the graph
    G.add_node(node_list) # 85545 shows up in node_list and is added to G

```python
for node in node_list:
  # Get the index of the node in the node_data array
  idx = int(np.where(node_data[:,0] == node)[0])
        
  # Add the great circle distance to the heuristic
  # The great circle distance (in m) is calculated using the Haversine formula
  H[int(node)] = float(haversine(ap_to,[node_data[idx,2],node_data[idx,1]]))
```

Direction vectors and wind components are calculated over the arc (see pdf for more detail).

The arc calculation part is made in a for loop that goes through each arc (again in the pdf there is more detail). 
> As a note, inside this loop the code access the perfomance table inside the bada files (so I propose a fix using a **cast to float**

![image](https://github.com/user-attachments/assets/fa499ca8-13ce-4ee6-b08d-1bb0f795f940)

I profiled this function using time function... let's see how it goes.
