# Drawing-with-LLM

## Objective 
Given a text prompt describing an image, the task is to generate Scalable Vector Graphics (SVG) code that renders it as an image as closely as possible. We can use any sort open source LLMs/ Vision Language Models.

## Approaches tried
First off tried with Zero shot/few shot learning to test the SVG code generational abilities of various opensource models. The models tested include:
  Qwen 2.5 LM
  LLama 3.2B instruct
  Deepseek R1
  Gemma2 

The best results were given by Qwen: We tried finetuning it using QLoRA/LoRA, not much improvement in the results though. Also tried GRPO/DPO but similar results.

Decided to build a pipeline which generates a png/jpg image from prompt then renders the svg code.
prompt->image->svg

  for the first step of this pipeline using prompt->image: we utillised tried diffusion models like **Stable diffusion** by compvis and **Flux**.

  for the second step image->svg of this pipeline: we defined a function which takes in a PIL image and performs the following steps.

    **Color Quantization (K-means)**:
    The RGB pixel data is clustered into a fixed number of dominant colors (default: 12) using K-means. This reduces the image to essential color regions.
    
   **Contour Detection per Color**:
    For each quantized color, OpenCV isolates that color's region using a mask, and then extracts its contours — shapes made of continuous pixels of that color.
    
    **Shape Simplification**:
    Each contour is simplified using approxPolyDP, which approximates the shape with fewer points (like turning curves into polygons).
    
    **Feature Ranking**:
    Each simplified polygon is scored based on:
    Its area
    Proximity to the image center
    Simplicity (fewer points → higher score)
    
    **SVG Building**:
    The SVG starts with an opening <svg> tag and a <rect> background. Then, for each high-ranking feature, it adds:
  


