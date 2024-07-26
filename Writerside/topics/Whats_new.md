# What's new

## [2.5.0](https://github.com/lllyasviel/Fooocus/releases/tag/v2.5.0)

This version includes various package updates. If the auto-update doesn't work you can do one of the following:
1. Open a terminal in the Fooocus folder (location of config.txt) and run `git pull`
2. Update packages
   - Windows (installation through zip file): open a terminal in the Fooocus folder (location of config.txt) `..\python_embeded\python.exe -m pip install -r .\requirements_versions.txt` (Windows using embedded python, installation method zip file) or download Fooocus again (zip file attached to this release)
   - other: manually update the packages using `python.exe -m pip install -r requirements_versions.txt` or use the docker image

---

* Update python dependencies, add segment_anything
* Add enhance feature, which offers easy image refinement steps (similar to adetailer, but based on dynamic image detection instead of specific mask detection models). See [documentation](https://github.com/lllyasviel/Fooocus/discussions/3281).
* Rewrite async worker code, make code much more reusable to allow iterations and improve reusability
* Improve GroundingDINO and SAM image masking
* Fix inference tensor version counter tracking issue for GroundingDINO after using Enhance (see [discussion](https://github.com/lllyasviel/Fooocus/discussions/3213))
* Move checkboxes Enable Mask Upload and Invert Mask When Generating from Developer Debug Mode to Inpaint Or Outpaint
* Add persistent model cache for metadata. Use `--rebuild-hash-cache X` (X = int, number of CPU cores, default all) to manually rebuild the cache for all non-cached hashes
* Rename `--enable-describe-uov-image` to `--enable-auto-describe-image`, now also works for enhance image upload
* Rename checkbox `Enable Mask Upload` to `Enable Advanced Masking Features` to better hint to mask auto-generation feature
* Get upscale model filepath by calling downloading_upscale_model() to ensure the model exists
* Rename tab titles and translations from singular to plural
* Rename document to documentation
* Update default models to latest versions
  * animaPencilXL_v400 => animaPencilXL_v500
  * DreamShaperXL_Turbo_dpmppSdeKarras => DreamShaperXL_Turbo_v2_1
  * SDXL_FILM_PHOTOGRAPHY_STYLE_BetaV0.4 => SDXL_FILM_PHOTOGRAPHY_STYLE_V1
* Add preset for pony_v6 (using ponyDiffusionV6XL)
* Add style `Fooocus Pony`
* Add restart sampler ([paper](https://arxiv.org/abs/2306.14878))
* Add config option for default_inpaint_engine_version, sets inpaint engine for pony_v6 and playground_v2.5 to None for improved results (incompatible with inpaint engine)
* Add image editor functionality to mask upload (same as for inpaint, now correctly resizes and allows more detailed mask creation)

## [2.4.3](https://github.com/lllyasviel/Fooocus/releases/tag/v2.4.3)

* Fix alphas_cumprod setter for TCD sampler
* Add parser for env var strings to expected config value types to allow override of all non-path config keys

## [2.4.2](https://github.com/lllyasviel/Fooocus/releases/tag/v2.4.2)

* Fix some small bugs (tcd scheduler when gamma is 0, chown in Dockerfile, update cmd args in readme, translation for aspect ratios, vae default after file reload)
* Fix performance LoRA replacement when data is loaded from history log and inline prompt
* Add support and preset for playground v2.5 (only works with performance Quality or Speed, use with scheduler edm_playground_v2)
* Make textboxes (incl. positive prompt) resizable
* Hide intermediate images when performance of Gradio would bottleneck the generation process (Extreme Speed, Lightning, Hyper-SD)

## [2.4.1](https://github.com/lllyasviel/Fooocus/releases/tag/v2.4.1)

* Fix some small bugs (e.g. adjust clip skip default value from 1 to 2, add type check to aspect ratios js update function)
* Add automated docker build on push to main, tagged with `edge`. See [available docker images](https://github.com/lllyasviel/Fooocus/pkgs/container/fooocus).

## [2.4.0](https://github.com/lllyasviel/Fooocus/releases/tag/v2.4.0)

* Change settings tab elements to be more compact
* Add clip skip slider
* Add select for custom VAE
* Add new style "Random Style"
* Update default anime model to animaPencilXL_v310
* Add button to reconnect the UI after Fooocus crashed without having to configure everything again (no page reload required)
* Add performance "hyper-sd" (based on [Hyper-SDXL 4 step LoRA](https://huggingface.co/ByteDance/Hyper-SD/blob/main/Hyper-SDXL-4steps-lora.safetensors))
* Add [AlignYourSteps](https://research.nvidia.com/labs/toronto-ai/AlignYourSteps/) scheduler by Nvidia, see 
* Add [TCD](https://github.com/jabir-zheng/TCD) sampler and scheduler (based on sgm_uniform)
* Add NSFW image censoring (disables intermediate image preview while generating). Set config value `default_black_out_nsfw` to True to always enable.
* Add argument `--enable-describe-uov-image` to automatically describe uploaded images for upscaling
* Add inline lora prompt references with subfolder support, example prompt: `colorful bird <lora:toucan:1.2>`
* Add size and aspect ratio recommendation on image describe
* Add inpaint brush color picker, helpful when image and mask brush have the same color
* Add automated Docker image build using Github Actions on each release.
* Add full raw prompts to history logs
* Change code ownership from @lllyasviel to @mashb1t for automated issue / MR notification
