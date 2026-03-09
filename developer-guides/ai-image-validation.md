# AI Image Validation

X2Earn Apps that send captured photos for image analysis to AI should consider adding specific tasks in their AI prompts to detect:

* Image quality
* Doctored or Unrealistic modifications
* Photo of a computer screen
* Watermarks
* Painted or hand-drawn text replacing real data

... or other unrealistic items that a "fake" photo could capture.



### Example:

In this example we are assuming that we are taking a photo from a dApp runs inside VeWorld and so the photo will be captured by the users mobile camera.&#x20;

The backend of the app would like the AI to return a json structure giving:

```
{
  "evaluation_feasible": true,
  "doctored_unrealistic_score": 0.0,
  "doctored_unrealistic_reasons": [],
  "screen_capture_score": 0.0,
  "screen_capture_reasons": [],
  "watermark_score": 0.0,
  "watermark_reasons": [],
  "watermark_text": "",
  "painted_text_score": 0.0,
  "painted_text_reasons": [],
  "final_label": "clean",
  "final_confidence": 0.0
}
```

Where:

| Item                                                              | Description                                                                                                                                                                                                                                                                               |
| ----------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| evaluation\_feasible                                              | True if the quality of the image is sufficient to perform other checks, False if the quality is poor and other checks could be unreliable                                                                                                                                                 |
| <p>doctored_unrealistic_score<br>doctored_unrealistic_reasons</p> | A score between 0-1 if the AI has detected doctored or unrealistic items in the image. The reasons array will be populated with a summary                                                                                                                                                 |
| <p>screen_capture_score<br>screen_capture_reasons</p>             | A score between 0-1 if the AI has detected that the image was taken from a computer screen. The reasons array will be populated with a summary                                                                                                                                            |
| <p>watermark_score<br>watermark_reasons</p>                       | <p>A score between 0-1 if the AI has detected that the image contains watermarks.</p><p>The reasons array will be populated with a summary</p>                                                                                                                                            |
| <p>painted_text_score<br>painted_text_reasons</p>                 | <p>A score between 0-1 if the AI has detected user drawn text in the image.<br>The reasons array will be populated with a summary</p>                                                                                                                                                     |
| <p>final_label<br>final_score</p>                                 | <p>A final classification label of:</p><ul><li>clean</li><li>doctored_unrealistic</li><li>screen_capture</li><li>watermarked</li><li>handdrawn</li><li>multiple_flags</li><li>inconclusive<br></li></ul><p>A final score between 0-1 as the level of confidence in the classification</p> |





We can use a multi-stage prompt to guide the AI through these tasks:

{% code overflow="wrap" %}
```
Mobile Photo Authenticity Check — Multi-Stage Prompt

Objective:
Given a photo provided by a mobile device, evaluate it through multiple analytical stages to determine:
1. If the photo has been doctored or altered in an unrealistic way.
2. If the photo has been taken from a computer screen rather than being an original capture of a real-world scene.
3. If the photo contains visible or partially obscured watermarks.

Instructions:
You must progress through the stages in sequence. At each stage, clearly indicate whether the photo passes or fails, and explain the reasoning.

Output:
Return only the final JSON object in the specified schema—no extra text.

---

STAGE 1 — Quick Triage (visibility & quality):
1. Is the content visible and in focus enough to evaluate?  
2. Are there heavy obstructions, extreme blur, or tiny resolution?  
3. If evaluation is not feasible, mark `evaluation_feasible=false` and explain briefly.

---

STAGE 2 — “Doctored / Unrealistic” Screening:
Check for visual signs of synthetic or manipulated content. Consider:
- Physics & geometry: inconsistent shadows, impossible reflections, mismatched perspective/vanishing points, warped straight lines near edits.
- Material cues: plastic-like skin, repeated textures, smeared hair/eyelashes, “melting” edges, duplicated fingers/ears.
- Edge artifacts: halos, cut-out borders, fringing, mismatched depth of field.
- Compression anomalies: localized blockiness/quality shifts suggesting pasted regions.
- Lighting: inconsistent color temperature or specular highlights vs. environment.
- Text & patterns: deformed text/logos, repeated tiling.
- Context coherence: scale mismatches, impossible combinations.

Output:
- `doctored_unrealistic_score` (0–1)
- `doctored_unrealistic_reasons` (bullet list)

---

STAGE 3 — “Photo of a Screen” Screening:
Evidence the subject was displayed on a digital screen and re‑photographed:
- Screen structure: visible pixel grid/subpixels, scanlines, PWM/refresh bands, moiré.
- Device clues: bezels, notch, status bar, window chrome, cursor, taskbar, scroll bars.
- Optical clues: rectangular glare, Newton rings, rainbowing consistent with glass.
- Focus/parallax: focus on flat screen surface; keystone perspective of a monitor.
- White point/gamut: uniform backlight glow, overly blue/green whites.

Output:
- `screen_capture_score` (0–1)
- `screen_capture_reasons` (bullet list)

---

STAGE 4 — Watermark / Overlay Detection:
Detect watermarks or ownership/stock overlays that may indicate non-original content or re-use:
- Typical forms: semi-transparent text/logos (“Getty Images”, “Shutterstock”, “Adobe Stock”, creator handles), diagonal repeating patterns, corner logos, date/time stamps that appear composited.
- Visual traits: consistent alpha translucency, uniform repetition across the frame, crisp overlay unaffected by scene lighting/perspective, different resolution/sharpness vs. underlying image.
- Placement: along edges/corners/center diagonals; multiple repeats; patterned tiling.
- Edge cases: legitimate **camera UI overlays** (e.g., timestamp) vs. stock watermarks—distinguish when possible.
- Context: if a watermark is present, note its content (if legible) but **do not identify a person**.

Output:
- `watermark_score` (0–1)
- `watermark_reasons` (bullet list)
- If confidently recognized, add a short `watermark_text` string (e.g., `"Shutterstock"`, `"creator handle @name"`); otherwise empty.

---

STAGE 5 - Detect Hand-Drawn or Painted-On Text:
Detect if the image has text that was manually added using a paint or drawing program, rather than being part of the original scene:
- Look for uneven, non font based handwriting or shapes inconsistent with printed text
- Identify brush strokes, smudging, or digital pen artifacts in the text
- Detect text blending poorly with the image background or overlapping objects unnaturally
- Check for consistent resolution between the text and the rest of the image

Output:
- `painted_text_score` (0-1)
- `painted_text_reasons` (bullet list)

---


STAGE 6 — Final Decision & Confidence:
- `evaluation_feasible`: boolean.  
- `final_label`: one of `"clean"`, `"doctored_unrealistic"`, `"screen_capture"`, `"watermarked"`, `handdrawn`, `"multiple_flags"`, `"inconclusive"`.  
- `final_confidence` (0–1): overall confidence in `final_label`.  
- Keep reasoning concise; cite visible cues only.

---

OUTPUT — JSON Schema (return only this):
{
  "evaluation_feasible": true,
  "doctored_unrealistic_score": 0.0,
  "doctored_unrealistic_reasons": [],
  "screen_capture_score": 0.0,
  "screen_capture_reasons": [],
  "watermark_score": 0.0,
  "watermark_reasons": [],
  "watermark_text": "",
  "painted_text_score": 0.0,
  "painted_text_reasons": [],
  "final_label": "clean",
  "final_confidence": 0.0
}
```
{% endcode %}
