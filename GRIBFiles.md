**2. GRIB Files and Timestamp Observations**

I focused on testing GRIB file timestamps using specific flight scenarios. Below are the summarized results and findings:

### Flight FDX383: Memphis to Chicago (KMEM to KORD)

| Name   | Date                       | Origin | Destination |
| ------ | -------------------------- | ------ | ----------- |
| FDX383 | 2024-09-28 (20:18 - 21:40) | KMEM   | KORD        |

I ran this flight simulation three times and observed the following results:

- Data location: `C:\Users\Andres\flitex\Python_4DOptimization_Rewrite_JAN2021\notebooks\TestDL\Result_shaify\20240928\FDX383\optim_flight_summary.csv`

| Length | Route                                               | Time    | Total Cost | Distance | Mass Burned | Total Cost (kg) | Total Cost (lb) | Takeoff Mass | Landing Mass | Cruise Ave Speed | Cost of Time | Cost of Fuel | Real Path Wind Distance | GRIB Data Time             | Carbon Output |
| ------ | --------------------------------------------------- | ------- | ---------- | -------- | ----------- | --------------- | --------------- | ------------ | ------------ | ---------------- | ------------ | ------------ | ----------------------- | -------------------------- | ------------- |
| 6      | KMEM FEVES ZEVPO JOROB CORKI SUBUE JILLY GORLC KORD | 1.07339 | 8008.34    | 795858   | 5633.7708   | 5633.7708       | 12420.3238      | 100024.6698  | 94390.9000   | 438.169          | 4469.112     | 0.570        | 425.87                  | 2024-11-08 08:03:38.772072 | 17802.716     |
| 6      | KMEM FEVES ZEVPO JOROB CORKI SUBUE JILLY GORLC KORD | 1.04005 | 7202.36    | 795858   | 4481.1621   | 4481.1621       | 9879.2595       | 98871.2490   | 94390.9000   | 449.278          | 4469.112     | 0.570        | 424.04                  | 2024-11-22 09:09:01.686902 | 14160.472     |
| 6      | KMEM FEVES ZEVPO JOROB CORKI SUBUE JILLY GORLC KORD | 1.04005 | 7202.36    | 795858   | 4481.1621   | 4481.1621       | 9879.2595       | 98871.2490   | 94390.9000   | 449.278          | 4469.112     | 0.570        | 424.04                  | 2024-11-29 09:53:40.124597 | 14160.472     |

**Observation:**

- The first run (November 8, 2024) produced a different result compared to subsequent runs. This was for a flight scheduled on September 28, 2024.

### Flight DAL3049: Atlanta to Knoxville (KATL to KTYS)

| Name    | Date                       | Origin | Destination |
| ------- | -------------------------- | ------ | ----------- |
| DAL3049 | 2024-12-19 (22:11 - 22:41) | KATL   | KTYS        |

Three runs were conducted for this future flight (i.e., simulations were performed before the flight’s departure date). Results:

| Length | Route                 | Time    | Total Cost | Distance | Mass Burned | Total Cost (kg) | Total Cost (lb) | Takeoff Mass | Landing Mass | Cruise Ave Speed | Cost of Time | Cost of Fuel | Real Path Wind Distance | GRIB Data Time             | Carbon Output |
| ------ | --------------------- | ------- | ---------- | -------- | ----------- | --------------- | --------------- | ------------ | ------------ | ---------------- | ------------ | ------------ | ----------------------- | -------------------------- | ------------- |
| 4      | KATL IPECA HINDE KTYS | 0.54486 | 2987.34    | 252151   | 968.9504    | 968.9504        | 2136.1675       | 32858.6552   | 31890.0000   | 319.731          | 4469.112     | 0.570        | 134.05                  | 2024-12-19 21:18:59.792271 | 3061.883      |
| 4      | KATL IPECA HINDE KTYS | 0.54486 | 2987.34    | 252151   | 968.9504    | 968.9504        | 2136.1675       | 32858.6552   | 31890.0000   | 319.731          | 4469.112     | 0.570        | 134.05                  | 2024-12-20 04:00:24.286897 | 3061.883      |

**Key Findings:**

1. For flight FDX383, the first run’s output (on November 8) differs from the others, raising questions about GRIB file handling.
2. For flight DAL3049, all outputs were identical, even though the GRIB file timestamp corresponded to the UTC time of code execution.

**Hypotheses:**

1. The code uses GRIB files based on the execution date.
   - This would be unexpected and illogical for flight optimization.
   - However, it does not explain why so many results are identical.
2. The code uses GRIB files for the flight date (past or future).
   - This aligns with expectations for the intended behavior.
   - Yet, the differing result in the first run of FDX383 contradicts this.
