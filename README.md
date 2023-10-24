# nginx-unit wasm learning

## build wasm

* install wasm  plugins

```code
rustup target add wasm32-wasi
```

* command

```code
cd wasm/wasm_on_unit
cargo build --target wasm32-wasi
```

## config unit


* config

> inside unit  container 

```code
curl -X PUT --data-binary '{
      "listeners": {
          "0.0.0.0:8080": {
              "pass": "applications/wasm"
          }
      },

      "applications": {
          "wasm": {
              "type": "wasm",
              "module": "/www/wasm_on_unit.wasm",
              "request_handler": "uwr_request_handler",
              "malloc_handler": "luw_malloc_handler",
              "free_handler": "luw_free_handler",
              "module_init_handler": "uwr_module_init_handler",
              "module_end_handler": "uwr_module_end_handler"
          }
      }
  }' --unix-socket /var/run/control.unit.sock http://localhost/config/
```

* check

```code
curl -i http://localhost:8080
```