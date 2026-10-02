# laya-mlx cheat sheet (OwnerAutomate ep 22)

Repo: https://github.com/mizorewww/laya-mlx · PyPI: https://pypi.org/project/laya-mlx/ · Apache-2.0 · checked 1 Oct 2026.
Everything below comes from the project's README. Speed and accuracy figures are the author's claims; OwnerAutomate did not re-run them.

## What it is
A Python library that runs the Laya decision models (by Convai Innovations) natively on Apple Silicon with Apple's MLX framework. You give it a message plus typed questions; it returns probabilities in one pass. It writes no text ("0 output tokens"), uses no PyTorch and no cloud API. It is an independent port, not an official Convai Innovations release.

## What you need
- Apple Silicon Mac, macOS 14+, Python 3.11+. No Windows, Linux or Intel Mac.
- The first load downloads the model from Hugging Face; after that, inference is local.

## Install
```bash
pip install laya-mlx
```
Demo (snake game in the terminal, needs a window of at least 104 x 35):
```bash
pip install 'laya-mlx[demo]'
hf download aac6fef/laya-multilingual-mlx
laya-snake
```

## The three question types
| Type | Returns | Example |
|---|---|---|
| `choice` | probabilities over named options | billing / technical / sales |
| `score` | expected level on an ordered rubric | not urgent / soon / critical |
| `noul` | probability that a statement is true | does the customer ask for money back? |

## Copy-paste: sort a customer email (from the README)
```python
import laya_mlx as laya

agent = laya.load("aac6fef/laya-mlx", dtype="float16")
result = agent.predict(
    "I was billed twice. Please refund the duplicate today.",
    {
        "department": {
            "type": "choice",
            "instructions": "Which team should handle this request?",
            "criteria": {
                "billing": "invoices, payments, refunds",
                "technical": "bugs and outages",
                "sales": "new purchases",
            },
        },
        "urgency": {
            "type": "score",
            "instructions": "How urgent is this request?",
            "criteria": ["not urgent", "soon", "critical"],
        },
        "refund": {
            "type": "noul",
            "instructions": "Does the customer ask for money back?",
        },
    },
)
print(result["answers"])
```
The message can be text, a JSON dictionary or a conversation list. Your own rule decides what to do with the probabilities.

## Which model to load
| Hugging Face ID | Size | Context limit | Use for |
|---|---|---|---|
| `aac6fef/laya-mlx` | 421M | 512 | English |
| `aac6fef/laya-multilingual-mlx` | 322M | 1,024 | Non-English messages |
| `aac6fef/laya-typed-decisions-mlx` | 421M | 1,024 | Upstream typed-decisions workflows |

The context limit includes your instructions, options and the message. The README warns the English models are not a substitute for the multilingual one. `from laya_mlx import Router` picks a model by language.

## Speed (README claims, M3 Max, 40 GPU cores, 128 GiB, model loading excluded)
- One short question, median: 13.4 ms (English), 7.4 ms (multilingual)
- 50 questions batched: 146.8 / sec (English), 395.0 / sec (multilingual)
- Peak memory for one question: 943.6 MiB / 687.6 MiB

## Limits to check before you trust it
- Created 19 Sep 2026, one contributor, 6 commits, no tagged release (as of 1 Oct 2026).
- Port fidelity was checked on 63/63 validation questions. The README says this is fidelity on those fixtures, not accuracy on every question, and that confidence does not guarantee accuracy. Test it on 50-100 of your own real messages first.
- Training and fine-tuning are not in this repo; they stay in the upstream Laya project.
- Model weights are by Convai Innovations: read each Hugging Face model card's license before commercial use.

## Sketch: self-hosted n8n on the same Mac (untested by OwnerAutomate)
The README ships a command line:
```bash
laya-mlx predict --model aac6fef/laya-mlx --state "I was billed twice." --questions questions.json
```
(The README shows it as `uv run laya-mlx predict` from a checkout; see `examples/questions.json` in the repo for the questions file.) A self-hosted n8n running on that Mac could call this from an Execute Command node and route on the result with an IF or Switch node. n8n Cloud cannot run local commands.

## Verdict
Not yet for most owners: keep the classifier you have. Yes if a Mac already runs in your office and you have a developer: free, private, millisecond email sorting is a real win. (OwnerAutomate's opinion, not a claim from the project.)
