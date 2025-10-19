# Extras

## `uv` install

```bash
# `uv` instead of conda
uv venv --python=python3.9
uv pip install --index-strategy unsafe-best-match -r requirements.txt 
uv pip install pip
```

## enable llama server 

https://github.com/microsoft/BitNet/issues/206#issuecomment-2820241826

To compile the server:

- first set build server flag

```bash
cmake -S . -B build -DLLAMA_BUILD_SERVER=ON
```

- then reset setup_env

```bash
python setup_env.py -md models/BitNet-b1.58-2B-4T -q i2_s
```

- In the build/bin/ directory you will see llama-server, standard llama-server usage

```bash
./build/bin/llama-server -m models/BitNet-b1.58-2B-4T/ggml-model-i2_s.gguf --port 18080 -t 3 -np 2 --prio 3
```

then test with curl:

```bash
curl --request POST \
    --url http://localhost:18080/completion \
    --header "Content-Type: application/json" \
    --data '{"prompt": "Building a website can be done in 10 simple steps:","n_predict": 128}'
```


## fix for macOS:

if you encounter an error e.g

```bash
python setup_env.py -md models/BitNet-b1.58-2B-4T -q i2_s
INFO:root:Compiling the code using CMake.
```

and the [logs/compile.log](logs/compile.log) seems to be hanging for ages, apply the fix described here: https://github.com/microsoft/BitNet/issues/202#issuecomment-2821483777
