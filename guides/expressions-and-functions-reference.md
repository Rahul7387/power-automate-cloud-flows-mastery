# Guide — Power Automate Expressions & Functions Reference
### The workflow definition language, with practical recipes

Every non-trivial flow eventually needs an expression the dynamic-content picker can't give you directly — a formatted date, a null-safe fallback, a filtered array. This guide is the reference to come back to across every project in this repo, especially Project 3 (Data Operations & Control Flow).

---

## 1. Syntax basics

- Expressions are written inside `@{ }` when embedded in a larger string (e.g., `Hello @{triggerBody()?['name']}`), or as a bare `@expression` when the entire field is the expression.
- The **`?.` / `?[]`** null-safe accessor (e.g., `triggerBody()?['Title']`) is critical — without the `?`, a missing property throws an error instead of returning null. **Default to using `?` on every dynamic content reference unless you are certain the property always exists.**
- Use the **"Peek code"** view (see the platform walkthrough guide) to see the raw expression Power Automate generated when you use the dynamic content picker — this is the fastest way to learn correct syntax by example.

## 2. Referencing data — the functions you'll use in almost every flow

| Function | What it does | Example |
|---|---|---|
| `triggerBody()` | The body of whatever triggered the flow | `triggerBody()?['Title']` |
| `triggerOutputs()` | Full trigger output, including headers | `triggerOutputs()?['headers']?['x-ms-workflow-id']` |
| `body('ActionName')` | The output body of a named prior action | `body('Get_file_metadata')?['DisplayName']` |
| `outputs('ActionName')` | Full output of a named prior action (status, headers, body) | `outputs('HTTP')?['statusCode']` |
| `items('Apply_to_each')` | The current item inside a loop | `items('Apply_to_each')?['OrderId']` |
| `variables('varName')` | The current value of a variable | `variables('varProcessedCount')` |
| `actions('ActionName')` | Full action result, including status — useful in Catch scopes | `actions('HTTP')?['error']?['message']` |
| `workflow()` | Metadata about the flow itself (run ID, name) | `workflow()?['run']?['name']` |

**A note specific to Project 5 (error handling):** inside a Catch scope, use `result('Try')` to get an array of every action's outcome inside the Try scope — this is how you build a Compose action that captures exactly which action failed and why, rather than a generic "something failed" message.

## 3. String functions

| Function | Purpose | Example |
|---|---|---|
| `concat(a, b, ...)` | Join strings | `concat('Order #', variables('orderId'))` |
| `replace(text, old, new)` | Replace substring | `replace(triggerBody()?['Notes'], '\n', ' ')` |
| `split(text, delimiter)` | Split into an array | `split(triggerBody()?['Tags'], ',')` |
| `substring(text, start, length)` | Extract part of a string | `substring(variables('sku'), 0, 3)` |
| `toUpper()` / `toLower()` | Case conversion | `toUpper(items('Apply_to_each')?['Status'])` |
| `trim()` | Remove leading/trailing whitespace | `trim(triggerBody()?['Email'])` |
| `indexOf(text, search)` | Position of a substring, -1 if absent | `indexOf(variables('url'), 'https')` |
| `guid()` | Generate a new GUID | Useful for idempotency keys on retried actions |
| `formatNumber(number, format)` | Format a number as a string | `formatNumber(variables('amount'), 'C2')` for currency |

## 4. Collection (array/object) functions

| Function | Purpose | Example |
|---|---|---|
| `length(collection)` | Count items | `length(body('Filter_array'))` |
| `first(collection)` / `last(collection)` | First/last item | `first(body('List_rows'))?['Id']` |
| `contains(collection, value)` | Membership check (works on strings, arrays, objects) | `contains(variables('approvedList'), triggerBody()?['Requester'])` |
| `intersection(c1, c2)` | Common items between two collections | Used to find records present in both a Dataverse export and an Excel import |
| `union(c1, c2)` | Combine, de-duplicated | Merging two notification recipient lists |
| `empty(collection)` | True if null/empty | `empty(body('List_rows')?['value'])` — **always check this before looping**, an empty-array Apply to each is harmless but a null value passed to it errors |
| `join(collection, delimiter)` | Combine array into a delimited string | `join(body('Select'), ', ')` for a summary email line |

## 5. Logical & comparison functions

| Function | Purpose | Example |
|---|---|---|
| `and()`, `or()`, `not()` | Boolean logic | `and(equals(variables('status'), 'New'), greater(variables('amount'), 1000))` |
| `equals()`, `greater()`, `less()`, `greaterOrEquals()`, `lessOrEquals()` | Comparisons | Used constantly in Condition actions' advanced mode |
| `if(condition, valueIfTrue, valueIfFalse)` | Inline conditional — avoids a full Condition action for simple cases | `if(equals(variables('type'), 'Express'), 'High', 'Normal')` |
| `coalesce(value1, value2, ...)` | First non-null value | `coalesce(triggerBody()?['MiddleName'], '')` — the standard null-safety pattern |

