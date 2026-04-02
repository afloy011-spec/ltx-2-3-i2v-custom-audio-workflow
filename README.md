# LTX Video: I2V Custom Audio (ComfyUI Workflow)

<p align="left">
  <a href="https://github.com/afloy011-spec/ltx-2-3-i2v-custom-audio-workflow"><img src="https://img.shields.io/badge/ComfyUI-Workflow-2ea44f?style=for-the-badge" height="28" alt="ComfyUI workflow"></a>
  <a href="https://github.com/afloy011-spec/ltx-2-3-i2v-custom-audio-workflow/blob/main/workflows/LTX-2-3-I2V-Custom-Audio.json"><img src="https://img.shields.io/badge/JSON-Workflow-1f6feb?style=for-the-badge" height="28" alt="Workflow JSON"></a>
  <a href="https://github.com/afloy011-spec/afloy_audio_tools"><img src="https://img.shields.io/badge/Afloy-Audio_Tools-f9a825?style=for-the-badge" height="28" alt="Afloy Audio Tools"></a>
</p>

Full **image-to-video** graph for **LTX Video** with a dedicated **custom-audio** path (LTX Audio VAE encode/decode, AV latent merge/split, preview, and video combine).

<p align="left">
  <a href="#quick-start"><img src="https://img.shields.io/badge/Quick_Start-⬇-2ea44f?style=for-the-badge" height="32" alt="Quick Start"></a>
  &nbsp;
  <a href="#get-the-workflow"><img src="https://img.shields.io/badge/Get_workflow-⬇-1f6feb?style=for-the-badge" height="32" alt="Get workflow"></a>
  &nbsp;
  <a href="#afloy-audio-trim-custom-node"><img src="https://img.shields.io/badge/Nodes-Trim-212121?style=for-the-badge" height="32" alt="Nodes"></a>
  &nbsp;
  <a href="https://github.com/afloy011-spec/ltx-2-3-i2v-custom-audio-workflow/archive/refs/heads/main.zip"><img src="https://img.shields.io/badge/Download-ZIP-555?style=for-the-badge" height="32" alt="Download ZIP"></a>
</p>

## Contents

