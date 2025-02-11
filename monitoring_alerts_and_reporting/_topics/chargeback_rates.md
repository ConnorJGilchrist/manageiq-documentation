### Chargeback Rates

{{ site.data.product.title_short }} provides a default set of rates for calculating chargeback costs, but you can create your own set of computing and storage costs by browsing to menu: **Overview > Chargeback** and clicking the **Rates** accordion.

You can configure chargeback rates for various resources by selecting either **Compute** or **Storage** in the **Rates** accordion. **Compute** sets chargeback rates for CPU, disk I/O, fixed compute cost, memory, and network I/O items, while **Storage** sets chargeback rates for fixed storage cost and disk storage.

Chargeback costs are computed using a set formula based on hourly cost per unit and hourly usage.

Chargeback can be calculated in the following currencies:

- Australian dollar (AUD)

- Brazilian real (BRL)

- Swiss franc (CHF)

- Chinese yuan renminbi (CNY)

- Euro (EUR)

- Pound sterling (GBP)

- Indian rupee (INR)

- Japanese yen (JPY)

- South Korean won (KRW)

- Mexican peso (MXN)

- Norwegian krone (NOK)

- Russian ruble (RUB)

- Swedish krona (SEK)

- Turkish lira (TRY)

- United States dollar (USD)

- South African rand (ZAR)

Chargeback rates can be assigned at a single rate or by tiers, where rates are assigned in ranges depending on level of usage.

Additionally, chargeback can be calculated at one fixed rate, or by a combination of fixed and variable rates per tier. Fixed rates are charged once per unit of time, and the variable rate is calculated by the level of usage multiplied by the number of resources used in a unit of time.

#### Memory Used Cost

Calculating the Memory Used Cost in dollars ($) for a day can be expressed in the following ways:

- Memory allocation per hour (in MB) \* Hourly Allocation cost per megabyte \* Number of Memory Allocation metrics available for the day

- Sum of Memory allocation for the day (in MB) \* Hourly Allocation cost per megabyte

- Sum of Memory allocation for the day (in MB) \* Daily Allocation cost per megabyte / 24

Memory costs can be measured in B, KB, MB, GB, or TB.

In a scenario where 9.29 GB of memory is used in a day with the chargeback rate set at one dollar ($1) per megabyte per day, the Memory Used Cost would be $396.42.

- 9.29 GB = 9514.08 MB

- 9514.08 MB \* $1 (per MB per day) = $9514.08

- $9514.08 / 24 = $396.42 Memory Used Cost

#### CPU Total Cost

The CPU Total Cost is defined as the number of virtual CPUs over the selected interval (hour, day, week, month). CPU costs can be measured in units of Hz, KHz, MHz, GHz, or THz, as specified when creating a chargeback rate.

In a scenario where 16 CPUs are used in a day with the chargeback rate set at one dollar per CPU per day, the CPU Total Cost would be $16.

- 16 CPUs \* $1 (per CPU per day) = $16 CPU Total Cost

#### CPU Used Cost

The CPU Used Cost is defined as the average CPU used in MHz over the selected rate interval (hour, day, week, month). CPU Used Cost is not supported for containers providers.

In a scenario where 2.5 GHz is used in a day with the chargeback rate set at $0.01 per MHz per day, the CPU Used Cost would be $25.

- 2.5 GHz = 2500 MHz

- 2500 MHz \* $0.01 (per MHz per day) = $25 CPU Used Cost

#### Storage Allocated Cost

The Storage Allocated Cost is defined as the Allocated Disk Storage in bytes over the selected rate interval (hour, day, week, month). Storage costs can be measured in B, KB, MB, GB, or TB.

In a scenario where 500 GB are used in a day with the chargeback rate set at $0.10 per GB per day, the Storage Allocated Cost would be $50.

- 536,870,912,000 bytes = 500 GB

- 500 GB \* $0.10 (per GB per day) = $50 Storage Allocated Cost

#### Storage Total Cost

The Storage Total Cost is defined as the Used Disk Storage in bytes over the selected rate interval (hour, day, week, month).

In a scenario where 250 GB are used in a day with the chargeback rate set at $0.10 per GB per day, the Storage Total Cost would be $25.

- 268,435,456,000 bytes = 250 GB

- 250 GB \* $0.10 (per GB per day) = $25 Storage Total Cost

#### Storage Used Cost

The Storage Used Cost is defined as the Used Disk Storage in bytes over the selected rate interval (hour, day, week, month).

In a scenario where 250 GB are used in a day with the chargeback rate set at $0.10 per GB per day, the Storage Used Cost would be $25.

- 268,435,456,000 bytes = 250 GB

- 250 GB \* $0.10 (per GB per day) = $25 Storage Used Cost

**Note:**

The following chargeback rates are not supported for containers providers:

- Allocated CPU count

- Used CPU

- Used disk I/O

- Allocated memory
