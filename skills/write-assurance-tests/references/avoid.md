# Patterns to avoid

These patterns are in public OpenSAFELY tests. Do not copy them.

## Expectation that the command ignores

`opensafely/pincer-measures` `analysis/ehrQL/test_measure_definition.py` sets `expected_columns` on a patient with `expected_in_population: False`. `assure` does not check those columns. The test can stay green when the column logic is wrong.

Put column expectations only on patients inside the population.

The same file uses an empty `expected_columns` for patients inside the population. That checks membership only. Use it only when membership is the behaviour under test.

## Comment that disagrees with the rows

`opensafely/pharmacy-first` `analysis/test_dataset_definition_tables.py` says the patient is 16 years old. The date of birth is 1 January 2000, and the expected age is 25.

Write the comment from the dates in the row.

## Empty patient row

`opensafely/open-pathology-sdr` uses `"patients": {}`. That is one patient row with a null date of birth. `date_of_birth` must be the first day of a month, and it cannot be null.

Give `patients` a valid date of birth.

## One blank event

A list that contains `{}` is one event with null columns. Use `[]` when the patient has no events.

## One patient for the whole dataset

`opensafely/raw_medicines_short_data_report` `analysis/test_dataset_definition.py` expects dozens of status counts, and most of them are zero.

Split the behaviours. A reader must see which row produces which expected value.

`opensafely/open-pathology-sdr` `analysis/test_dataset_definition.py` has one patient and no patient outside the population. Add the exclusion patients that the population rule needs.

## Hidden parameter branch

`opensafely/open-pathology` `analysis/test_dataset.py` selects patients from `args.test` and has no branch for an unknown value. An unknown value leaves the patient list unset.

Raise an error for an unknown parameter. Keep the negative-control patient in every selection.

## Code with no clinical meaning

`opensafely/pincer-measures` takes `Codelists.ORAL_NSAID.codes[0]`. A reader cannot see which drug the row represents, and a codelist edit can change the code.

Use a named constant and a comment, as in `opensafely/pharmacy-first-data-development` `analysis/test_dataset_definition_med_status_data_development.py`.

## Second copy of the query

Do not rewrite the dataset definition inside the test file. Import `dataset`.

Do not put `test_data` in the production dataset file. The pipeline file stays free of test patients.
