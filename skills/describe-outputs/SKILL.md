---
name: describe-outputs
description: >
  Write outputs.yaml for an OpenSAFELY study. The file lists every pipeline
  output with a file name, path, a description, and the disclosure controls 
  applied to that output. Use when the user asks for outputs.yaml, an output 
  catalogue, output descriptions, table or figure captions, or 
  disclosure-control text.
metadata:
  version: "0.1.0"
---

# Describe outputs

Write `outputs.yaml` at the root of the study repository, next to `project.yaml`.

The file has one entry for every output named in `project.yaml`. Each entry has a name, a path, a description of what the output shows, and the disclosure controls applied to it.

## 1. List the outputs

Read `project.yaml`. Add one entry for each file under `outputs` (only moderately sensitive files).

A path can contain `*`. Read the action code and list the files that the pattern writes.

Write one entry for each file when the files differ in population, measure, time period, or disclosure control. Use the concrete path. Add `pattern` with the wildcard from `project.yaml`.

Write one entry for the pattern when every file is the same kind of output and uses the same disclosure control. Keep the wildcard in `path`. The description states the rule that names each file.

If the code does not show the file list, ask the user. Do not guess the names.

Read the action that writes each file before you describe it. Use `analysis/`, including the dataset definition, for the population, the time period, the setting, and the data source. OpenSAFELY-TPP and OpenSAFELY-EMIS are different data sources. Use the source that the code imports.


## 2. Number the tables and figures

Give each table and each figure a number. Number the two series separately. Start each series at 1. Use the order in `project.yaml`, unless the user gives the order from the manuscript.

An image is a figure. An aggregated table is a table. A patient-level file is neither. Do not give a patient-level file a table number or a figure number. If you cannot tell the type, ask the user.

The `name` is `Table 1` or `Figure 2`.

The filename is the type, the number, an underscore, and a short title:

```text
table2_baseline_characteristics.csv
figure1_time_series_covid.png
```

Use lowercase words and underscores in the short title. Keep the directory and the file extension.

If the current filename does not match that pattern, rename it. Update the path in `project.yaml`, in the action command, and in the code that writes the file.

When the files in a wildcard differ, replace the wildcard with one numbered path for each file. When they do not differ, keep one number and put the `*` in the short title, for example `figure1_time_series_*.png`.

## 3. Write each description

A description must let a reader understand that output on its own.

The description checklist is in [references/reporting.md](references/reporting.md).

## 4. Write the disclosure controls

State the controls that the code applies to that file. The OpenSAFELY rules are in [references/disclosure.md](references/disclosure.md).

Read the code. If a required control is absent, say that it is absent. Do not claim that a count was redacted or rounded unless the code or the user confirms it.

## 5. Write the file

Use this shape:

```yaml
outputs:
  - name: Table 1
    path: output/table1_baseline_characteristics.csv
    description: >
      Baseline characteristics of adults registered with a general
      practice in England that contributes to OpenSAFELY-TPP. The
      registration date is from 1 March 2020 through 28 February 2021.
      The table shows the count and the percentage of people in each
      age group and sex category. TPP is the provider of the SystmOne
      primary care record.
    disclosure_controls: >
      The code replaces a count of 7 or fewer patients with [REDACTED],
      then rounds each remaining count to the nearest 5. Totals are the
      sums of the rounded counts. Percentages use the rounded counts.

  - name: Figure 1
    path: output/figures/figure1_time_series_covid.png
    description: >
      Monthly count of recorded COVID-19 among adults registered with a
      general practice in England that contributes to OpenSAFELY-TPP,
      from 1 March 2020 through 28 February 2021. COVID-19 is coronavirus
      disease 2019. TPP is the provider of the SystmOne primary care
      record. The figure shows one bar for each calendar month.
    disclosure_controls: >
      The code replaces a monthly count of 7 or fewer patients with
      [REDACTED], then rounds each remaining count to the nearest 5.
```

After you write the file, tell the user the number of entries, and any output whose control is missing from the code.
