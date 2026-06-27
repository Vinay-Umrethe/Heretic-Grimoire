# Heretic Grimoire

<p align="center">
  <a href="https://huggingface.co/heretic-org">
    <img src="https://img.shields.io/badge/%F0%9F%A4%97-Hugging%20Face-orange?style=for-the-badge">
  </a>
  <a href="https://github.com/p-e-w/heretic">
    <img src="https://img.shields.io/badge/GitHub-Heretic-181717?style=for-the-badge&logo=github">
  </a>
  <a href="https://discord.gg/gdXc48gSyT">
    <img src="https://img.shields.io/badge/Discord-Join-5865F2?style=for-the-badge&logo=discord&logoColor=white">
  </a>
</p>

<div align="center"><img src="assets/Heretic-Grimoire-Logo.png" alt="Heretic Grimoire Logo" width="25%" /></div>

This app is for collecting, storing and browsing public [Heretic](https://github.com/p-e-w/heretic) generated models' `reproduce.json` files. It runs the Heretic collector on a fixed twice-daily schedule and indexes any new records it finds on [Hugging Face](https://huggingface.co/models). Collected records are kept as an archive, so previously saved data remains available even if the source repo of a heretic model later gets deleted. Collector logs are shown in the UI and cleared at the start of each collection run.

---

## Information

The app provides a searchable table of reproducibility metadata:

* `Heretic Model`
* `Base Model`
* `Created On`
* `Heretic Version`
* `KLD`
* `Refusals`
* `Base Refusals`
* `Trials`
* `Accelerator`
* `JSON`

---

## Users can

1. **Apply search filters.**
1. **Download the full archive as CSV.**
1. **Refresh the visible table and collector logs.**

---

## Deploy

### 1. Run on Hugging Face Spaces:

Clone this repository and push [`app.py`](app.py) in the space with **Gradio SDK** along with other files.

> [!NOTE]
> Persistent storage (A bucket with read and write permission) should be attached when deploying on Hugging Face Spaces so the archive survives restarts.

### 2. Run locally with:

```bash
python app.py
```

This creates your own local **Heretic Grimoire** archiving system, it updates on a fixed twice-daily schedule using heretic's collector.

---

## License

Copyright &copy; 2026  Vinay Umrethe <umrethevinay@gmail.com>.

This program is free software: you can redistribute it and/or modify it under the terms of the GNU Affero General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU Affero General Public License for more details.

You should have received a copy of the GNU Affero General Public License along with this program. If not, see <https://www.gnu.org/licenses/>.

---

# Appendix

<div align="center"><img src="assets/Heretic-Reproducibility-System-Summary.png" alt="Reproducibility System Summary" width="50%" /></div>

## Abstract

**Heretic** has a reproduction system designed to prevent any *Takedowns*, *Bans*, *Removals* of any **Publicly available** heretic model.

As of 2025-2026, *hostility, need, love and craze* towards local LLMs are all growing everywhere, and this is especially true for decensored models like those created by Heretic. In May 2026, the project was targeted with a [legal notice](https://www.reddit.com/r/LocalLLaMA/comments/1tjmvx6/heretic_has_been_served_a_legal_notice_by_meta_inc/) from Meta, and is already demonized in mainstream media publications. Unfortunately, the AI world remains dependent on a massive single point of failure for model hosting, which is very difficult to replace because LLMs are *huge*.

What if that single point of failure actually fails one day, for one reason or another? What if, in order to obtain Heretic models, you can't simply visit Hugging Face anymore? What if tens of thousands of hours invested by the community to create those models simply vanish?

Don't worry, we have the solution: *Everyone simply downloads all Heretic models to their own system!* That way, if the original model is deleted, you still have a local copy. Easy, right? Now you're probably thinking that this is a silly joke. Well, here's the punchline: **Those models are just ~9 kilobytes each**, so you can store thousands of them on your phone without even noticing.

---

## Reproducible models

In Heretic `v1.3.0`, we introduced **reproducible models**: When uploading an abliterated model to Hugging Face with Heretic, users can choose to include *reproducibility information*, which will be stored in the model repository in human-readable format:

- `reproduce/requirements.txt`: The exact versions of all Python packages.
- `reproduce/config.toml`: The exact configuration used, including the RNG seed.
- `reproduce/checkpoint.jsonl`: The Optuna study journal containing the history of all trials.
- `reproduce/SHA256SUMS`: Cryptographic hashes for all weight files.

There is also a machine-readable file that contains all information needed to reproduce the model:

- `reproduce/reproduce.json`: A machine-readable file containing all reproducibility information.

That file is like a spell in a grimoire, allowing you to summon not a demonic entity, but the very same model it belongs to. An entire heretic model can be reproduced exactly **byte-for-byte** using just a tiny **~9 KB** json file.

### How to reproduce

1. Ensure your system matches the specifications in the **System** section of `reproduce/README.md`. Exact reproducibility is only guaranteed if all aspects of your system are identical to the one the model was originally generated on.
1. Install the exact version of Heretic indicated in the **Environment** section, from its original source (PyPI or Git source installation).
1. Install the packages listed in `requirements.txt`: `pip install -r requirements.txt`
1. Install the correct version of PyTorch: `pip install torch==A.B.C+X --index-url https://download.pytorch.org/whl/X` (Where `A.B.C` is the PyTorch version: e.g. `2.8.0` and `X` is the platform: e.g. `cu128`, `rocm7.1`)
1. Place the provided `config.toml` in your working directory.
1. Run Heretic without any additional arguments: `heretic`
1. Wait for the run to finish, then select trial index and export the model.
1. Verify that the weight files have been exactly reproduced by comparing their SHA-256 hashes against those in `SHA256SUMS`: `sha256sum -c SHA256SUMS` (or look at the hashes online if you uploaded to Hugging Face)

> [!TIP]
> You can automate this entire process for any heretic models created *after* `v1.3.0` release (`v1.4.0` or later).

---

## Automated reproduction

In Heretic `v1.4.0`, we introduced a comprehensive functionality for working with these files, a system we call the **Heretic Grimoire**. Here's how it works:

1. First, make sure you actually have the latest Heretic version, which is required to use these features:

```bash
pip install -U heretic-llm
```

2. Now you can fetch all `reproduce.json` files from publicly available Heretic models on Hugging Face, and store them in a directory of your choice (in this case, my_grimoire):

```bash
heretic --collect-reproducibles my_grimoire
```

You now have a local backup of all reproducible Heretic models, properly catalogued. To update this collection, simply run the command again. It functions as an append-only backup, never deleting files even if the corresponding model no longer exists on Hugging Face.

3. To restore one of those models, simply run

```bash
heretic --reproduce path/to/reproduce.json OR url/to/reproduce.json
```

Heretic will guide you through the process, checking your environment against the one that was originally used to create the model, and pointing out potentially problematic mismatches. The multi-hour computations that were required to make the original model do not have to be re-done, and the entire process typically takes around a minute. After you have exported the resulting model, Heretic will verify the SHA256 hashes of the weight files against those stored in the reproduction manifest.

> [!NOTE]
> The hashes may or may not be identical, depending on how closely your system resembles the original one.

> [!WARNING]
> This automation via `--reproduce` feature does not not guarantee *Exact* reproduction for models created *before* `v1.4.0` (like for `v1.3.0`) and even the environment check will mark it as a critical mismatch.
>
> You can still continue the reproduction anyway (not recommended for **byte-for-byte** identical SHA256 hashes to match the original model weights). For exact reproduction of those old models having `"version": "1",` of `reproduce.json` files, see [How to reproduce section](https://huggingface.co/spaces/heretic-org/Heretic-Grimoire/blob/main/README.md#how-to-reproduce).
>
> We highly recommend to use `--reproduce` feature for any models created *after* `v1.3.0` and have `"version": "2",` or later of their respective `reproduce.json` files.

> [!TIP]
> You can create your own local grimoire or deploy it on cloud easily, which updates daily. See [Deploy section](https://huggingface.co/spaces/heretic-org/Heretic-Grimoire/blob/main/README.md#deploy) for guidance.

---

## References

1. Philipp Emanuel Weidmann, 2025. Heretic: Fully automatic censorship removal for language models. [GitHub.](https://github.com/p-e-w/heretic)

2. Reproducibility implementation, feat: reproducibility when saving & uploading a heretic model #191. Vinay Umrethe, 2026. [GitHub.](https://github.com/p-e-w/heretic/pull/191)

3. Reproducibility improvements, fix: improve the reproducibility system #303. Philipp Emanuel Weidmann, 2026. [GitHub.](https://github.com/p-e-w/heretic/pull/303)

4. Reproducibility automation, feat: automatically reproduce model from reproduce.json #326. Philipp Emanuel Weidmann and Vinay Umrethe, 2026. [GitHub.](https://github.com/p-e-w/heretic/pull/326)

5. Reproducibility automation fixes, fix: fix issues in automatic reproduction system #352. Vinay Umrethe, 2026. [GitHub.](https://github.com/p-e-w/heretic/pull/352)

6. Philipp Emanuel Weidmann's Reddit post about Heretic 1.3 release. 2026. [Reddit.](https://www.reddit.com/r/LocalLLaMA/comments/1t4hwup/heretic_13_released_reproducible_models/)

7. Philipp Emanuel Weidmann's Reddit post about Meta's legal notice on a Llama3.1 8B Instruct heretic model. 2026. [Reddit.](https://www.reddit.com/r/LocalLLaMA/comments/1tjmvx6/heretic_has_been_served_a_legal_notice_by_meta_inc/)

8. Philipp Emanuel Weidmann's Reddit post about Heretic 1.4 release and Heretic grimoire. 2026. [Reddit.](https://www.reddit.com/r/LocalLLaMA/comments/1u5lmge/introducing_the_heretic_grimoire_the/)
