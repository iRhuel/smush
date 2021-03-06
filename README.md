# _smushr_

A command line tool to aggregate discrete data points across json files into a single readable result. It preserves 
structure of nested objects and arrays of indeterminant depth, allowing you to conveniently view all valid values for 
all keys at all levels. This would be useful for large sets of similar or repetitive .json files.

smushr searches target directories and optionally all subdirectories for .json files. It then creates a result object
with arrays under matching keys. It inserts all non-repeat data into its result key-array. It compares objects for key
parity and recurses on matching objects to preserve nested structure. Lastly, it writes the resultant object to a file
`smushed.json` (or optionally a file with custom name), in the directory where the command was executed.

## Getting Started

- install with `npm install -g smushr`

## Usage

- open a terminal
- in terminal, navigate to location with .json files to be combined
- run `smush [paths...]`, where `[paths...]` is a list of relative paths to the target folders from the current directory, separated by spaces.
- use the `-o` or `--output` flag to specify the output file name ( e.g. running `smush . -o test` will produce `./test.json` )
- include the `-r` or `--recursive` flag to also include subdirectories for parsing.

## Example

###### ./A.json
```$xslt
{
    "name": "Henry",
    "height": 167,
    "props": {
        "tasks": 
            [
                { "name": "doStuff" },
                { "id": 1 }
            ],
        "roles": "admin"
    }
}
```

###### ./B.json

```$xslt
{
    "name": "Max",
    "weight": 120,
    "props": {
        "tasks": 
            [
                { "name": "doDifferentStuff" },
                { "desc": "things need doing" }
            ],
        "roles": "user"
    }
}
```

###### ./smushed.json

```$xslt
{
    "name": ["Henry", "Max"],
    "height": [167],
    "weight": [120],
    "props": {
        "tasks": 
            [
                { "name": ["doStuff", "doDifferentStuff"] },
                { "id": [1] },
                { "desc": ["things need doing"] }
            ],
        "roles": ["admin", "user"]
    }
}
```