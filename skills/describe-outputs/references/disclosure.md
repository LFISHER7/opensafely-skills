# Disclosure controls

Use this file when you write `disclosure_controls` for an entry in `outputs.yaml`.

The source is the OpenSAFELY page [Applying statistical disclosure control](https://docs.opensafely.org/outputs/sdc/). Output checkers also use the context list on [Requesting release of research outputs](https://docs.opensafely.org/outputs/requesting-file-release/). If a study needs a rule that is not summarised here, follow those pages.

Describe the controls that the code applies. If the code does not apply a required control, write that the control is absent.

## Patient-level files

A patient-level file is `highly_sensitive`. It is not for release from the secure environment. Do not write descriptions and controls for these files.

## Aggregated files

For a `moderately_sensitive` file that contains counts:

1. Redact a count of 7 or fewer patients.
2. Round each remaining count to the nearest 5.
3. Recalculate a total from the rounded values, so a reader cannot recover a redacted cell from the total.
4. Calculate a percentage, a rate, or another derived statistic from the rounded counts.

Redact a summary, a minimum, a maximum, or a rate when it describes 7 or fewer patients. Redact a figure whose underlying values describe 7 or fewer patients. The underlying counts for a figure are a separate output when they are requested for release.

A count of zero may remain, except where the zero shows that every person or no person in a group has the attribute. Treat that cell as disclosive.

A model coefficient, a test statistic, or a goodness-of-fit measure can remain when it is a genuine analysis and the group size is large enough. Give the population size and the degrees of freedom in the description for a regression output. Do not copy those numbers into `outputs.yaml`, but say that the output file contains them.

## Midpoint 6 rounding

Use this only when the code uses midpoint 6 rounding, for example for a rate where zero must stay zero. Say that the code uses midpoint 6 rounding. A column that uses it has the suffix `_midpoint6`. A value derived from those rounded values has the suffix `_midpoint6_derived`.

## Relationships between outputs

Name another output when the two files describe the same population and could be combined to recover a small count. Say that in `disclosure_controls`.

## Organisations and regions

Also follow the OpenSAFELY permitted study results policy linked from the statistical disclosure control page when the output describes an organisation or a region.
