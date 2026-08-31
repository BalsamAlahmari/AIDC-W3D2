# W3D2 — inference anatomy, by hand

## Objective
Measure the two halves of a response (time to first token, then the per-token gap), watch the KV cache grow and check it against the arithmetic, and hand-roll static batching to feel where it ceilings.

## Predict 
* Time to first token (TTFT) is dominated by prefill (reading the whole prompt). A longer prompt makes TTFT go **UP**.
* After the first token, decode emits one token at a time. The mean gap between tokens (TPOT) depends mostly on **model size and memory bandwidth**.
* KV cache math for Qwen2.5-1.5B: 28 layers, 2 KV heads, head_dim 128, fp16. Per token that is 2 (K and V) x 28 x 2 x 128 x 2 bytes = **28 KB** per token. So a 4096-token context holds about **0.11 GB** of KV.
* Static batching: if you pad 8 prompts of different lengths and run them as one batch, the batch finishes when the **longest** prompt finishes.

