# CUDA Image Enhancer

This project is a small CUDA-based command-line tool for image enhancement. It uses `stb_image.h` and `stb_image_write.h` for image I/O and runs a three-step GPU pipeline consisting of denoising, sharpening, and contrast adjustment.

## Features

- GPU-accelerated denoising with a bilateral filter
- Sharpening with an unsharp mask
- Contrast adjustment
- Input support through `stb_image`
- Output format inferred from the destination file extension
- Alpha channel preservation for `GA` and `RGBA` images

## Requirements

- NVIDIA GPU with CUDA support
- NVIDIA driver installed and working
- CUDA Toolkit with `nvcc` available in `PATH`

The STB headers required by the project are already included in this repository.

## Build

Compile the program with `nvcc`:

```bash
nvcc -o image_enhancer image_enhancer.cu -O3
```

## Usage

Default run:

```bash
./image_enhancer input.jpg output.png
```

Custom parameters:

```bash
./image_enhancer input.jpg output.png 2.0 50 1.5
```

Parameter meanings:

| Parameter | Description | Valid range | Default |
| --- | --- | --- | --- |
| `sharpen` | Sharpening strength | `>= 0.0` | `1.5` |
| `denoise` | Denoising strength | `> 0.0` | `30.0` |
| `contrast` | Contrast multiplier around mid-gray | `> 0.0` | `1.2` |

The program accepts either:

```text
./image_enhancer <input_gambar> <output_gambar>
./image_enhancer <input_gambar> <output_gambar> <sharpen> <denoise> <contrast>
```

## Examples

Standard enhancement:

```bash
./image_enhancer my_photo.jpg result.png
```

Stronger sharpening:

```bash
./image_enhancer photo.jpg output.png 3.0 30 1.2
```

Heavier denoising:

```bash
./image_enhancer noisy_photo.jpg clean.jpg 1.0 80 1.0
```

Format conversion while enhancing:

```bash
./image_enhancer image.png image.jpg
```

## Output Formats

The output writer is selected from the file extension:

- `.png`
- `.jpg`
- `.jpeg`
- `.bmp`

If the extension is not recognized, the program falls back to PNG output.

## Runtime Behavior

- The program checks for a CUDA-capable device before loading the input image.
- If CUDA is not available, the program exits with an explicit error message.
- Invalid numeric parameters are rejected before processing starts.

## Repository Contents

- [image_enhancer.cu](./image_enhancer.cu): CUDA implementation and CLI entry point
- [stb_image.h](./stb_image.h): image loading library
- [stb_image_write.h](./stb_image_write.h): image writing library
- [input.jpg](./input.jpg): sample input image
- [output.png](./output.png): sample output image
