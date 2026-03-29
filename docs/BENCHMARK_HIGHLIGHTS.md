# Benchmark Highlights

This page is the quick public summary of what the lab has shown so far.

## Read This First

These numbers are not presented as paper-parity claims.

They are useful because they show:

- what worked in the local lab
- what tradeoffs appeared between speed and memory
- where the current implementation is strongest

## Main Patterns

### 1. TurboQuant Is Most Helpful As A Memory Tool

The strongest current wins are in KV reduction and long-context headroom, not in
universal speedups across every profile.

### 2. There Is No Single Best Profile

The best speed profile and the best memory profile are different:

- speed-first profiles use lighter rollout and less aggressive `V` compression
- memory-first profiles use broader rollout and lower-bit `V`

### 3. Model And Hardware Matter A Lot

The current lab evidence is centered on:

- `Qwen3.5-9B-Q8_0.gguf`
- `Qwen3.5-27B-Q3_K_M.gguf`
- single-GPU local experimentation

## Practical Takeaways

### Speed-Oriented Direction

Use a lighter rollout with later GPU-resident layers and moderate `V` compression.

This is the profile family that is most useful for:

- live interaction
- control loops
- lower-latency long-context operation

### Memory-Oriented Direction

Use a broader rollout with stronger `V` compression.

This is the profile family that is most useful for:

- headroom experiments
- extreme context pressure
- cases where fitting the run matters more than latency

## How To Reproduce

Use the validator:

```bash
python3 scripts/validate_llama_cli.py compare \
  --bin /path/to/llama-cli \
  --model /path/to/model.gguf \
  --output-dir /tmp/turboquant-compare
```

Then compare the results against the profile guidance in:

- [USAGE_PROFILES.md](USAGE_PROFILES.md)
- [VALIDATED_MODELS.md](VALIDATED_MODELS.md)

## Future Improvement Areas

- faster prompt-time packing
- wider stable rollout coverage
- more validated model families
- cleaner publication of reproducible benchmark bundles
