# We've Got Poems

## Running the application

We assume a unix environment and offer no guarantees for Windows. You could use WSL or if any errors are encountered file an issue or reach out to the maintainers.

We assume you have followed the steps in the [main README](../../README.md) for the tools required to run this application.

Wwe additionally need to install [npm](https://www.npmjs.com/get-npm) to run the application.

### Building from source

1. [Rust installed](https://www.rust-lang.org/tools/install)
2. Run the following commands:

    ```sh
    rustup target add wasm32-unknown-unknown
    rust install wasm-pack
    ```

3. Build the wasm modules:

    ```sh
    git clone git@github.com:muse254/layer8-interceptor-rs.git
    git clone git@github.com:muse254/layer8-middleware-rs.git
    cd layer8-interceptor-rs && wasm-pack build --target bundler --all-features --debug # output wasm is larger than release
    cd ../layer8-middleware-rs && wasm-pack build --target nodejs --release # debug has no extra logging
    cd ..
    ```

If any issues were encountered in this process, refer to the CI script for insights or the projects respective READMEs. :)

This assumes the interceptor and middleware wasm modules have been built and their root directories are in the same directory.

1. Making sure the backend points to the built middleware. In [packages.json](./backend/package.json) we replace `"layer8-middleware-rs": "^0.1.18",`  
with`"layer8-middleware-rs": "file:../../../../layer8-interceptor-rs/pkg",`
2. Making sure the frontend points to the built interceptor. In [package.js](./frontend/package.json) we replace `"layer8-interceptor-rs": "^0.0.5",`
with `"layer8-interceptor-rs": "file:../../../../layer8-interceptor-rs/pkg",`
3. Make sure we have the [vite.config.js](./frontend/vite.config.js) file in the frontend directory. This file is used to bootstrap the wasm module to the frontend.

### The easy way

Run the server and client in separate terminals:

```sh
cd backend && npm install && npm run dev
```

```sh
cd frontend && npm install && npm run dev
```
