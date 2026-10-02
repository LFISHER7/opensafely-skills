# OpenSAFELY skills

> [!IMPORTANT]
> These are skills that I have found useful for my personal OpenSAFELY projects. They are not official skills, and OpenSAFELY does not endorse them. I have made them available in case other people find them useful. Review any outputs from these skills against the OpenSAFELY documentation. There is no guarantee that these skills match the latest guidance or processes.

Skills that guide AI coding agents through work on [OpenSAFELY](https://www.opensafely.org/) studies.

A study repository follows the [OpenSAFELY research template](https://github.com/opensafely/research-template).

## Installation

Install with [npx skills](https://github.com/vercel-labs/skills):

```bash
npx skills add https://github.com/LFISHER7/opensafely-skills
```

Install one skill only:

```bash
npx skills add https://github.com/LFISHER7/opensafely-skills --skill <skill-name>
```

Check for updates:

```bash
npx skills check
npx skills update
```

## Available skills

| Skill | What it does |
| --- | --- |
| [describe-outputs](skills/describe-outputs/SKILL.md) | Write `outputs.yaml` with one entry for every pipeline output. Each entry has a file name, a path, a description that follows the matching medical reporting guideline, and the disclosure controls applied to that output. |