## 6. Conversion functions

| Function | Purpose | Example |
|---|---|---|
| `string()`, `int()`, `float()`, `bool()` | Type conversion | `int(triggerBody()?['QuantityText'])` |
| `array(value)` / `createArray(v1, v2, ...)` | Build arrays | `createArray('Manager', 'Finance')` for a multi-recipient list |
| `json(string)` | Parse a string as JSON | `json(body('HTTP'))?['data']` when a connector returns JSON as raw text |
| `xml(string)` | Parse a string as XML | Used against older SOAP-based integrations |
| `base64(string)` / `base64ToString()` | Encoding for file/attachment handling | Common when passing file content between connectors that expect base64 |
| `decodeUriComponent()` | Decode URL-encoded text | Cleaning up values pulled from query strings |

## 7. Date & time functions

| Function | Purpose | Example |
|---|---|---|
| `utcNow()` | Current UTC timestamp | `utcNow()` |
| `addDays()`, `addHours()`, `addMinutes()`, `addToTime()` | Date arithmetic | `addDays(utcNow(), 30)` for a 30-day expiry |
| `formatDateTime(timestamp, format)` | Format for display | `formatDateTime(utcNow(), 'dd-MMM-yyyy')` |
| `convertTimeZone()` / `convertFromUtc()` / `convertToUtc()` | Timezone-aware conversion | **Critical for recurrence triggers** (see Project 3's limitation note on UTC defaults) — `convertFromUtc(utcNow(), 'India Standard Time')` |
| `ticks(timestamp)` | Numeric tick value, useful for precise duration math | Comparing exact elapsed time between two timestamps |
| `dayOfWeek()`, `dayOfMonth()`, `dayOfYear()` | Extract date parts | Skipping weekend-only logic in a scheduled flow |

## 8. Math functions

| Function | Purpose | Example |
|---|---|---|
| `add()`, `sub()`, `mul()`, `div()`, `mod()` | Arithmetic | `mul(variables('unitPrice'), variables('quantity'))` |
| `min()`, `max()` | Bounds | `min(variables('requestedDays'), 30)` — capping a value |
| `rand(min, max)` | Random integer | Useful for jitter in retry-delay logic |

---

## 9. Practical recipes (combining the above)

**Null-safe dynamic content with a fallback default:**
```
coalesce(triggerBody()?['Department'], 'Unassigned')
```

**Building a dynamic OData filter query for a Dataverse "List rows" action** (filtering to records modified in the last 24 hours, referenced in Project 4):
```
concat('modifiedon ge ', formatDateTime(addHours(utcNow(), -24), 'yyyy-MM-ddTHH:mm:ssZ'))
```

**Checking an array isn't empty before looping** (avoids a wasted Apply to each and clarifies intent):
```
not(empty(body('Filter_array')))
```

**Extracting the failed action's error message inside a Catch scope** (Project 5's core pattern):
```
first(result('Try'))?['error']?['message']
```
— in practice you'll usually filter `result('Try')` down to just the item where `status` equals `Failed` first, then pull the message from that specific item.

**Building a simple HTML table row from a Select output** (Project 3's summary email):
```
concat('<tr><td>', item()?['OrderId'], '</td><td>', item()?['Amount'], '</td></tr>')
```
used inside a Select action's "Map" field, then joined together and wrapped in `<table>` tags in a Compose action before the email is sent.

**A safe, capped Do-until exit condition** (Project 3's limitation about runaway loops):
```
or(greaterOrEquals(variables('varProcessedCount'), length(body('Filter_array'))), greaterOrEquals(variables('varIterationCount'), 20))
```

---

## 10. Common mistakes to avoid

- **Forgetting the `?` null-safe operator** and having a flow break the first time a field is legitimately empty, weeks after it "worked fine" in testing with only fully-populated test data.
- **Using `equals()` on values of different types** (comparing a string `"5"` to a number `5`) — wrap with explicit `int()`/`string()` conversion when the source types aren't guaranteed to match.
- **Hardcoding a timezone offset manually** instead of using `convertTimeZone()`/`convertFromUtc()` — manual offset math breaks across daylight saving transitions.
- **Nesting expressions so deeply they become unreadable** — when an expression exceeds roughly one line of real complexity, break it into a Compose action instead so run history shows the intermediate value (the same debugging discipline covered in Project 3).

---

## What to do next
Apply this reference directly inside **[Project 3 — Data Operations & Control Flow](../projects/03-intermediate-data-ops-control-flow/README.md)**, where nearly every function category above gets used in one coherent scenario.
