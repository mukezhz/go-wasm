# Compile Go code to Wasm

```bash
GOOS=js GOARCH=wasm go build -o main.wasm
```

That will build the package and produce an executable WebAssembly module file named main.wasm. 

The .wasm file extension will make it easier to serve it over HTTP with the correct Content-Type header later on.

**NOTE:** you can only compile main packages

```
Otherwise, you will get an object file that cannot be run in WebAssembly. If you have a package that you want to be able to use with WebAssembly, convert it to a main package and build a binary.
```

### Copy the JavaScript support file:
```bash
cp "$(go env GOROOT)/misc/wasm/wasm_exec.js" .
```
```
To execute main.wasm in a browser, we’ll also need a JavaScript support file, and a HTML page to connect everything together.
```

**Note:** The same major Go version of the compiler and wasm_exec.js support file must be used together. That is, if main.wasm file is compiled using Go version 1.N, the corresponding wasm_exec.js file must also be copied from Go version 1.N. Other combinations are not supported.

**NOTE:** If your browser doesn’t yet support WebAssembly.instantiateStreaming, you can use a [polyfill](https://github.com/golang/go/blob/b2fcfc1a50fbd46556f7075f7f1fbf600b5c9e5d/misc/wasm/wasm_exec.html#L17-L22).

### Serve the HTML file using a server:
```bash
Install caddy and serve
caddy file-server --browse --listen :8000

Or if you have python in your system
python3 -m http.server
```
### On navigating to url open console 🎉🥳

## Executing WebAssembly with Node.js
- Allow go run and go test find go_js_wasm_exec in a PATH search and use it to just work for js/wasm:
```
export PATH="$PATH:$(go env GOROOT)/misc/wasm"
```
- Run the 
```
GOOS=js GOARCH=wasm go run .
```

- go_js_wasm_exec is a wrapper that allows running Go Wasm binaries in Node. By default, it may be found in the misc/wasm directory of your Go installation.
- Alternatively:

```
$ GOOS=js GOARCH=wasm go run -exec="$(go env GOROOT)/misc/wasm/go_js_wasm_exec" .
```