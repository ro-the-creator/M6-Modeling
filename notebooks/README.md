# **Initial EDA Tracker**

<div align='center'>

This markdown documents the initial EDA, cleaning, and proposed scope of the project.
</div>

# **Scope**

### Proposed Business Question

<div align='center'>

**Do drivers of government vehicles need more training??**
</div>

### Proposed Variables

- y = `number_of_persons_injured` (Continuous)

<br>

- X1 = `is_government` (Binary)
    - From `vehicle_type_code_#`, flag for government-owned vehicles, ambulance, etc.

    - No scaling needed?

<br>

- X2 = `season` (Categorical)
    - From `crash_date`

    - **Winter:** Dec, Jan, Feb

    - **Spring:** Mar, Apr, May

    - **Summer:** Jun, Jul, Aug

    - **Fall:** Sep, Oct, Nov

    <br>

- X3 = `government_crash_date` (Interaction)
    - How the effect of `is_government` changes given varying `season`.

### Factors to Tweak (If Output is Poor)

- Try phases of the day instead of seasons of the year

- Pull from Dec 2021 as well (for even season coverage)

- Try other injury/death statistic columns
    - `number_of_persons_killed`
    - `number_of_pedestrians_injured`
    - etc.

<br>

# Cleaning, Feature Engineering, & EDA

- There are no missing rows for `crash_date` or `number_of_persons_injured`. While there are many NaNs for all `vehicle_type_code_#` columns, we are only using them to create a new flag column, and can make reasonable assumptions based on the missing values (Info about assumptions detailed in the [assumptions](#assumptions) section).

- There was a lot of inconsistency in the strings. Specifically, the `vehicle_type_code_#` strings had many issues to deal with, including inconsistent letter case, leading & trailing whitespace, and invalid entries.

- Missing values in the `vehicle_type_code_#` columns were filled with an empty space to get rid of all NaNs.

- Mapped every vehicle entry to 1 of 19 categories: 
    `Sedan`, `SUV`, `Taxi`, `Pickup Truck`, `Van`, `Truck`, `Bus`,
    `Motorcycle`, `Moped`, `Scooter`, `Ambulance`, `Fire Truck`,
    `Garbage Truck`, `Delivery Truck`, `Forklift / Construction`,
    `Trailer`, `Government Vehicle`, `Commercial Vehicle`, or `Unknown`

- *Used keywords with theFuzz library to map certain keywords to government-owned vehicles:
    `fdny`, `nypd`, `usps`, `postal`, `mta`, `dot`,
        `city`, `nyc`, `ems`, `sanitation`,
        `gov`, `government`, `parks`, `transit`     
  
### Feature Engineering

`month` & `season`

### EDA Findings

- Government vehicles are involved in 157817/370001(~43%) of collisions in NYC
<details>
  <summary>
 KPIs from collisions with government-owned vehicles (As listed in DataFrame):</summary>

###### Casualties:
    - Total persons injured in government vehicle crashes: 88938
    - Total persons killed in government vehicle crashes: 384
    - Number of government vehicle crashes with no injury or death: 95305
    - Total people affected by vehicle crashes: 184627

###### Causes:
    - Unspecified                                              169504
    - Driver Inattention/Distraction                            47387
    - Following Too Closely                                     13498
    - Failure to Yield Right-of-Way                             12257
    - Passing or Lane Usage Improper                             7886
    - Other Vehicular                                            7562
    - Unsafe Speed                                               6973
    - Passing Too Closely                                        5886
    - Traffic Control Disregarded                                5381
    - Backing Unsafely                                           5337
    - Turning Improperly                                         3832
    - Unsafe Lane Changing                                       3678
    - Alcohol Involvement                                        3678
    - Driver Inexperience                                        3675
    - Reaction to Uninvolved Vehicle                             2463
    - View Obstructed/Limited                                    1801
    - Pedestrian/Bicyclist/Other Pedestrian Error/Confusion      1706
    - Aggressive Driving/Road Rage                               1609
    - Pavement Slippery                                          1511
    - Fell Asleep                                                 930
    - Brakes Defective                                            736
    - Oversized Vehicle                                           497
    - Outside Car Distraction                                     465
    - Steering Failure                                            413
    - Obstruction/Debris                                          411
    - Passenger Distraction                                       404
    - Lost Consciousness                                          386
    - Illnes                                                      348
    - Glare                                                       304
    - Tire Failure/Inadequate                                     298
    - Fatigued/Drowsy                                             229
    - Failure to Keep Right                                       229
    - Driverless/Runaway Vehicle                                  186
    - Drugs (illegal)                                             143
    - Animals Action                                              135
    - Pavement Defective                                          125
    - Traffic Control Device Improper/Non-Working                 122
    - Accelerator Defective                                       110
    - Cell Phone (hand-Held)                                       92
    - Lane Marking Improper/Inadequate                             86
    - Physical Disability                                          85
    - Tinted Windows                                               39
    - Prescription Medication                                      33
    - Eating or Drinking                                           26
    - Vehicle Vandalism                                            25
    - Headlights Defective                                         25
    - Other Lighting Defects                                       19
    - Using On Board Navigation Device                             16
    - Tow Hitch Defective                                          13
    - Other Electronic Device                                      12
    - Cell Phone (hands-free)                                       9
    - Windshield Inadequate                                         7
    - Listening/Using Headphones                                    6
    - Shoulders Defective/Improper                                  6
    - Texting                                                       4
</details>

<img width="400" height="300" alt="image" src="https://github.com/user-attachments/assets/8cd7acb1-4b2e-4149-8a09-4230d5df2ee5" />

<sub>Barchart displaying Season vs. Number of People Injured</sub>

### Assumptions & Limitations(*)

- For all columns regarding vehicle types, we assumed that missing values in these rows did not indicate government vehicle involvement unless explicitly stated.

- For the vehicles `Ambulance`, `Bus`, `Garbage Truck`, `Delivery Truck`, and `Fire Truck`, we assumed that they were all government-owned vehicles, not commercial or private.

-  The EDA puts all vehicles contributing to a collision on one line, separated by "|". This may affect the specific count of government vehicles, but it correctly gives the number of collisions involving government vehicles.

-  We assumed that the type of government-owned vehicles is listed with priority over the style of vehicle. E.g., we assume that none of the vehicles listed as a Sedan are government-owned, or else they would have been listed as such.
<br>

***

