# Data Literacy Field Notes & Terminal Log

**The scenario:** A climate-and-energy nonprofit wants to use this
country-level energy and emissions dataset to help decide where to direct
community-protection and climate-support efforts — but the file just
landed with zero documentation. Before anyone can trust a decision built
on top of it, you need to figure out what's actually in it — what each
column really holds, and anything that looks off. That's what this whole
file walks through: the real, everyday first pass any analyst runs on an
unfamiliar file, and the real answers you need before this data is safe
to act on.

Dataset: `../data/co2_emissions_2022.csv` (see `../data/SOURCE.md`). Each
section below describes what you're trying to find out — **figuring out
which command (and which flags) actually gets you there is part of what
this project tests**, so the exact command isn't given. Where a command
belongs, you'll see a `# TODO:` comment describing the goal instead of a
ready-to-run command; replace the comment with the real command you used.
Paste the **real output** you got underneath, then write your answer based
on what you actually saw, not what you'd expect. Work through this in
order; open the raw file before anything else.

Run all commands from the repo root — the top-level folder you land in
after cloning *your own* copy of this repo (`git clone ...` then `cd
your-repo-name`, see `README.md` Step 3; not this template) — or `cd` into
`data/` first and drop the `data/` prefix — your choice, just be
consistent.

**This one file is your whole record** — the commands you ran, their real
output, and your written answers, all together in the order you actually
did the work. There's no separate log file to keep in sync with this one;
whenever you write a command down anywhere in this file, use a fenced code
block tagged `bash`, like this:

````
```bash
git status
```
````

Three backticks, the word `bash`, your command, three backticks to close —
it renders with syntax highlighting and makes the command copy-pasteable.

## Setup log

Before jumping into the dataset, log the commands you actually ran to set
this repo up (see `README.md`) — `git init`/clone, creating your
`.gitignore`, your first commit. Add to this as you go, don't
reconstruct it from memory at the end.

```bash
git init/clone
touch .gitignore
```

## 1. First look at the raw file

Before trusting anything about this file's structure, you want a quick,
safe peek at it — not the whole 254-row file, just the **first 5 lines**,
enough to see the shape of it without scrolling past everything.

```bash
head -5 co2_emissions_2022.csv  
```

**Output:**
The the header appeared with the first 5 lines. 

**What I notice:** _Africa is documented twice. Africa is mentioned, but is not a county. there are certain values within row 3 and 4 missing which also coul be pertaining to Africa as a continent._

## 2. Column names

Next you want to see just the column names by themselves — one per line is
a lot easier to scan than one long comma-separated row.

```bash
head -n 1 co2_emissions_2022.csv
head  -n 1 co2_emissions_2022.csv | tr ',' '\n'
```

**Output:**
1st command gave only the header of the file from left to right. While the second command gave the header of the file in a column top to bottom.

## 3. Column-by-column data type table

For each column, you want to preview real values — **the first 6 values**
of just that one column — without scrolling through every field on every
row. Column numbers for reference: `country`=1, `iso_code`=2, `year`=3,
`population`=4, `gdp`=5, `co2`=6.

```bash 
cut -d ',' -f 1 co2_emissions_2022.csv | head -n 6 
a command that prints the first 6 values from a single column
cut -d ',' -f 1 co2_emissions_2022.csv | head -n 6
cut -d ',' -f 2 co2_emissions_2022.csv | head -n 6 
cut -d ',' -f 3 co2_emissions_2022.csv | head -n 6
cut -d ',' -f 4 co2_emissions_2022.csv | head -n 6
cut -d ',' -f 5 co2_emissions_2022.csv | head -n 6
cut -d ',' -f 6 co2_emissions_2022.csv | head -n 6 
```

Fill in the table from what you actually saw — not a guess from the column
name alone:

| Country | String | this is text to defne a country |
|---|---|---|
| Iso_code | String | this is a text code to define a country through a registed code  |
| Year | Date | used to label specific countries yearly output in the data|
| Population | Integer | there cant be a fraction of a person accounted for in the data  |
|Gdp| Float | recorded as a continuous figure|
| Co2| Float | recorded as a contiunous figure |

**In your own words:** The difference between a lis/array is a list is just a bunch of text on a page that may have a structure to it but is not stuctured for someone to understand. An Array is a list with a set structure.

```bash
_List Array is a table with a fixed size or slots; where each indivisual section means something or has a purpose for one specific thype of data being there.
Table is a set of data structured by lines and colums with an infinitley that may not be as particular as an array.
```

## 4. Structured or unstructured?

State whether this dataset is structured or unstructured, and why.

```bash
This dataset is structured but there ia alsot of missing data or missplaced data; this is a structured dataset becasue there is the same data type within each colomn.
```

## 5. Before you trust this data

Your first look (see section 1) turned up a row with a suspiciously
**blank** `iso_code`. Before you build anything on this data, you want to
know exactly how many of the 254 rows share that blank — is it a one-off,
or a real pattern worth flagging before anyone draws conclusions from it?
You may need more than one command for this — piping (`|`) feeds one
command's output into the next as input.

```bash
cut -d ',' -f 2 co2_emissions_2022.csv | tail -n +2 | grep -c '^$'
```

**Output:**
_36 rows have a blank iso code_

```bash
cut -d ',' -f 2 data.csv | tail -n +2 | grep -c '^$'
```

**Output:**
89 rows have a missing gdp

List **at least 2** specific things you'd want to know about how this
dataset was collected, before trusting any conclusion drawn from it — ground
at least one in the counts you just found. 

**My 2 concerns:**
1. _7 values are missing from Co2
25 entries are missing from population
the rest (country,year) have NO missing values._
2. _What values are imputted as country but aren't?_
3._What is the signifignace of the continents?_

## 6. Data-quality question for the source

Take the **most specific and checkable** of the two concerns above, and
phrase it as a real question you'd send to the data's source/maintainer. A
good test: could they actually answer it with a concrete yes/no or fact —
not "is this data good?"

_What values are imputted as country but aren't? And why?_

## 7. Something you're actually curious about

Different question from #6 — that one was about checking the data's
trustworthiness. This one's about you: what's something you're genuinely
curious to explore using this data? Pick two specific countries you're
curious to compare, and look up their full rows.

```bash
grep "Mexico" co2_emissions_2022.csv   
grep "Germany" co2_emissions_2022.csv   
```

**Output:**
_Row of each column_

**My question:** _Does Population affect GDP or does a Ppoulation Affect Co2 or Is GDP afeccted by co2 and vice versa/_

## 8. AI-literacy reflection

Name **2 concrete risks** of trusting an AI assistant's output about this
dataset (or any data question) without checking it yourself. Then state
**one specific personal rule** for when you'll double-check an AI-provided
fact before relying on it professionally — specific enough that you could
actually follow it, not "I'll be careful."

**Risks:**
1. _AI Hallucination_
2. _AI may not have full context_

**My rule:** _always use AI as a tool to teach you rather than tell you and always give it context._

## 9. Something that went wrong (and how you figured it out)

Somewhere in this project you're going to run a command that either throws
an error or gives you output that isn't what you expected — everyone hits
this at least once. Paste the actual command and what actually happened
(not a paraphrase), then explain what that told you and how it led to the
fix — not "I googled it and it worked."

```bash
cut -d ',' -f 6 co2_emissions_2022.csv
```

**What happened:**
_TODO: the actual error message, or the unexpected output you got_

**What it told me and how I fixed it:** _TODO_
