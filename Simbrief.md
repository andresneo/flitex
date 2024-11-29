# Simbrief Software

I will use [SimBrief software](https://www.simbrief.com/home/) which is a web-based flight planning tool that generates flight plans and simulates routes.

From what I have seen **this tool uses the weather data relative to the entered date, not the date where you run or generate the flight**. 

On the other hand it seems (at least from the generated output csv files) like **the notebook uses the GRIB data relative to the date the code is compiled not to the date of the flight.**

The flights I had previously worked with are:

|name|date|origin|destination|number_waypoints|number_arcs|arc_cost_time(seconds)|
|---|---|---|---|---|---|---|
|DAL1743|2024-10-22|KATL(Atlanta)|KTYS(Knoxville-Tennessee)|8|128772|4.8|
|FDX383|2024-09-28|KMEM(Memphis)|KORD(Chicago)|17|482218|18.1|
|DAL960|2024-10-20|KLAX(Los Angeles)|KJFK(NY)|44|4394622|170.4|

As I already have those, I will still use Simbrief to compare some key points.

# Comparison Between SimBrief and Flitex
## DAL1743 KATL (Atlanta) -> KTYS (Knoxville) 

As I already have those, I will still use SimBrief to compare some key points, which will be:

| SimBrief     | Flitex                  | Description                                                                                        |
|--------------|-------------------------|----------------------------------------------------------------------------------------------------|
| `Route distance` | `real_path_wind_distance/Air distance (nm)` | This is in nautical miles ([nautical miles](https://en.wikipedia.org/wiki/Nautical_mile)). 1 nm is about 1852 meters. |
| `air time`       | `time/Flight Time (hr)`                   | This is in hours.                                                                                   |
| `Enroute Burn`   | `mass_burned/Total Fuel (kg)`            | This is in kilograms.                                                                               |

To do this process the steps are the following:

### Step 1. Get the flight
As I have the flights inside the vscode folder already I don't have to see pgadmin and run the shaify script (but if needed refer to [how to use pgadmin](How_to_use_PGAdmin4.md) 
to look for a desired flight and [how to run optimizer](How_to_Run_Optimizer_Notebook.md) to get the data from that flight inside vscode).

### Step 2. Change the date cell
As the flight has some associated date with it, we have to change the `date` variable inside the loading data section of the notebook so that we refer to that flight.
![image](https://github.com/user-attachments/assets/ddf02a47-093b-470f-8810-e1a8d0f870fb)

***Here we have a problem because, as currently written, the notebook will just let us take one flight per date/day.</span>***

### Step 3. Run all cells
Restartin kernel and variables is a good idea to have everything fresh.
Right now everything is working under `optim_altitude` option and in the vertical analysis section is where the csv output files will be generated (summary and detailed).

### Step 4. Convert the output csv to tables
Go to the generated output csv files, in this case located at 

> `C:\Users\Andres\flitex\Python_4DOptimization_Rewrite_JAN2021\notebooks\TestDL\Result_shaify\20241022\DAL1743`

and use the vscode extension *csv to table* or *csv to markdown table* to make the csv files look nicer (use command palette `Ctrl` + `Shift` + `P` to activate the option.)

### Step 5. Use Simbrief
The Flitex notebook part has been done to this point. Now we go to the Simbrief software to try to see the same flight. The important things to know are:

> Airline (optional). In this case `DAL`.
> Flight number (optional). In this case `1743`.
> Depart (Origin airport). In this case `KATL` (Atlanta).
> Arrive (Destination airport). In this case `KTYS` (Knoxville-Tennessee).
> Departure time (This can be found in the detailed generated csv file, in the printed cell after `get_waypoint_info` funtion or under etd column in pgadmin). In this case `14.32` (UTC Always).
> Aircraft type (Can be found in same places as departure time). In this case `B739`.

We input these and it should look something like this
![image](https://github.com/user-attachments/assets/978794fc-4ed9-4013-910b-a080b404ea7c)

At the end we click on `Generate flight`.

### Step 6. Compare
![image](https://github.com/user-attachments/assets/65717def-5970-4679-a1f5-db11026ead71)
![image](https://github.com/user-attachments/assets/f71180c6-80ab-414a-9d71-42c739457bd4)

In this case, every metric is better in Flitex side and it is also worth to notice that right at the end of the simbrief picture we can see the route taken/recommended and we can also compare 
that one to the Flitex one. A quick summary:

| DAL1743 Route Comparison      | SimBrief  | Flitex     |
|-------------------------------|-----------|------------|
| **Time**                      | 00:42     | 00:30      |
| **Distance**                  | 166       | 131.95     |
| **Fuel**                      | 1891      | 1218.0069  |

---

## FDX 383 KMEM(Memphis) -> KORD(Chicago)
![image](https://github.com/user-attachments/assets/e8b77f47-f487-41dc-8071-1711cc894b52)
![image](https://github.com/user-attachments/assets/514f697d-f36c-4c9d-b8f7-38c408181d5f)

| Flight Route Comparison       | SimBrief  | Flitex     |
|-------------------------------|-----------|------------|
| **Time**                      | 01:25     | 01:02      |
| **Distance**                  | 506 nm    | 424.04 nm  |
| **Fuel**                      | 6026 kg   | 4481.1621 kg |

Here again the Flitex route seems to be better.

About GRIB Files:

I will take flight FDX 383 Memphis to Chicago to make some testing about the Grib Files taken.

|name|date|origin|destination|
|---|---|---|---|
|FDX383|2024-09-28|KMEM(Memphis)|KORD(Chicago)|

I have run this flight three times and got the following results (can be found in `C:\Users\Andres\flitex\Python_4DOptimization_Rewrite_JAN2021\notebooks\TestDL\Result_shaify\20240928\FDX383\optim_flight_summary.csv`)

| length | route                                               | time    | total_cost | distance | mass_burned | Total_cost_kg | Total_cost_lb | takeoff_mass | landing_mass | cruise_ave_speed | cost_of_time | cost_of_fuel | real_path_wind_distance | cost_index_kg_min | cost_index_lbUSD_centsHr | origin | destination | number_of_arcs | grib_data_time             | carbon_output |
| ------ | --------------------------------------------------- | ------- | ---------- | -------- | ----------- | ------------- | ------------- | ------------ | ------------ | ---------------- | ------------ | ------------ | ----------------------- | ----------------- | ------------------------ | ------ | ----------- | -------------- | -------------------------- | ------------- |
| 6      | KMEM FEVES ZEVPO JOROB CORKI SUBUE JILLY GORLC KORD | 1.07339 | 8008.34    | 795858   | 5633.7708   | 5633.7708     | 12420.3238    | 100024.6698  | 94390.9000   | 438.169          | 4469.112     | 0.570        | 425.87                  | 784054.74         | 784054.74                | KMEM   | KORD        | 0              | 2024-11-08 08:03:38.772072 | 17802.716     |
| 6      | KMEM FEVES ZEVPO JOROB CORKI SUBUE JILLY GORLC KORD | 1.04005 | 7202.36    | 795858   | 4481.1621   | 4481.1621     | 9879.2595     | 98871.2490   | 94390.9000   | 449.278          | 4469.112     | 0.570        | 424.04                  | 784054.74         | 784054.74                | KMEM   | KORD        | 0              | 2024-11-22 09:09:01.686902 | 14160.472     |
|      6 | KMEM FEVES ZEVPO JOROB CORKI SUBUE JILLY GORLC KORD | 1.04005 |    7202.36 |   795858 |   4481.1621 |     4481.1621 |     9879.2595 |   98871.2490 |   94390.9000 |          449.278 |     4469.112 |        0.570 |                  424.04 |         784054.74 |                784054.74 | KMEM   | KORD        |              0 | 2024-11-29 09:53:40.124597 |     14160.472 |



## DAL960 KLAX(LA) -> KJFK(NY)
![image](https://github.com/user-attachments/assets/34a42bd4-c95f-42f6-bc7c-7042c72101c6)
![image](https://github.com/user-attachments/assets/1e59e3fd-aa8e-4105-8314-750e14bed408)

| Flight Route Comparison       | SimBrief | Flitex     |
|-------------------------------|----------|------------|
| **Time**                      | 04:32    | 04:48      |
| **Distance**                  | 2231 nm  | 2102.75 nm |
| **Fuel**                      | 19565 kg | 15900.9962 kg |

Here Flitex took a bit more time but less distance and fuel than simbrief. 

## DAL960 KLAX(LA) -> KJFK(NY) More recent
![image](https://github.com/user-attachments/assets/d800a94c-b42e-42a1-bc47-eb6342c92c5c)
![image](https://github.com/user-attachments/assets/ccbfc488-4901-4ba5-a4bd-8a61144c7e5b)

| Flight Route Comparison       | SimBrief | Flitex     |
|-------------------------------|----------|------------|
| **Time**                      | 04:44    | 04:47      |
| **Distance**                  | 2220 nm  | 2103.13 nm |
| **Fuel**                      | 21149 kg | 15888.3988 kg |

Again here flitex took a bit more time.


## SWA 853 KMKE(Milwaukee) -> KMCO (Orlando) 
![image](https://github.com/user-attachments/assets/9d99cfdb-381d-4299-8559-07d54c600b4a)
![image](https://github.com/user-attachments/assets/fe2442cd-77c2-400c-8595-168569ad6e4f)

| Flight Route Comparison       | SimBrief | Flitex     |
|-------------------------------|----------|------------|
| **Time**                      | 02:19    | 02:18      |
| **Distance**                  | 992 nm   | 973.76 nm  |
| **Fuel**                      | 4786 kg  | 4219.9521 kg |




