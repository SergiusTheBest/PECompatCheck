# GetPEExports

Simple bash scripts to extract and list exported and imported function names from Windows PE (Portable Executable) files (DLL and EXE).

## Description

This repository contains two complementary tools for analyzing Windows PE files:
- **get_pe_exports.sh**: Extracts exported function names from PE files
- **get_pe_imports.sh**: Extracts imported function names (with their source DLL) from PE files

Both tools use the `readpe` utility to parse PE files and output clean, plain text lists. This is useful for analyzing Windows binaries, understanding their APIs, or preparing function lists for further processing.

## Features

### get_pe_exports.sh
- Extracts all exported function names from PE files (DLL/EXE)
- Outputs clean, plain text list of function names
- Skips exports with empty names
- Includes error checking for missing dependencies and invalid files

### get_pe_imports.sh
- Extracts all imported function names from PE files (DLL/EXE)
- Outputs two-column format: source DLL/module name and imported function name
- Skips imports with empty names
- Includes error checking for missing dependencies and invalid files

### General
- Works on Linux systems (useful for cross-platform development and analysis)
- Easy integration into automation pipelines

## Requirements

- **readpe**: PE file parser
- **awk**: Text processing tool
- **bash**: Bourne Again Shell

## Usage

### get_pe_exports.sh

Extract exported function names:

```bash
./get_pe_exports.sh <path/to/binary.dll|.exe>
```

#### Examples

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

### get_pe_imports.sh

Extract imported function names with their source DLL:

```bash
./get_pe_imports.sh <path/to/binary.dll|.exe>
```

#### Examples

Extract imports from an EXE:
```bash
./get_pe_imports.sh /path/to/program.exe
```

Save output to a file:
```bash
./get_pe_imports.sh program.exe > imports.txt
```

Filter imports from a specific DLL:
```bash
./get_pe_imports.sh program.exe | grep "^kernel32.dll"
```

Count imports from each DLL:
```bash
./get_pe_imports.sh program.exe | awk '{print $1}' | sort | uniq -c
```

Get only function names (second column):
```bash
./get_pe_imports.sh program.exe | awk '{print $2}'
```

## Output Format

### get_pe_exports.sh

The script outputs one function name per line:

```
FunctionName1
FunctionName2
FunctionName3
...
```

### get_pe_imports.sh

The script outputs two tab-separated columns: source DLL name and imported function name:

```
kernel32.dll	CreateFileA
kernel32.dll	ReadFile
kernel32.dll	WriteFile
user32.dll	MessageBoxA
...
```

## Error Codes

Both scripts use the same error codes:

- `1`: Invalid usage (no file specified)
- `2`: Specified file does not exist
- `3`: `awk` command not found
- `4`: `readpe` command not found

## How It Works

### get_pe_exports.sh

1. Validates that a file path was provided
2. Checks if the file exists
3. Verifies that required tools (`awk` and `readpe`) are installed
4. Runs `readpe -e` to extract export information
5. Parses the output using `awk` to extract function names
6. Filters out empty names and outputs the clean list

### get_pe_imports.sh

1. Validates that a file path was provided
2. Checks if the file exists
3. Verifies that required tools (`awk` and `readpe`) are installed
4. Runs `readpe -i` to extract import information
5. Parses the output using `awk` to extract DLL names and imported function names
6. Filters out empty names and outputs the two-column list

## Use Cases

- **Reverse Engineering**: Quickly identify exported and imported APIs in Windows binaries
- **API Documentation**: Generate function lists for documentation
- **Dependency Analysis**: 
  - Understand what functions a DLL provides (exports)
  - Discover what external functions a binary depends on (imports)
- **Security Analysis**: Identify which system APIs a binary uses
- **Cross-Platform Development**: Analyze Windows binaries on Linux systems
- **Automation**: Integrate into build scripts or analysis pipelines
- **Library Comparison**: Compare exported interfaces between different versions of DLLs

## License

MIT License - see [LICENSE](LICENSE) file for details.

Copyright (c) 2025 Sergey Podobry

