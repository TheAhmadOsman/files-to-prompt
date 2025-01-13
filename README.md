# files-to-prompt

This is my enhanced, or `v2`, of [files-to-prompt](https://github.com/simonw/files-to-prompt/) by Simon Willison.

[![PyPI](https://img.shields.io/pypi/v/files-to-prompt.svg)](https://pypi.org/project/files-to-prompt-v2/)
[![Changelog](https://img.shields.io/github/v/release//TheAhmadOsman/files-to-prompt?include_prereleases&label=changelog)](https://github.com/TheAhmadOsman/files-to-prompt/releases)
[![Tests](https://github.com//TheAhmadOsman/files-to-prompt/actions/workflows/test.yml/badge.svg)](https://github.com//TheAhmadOsman/files-to-prompt/actions/workflows/test.yml)
[![License](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](https://github.com//TheAhmadOsman/files-to-prompt/blob/master/LICENSE)

Concatenate a directory full of files into a single prompt for use with LLMs

For background on this project, see [Building files-to-prompt entirely using Claude 3 Opus](https://simonwillison.net/2024/Apr/8/files-to-prompt/).

## Installation

Install this tool using `pip`:

```bash
pip install files-to-prompt
```

## Usage

To use `files-to-prompt`, provide the path to one or more files or directories you want to process:

```bash
files-to-prompt path/to/file_or_directory [path/to/another/file_or_directory ...]
```

This will output the contents of every file, with each file preceded by its relative path and separated by `---`.

### Options

- `-e/--extension <extension>`: Only include files with the specified extension. Can be used multiple times.

  ```bash
  files-to-prompt path/to/directory -e txt -e md
  ```

- `--include-hidden`: Include files and folders starting with `.` (hidden files and directories).

  ```bash
  files-to-prompt path/to/directory --include-hidden
  ```

- `--ignore-gitignore`: Ignore `.gitignore` files and include all files.

  ```bash
  files-to-prompt path/to/directory --ignore-gitignore
  ```

- `--ignore <pattern>`: Specify one or more patterns to ignore. Can be used multiple times.

  ```bash
  files-to-prompt path/to/directory --ignore "*.log" --ignore "temp*"
  ```

- `--ignore-paths <path>`: Specify one or more paths to ignore. Can be used multiple times.

  ```bash
  files-to-prompt path/to/directory --ignore-paths path/to/ignore --ignore-paths path/to/another_ignore
  ```

- `--ignore-default`: Ignore default patterns: `.github/workflows`, `LICENSE`, `.gitignore`, CI/CD files, `env/venv`, `node_modules`, `pyproject.toml`, `uv.lock`, `requirements.txt`, and other common development artifacts.

  ```bash
  files-to-prompt path/to/directory --ignore-default
  ```

- `--cxml`: Output in Claude XML format.

  ```bash
  files-to-prompt path/to/directory --cxml
  ```

- `-o/--output <file>`: Write the output to a file instead of printing it to the console.

  ```bash
  files-to-prompt path/to/directory -o output.txt
  ```

### Examples

#### Basic Usage

Suppose you have a directory structure like this:

```
my_directory/
├── file1.txt
├── file2.txt
├── .hidden_file.txt
├── temp.log
└── subdirectory/
    └── file3.txt
```

Running `files-to-prompt my_directory` will output:

```
my_directory/file1.txt
---
Contents of file1.txt
---
my_directory/file2.txt
---
Contents of file2.txt
---
my_directory/subdirectory/file3.txt
---
Contents of file3.txt
---
```

#### Including Hidden Files

If you run `files-to-prompt my_directory --include-hidden`, the output will also include `.hidden_file.txt`:

```
my_directory/.hidden_file.txt
---
Contents of .hidden_file.txt
---
my_directory/file1.txt
---
Contents of file1.txt
---
my_directory/file2.txt
---
Contents of file2.txt
---
my_directory/subdirectory/file3.txt
---
Contents of file3.txt
---
```

#### Ignoring Files with Patterns

If you run `files-to-prompt my_directory --ignore "*.log"`, the output will exclude `temp.log`:

```
my_directory/file1.txt
---
Contents of file1.txt
---
my_directory/file2.txt
---
Contents of file2.txt
---
my_directory/subdirectory/file3.txt
---
Contents of file3.txt
---
```

#### Ignoring Specific Paths

If you run `files-to-prompt my_directory --ignore-paths "my_directory/subdirectory"`, the output will exclude the `subdirectory`:

```
my_directory/file1.txt
---
Contents of file1.txt
---
my_directory/file2.txt
---
Contents of file2.txt
---
```

#### Ignoring Default Patterns

If you run `files-to-prompt my_directory --ignore-default`, the output will exclude common development artifacts:

```
my_directory/file1.txt
---
Contents of file1.txt
---
my_directory/file2.txt
---
Contents of file2.txt
---
```

#### Claude XML Output

To structure the output in Claude XML format, use the optional `--cxml` flag, which will produce output like this:

```xml
<documents>
<document index="1">
<source>my_directory/file1.txt</source>
<document_content>
Contents of file1.txt
</document_content>
</document>
<document index="2">
<source>my_directory/file2.txt</source>
<document_content>
Contents of file2.txt
</document_content>
</document>
<document index="3">
<source>my_directory/subdirectory/file3.txt</source>
<document_content>
Contents of file3.txt
</document_content>
</document>
</documents>
```

#### Writing Output to a File

To write the output to a file instead of printing it to the console, use the `-o` or `--output` option:

```bash
files-to-prompt my_directory -o output.txt
```

## Development

To contribute to this tool, first check out the code. Then create a new virtual environment:

```bash
cd files-to-prompt
python -m venv venv
source venv/bin/activate
```

Now install the dependencies and test dependencies:

```bash
pip install -e '.[test]'
```

To run the tests:

```bash
pytest
```

## Additional Notes

- The tool will skip binary files and output a warning message if it encounters a `UnicodeDecodeError`.
- The `--ignore-paths` option can be used to exclude specific directories or files from being processed.
- The `--ignore-default` option excludes a predefined list of common development artifacts.
- The `--cxml` option formats the output in a way that is suitable for Claude's long context window.
- The `--output` option allows you to write the concatenated output to a file.
