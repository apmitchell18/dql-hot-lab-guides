## Hands-On Exercise 3 - Entity Queries in Practice

In this section, you will work through a practical use case using entity queries in DQL.

---

### Step 1: Filter entities based on specific criteria
One of the things we can use DQL and entities is to provide as much insight and visibility into potential affectation of known vulnerabilities. For example the recent situation with the Windows patch and .NET Framework 3.5.

For this kind of situations, the entity information available in GRAIL gives us the means to look for the PGIs that could potentially be affected by this situation.

We will begin by querying a list of process group instances based on their technology type. In a new notebook, add a new section and choose "Query Grail".

#### Write and execute a query to obtain a list of PGIs which technology is .NET and the version contains '3.5'. Make sure to add the 'softwareTechnologies' column to confirm that the .NET version 3.5 is being met. Here is an example of a result we are looking for.

![Notebooks](../../assets/images/NET%203_5.png)

<H4><details>
<summary>Click to Expand Solution</summary>
<br>

```
fetch dt.entity.process_group_instance
| filter processType == "DOTNET" and contains(toString(softwareTechnologies), "3.5")
| fieldsAdd softwareTechnologies
```
</details></H4>

---

### Step 2: Add additional context information regarding these applications
As you can see, that simple query is giving us the list of potentially affected processes by a known vulnerability. With this we can start adding context information, for example which is the host the application is running on.
#### Update the query to add information about the underlying host.

![Notebooks](../../assets/images/NET3_5_wHost.png)

(**Hint**: You can use the **belongs_to** field that provides the underlying host information. In order to display the ID, you will need to process the contents of this field. Check out [String functions](https://www.dynatrace.com/support/help/platform/grail/dynatrace-query-language/functions#dql-string-functions), in this case **toString** and **substring**)

<H4><details>
<summary>Click to Expand Solution</summary>
<br>

```
fetch dt.entity.process_group_instance
| filter processType == "DOTNET" and contains(toString(softwareTechnologies), "3.5")
| fieldsAdd softwareTechnologies
| fieldsAdd belongs_string = toString(belongs_to)
| fieldsAdd host = substring(belongs_string, from:indexOf(belongs_string, ":")+2, to:lastIndexOf(belongs_string, "\""))
| fieldsRemove belongs_string
```
</details></H4>
---

