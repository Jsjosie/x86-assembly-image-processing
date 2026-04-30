# x86-assembly-image-processing

This is a small project I built to get more hands-on with x86 Assembly.
The idea is pretty simple:
take a BMP image, convert it to grayscale, and then run Sobel edge detection on it.

Everything is done manually — no image processing libraries, just raw memory access and registers.

## What it does

  Converts an RGB image to grayscale
  Applies Sobel edge detection
  Outputs a black/white image with detected edges

## How it works

### Grayscale

For each pixel:

gray = (R * 77 + G * 150 + B * 29) / 256

I used this form to avoid floating point operations and divisions.

### Sobel

I compute the gradients:

  Gx (horizontal change)
  Gy (vertical change)

Then:

magnitude = sqrt(Gx² + Gy²)

If the magnitude is above a threshold (90), the pixel becomes white, otherwise black.

The square root is done using the x87 FPU (`fsqrt`).



## What I found interesting

  Manually calculating pixel offsets (using shifts instead of multiplications)
  Thinking in registers instead of variables
  Implementing Sobel in assembly (a bit painful 😄)
  Using the x87 FPU for math


## Notes

  The image stride is hardcoded (2048), so it currently works for a specific layout
  No SIMD optimizations yet (would be a good next step)
  The threshold value is fixed
## Usage

A simple C program (`paok.c`) is included to demonstrate how the assembly functions can be used.

It allocates memory buffers and calls:

  `bmptogray_conversion`
  `sobel_detection`

The C code acts as a bridge between high-level logic and the low-level Assembly implementation.
## Build & Run

Example (Windows / MASM-style workflow):

1. Assemble the `.asm` file  
2. Compile the `main.c` file  
3. Link them together  
