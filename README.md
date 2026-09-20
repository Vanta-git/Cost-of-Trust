# Web11 — 5 MB maximum hosted files

This build replaces the original WASM and Unity data files with raw chunks. Every hosted file is below 5 MB. `Build/Web11.loader.js` intercepts Unity's requests for `Build/Web11.wasm` and `Build/Web11.data`, downloads the corresponding chunks, joins each file in the browser, and returns the original file to Unity.

## Hosting

Upload the entire folder without renaming files. Serve the files over HTTP(S); do not open `index.html` with `file://`.

The chunk files are raw binary files, so they must **not** be served with `Content-Encoding: br` or `Content-Encoding: gzip`. WASM chunks should be served with `application/wasm`; data chunks can use `application/octet-stream`.

The generated `index.html` requests the logical filenames `Web11.wasm` and `Web11.data`; the patched loader maps those requests to the chunk sets. Keep every chunk in the same `Build` folder.

## Important limitation

The WASM and data files are still single files logically; they are split only to satisfy per-file upload limits. The browser joins the pieces before Unity uses them, so the initial download still includes the complete game binary and data file.
