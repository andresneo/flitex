# Updated How to Run Optimizer Notebook after machine change.



## Step 1 Download the folder

---
Create a new (empty) 'flitex' folder where all the project is gonna be located:

> 'C:Users/Andres/flitex'

Open bash or terminal in that folder and run:

```bash
$ git clone https://github.com/fliteX-ORG/Python_4DOptimization_Rewrite_JAN2021.git
```
Then cd into `flitex/Python_4DOptimization_Rewrite_JAN2021/` and do

```bash
$ git fetch

$ git status

$ git checkout andres_branch

$ git pull

$ git status

$ exit
```

---

# Step 2 Activate virtual environment
---

A good advice is to delete env folder first! This is located in `C:/Users/Andres/flitex/Python_4DOptimization_Rewrite_JAN2021/env`

Then run

```bash
python -m venv env
```

in the project folder.

To activate (in bash) use:

```bash
source env/Scripts/activate
```

Install Dependencies (make sure the command prompt line shows (env) at the beginning)

```bash
pip install -r requirements.txt
```

# Select the kernel and run the first cell of the notebook

I had a lot of trouble in this step. The conclusion is:

- Be sure to download python 3.10.2 version as this one seems to be the one compatible with every package used.
- delete env folder (if it was not done in ) and RE-DO [Step 2](#Step-2-Activate-virtual-environment) again!

# Step 3.5 

Deactivate virtual environment (optional). As the virtual environment was only used to setup purposes is safe to deactivated because the kernel and everything has all been done.

To do this only write `deactivate` in the terminal. The (env) should disappear after this. I did not do this step in this readme instructions.

---
# Step 4

Update all the paths (remember to use forward slash format `'C:/Users/Andres/flitex/Python_4DOptimization_Rewrite_JAN2021'`)

---
# Step 5

Run the cells (right now some things are not getting accessed like numpy or pandas not sure why)

To run some of them we have to get the data first. This is done picking a flight (from pgadmin) and inside `shaify2/FDX` folder we run the the command to run the tfms script: Here is how these steps look like (inside integrated bash terminal)


```bash
Andres@AndresPC MINGW64 ~/flitex/Python_4DOptimization_Rewrite_JAN2021 (andres_branch)
$ cd shaify2
(env) 
Andres@AndresPC MINGW64 ~/flitex/Python_4DOptimization_Rewrite_JAN2021/shaify2 (andres_branch)
$ cd FDX/
(env) 
Andres@AndresPC MINGW64 ~/flitex/Python_4DOptimization_Rewrite_JAN2021/shaify2/FDX (andres_branch)
$ LS
20240928  20241022  20241105  GEN_20240928.csv  GEN_20241022.csv  GEN_20241105.csv  GEN_20241107.csv  populateFDX.sh  Schedule           tfms_dbquery12_windows.py
20241020  20241025  20241107  GEN_20241020.csv  GEN_20241025.csv  GEN_20241106-01   GEN_20241108-02   real_flights    tfms_dbquery12.py
(env) 
Andres@AndresPC MINGW64 ~/flitex/Python_4DOptimization_Rewrite_JAN2021/shaify2/FDX (andres_branch)
$ python tfms_dbquery12.py --idents FDX383 -d 20240928

 20/11/2024 23:04:54 ---  Running Flightaware pull script:
Airline : None
Flights : {'FDX383'}
Out-Dir : .
exiting
1 new Flights found.
Writing new data for flight: FDX383 at 2024-09-28 20:18:00, with 17 waypoints.
```
The flight I picked here was one I had already used. This should create two files:

The 'schedule_filepath' file which is created here:

`C:/Users/Andres/flitex/Python_4DOptimization_Rewrite_JAN2021/shaify2/FDX/20240928/FDX383_20240928.csv`

and looks like this (I used csv to markdown extension to make it look nicer and copied that here)

| Name  | Latitude | Longitude | Outbound_Course | Distance_this_Leg | Distance_Remaining | Distance_Flown | Type |
| ----- | -------- | --------- | --------------- | ----------------- | ------------------ | -------------- | ---- |
| MEM   | 35.04    | -89.98    | 355             | 0                 | 0                  | 0              | WPT  |
| HSTON | 35.63    | -90.04    | 356             | 35                | 0                  | 35             | WPT  |
| JTEEE | 35.75    | -90.05    | 360             | 7                 | 0                  | 42             | WPT  |
| ODATE | 35.78    | -90.05    | 346             | 2                 | 0                  | 44             | WPT  |
| FTZ   | 38.69    | -90.97    | 23              | 180               | 0                  | 224            | WPT  |
| GROOV | 39.43    | -90.57    | 24              | 48                | 0                  | 272            | WPT  |
| CASHN | 39.96    | -90.26    | 41              | 35                | 0                  | 307            | WPT  |
| VINCA | 40.35    | -89.82    | 41              | 31                | 0                  | 338            | WPT  |
| MAROC | 40.74    | -89.37    | 11              | 31                | 0                  | 369            | WPT  |
| KELTS | 41.12    | -89.27    | 8               | 23                | 0                  | 392            | WPT  |
| FNBAR | 41.28    | -89.24    | 9               | 10                | 0                  | 402            | WPT  |
| TRIDE | 41.47    | -89.2     | 15              | 12                | 0                  | 414            | WPT  |
| SHAIN | 41.64    | -89.14    | 16              | 11                | 0                  | 425            | WPT  |
| RAGSS | 41.72    | -89.11    | 21              | 5                 | 0                  | 430            | WPT  |
| JUMPN | 41.82    | -89.06    | 37              | 6                 | 0                  | 436            | WPT  |
| NUNWS | 41.92    | -88.96    | 85              | 7                 | 0                  | 443            | WPT  |
| ORD   | 41.98    | -87.91    | -1              | 47                | 0                  | 490            | WPT  |

and the 'data' file which is created here:

`C:/Users/Andres/flite/Python_4DOptimization_Rewrite_JAN2021/shaify2/FDX/GEN_20240928.csv`

and looks like this

| origin_airport | destination_airport | arrival_time | dept_time | aircraft_type | flight_names        | identity_num | route                                   | filed_altitude | filed_airspeed_kts | flight_ref |
| -------------- | ------------------- | ------------ | --------- | ------------- | ------------------- | ------------ | --------------------------------------- | -------------- | ------------------ | ---------- |
| KMEM           | KORD                | Sat 21.40    | Sat 20.18 | B763          | FDX383_20240928.csv | FDX383       | KMEM.JTEEE5.ODATE..FTZ.SHAIN2.KORD/0121 | 340            | 466                | 99361631   |
|                |                     |              |           |               |                     |              |                                         |                |                    |            |

# Step 6 

Re update the paths if necessary (check all the cells) so that they are pointing to this data files locations.

For example in the loading data section, in the second cell I was having trouble running this:

```python
schedule_filepath = 'C:/Users/Andres/flitex/Python_4DOptimization_Rewrite_JAN2021/shaify2/FDX/GEN_{date}.csv'
```
as it was not reeding it! I needed to put an f in the front of the path (f string) to be able to treat `date` as a variable.

Ok with this it seems like setup is all good! :smile:

