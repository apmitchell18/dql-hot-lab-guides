## Hands-On Exercise 2 - Querying Entities

In this section, you will practice querying business events with DQL.

---

### Step 1: Create a New Notebook

For this exercise, we will be querying Grail from within a Notebook.

Navigate to the **Notebooks** application and create a new notebook.

Name this notebook "Bizevent Exercises".

---

### Step 2: Find the Existing Business Events

Before you begin to analyze business event data with DQL, you need to understand what business events are currently configured in your environment.

Three business events have been preconfigured in this environment for the EasyTrade application.  Write a query that does the following:
- Lists all currently configured business events for the EasyTrade app.
- Counts the number of occurrences of each in the last two hours.

**Hint:** There are two mandatory data fields that all business events must have: _event.category_, and _event.type_.  

<H3><details>
    <summary>Click to Expand Solution</summary>

```
	fetch bizevents
	| filter event.provider == "www.easytrade.com"
	| summarize count = count(), by:{event.provider, event.type}
```

![Bizevent Exercise 2 Solution](../../assets/images/bizevent_solution_ex2.png)

</H3></details>

By understanding what business events exist in this environment, we can begin using this data to perform business analysis. 



---

### Step 3: Find the Number of Depositors

In addition to representing an occurrence of the event, each business event object can contain extracted data which provides additional information about the event.

One of the business events, _com.easytrade.deposit-money_, represents a user using the application to deposit money.  This event is configured to extract additional information including the user's account id (**Account ID**) and the amount of money deposited (**Amount**).

Write a query that will provide the number of **unique users** who have deposited money in the last 2 hours.

**Hint:** Check the documentation for a DQL function that can provide this kind of result: https://www.dynatrace.com/support/help/shortlink/dql-functions


<H3><details>
    <summary>Click to Expand Solution</summary>

```
	fetch bizevents
	| filter event.type == "com.easytrade.deposit-money"
    | summarize distinctUsers = countDistinct(accountId)
```

![Bizevent Exercise 3 Solution](../../assets/images/bizevent_solution_ex3.png)

</H3></details>


---


### Step 4: Find the Top Depositors

There are many ways the Summarize command can be used to manipulate the captured data to perform analysis.

Using the summarize command and the available fields (Account ID and Amount), write a query to provide the following:
- The total amount of money deposited by each user.
- The total number of deposit transactions initiated by each user.
- Order the list to show the users who have deposited the most amount of money first.

<H3><details>
    <summary>Click to Expand Solution</summary>

```
    fetch bizevents
    | filter event.type == "com.easytrade.deposit"
    | summarize {totalMoneyDeposited = sum(Amount), numberOfTransactions = count()}, by:{`Account ID`}
    | sort totalMoneyDeposited desc
```

![Bizevent Exercise 4 Solution](../../assets/images/bizevent_solution_ex4.png)

</H3></details>


---


### Step 5: TBD