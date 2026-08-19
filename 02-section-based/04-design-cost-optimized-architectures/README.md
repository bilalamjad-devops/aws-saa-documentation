# Your S3 Storage Class Cheat Sheet

| Storage class                     | Access           | Retrieval         | AZs      | Main use                             |
| --------------------------------- | ---------------- | ----------------- | -------- | ------------------------------------ |
| **S3 Standard**                   | Frequent         | Milliseconds      | Multiple | Active data                          |
| **S3 Standard-IA**                | Infrequent       | Milliseconds      | Multiple | Important infrequently accessed data |
| **S3 One Zone-IA**                | Infrequent       | Milliseconds      | **One**  | Reproducible/non-critical data       |
| **S3 Intelligent-Tiering**        | Unknown/changing | Automatic tiering | Multiple | Unpredictable access                 |
| **S3 Glacier Instant Retrieval**  | Rare             | **Milliseconds**  | Multiple | Archive + immediate retrieval        |
| **S3 Glacier Flexible Retrieval** | Rare             | Minutes–hours     | Multiple | Archive where retrieval can wait     |
| **S3 Glacier Deep Archive**       | Very rare        | Hours             | Multiple | Long-term archival                   |

### One very important correction to your mental model

Don't memorize:

> "Glacier = slow."

That's **too broad**.

There are different Glacier classes:

**Glacier Instant Retrieval** → milliseconds
**Glacier Flexible Retrieval** → minutes to hours
**Glacier Deep Archive** → hours

So always look at the **specific Glacier class**.

---

## 🧠 SAA decision tree

When you get an S3 storage question, ask these in order:

**1. Frequently accessed?**

→ **S3 Standard**

**2. Infrequently accessed but immediate/millisecond retrieval?**

→ **Standard-IA**

**3. Same as above, but data is reproducible and single-AZ is acceptable?**

→ **One Zone-IA**

**4. Access pattern unknown/unpredictable?**

→ **Intelligent-Tiering**

**5. Archive but millisecond retrieval required?**

→ **Glacier Instant Retrieval**

**6. Archive and can wait minutes/hours?**

→ **Glacier Flexible Retrieval**

**7. Very long-term archive and rarely accessed?**

→ **Glacier Deep Archive**

That decision tree is much more useful for SAA than simply memorizing the names.


19-August-2026



### 🧠 Your EC2 shortcut

| Requirement in question                 | Think                                   |
| --------------------------------------- | --------------------------------------- |
| Can be interrupted                      | 🟢 **Spot**                             |
| Non-production / batch processing       | 🟢 **Spot**                             |
| Need guaranteed capacity in specific AZ | 🔵 **Capacity Reservation**             |
| Predictable 1–3 year workload           | 🟡 **Reserved Instance / Savings Plan** |
| Short-term / unpredictable              | 🟠 **On-Demand**                        |
| Entire physical server / licensing      | 🔴 **Dedicated Host**                   |

