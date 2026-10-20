# Thonburian Whisper Distil (Thai) — whisper.cpp ggml q5_0

A converted copy of [biodatlab/distill-whisper-th-large-v3](https://huggingface.co/biodatlab/distill-whisper-th-large-v3)
at revision `62df42cecab9f484226ad5f9afdb557552021bbb`, a Thai fine-tune of Whisper large-v3-turbo by the
[Biomedical and Data Lab, Mahidol University](https://biodatlab.github.io/) ([Thonburian Whisper](https://github.com/biodatlab/thonburian-whisper)),
licensed under **MIT** (see [`LICENSE`](LICENSE), copied from the Thonburian Whisper repository). The model file is
attached to the [`v1` release](../../releases/tag/v1).

It is used by VietScribe (live transcription and translation for macOS) for Thai speakers.

## What was changed

- `model.safetensors` (F32) was converted to whisper.cpp's ggml format (f16), tensor by tensor, with the
  names and data types of whisper.cpp v1.9.4 `models/convert-h5-to-ggml.py`. The mel filters and the
  tokenizer section were copied from whisper.cpp's `ggml-large-v3-turbo` (same 128 mel bins, same
  51,866-token vocabulary). The weights are otherwise unchanged.
- Then quantised with whisper.cpp's `whisper-quantize … q5_0`, like `ggml-large-v3-turbo-q5_0.bin`.
- Check of the method: the same steps on `openai/whisper-large-v3-turbo` give a file equal to whisper.cpp's
  `ggml-large-v3-turbo-q5_0.bin` on all 587 tensors (name, shape, type and data).

| File | Bytes | SHA-256 |
|---|---:|---|
| whisper-th-thonburian-distil-q5_0.bin | 574,041,195 | 1072402757c6ecaf829b61c1524fcca1bd29193920a5c9df1aebc6b04681c5e4 |

## Use

```sh
whisper-cli -m whisper-th-thonburian-distil-q5_0.bin -l th -f audio.wav
```

Thai only: the fine-tune hears Thai everywhere, so do not use it for other languages or for language detection.

## Measured (2026-10-20, whisper.cpp 1.9.4, Apple M4 Pro)

Character error rate (spaces and punctuation removed), greedy decoding, best of 2, no context:

| Test set | large-v3-turbo q5_0 | this model |
|---|---:|---:|
| Real Thai video: [TVSpeech](https://huggingface.co/datasets/typhoon-ai/TVSpeech), 53 utterances, 22.6 min (numbers spelled as words) | 49.7 % | 34.0 % |
| FLEURS Thai test, first 40 files | 16.4 % | 9.0 % |
| FLEURS + pink noise at 10 dB SNR | 23.0 % | 11.5 % |

Same size and speed as large-v3-turbo q5_0 (4 decoder layers).
