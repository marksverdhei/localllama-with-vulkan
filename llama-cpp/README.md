# Running in docker  

```
git clone ggml-org/llama.cpp
cd llama.cpp
```

```
docker build -t llama-cpp-vulkan --target full -f .devops/vulkan.Dockerfile .
```

If you don't already have one, pull a model
<!-- huggingface-cli download Qwen/Qwen3-14B-GGUF Qwen3-14B-Q4_K_M.gguf --local-dir models/ -->
```sh
# pip install huggingface-hub[cli]
huggingface-cli download unsloth/Phi-4-mini-reasoning-GGUF Phi-4-mini-reasoning-Q4_K_M.gguf --local-dir models/
```

Identify your GPU and driver:  
```
docker run -it --rm -v "$(pwd)/models:/models" --device /dev/dri/renderD128:/dev/dri/renderD128 --device /dev/dri/card1:/dev/dri/card1 llama-cpp-vulkan
```

Then you're good to go.

Benchmark example:
```
docker run -it --rm -v "$(pwd)/models:/models" --device /dev/dri/renderD128:/dev/dri/renderD128 --device /dev/dri/card1:/dev/dri/card1 llama-cpp-vulkan -b -m models/Phi-4-mini-reasoning-Q4_K_M.gguf
```


## Benchmark results  


`RX 470 8GB (16xpcie 3.0)`
| model                          |       size |     params | backend    | ngl |            test |                  t/s |
| ------------------------------ | ---------: | ---------: | ---------- | --: | --------------: | -------------------: |
| phi3 3B Q4_K - Medium          |   2.31 GiB |     3.84 B | Vulkan     |  99 |           pp512 |        273.82 ± 2.96 |
| phi3 3B Q4_K - Medium          |   2.31 GiB |     3.84 B | Vulkan     |  99 |           tg128 |         44.48 ± 1.14 |


`RX 470 8GB (riser) (1xpcie 3.0)`  

| model                          |       size |     params | backend    | ngl |            test |                  t/s |
| ------------------------------ | ---------: | ---------: | ---------- | --: | --------------: | -------------------: |
| phi3 3B Q4_K - Medium          |   2.31 GiB |     3.84 B | Vulkan     |  99 |           pp512 |        289.63 ± 2.32 |
| phi3 3B Q4_K - Medium          |   2.31 GiB |     3.84 B | Vulkan     |  99 |           tg128 |         33.87 ± 0.08 |



`RX 6600 8GB (16xpcie 3.0)`  


| model                          |       size |     params | backend    | ngl |            test |                  t/s |
| ------------------------------ | ---------: | ---------: | ---------- | --: | --------------: | -------------------: |
| phi3 3B Q4_K - Medium          |   2.31 GiB |     3.84 B | Vulkan     |  99 |           pp512 |        626.25 ± 0.50 |
| phi3 3B Q4_K - Medium          |   2.31 GiB |     3.84 B | Vulkan     |  99 |           tg128 |         62.82 ± 0.52 |



### Multi-GPU  

Todo