## ⚠️ OneDiffusion project has been deprecated

To learn more about how to deploy diffusion models, see [the BentoML documentation](https://docs.bentoml.com/en/latest/use-cases/diffusion-models/index.html).

<div align="center">
    <h1 align="center">🖼️ OneDiffusion</h1>
    <a href="https://pypi.org/project/onediffusion">
        <img src="https://img.shields.io/pypi/v/onediffusion.svg?logo=pypi&label=PyPI&logoColor=gold" alt="pypi_status" />
    </a>
    <a href="https://twitter.com/bentomlai">
        <img src="https://badgen.net/badge/icon/@bentomlai/1DA1F2?icon=twitter&label=Follow%20Us" alt="Twitter" />
    </a>
    <a href="https://l.bentoml.com/join-openllm-discord">
        <img src="https://badgen.net/badge/icon/AI%20Developers/7289da?icon=discord&label=Join%20Us" alt="Discord" />
    </a>
</div>
<br>

OneDiffusion is an open-source one-stop shop for facilitating the deployment of any diffusion models in production. It caters specifically to the needs of diffusion models, supporting both pretrained and fine-tuned diffusion models with LoRA adapters.

Key features include:

- 🌐 **Broad compatibility**: Support both pretrained and LoRA-adapted diffusion models, providing flexibility in choosing and deploying the appropriate model for various image generation tasks.
- 💪 **Optimized performance and scalability**: Automatically select the best optimizations like half-precision weights or xFormers to achieve best inference speed out of the box.
- ⌛️ **Dynamic LoRA adapter loading**: Dynamically load and unload LoRA adapters on every request, providing greater adaptability and ensuring the models remain responsive to changing inputs and conditions.
- 🍱 **First-class support for BentoML**: Seamless integration with the [BentoML](https://github.com/bentoml/BentoML) ecosystem, allowing you to build Bentos and push them to [BentoCloud](https://www.bentoml.com/cloud). 

OneDiffusion is designed for AI application developers who require a robust and flexible platform for deploying diffusion models in production. The platform offers tools and features to fine-tune, serve, deploy, and monitor these models effectively, streamlining the end-to-end workflow for diffusion model deployment.

## Supported models

Currently, OneDiffusion supports the following models:

- Stable Diffusion v1.4, v1.5 and v2.0
- Stable Diffusion XL v1.0
- Stable Diffusion XL Turbo

More models (for example, ControlNet and DeepFloyd IF) will be added soon.

> [!NOTE]
> If you want to deploy Stable Video Diffusion, see the project [BentoSVD](https://github.com/bentoml/BentoSVD).

## Get started

To quickly get started with OneDiffusion, follow the instructions below or try this tutorial in Google Colab: [Serving Stable Diffusion with OneDiffusion](https://colab.research.google.com/github/bentoml/OneDiffusion/blob/main/examples/onediffusion_demo.ipynb).

### Prerequisites

You have installed Python 3.8 (or later) and `pip`.

### Install OneDiffusion

Install OneDiffusion by using `pip` as follows:

```bash
pip install onediffusion
```

To verify the installation, run:

```bash
$ onediffusion -h

Usage: onediffusion [OPTIONS] COMMAND [ARGS]...

       ██████╗ ███╗   ██╗███████╗██████╗ ██╗███████╗███████╗██╗   ██╗███████╗██╗ ██████╗ ███╗   ██╗
      ██╔═══██╗████╗  ██║██╔════╝██╔══██╗██║██╔════╝██╔════╝██║   ██║██╔════╝██║██╔═══██╗████╗  ██║
      ██║   ██║██╔██╗ ██║█████╗  ██║  ██║██║█████╗  █████╗  ██║   ██║███████╗██║██║   ██║██╔██╗ ██║
      ██║   ██║██║╚██╗██║██╔══╝  ██║  ██║██║██╔══╝  ██╔══╝  ██║   ██║╚════██║██║██║   ██║██║╚██╗██║
      ╚██████╔╝██║ ╚████║███████╗██████╔╝██║██║     ██║     ╚██████╔╝███████║██║╚██████╔╝██║ ╚████║
       ╚═════╝ ╚═╝  ╚═══╝╚══════╝╚═════╝ ╚═╝╚═╝     ╚═╝      ╚═════╝ ╚══════╝╚═╝ ╚═════╝ ╚═╝  ╚═══╝
          
          An open platform for operating diffusion models in production.
          Fine-tune, serve, deploy, and monitor any diffusion models with ease.
          

Options:
  -v, --version  Show the version and exit.
  -h, --help     Show this message and exit.

Commands:
  build     Package a given model into a Bento.
  download  Setup diffusion models interactively.
  start     Start any diffusion models as a REST server.
```

## Start a diffusion server

OneDiffusion allows you to quickly spin up any diffusion models. To start a server, run:

```bash
onediffusion start stable-diffusion
```

This starts a server at http://0.0.0.0:3000/. You can interact with it by visiting the web UI or send a request via `curl`.

```bash
curl -X 'POST' \
  'http://0.0.0.0:3000/text2img' \
  -H 'accept: image/jpeg' \
  -H 'Content-Type: application/json' \
  --output output.jpg \
  -d '{
  "prompt": "a bento box",
  "negative_prompt": null,
  "height": 768,
  "width": 768,
  "num_inference_steps": 50,
  "guidance_scale": 7.5,
  "eta": 0
}'
```

By default, OneDiffusion uses `stabilityai/stable-diffusion-2` to start the server. To use a specific model version, add the `--model-id` option as below:

```bash
onediffusion start stable-diffusion --model-id runwayml/stable-diffusion-v1-5
```

To specify another pipeline, use the `--pipeline` option as below. The `img2img` pipeline allows you to modify images based on a given prompt and image.

```bash
onediffusion start stable-diffusion --pipeline "img2img"
```

OneDiffusion downloads the models to the BentoML local Model Store if they have not been registered before. To view your models, install BentoML first with `pip install bentoml` and then run:

```bash
$ bentoml models list

Tag                                                                                         Module                              Size        Creation Time
pt-sd-stabilityai--stable-diffusion-2:1e128c8891e52218b74cde8f26dbfc701cb99d79              bentoml.diffusers                   4.81 GiB    2023-08-16 17:52:33
pt-sdxl-stabilityai--stable-diffusion-xl-base-1.0:bf714989e22c57ddc1c453bf74dab4521acb81d8  bentoml.diffusers                   13.24 GiB   2023-08-16 16:09:01
```

### Start a Stable Diffusion XL server

OneDiffusion also supports running Stable Diffusion XL 1.0, the most advanced development in the Stable Diffusion text-to-image suite of models launched by Stability AI. To start an XL server, simply run:

```bash
onediffusion start stable-diffusion-xl
```

It downloads the model automatically if it does not exist locally. Options such as `--model-id` are also supported. For more information, run `onediffusion start stable-diffusion-xl --help`. 

Similarly, visit http://0.0.0.0:3000/ or send a request via `curl` to interact with the XL server. Example prompt:

```
{
  "prompt": "the scene is a picturesque environment with beautiful flowers and trees. In the center, there is a small cat. The cat is shown with its chin being scratched. It is crouched down peacefully. The cat's eyes are filled with excitement and satisfaction as it uses its small paws to hold onto the food, emitting a content purring sound.",
  "negative_prompt": null,
  "height": 1024,
  "width": 1024,
  "num_inference_steps": 50,
  "guidance_scale": 7.5,
  "eta": 0
}
```

Example output:

![sdxl-cat](/example-images/sdxl-cat.jpeg)

### Start a Stable Diffusion XL Turbo server

[SDXL Turbo](https://stability.ai/news/stability-ai-sdxl-turbo) is a distilled version of SDXL 1.0 and is capable of creating images in a single step, with improved real-time text-to-image output quality and sampling fidelity.

To serve SDXL Turbo locally, run:

```bash
onediffusion start stable-diffusion-xl --model-id stabilityai/sdxl-turbo
```

Visit http://0.0.0.0:3000/ or send a request via `curl` to interact with the server. Example prompt:

```
{
  "prompt": "Create a serene landscape at sunset, with a tranquil lake reflecting the vibrant colors of the sky. Surrounding the lake are lush, green forests and distant mountains.",
  "height": 512,
  "width": 512,
  "num_inference_steps": 1,
  "guidance_scale": 0.0
}
```

> [!NOTE]
> SDXL Turbo can run inference with only one step, so you can set `num_inference_steps` to `1` and this is enough to generate high quality images. However, increasing the number of steps to `2`, `3` or `4` should improve image quality. In addition, make sure you set `guidance_scale` to `0.0` to disable it as the model was trained without it. See [the official release notes](https://github.com/huggingface/diffusers/releases/tag/v0.24.0) to learn more.

Example output:

![sdxl-turbo-output](/example-images/sdxl-turbo-output.jpeg)

### Add LoRA weights

Low-Rank Adaptation (LoRA) is a training method to fine-tune models without the need to retrain all parameters. You can add LoRA weights to your diffusion models for specific data needs.

Add the `--lora-weights` option as below:

```bash
onediffusion start stable-diffusion-xl --lora-weights "/path/to/lora-weights.safetensors"
```

Alternatively, dynamically load LoRA weights by adding the `lora_weights` field:

```
{
  "prompt": "the scene is a picturesque environment with beautiful flowers and trees. In the center, there is a small cat. The cat is shown with its chin being scratched. It is crouched down peacefully. The cat's eyes are filled with excitement and satisfaction as it uses its small paws to hold onto the food, emitting a content purring sound.",
  "negative_prompt": null,
  "height": 1024,
  "width": 1024,
  "num_inference_steps": 50,
  "guidance_scale": 7.5,
  "eta": 0,
  "lora_weights": "/path/to/lora-weights.safetensors"
}
```

By specifying the path of LoRA weights at runtime, you can influence model outputs dynamically. Even with identical prompts, the application of different LoRA weights can yield vastly different results. Example output (oil painting vs. pixel):

![dynamic loading](/example-images/dynamic-loading.gif)

## Download a model

If you want to download a diffusion model without starting a server, use the `onediffusion download` command. For example:

```
onediffusion download stable-diffusion --model-id "CompVis/stable-diffusion-v1-4"
```

## Create a BentoML Runner

You can create a BentoML Runner with `diffusers_simple.stable_diffusion.create_runner()`, which downloads the model specified automatically if it does not exist locally.

```python
import bentoml

# Create a Runner for a Stable Diffusion model
runner = bentoml.diffusers_simple.stable_diffusion.create_runner("CompVis/stable-diffusion-v1-4")

# Create a Runner for a Stable Diffusion XL model
runner_xl = bentoml.diffusers_simple.stable_diffusion_xl.create_runner("stabilityai/stable-diffusion-xl-base-1.0")
```

You can then wrap the Runner into a BentoML Service. See the [BentoML documentation](https://docs.bentoml.com/en/latest/concepts/service.html) for more details.

## Build a Bento

A [Bento](https://docs.bentoml.com/en/latest/concepts/bento.html) in BentoML is a deployable artifact with all the source code, models, data files, and dependency configurations. You can build a Bento for a supported diffusion model directly by running `onediffusion build`.

```bash
# Build a Bento with a Stable Diffusion model 
onediffusion build stable-diffusion

# Build a Bento with a Stable Diffusion XL model 
onediffusion build stable-diffusion-xl
```

To specify the model to be packaged into the Bento, use `--model-id`. Otherwise, OneDiffusion packages the default model into the Bento. If the model does not exist locally, OneDiffusion downloads the model automatically. In addition, the pipeline to use can also be specified through `--pipeline`. By default, OneDiffusion uses the `text2image` pipeline.

To package LoRA weights into the Bento, use the `--lora-dir` option to specify the directory where LoRA files are stored. These files can be dynamically loaded to the model when deployed with Docker or BentoCloud to create task-specific images.

```bash
onediffusion build stable-diffusion-xl --lora-dir "/path/to/lorafiles/dir/"
```

If you only have a single LoRA file to use, run the following instead:

```bash
onediffusion build stable-diffusion-xl --lora-weights "/path/to/lorafile"
```

Each Bento has a `BENTO_TAG` containing both the Bento name and the version. To customize it, specify `--name` and `--version` options.

```bash
onediffusion build stable-diffusion-xl --name sdxl --version v1
```

Once your Bento is ready, log in to [BentoCloud](https://docs.bentoml.com/en/latest/bentocloud/how-tos/manage-access-token.html) and run the following command to push the Bento.

```bash
bentoml push BENTO_TAG
```

Alternatively, create a Docker image by containerizing the Bento with the following command. You can retrieve the `BENTO_TAG` by running `bentoml list`.

```bash
bentoml containerize BENTO_TAG
```

You can then deploy the image to any Docker-compatible environments.

## Roadmap

We are working to improve OneDiffusion in the following ways and invite anyone who is interested in the project to participate 🤝.

- Support more models, such as ControlNet and DeepFloyd IF
- Support more pipelines, such as inpainting
- Add a Python API client to interact with diffusion models
- Implement advanced optimization like AITemplate
- Offer a unified fine-tuning training API

## Contribution

We weclome contributions of all kinds to the OneDiffusion project! Check out the following resources to start your OneDiffusion journey and stay tuned for more announcements about OneDiffusion and BentoML.

- Submit a pull request or create an issue in the [OneDiffusion GitHub repository](https://github.com/bentoml/OneDiffusion).
- Join the [BentoML community on Slack](https://l.bentoml.com/join-slack).
- Follow us on [Twitter](https://twitter.com/bentomlai) and [Linkedin](https://www.linkedin.com/company/bentoml/).
