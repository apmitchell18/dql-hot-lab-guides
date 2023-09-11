## Hands-On Exercise 3 - Entity Queries in Practice

In this section, you will work through a practical use case using entity queries in DQL.

---

### Step 1: Filter entities based on specific criteria
One of the things we can use DQL and entities is to provide as much insight and visibility into potential affectation of known vulnerabilities. For example the recent situation with the Windows patch and .NET Framework 3.5.

For this kind of situations, the entity information available in GRAIL gives us the means to look for the PGIs that could potentially be affected by this situation.

We will begin by querying a list of process group instances based on their technology type. In a new notebook, add a new section and choose "Query Grail".

#### Write and execute a query to obtain a list of PGIs which technology is .NET and the version contains '3.5'.

![Notebooks](../../assets/images/NET 3_5.png)
...