- [Quick start](#quick-start)
- [Get the workflow](#get-the-workflow)
- [Afloy Audio Trim (custom node)](#afloy-audio-trim-custom-node)
- [Other dependencies](#other-dependencies)
- [License & model weights](#license--model-weights)

---

## Quick start

> [!TIP]
> Drag and drop the JSON into the ComfyUI canvas, or use **Load** from the menu. Point all **Load** nodes (checkpoint, VAE, LoRA, etc.) at files that exist on your machine.

[↑ Back to top](#ltx-video-i2v-custom-audio-comfyui-workflow)

---

## Get the workflow

<table style="width:100%; border-collapse: collapse;">
  <thead>
    <tr>
      <th align="left" style="padding:8px;">File</th>
      <th align="left" style="padding:8px;">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="padding:8px;"><a href="workflows/LTX-2-3-I2V-Custom-Audio.json"><code>workflows/LTX-2-3-I2V-Custom-Audio.json</code></a></td>
      <td style="padding:8px;">Exported ComfyUI graph (full pipeline).</td>
    </tr>
  </tbody>
</table>

**Option A — Download ZIP**

Use the **Download ZIP** button above, extract, and copy `workflows/LTX-2-3-I2V-Custom-Audio.json` wherever you keep workflows (or open it from the folder).

**Option B — Git clone**

```bash
git clone https://github.com/afloy011-spec/ltx-2-3-i2v-custom-audio-workflow.git
```

Open `workflows/LTX-2-3-I2V-Custom-Audio.json` in ComfyUI.

[↑ Back to top](#ltx-video-i2v-custom-audio-comfyui-workflow)

---

## Afloy Audio Trim (custom node)

This workflow uses the **Afloy Audio Tools** package, specifically **`afloy_TrimAudio`** (UI title: **Afloy Audio Trim**, category: `audio/Afloy Audio Tools`). It trims the incoming audio by `start_sec` / `end_sec` (use `end_sec = -1` for “until end of file”) and is ideal right after **Load Audio** so the segment matches what you feed into the LTX audio VAE branch.

> [!IMPORTANT]
> Without **`afloy_TrimAudio`** installed, the graph will not load correctly or nodes will be missing. Install [Afloy Audio Tools](https://github.com/afloy011-spec/afloy_audio_tools) under `ComfyUI/custom_nodes/`, then restart ComfyUI.

### Inputs

<table style="width:100%; border-collapse: collapse;">
  <thead>
    <tr>
      <th align="left" style="padding:8px;">Name</th>
      <th align="center" style="padding:8px;">Type</th>
      <th align="left" style="padding:8px;">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="padding:8px;"><code>audio</code></td>
      <td align="center" style="padding:8px;">AUDIO</td>
      <td style="padding:8px;">Audio tensor (e.g. from <strong>Load Audio</strong>).</td>
    </tr>
    <tr>
      <td style="padding:8px;"><code>start_sec</code></td>
      <td align="center" style="padding:8px;">FLOAT</td>
      <td style="padding:8px;">Trim start time in seconds.</td>
    </tr>
    <tr>
      <td style="padding:8px;"><code>end_sec</code></td>
      <td align="center" style="padding:8px;">FLOAT</td>
      <td style="padding:8px;">Trim end in seconds; <code>-1</code> means end of clip.</td>
    </tr>
  </tbody>
</table>

### Outputs

<table style="width:100%; border-collapse: collapse;">
  <thead>
    <tr>
      <th align="left" style="padding:8px;">Name</th>
      <th align="center" style="padding:8px;">Type</th>
      <th align="left" style="padding:8px;">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="padding:8px;"><code>audio</code></td>
      <td align="center" style="padding:8px;">AUDIO</td>
      <td style="padding:8px;">Trimmed audio.</td>
    </tr>
    <tr>
      <td style="padding:8px;"><code>waveform_preview</code></td>
      <td align="center" style="padding:8px;">IMAGE</td>
      <td style="padding:8px;">Waveform preview image.</td>
    </tr>
    <tr>
      <td style="padding:8px;"><code>duration_sec</code></td>
      <td align="center" style="padding:8px;">FLOAT</td>
      <td style="padding:8px;">Duration of trimmed segment (seconds).</td>
    </tr>
    <tr>
      <td style="padding:8px;"><code>timecode</code></td>
      <td align="center" style="padding:8px;">STRING</td>
      <td style="padding:8px;">Formatted timecode string.</td>
    </tr>
  </tbody>
</table>

[↑ Back to top](#ltx-video-i2v-custom-audio-comfyui-workflow)

---

## Other dependencies

The JSON references many built-in and community nodes, including but not limited to:

<table style="width:100%; border-collapse: collapse;">
  <thead>
    <tr>
      <th align="left" style="padding:8px;">Area</th>
      <th align="left" style="padding:8px;">Examples</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="padding:8px;"><strong>LTX Video</strong></td>
      <td style="padding:8px;"><code>LTXVImgToVideoInplace</code>, <code>LTXVAudioVAEEncode</code>, <code>LTXVAudioVAEDecode</code>, <code>LTXVConcatAVLatent</code>, <code>LTXVSeparateAVLatent</code>, schedulers / sampling, etc.</td>
    </tr>
    <tr>
      <td style="padding:8px;"><strong>Video</strong></td>
      <td style="padding:8px;"><code>VHS_VideoCombine</code> (Video Helper Suite)</td>
    </tr>
    <tr>
      <td style="padding:8px;"><strong>UI / utilities</strong></td>
      <td style="padding:8px;"><strong>Power Lora Loader (rgthree)</strong>, KJNodes (<code>SimpleCalculatorKJ</code>, <code>VAELoaderKJ</code>, …), resize / mask nodes</td>
    </tr>
    <tr>
      <td style="padding:8px;"><strong>Audio (extra)</strong></td>
      <td style="padding:8px;"><strong>MelBand RoFormer</strong> loader + sampler nodes in the graph</td>
    </tr>
  </tbody>
</table>

Install the matching custom-node packs and place all model files where your ComfyUI loaders expect them; then fix paths in each **Load** node.

[↑ Back to top](#ltx-video-i2v-custom-audio-comfyui-workflow)

---

## License & model weights

This repository only ships the workflow JSON. **Model weights are not included** (see `.gitignore`).

[↑ Back to top](#ltx-video-i2v-custom-audio-comfyui-workflow)
