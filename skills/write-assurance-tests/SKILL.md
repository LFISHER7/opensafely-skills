---
name: write-assurance-tests
description: >
  Write ehrQL assurance tests for an OpenSAFELY dataset definition. The test
  file defines test_data and imports dataset. Use when the user asks for
  assurance tests, an assure command, test_data, expected_in_population, or
  a test of a dataset definition.
metadata:
  version: "0.1.0"
---

# Write assurance tests

Write one test file for each dataset definition that a `generate-dataset` action runs.

The ehrQL `assure` command checks two things. The test rows must meet the table constraints. The dataset that those rows produce must match `expected_in_population` and `expected_columns`.

The command does not check a measures output file. Test the dataset columns that the measure uses as the numerator and the denominator.

Official guide: [How to test your dataset definition](https://docs.opensafely.org/ehrql/how-to/test-dataset-definition/).

Reference examples: [opensafely/dataset-definition-testing-examples](https://github.com/opensafely/dataset-definition-testing-examples).

## 1. Read the dataset definition

Read the file named in the `generate-dataset` action.

List each population rule. List each output column and the rule that sets it. Read the codelist file when a column uses a code.

If a date cut-off, an age cut-off, or a clinical rule is not in the code or in the user request, ask the user. Do not invent the expected value.

## 2. Create the test file

Put the test file next to the dataset definition.

Name it `test_` plus the dataset file name. Example: `analysis/dataset_definition.py` has `analysis/test_dataset_definition.py`.

The test module must bind the name `dataset` and the name `test_data`. Import `dataset` from the module that the pipeline runs. Do not define a second copy of the query in the test file.

When the dataset file calls `get_parameter`, or builds the dataset inside a function, test the small file that calls that function with fixed arguments. [Example 3](https://github.com/opensafely/dataset-definition-testing-examples/tree/main/example-003-parameterised) uses this shape. Import `dataset` from that small file.

When the pipeline passes arguments after `--`, the assure command needs the same arguments. Write the full command in a comment at the top of the test file.

## 3. Choose the patients

Use one patient for one behaviour. The patient list is in [references/patients.md](references/patients.md).

## 4. Write `test_data`

The dictionary rules are in [references/contract.md](references/contract.md).

The patterns to avoid are in [references/avoid.md](references/avoid.md).

## 5. Run the tests

From the repository root:

```text
opensafely exec ehrql:v1 assure analysis/test_dataset_definition.py
```

A pass prints both of these lines:

```text
Validate test data: All OK!
Validate results: All OK!
```

A failure is on stderr. The process exits with code 13.

Fix constraint errors before result errors. A result check does not replace an invalid row.

You can also pass the test file to the dataset action:

```text
ehrql:v1 generate-dataset analysis/dataset_definition.py
  --test-data-file analysis/test_dataset_definition.py
  --output output/dataset.arrow
```

Add that flag when the user wants the tests to run with the pipeline. Do not add it unless the user asks, or the study already uses it.

After you write the file, tell the user the path, the number of patients, and the behaviour that each patient checks. If you cannot run `assure`, say that the tests were not run.
