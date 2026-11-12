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
