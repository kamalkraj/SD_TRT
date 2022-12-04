# SD_TRT


### Build Docker

```bash
mkdir -p onnx engine output
docker build -t sd_trt .
```

### Run Docker

```
docker run --gpus all \
           --ipc=host \
           --ulimit memlock=-1 \
           --ulimit stack=67108864 \
           -v $(pwd)/engine:/workspace/TensorRT/demo/Diffusion/engine \
           -v $(pwd)/onnx:/workspace/TensorRT/demo/Diffusion/onnx \
           -v $(pwd)/output:/workspace/TensorRT/demo/Diffusion/output \
           -it --rm sd_trt
```

# Running demoDiffusion

### HuggingFace user access token

To download the model checkpoints for the Stable Diffusion pipeline, you will need a `read` access token. See [instructions](https://huggingface.co/docs/hub/security-tokens).

```bash
export HF_TOKEN=<your access token>
```

### Generate an image guided by a single text prompt

```bash
cd demo/Diffusion/
LD_PRELOAD=${PLUGIN_LIBS} python3 demo-diffusion.py "a beautiful photograph of Mt. Fuji during cherry blossom" --hf-token=$HF_TOKEN -v
```

NB: First time run build the trt engine file