# 🖌️ Drawing with LLMs: Prompt-to-SVG Generation

## 🚀 Objective  
Transform a **natural language prompt** into a **scalable vector drawing (SVG)**.  
The goal? Push the boundaries of multimodal AI by turning visual imagination into crisp, vectorized art — powered by **open-source language and vision models**.

---

## 🧪 Approaches Explored

### 🧠 Phase 1: Can LLMs Draw?

We began by testing the SVG-generating capabilities of popular LLMs using **zero-shot** and **few-shot** prompting. Models evaluated:

- 🤖 **Qwen 2.5 LM**
- 🦙 **LLaMA 3 (3.2B Instruct)**
- 🔬 **Deepseek R1**
- 🌟 **Gemma 2**

**Best zero-shot performer:** 🔥 Qwen 2.5  
We then tried fine-tuning with **QLoRA/LoRA**, and even reward-model techniques like **GRPO** and **DPO** — but gains were minimal.

---

## 🔁 New Pipeline: Prompt → Image → SVG

To tackle the problem more effectively, we restructured the task into a **two-step pipeline**:

### 🖼️ Step 1: Prompt → Image  
For generating raster images from prompts, we explored:

- 🎨 **Stable Diffusion** (by CompVis)
- 🌈 **Flux**

These diffusion models offered flexible and high-quality renderings as input for the next stage.

---

### ✂️ Step 2: Image → SVG  

To convert a raster image (e.g., PNG, JPG) into SVG format, we built a **custom vectorizer** that processes a `PIL.Image` with the following steps:

#### 🧵 Color Quantization (K-Means Clustering)
- Dominant colors (default: 12) are extracted using **K-Means**, simplifying the image into essential visual elements.

#### ✏️ Contour Detection
- For each color, **OpenCV** extracts region contours — identifying the object-like shapes in the image.

#### 🔍 Shape Simplification
- Contours are simplified using `approxPolyDP`, converting blobs into **clean polygon outlines**.

#### 📊 Feature Ranking
Each shape is ranked based on:
- **Area**
- **Proximity to image center**
- **Geometric simplicity** (fewer polygon points)

#### 🧱 SVG Construction
The output is a layered SVG file:
```xml
<svg>
  <rect fill="bg_color" />
  <polygon points="..." fill="#hex" />
  ...
</svg>


##### The final best results came through using a pipeline https://github.com/yuval-alaluf/Attend-and-Excite which tends to focus on some specified tokens whose indices are passed. The notebook is attached with for the best results.
