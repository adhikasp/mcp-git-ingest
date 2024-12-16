# MCP Git Ingest

A Model Context Protocol (MCP) server that helps read GitHub repository structure and important files.

Inspired by [gitingest](https://gitingest.com/).

<a href="https://glama.ai/mcp/servers/un2zatig9e"><img width="380" height="200" src="https://glama.ai/mcp/servers/un2zatig9e/badge" /></a>

## Configuration

```json
{
    "mcpServers": {
        "mcp-git-ingest": {
            "command": "uvx",
            "args": ["--from", "git+https://github.com/adhikasp/mcp-git-ingest", "mcp-git-ingest"],
        }
    }
}
```

## Usage

Using [mcp-client-cli](https://github.com/adhikasp/mcp-client-cli):

```
$ llm read https://github.com/adhikasp/mcp-git-ingest and determine how the code technically works

I'll help you understand the technical details of the mcp-git-ingest repository. I'll first fetch the directory structure and then read the important files to analyze how the code works.

Tool Calls:
  github_directory_structure
  Args:
    repo_url: https://github.com/adhikasp/mcp-git-ingest

Now, I'll read the important files to understand the technical implementation:

Tool Calls:
  github_read_important_files
  Args:
    repo_url: https://github.com/adhikasp/mcp-git-ingest
    file_paths: ['README.md', 'src/mcp_git_ingest/main.py', 'pyproject.toml']

Let me break down how this code technically works:

1. Purpose:
The `mcp-git-ingest` is a Model Context Protocol (MCP) server designed to help read GitHub repository structures and important files. It provides two main tools:
- `github_directory_structure`: Returns a tree-like representation of a repository's directory structure
- `github_read_important_files`: Reads and returns the contents of specified files in a repository

2. Technical Implementation:

a. Dependencies:
- Uses `fastmcp` for creating an MCP server
- Uses `gitpython` for Git repository operations
- Requires Python 3.8+

b. Key Functions:

`clone_repo(repo_url: str) -> str`:
- Creates a deterministic temporary directory based on the repository URL's hash
- Checks if the repository is already cloned
- If not, clones the repository
- Handles error cases and cleanup
- Returns the path to the cloned repository

`get_directory_tree(path: str, prefix: str = "") -> str`:
- Recursively generates a tree-like directory structure
- Skips `.git` directories
- Uses Unicode box-drawing characters to create a visual tree representation
- Handles sorting of entries

`github_directory_structure(repo_url: str) -> str`:
- Clones the repository
- Generates directory tree
- Cleans up the temporary repository after processing
- Returns the tree structure or an error message

`github_read_important_files(repo_url: str, file_paths: List[str]) -> dict[str, str]`:
- Clones the repository
- Reads specified files
- Returns a dictionary mapping file paths to their contents
- Handles file reading errors
- Cleans up the temporary repository

3. Error Handling:
- Uses try-except blocks to handle repository cloning, file reading errors
- Ensures temporary directories are always cleaned up using `finally` blocks
- Returns descriptive error messages

4. Performance Optimizations:
- Uses a hash-based temporary directory naming to potentially reuse cloned repositories
- Checks for existing repositories before cloning
- Implements cleanup to prevent accumulation of temporary files

5. Unique Features:
- Deterministic temporary directory creation
- Unicode tree representation
- Flexible file reading with error handling

6. Execution:
- Can be run as a CLI tool via `mcp-git-ingest`
- Configured through `pyproject.toml`
- Depends on `fastmcp` for MCP server functionality

The code is a robust, flexible tool for programmatically exploring and reading GitHub repositories, with a focus on error handling and clean implementation.

Would you like me to elaborate on any specific aspect of the implementation?
```
