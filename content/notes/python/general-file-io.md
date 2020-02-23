---
title: File I/O and the CSV Module
linktitle: File I/O
toc: true
type: docs
date: "2019-11-27T00:00:00+01:00"
draft: false
menu:
  python:
    parent: Basic Concepts
    weight: 4

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 4
---

## File I/O

_It is not good having multiple files opened at the same time, so it is a good practice to open and work on files using the context manager._

```py
# Open file with context manager
with open(path, mode) as file:
    # Loop on all line of the file
    for line in file:
        pass
```

`mode` determine what can be done on the file after opening it. The most used are `r` (read), `w` (write), `a` (append) and `r+` (read-write).

{{% alert note %}}
If you open a file using `w` while the file already exists, the old contents will be deleted as soon as the file is opened!
{{% /alert %}}

- `file = open(path)` &rarr; **open the file** in the path and return a _file object._ [doc](https://docs.python.org/3/library/functions.html#open)
- `file.close()` &rarr; close the file.
- `file.read()` &rarr; return a string with all the content of the file.
- `file.readline()` &rarr; return a single line of the file (starting from the beginning). Successive calls will return the following line each time.
- `file.readlines()` &rarr; return a list with all the lines in the file. _Be careful to memory usage when reading large files._
- `file.write(str)` &rarr; write string in the file.

---
## `csv` Module

```py
import csv
```

When reading and writing on csv files, multiple conventions are possible. Before reading the content of a file, one can define a _Dialect_ class in which the attributes specifies all the conventions. This class can then be passed to the _reader_ function when accessing the file.

More on dialects in the resources.

### Reading CSV Files

When reading a file written on a different OS, problems can occur. This because on Unix the newline character is represented by the ASCII 10 (_line feed_), while in Windows is represented by the combination of ASCII 13 (_carriage return_) and ASCII 10 (i.e. 2 characters are used).

_For this reason, to avoid errors when reading files written on other OS, one should use the `U` (universal) flag while opening them._

```py
# Opening the file WITHOUT using the context manager
f = open(path)
csv_f = csv.reader(f) # create a csv reader object

# Opening file USING the context manager
with open(path, 'Ur') as csv_file:
    csv_reader = csv.reader(csv_file, delimiter=' ')

    # skip over the header line
    next(csv_reader)

    # each row is a list of the values that
    # were originally separater by commas
    for row in csv_reader:
	    value_1, value_2, …  = row # unpacking
```

### Write on CSV files

```py
data_list = [ [value_1, value_2, ...], [value_n, value_n+1, ...]]

with open(path, 'w') as csv_file:
	csv_writer = csv.writer(csv_file, delimiter='\t')
	csv_writer.writerows(data_list)
```

### Reading with Dictionary Reader

If more "human readable" files are needed, use:

```py
with open('data.csv','r') as csv_file:
    csv_reader = csv.DictReader(csv_file,delimiter=' ')

    for line in csv_reader:
        print(line)

with open(path, 'r') as csv_file:
	csv_reader = csv.DictReader(csv_file)

	for row in csv_reader:
		pass
```

- `csv.DictReader(csv_file)` &rarr; return a list of dictionaries (one dictionary for every row of the file). The keys are taken from the file header.

### Writing with Dictionary Writer

```py
with open(path, 'w') as csv_file:
	csv_writer = csv.DictWriter(csv_file, fieldnames=keys, delimiter='\t') 
	csv_writer.writeheader()
	csv_writer.writerows(list_of_dict)
```

---
## Resources

- `csv` module [documentation](https://docs.python.org/3/library/csv.html)
- Dialects and formatting parameters [documentation] for the `csv` module(https://docs.python.org/3/library/csv.html#dialects-and-formatting-parameters)
- Dialects functions [documentation](https://docs.python.org/3/library/csv.html#csv.register_dialect)