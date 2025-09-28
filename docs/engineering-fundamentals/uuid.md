
<h1 id="top">UUID</h1>

<h3>📑Table of Contents</h3>

1. [UUID Structure](#s)
2. [UUID Variants](#v)
3. [UUID Versions](#ve)
4. [UUID Randomness Capacity](#c)
5. [Probability of Collision Between Two UUIDs](#p)
6. [Number of UUIDs Needed for a Collision (Birthday Bound)](#n)
7. [UUID Collision Handling](#h)
8. [Integer vs UUID Storage Across Databases](#i)
9. [How to Generate a UUID](#hu)
10. [Use Case of UUID](#u)


---
> A **UUID (Universally Unique Identifier)** is a nearly unique 128-bit value that can identify objects across different systems or datasets, functioning like a digital fingerprint, and does not follow a sequential order.



 <h1 id="s">1. UUID Structure</h1>

* 128-bit value represented as 32 hexadecimal characters.
* Often displayed in five groups: `8-4-4-4-12` with hyphens.
* Hyphens are optional, included for readability and historical reasons.
* Example: `67e55044-10b1-426f-9247-bb680e5fe0c8`


<h1 id="v">2. UUID Variants</h1>

* **Variant 0:** Obsolete
* **Variant 1:** Main variant used today
* **Variant 2:** Reserved for Microsoft backward compatibility

<h1 id="ve">3. UUID Versions</h1>

* **v1:** Time and MAC address based; unique but reveals creation time/location.

  * Structure: `time_low`, `time_mid`, `time_high_and_version`, `clock_seq_and_version`, `node`
* **v2:** POSIX UID replaces low_time; rarely used due to higher collision risk.
* **v3 & v5:** Name-based using namespace + name; v3 uses `MD5 hashing` algorithm, v5 uses `SHA1 hashing` algorithm.
* **v4:** Generated completly randomly and have no identifying information thus small chance of collision.
* **v6:** Like v1, but timestamp bits reordered (most significant first).
* **v7:** Time-based using Unix Epoch; node replaced with randomness for privacy.
* **v8:** Vendor-specific implementations following RFC rules; version indicated in third segment.

       

>A collision occurs when the same UUID is generated more than once and is assigned to different objects. 

[↑ Back to top](#top)

<h3 id="c">4. UUID Randomness Capacity</h3>

* A UUID is a 128-bit number.
* Not all 128 bits are available for randomness:

  * **6 bits** are reserved for version + variant.
  * That leaves **122 random bits**.
* So maximum unique random UUIDs = 2×10²² ≈ 5.3×10³⁶





<h3 id="p">5. Probability of Collision Between Two UUIDs</h3>
If we generate two UUIDs, the probability of them colliding is: 1 / 2¹²² ≈ 1.88 × 10⁻³⁷


which is extremly small and therefore can be regarded as practically impossible .




 <h3 id="n">6. How many UUID needs to be generated to get a collision with a certain probability ?</h3>

To calculate the number of UUIDs required for a collision with a given probability (p), <Br> 
**The Birthday Bound Formula** is used: n = √(2·N·ln(1 / (1 - p))) <br>
<br>
<h4>For a 10% probability of collision:</h4>
n = √(2 · 2¹²² · ln(1 / (1 - 0.1))) ≈ 1,058,482,487,574,424,704 × 10¹⁸ UUIDs


**Memory Requirements**
Storing all generated UUIDs (128 bits = 16 bytes each):
- Decimal: 16,935,719.80 TB (≈ 17 million terabytes)
- Binary (TiB): 15,402,947.43 TiB


**Time to Generate**
At a rate of 1,000,000 UUIDs per second:
- Seconds required: ≈ 1.058 × 10¹²
- Years required: 33,564.26 years


<h4>Key Points </h4>

* Collisions are theoretically possible.
* A **10% chance** requires generating about (1.06 × 10¹⁸) UUIDs, needing **~17 million TB** of storage and **33,564 years** at **1M UUIDs/sec**.
* so Collisions are practically impossible for real systems.



[↑ Back to top](#top)
 <h1 id="h">7. UUID Collision Handling</h1>

- **Local collisions**: If a UUID field is UNIQUE, a duplicate insert will fail and would not be saved. Generating a new UUID resolves the issue.

- **Global collisions**: Extremely unlikely; collisions across different systems are virtually impossible.

- **Merging datasets** : UUIDs allow multiple sets to combine without remapping. Rare collisions during merging can be detected and handled like local collisions.
 


<h1 id="i">8. Integer vs UUID Storage Across Databases</h1>

- **Storage size per value:**

  * Auto-incrementing integer: 32 bits (4 bytes)
  * UUID (binary): 128 bits (16 bytes)
  * UUID as CHAR(36): 288 bits (36 bytes)


- Best Practices for Using UUIDs
    - Store UUIDs in a BINARY(16) column instead of CHAR(36) to reduce storage overhead.



<h1 id="hu">9. How to Generate a UUID</h1>

| Vendor     | Data Type for UUID     | Function to Generate UUID                 |
| ---------- | ---------------------- | ----------------------------------------- |
| Oracle     | RAW(16)                | `SYS_GUID()`                              |
| SQL Server | UNIQUEIDENTIFIER       | `NEWID()` or `NEWSEQUENTIALID()`          |
| MySQL      | BINARY(16) or CHAR(36) | `UUID()` or `UUID_TO_BIN(UUID())`         |
| PostgreSQL | `uuid`                 | `gen_random_uuid()` (requires `pgcrypto`) |


<h1 id="u">10. Use Case of UUID</h1>

- Decentralized generation: UUIDs can be generated anywhere (backend, client, database) and are reliably unique.
- Simplifies object identity: Maintains uniqueness across disconnected systems.
- Early ID assignment: Unlike auto-incrementing integers, UUIDs allow determining an object's ID before inserting into a database.
<br>

[↑ Back to top](#top)