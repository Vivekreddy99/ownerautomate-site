# gpu-unlock cheat sheet (OwnerAutomate ep 036)

Repo: https://github.com/Zagitta/gpu-unlock (created 2026-10-18, v2.0.0, GPL-2.0 kernel module)
Everything below is what the project's README says, checked on 2026-10-28. Not installed or benchmarked for this video.

## What it is
A Linux kernel module that, per the README, removes NVIDIA's half-rate cap on GeForce tensor cores for FP16 / BF16 / TF32 math (FP32 accumulation). It runs at boot before the stock NVIDIA driver loads. No patched driver, no VBIOS flash.

## Supported cards (README)
| Generation | Cards | Status |
|---|---|---|
| RTX 30 (Ampere) | 3050, 3060, 3060 Ti, 3070, 3070 Ti, 3080, 3080 Ti, 3090, 3090 Ti, 3070/3080 Laptop | Stable, 100+ user reports |
| RTX 40 (Ada) | 4060 to 4090 D, 4050/4060 Laptop | Experimental |
| RTX 20 (Turing) | 2060 to 2080 Ti, Titan RTX | Experimental |
| RTX 50 (Blackwell) | none | Not supported |

Linux only. Laptops lose the unlock if the GPU powers off when idle.

## What gets faster (README, from user reports)
- LLM prefill / prompt processing (vLLM, PyTorch, Transformers): +25 to 55%
- Time to first token: 25 to 30% lower
- LoRA and full fine-tuning in BF16: +25 to 50%
- Image generation FP16/BF16 (SDXL, SD 1.5, ComfyUI): +30 to 55%
- Video generation, Wan 2.2: +15 to 20%
- llama.cpp / Ollama with quantized GGUF: 0 to 15%
- Token generation (writing the reply): no change
- INT8, FP8, INT4 models: no change
- Gaming and FP32 CUDA: no change

## Install (README steps)
```sh
git clone https://github.com/Zagitta/gpu-unlock
cd gpu-unlock
make install          # builds and installs the package, asks for sudo
```
Add `gpu_unlock.targets=all` to the kernel command line (keep existing options):
- Ubuntu/Debian/Mint: add inside `GRUB_CMDLINE_LINUX_DEFAULT` in `/etc/default/grub`, then `sudo update-grub`
- Arch/CachyOS (GRUB): same edit, then `sudo grub-mkconfig -o /boot/grub/grub.cfg`
- Fedora/RHEL: `sudo grubby --update-kernel=ALL --args="gpu_unlock.targets=all"`
- systemd-boot: append to the `options` line of your boot entry

Reboot, then check: `journalctl -k -b | grep gpu_unlock`

## Undo
Remove `gpu_unlock.targets=` from the kernel command line and reboot. Removing the package removes the module.

## Risks and limits
- README: may run slightly warmer; throttling and power management stay intact.
- Display goes blank briefly at boot during the card reset.
- Suspend re-unlocks on resume; hibernation is not covered.
- A doc in the repo (quoted on Hacker News) says the unlock uses a buffer overflow in the card's boot firmware.
- Windows: not maintained by the author. Community port impacifist/Windows-GPU-UNLOCK exists; the README warns to be very careful with Windows installers.
- FAQ: no official website, coin or installer. The GitHub repo is the only source.

## Verdict for a business owner
Yes if you already run local AI on a GeForce under Linux (image generation, fine-tuning, long-document prompts). No if your AI is a browser chatbot or Ollama chat on Windows. Try it on a machine you can afford to reboot.
