Execute one or more queries on a DataFrame using the `xquery()` accessor. A query can be:
- A query string
- A boolean index
- A callable that evaluates to True or False for each row

A query can also contain a filter for the columns to be returned. Some statistics about the intermediate results can be inspected through the `history` attribute.

### Query language
A query statement will be compiled according to registered translators. The following translations will be applied:

translator      | input                   | output
----------------|-------------------------|-----------------------
multiline       | multiline query         | single line
equals          | '='                     | `==`
is_na           | 'is na/null'            | `.isna()`
is_not_na       | 'is not na/null'        | `.notna()`
pattern_match   | "contains 'x'"          | `.str.contains('x')`
pattern_match   | "matches 'x'"           | `.str.match('x')`
pattern_match   | "full matches 'x'"      | `.str.fullmatch('x')`
pattern_match   | "starts with 'x'"       | `.str.startswith('x')`
pattern_match   | "ends with 'x'"         | `.str.endswith('x')`
string_test     | "is alphanumeric"       | `.str.isalnum()`
string_test     | "is alphabetic"         | `.str.isalpha()`
string_test     | "is numeric"            | `.str.isnumeric()`
string_test     | "is digit"              | `.str.isdigit()`
string_test     | "is decimal"            | `.str.isdecimal()`
string_test     | "is lowercase"          | `.str.islower()`
string_test     | "is uppercase"          | `.str.isupper()`
string_test     | "is titlecase"          | `.str.istitle()`
string_test     | "is space"              | `.str.isspace()`
is_duplicated   | "is duplicated"         | `.duplicated(False)`
is_duplicated   | "is first duplicated"   | `.duplicated('first')`
is_duplicated   | "is last duplicated"    | `.duplicated('last')`
date            | 'xxxx-xx-xx'            | "'xxxx-xx-xx'"

Pattern matching and string testing can also be negated using "not".

### Queries in parallel
Queries can be run sequentially or in parallel. The latter meaning that the result of each query will be concatenated (and afterwards deduplicated).

#### Parameters
- `*args`: Queries to apply. Can be a query string, a boolean index, or a callable that returns True or False for each column.
- `columns` (list[str]|BooleanIndex|Callable, optional): Columns to select. Can be a list of column names, a boolean index, or a callable that returns True or False for each column. If set to None returns all columns. Default is None.
- `in_parallel` (bool): Execute queries in parallel or sequentially. Default False.
- `store_keys` (bool): Store queries as keys in resulting df if queries are executed in parallel. Default False.
- `**kwargs`: Additional arguments are passed to the pandas query engine.

#### Returns
`pd.DataFrame`: Resulting DataFrame.
