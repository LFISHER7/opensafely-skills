# Patients to write

Read the population rules and the column rules first. Add a patient only when the code contains that rule.

Give each patient one behaviour. A later patient can repeat the inclusion facts and change one fact.

## Population

Add these patients when the dataset definition has the matching rule:

- The patient meets every inclusion rule.
- The patient fails one inclusion rule. Use a separate patient for each rule: age, sex, death, registration, and any clinical inclusion.
- The event that would qualify the patient is after the index date, or on the cut-off when the comparison excludes that day.
- The patient has no rows in an event table that the query uses.

For a registration rule, include a patient with no registration, a patient who leaves before the index date, and a patient who is registered on the index date.

For a death rule, include a patient who dies before the index date and a patient who dies after it.

## Columns

For each output column, add the smallest set of patients that can show a wrong query:

- No matching code. A boolean column is `False`. A date column is `None`. A count column is `0`.
- One matching event before the index date, or inside the time window.
- One matching event after the index date, or outside the time window.
- One matching event on the boundary day. Use the comparison in the code (`<`, `<=`, `is_before`, `is_on_or_before`).
- Two events when the column keeps the first or the last. The expected date is the date the code keeps.
- A value equal to a numeric cut-off, a value on the included side, and a value on the excluded side.
- A code that is not in the codelist, when a similar code could be selected by mistake.

When a column combines two facts, test each fact alone and then both facts together.

When a column maps categories, test one code from each category that the study uses, and a patient with no code. The expected missing label must match the code, such as `"Missing"` or `"missing"`.

## Measures

Test the numerator column and the denominator column on the dataset. Do not expect a ratio, a rounded count, or a measures CSV row. `assure` does not build the measures output.

## Stop

Do not add a patient that repeats a behaviour you already test.

Do not assert a long list of zeros for columns that the patient does not exercise. Assert the columns that the rows change. Assert one explicit zero when the absence of another code is the behaviour under test.
