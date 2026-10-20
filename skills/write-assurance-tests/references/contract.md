# `test_data` contract

`assure` loads the test module and reads `dataset` and `test_data`. Both names are required.

`test_data` is a dictionary. Each key is a patient id. Use integers. Start at 1. Keep the ids stable.

Each value is a dictionary. It has one key for every table that the query uses, plus the expectation keys.

## Tables

The table key is the table name in the query, such as `patients`, `medications`, or `medications_raw`.

A one-row table is a dictionary. `patients` is a one-row table. A list of one dictionary is also valid. A list of two dictionaries fails.

An event table is a list of dictionaries. One dictionary is one row. An empty list means no rows.

An empty dictionary is one row, and every omitted column is null. Use it only when null is valid for that table. `date_of_birth` cannot be null, and it is the first day of a month.

Do not use `[{}]` as an empty event list. That value is one event row with null columns.

Set only the columns that the query reads. A missing column is null when the schema allows null.

Use `datetime.date` for dates. Use `None` for null. A string date in `expected_columns` does not match a date result.

## Expectations

Set `expected_in_population` on every patient. The default is `True` when the key is absent, so a missing key hides an exclusion error.

When `expected_in_population` is `True`, `expected_columns` is required. It can list the columns that this patient checks. It does not need every dataset column. An empty dictionary checks membership only. Use an empty dictionary only for a patient whose only question is membership.

When `expected_in_population` is `False`, omit `expected_columns`. The command does not read `expected_columns` for a patient who is outside the population.

A key in `expected_columns` that is not a dataset column fails the test.

The check is equality. `1` is not `True`. A count column expects an integer. `exists_for_patient()` expects `True` or `False`. A category string must match the dataset string, including capital letters.

## Codes

Put each code in a named constant at the top of the test file. The comment states the clinical meaning and whether the code is in the codelist or outside it.

Use a code that is in the study codelist. Read the codelist CSV. Do not use the first code of a codelist object with no comment.

## Shared rows

When many patients share a registration or a date of birth, bind that dictionary once and reuse it. Change one fact in the patient that tests the next behaviour.

## Comments

Put one comment above each patient. The comment states the behaviour in clinical words, such as "Died before the index date. Outside the population."

The dates in the rows must agree with the comment. State the index date in a comment at the top of the file.

## Parameter filter

When one dataset file serves several codelists, you can keep every patient in one dictionary and copy the relevant patients into `test_data`. Include the negative-control patient in every run. If the parameter value is unknown, raise an error. Do not leave `test_data` unset.
