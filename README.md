# LearnCLI

An offline, on-device study companion that runs entirely on a phone — no internet, no API key, no data leaving the device.

Built for Hacktoberfest's **Build for a Friend** weekend challenge, for a friend prepping for JEE who needed a way to get quick explanations and practice questions without relying on an internet connection or a paid tool.

## What it does

Type in a topic, and LearnCLI explains it in plain text and generates a short practice question — all using a small language model running locally.

```
❯ What topic? newton's third law
  thinking...

Newton's third law states that for every action there is
an equal and opposite reaction...

Practice question: A rocket pushes exhaust gas downward.
Why does this make the rocket move upward?
```

## How it works

- **Model:** Gemma 3 1B (GGUF, Q4_K_M quantization) — small enough to run on phone-class hardware
- **Inference:** [llama.cpp](https://github.com/ggerganov/llama.cpp), compiled natively in Termux
- **Runtime:** Python 3, calling the compiled `llama-cli` binary via `subprocess`
- **Environment:** Android phone, via Termux — no laptop, no cloud, no internet required after setup

## Why it matters

- **Works with no internet.** Exam prep doesn't stop when the connection drops.
- **Costs nothing to run.** No per-query API billing — the model runs once it's downloaded.
- **Keeps study data on-device.** Nothing typed into it is sent anywhere.
- **Open-weight model + open-source inference engine.** Both Gemma's weights and llama.cpp are open, which is what makes running this entirely offline possible in the first place — a closed API couldn't do this.

## Setup (Termux)

```bash
pkg update && pkg upgrade
pkg install git python clang cmake wget

git clone https://github.com/ggerganov/llama.cpp
cd llama.cpp
cmake -B build
cmake --build build --config Release -j4

# download Gemma 3 1B (Q4_K_M, ~0.8GB) into this folder as gemma-1b.gguf
wget https://huggingface.co/bartowski/google_gemma-3-1b-it-GGUF/resolve/main/gemma-3-1b-it-Q4_K_M.gguf -O gemma-1b.gguf
```

Model source: [bartowski/google_gemma-3-1b-it-GGUF](https://huggingface.co/bartowski/google_gemma-3-1b-it-GGUF)

## Run

```bash
./learncli
```

Type a topic, get an explanation and a practice question. Type `quit` to exit.

##Demo

![LearnCLI demo](screenshot/Screenshot_20261005-000631_Termux.png)

## Notes

- The model file (`gemma-1b.gguf`) is not included in this repo — download it separately from Hugging Face and place it in the `llama.cpp` folder.
- Tested on a Poco M4 Pro (Termux, rooted).
