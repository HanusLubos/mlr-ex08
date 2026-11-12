# Exercise 8 — leaf classifier in the browser

**Live page: <https://hanuslubos.github.io/mlr-ex08/>**

Classifies a leaf live from your phone's rear camera, entirely on the device.
No backend, no upload — one `index.html`, ONNX Runtime Web from a CDN, and the
trained `leaf_model.onnx`.

Point your phone's camera at the **Live page** URL above (a QR code is rendered
at the bottom of the page), or open it on a desktop and use the **Photo** button.

## What it does

Preprocessing follows `model_info.json` exactly, with nothing hardcoded:

1. take the **central square** of the current camera frame,
2. resize it to `input_size` (`96×96`) in the same `drawImage()` call that crops,
   so the resample operates on the crop rather than on an already-resized frame,
3. scale RGB to `0–1`,
4. standardise each channel with `mean` / `std`,
5. feed NCHW `1×3×96×96` to the model, whose input is `image` and whose output
   is `logits` — softmax is applied in the page,
6. show the probability of all four classes (`apple`, `cherry`, `chestnut`,
   `maple`), updated every frame.

A **Photo** button is the fallback wherever the camera is unavailable, and a
**Run self-test** button re-runs `check_image.jpg` through the exact code path
the camera uses and diffs the result against `check_expected.json`.

## Running it

The camera needs a secure context, so serve the folder over **HTTPS** (or
`localhost`):

```sh
python -m http.server 8000      # then open http://localhost:8000
```

On a phone, HTTPS is required for `getUserMedia`; GitHub Pages already provides
it. Plain `http://` on a LAN IP will not offer the camera — the page says so and
falls back to the photo picker.

Notes on iPhone Safari: the video element is `playsinline` (otherwise Safari
hijacks `play()` into fullscreen), `numThreads = 1` (ORT's threads need COOP/COEP
headers a static host won't send), and `facingMode` constraints degrade in steps
for older iOS builds. A **`Frame: auto`** control handles the fact that a phone
held portrait still delivers a *landscape* camera frame; it rotates 90° only in
that case and leaves uploads (already EXIF-oriented) alone.

## Verified

Executed in a real browser against a local server:

| Check | Result |
| --- | --- |
| `check_image.jpg` vs `check_expected.json` | PASS, max abs diff `8.3e-8` |
| Live camera loop | 640×480 at ~30 fps, bars updating |
| Photo upload | `cherry 100.0%` |
| Uncaught JS errors | none |

`check_image.jpg` is already `96×96`, so it cannot exercise the crop. That was
covered separately with a probe image (the leaf centred on a loud background at
`384×192`): a correctly centred crop gives `1.9e-5` error, a top-left crop gives
`0.47` — the page lands on the former.

## Files

| File | Purpose |
| --- | --- |
| `index.html` | the whole app |
| `leaf_model.onnx` | trained model, `image [1,3,96,96] → logits [1,4]` |
| `model_info.json` | classes, input size, channel mean/std |
| `check_image.jpg`, `check_expected.json` | self-test fixture and reference probabilities |

Course exercise MLR ex08.
