# GetPEExports

A simple bash script to extract and list exported function names from Windows PE (Portable Executable) files (DLL and EXE).

## Description

GetPEExports uses the `readpe` tool to parse PE files and extract all exported function names, outputting them as a plain text list. This is useful for analyzing Windows binaries, understanding their exported APIs, or preparing function lists for further processing.

## Features

- Extracts all exported function names from PE files (DLL/EXE)
- Outputs clean, plain text list of function names
- Skips exports with empty names
- Includes error checking for missing dependencies and invalid files
- Works on Linux systems (useful for cross-platform development and analysis)

## Requirements

- **readpe**: PE file parser
- **awk**: Text processing tool
- **bash**: Bourne Again Shell

## Usage

```bash
./get_pe_exports.sh <path/to/binary.dll|.exe>
```

### Examples

Extract exports from a DLL:
```bash
./get_pe_exports.sh /path/to/library.dll
```

Save output to a file:
```bash
./get_pe_exports.sh library.dll > exports.txt
```

Count the number of exports:
```bash
./get_pe_exports.sh library.dll | wc -l
```

Search for specific functions:
```bash
./get_pe_exports.sh library.dll | grep "CreateFile"
```

## Output Format

The script outputs one function name per line:

```
FunctionName1
FunctionName2
FunctionName3
...
```

## Error Codes

- `1`: Invalid usage (no file specified)
- `2`: Specified file does not exist
- `3`: `awk` command not found
- `4`: `readpe` command not found

## How It Works

1. Validates that a file path was provided
2. Checks if the file exists
3. Verifies that required tools (`awk` and `readpe`) are installed
4. Runs `readpe -e` to extract export information
5. Parses the output using `awk` to extract function names
6. Filters out empty names and outputs the clean list

## Use Cases

- **Reverse Engineering**: Quickly identify exported APIs in Windows binaries
- **API Documentation**: Generate function lists for documentation
- **Dependency Analysis**: Understand what functions a DLL provides
- **Cross-Platform Development**: Analyze Windows binaries on Linux systems
- **Automation**: Integrate into build scripts or analysis pipelines

## License

MIT License - see [LICENSE](LICENSE) file for details.

Copyright (c) 2025 Sergey Podobry

