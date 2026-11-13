# file-mcp-server

A standalone C++20 Model Context Protocol (MCP) server for file reading, metadata, hashing, searching, writing, replacement, truncation, moving, and deletion.

## Tools

read_file, read_file_base64, get_file_info, hash_file, search_in_file, write_file, replace_in_file, truncate_file, move_file, delete_file

## Dependencies

The MCP protocol, server, transport, process, JSON, and HTTP implementations are provided by [`pooriayousefi/mcpxx`](https://github.com/pooriayousefi/mcpxx), included as a pinned Git submodule at `external/mcpxx` (commit `c7753ec6b15eb15b6ecdfed82b93ace745ba0c6b`).

## Requirements

- C++20 compiler (Clang or GCC)
- POSIX threads
- Git with submodule support

## Clone and build

```sh
git clone --recurse-submodules https://github.com/pooriayousefi/file-mcp-server.git
cd file-mcp-server
make
```

If already cloned without submodules, run `git submodule update --init --recursive` first.

To compile directly:

```sh
mkdir -p bin
clang++ -std=c++20 -O2 -pthread -I external/mcpxx/include \\
  src/file_mcp_tool_server.cpp -o bin/file_mcp_tool_server
```

On Windows with MinGW, add `-lws2_32`.

## Run

The file server confines every tool path to a filesystem root. By default this is the working directory from which the server starts. Set `FILE_MCP_ROOT` to an explicit directory to change the boundary; relative paths are resolved beneath that root, and absolute paths are accepted only when they remain inside it. Existing symlinks are resolved for boundary checks, including symlinks in intermediate path components. Run the server with an appropriately restricted OS account: path checks do not eliminate filesystem race conditions if an untrusted local process can concurrently replace path components.

The default transport is stdio:

```sh
./bin/file_mcp_tool_server
```

For Streamable HTTP:

```sh
./bin/file_mcp_tool_server http 127.0.0.1 8093
```

The HTTP endpoint is `/mcp`. Bind to loopback unless suitable network controls are configured; HTTP transport is not an authentication boundary.

## License

Apache-2.0. See [LICENSE](LICENSE).
