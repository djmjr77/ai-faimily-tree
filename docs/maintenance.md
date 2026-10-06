# Maintenance guide: The family tree of AI base brains

This document is a complete briefing for an AI model (or a person) who has been asked to update or extend this project with no other context. Read all of it before changing anything.

**You also need the current `ai-base-brain-family-tree.html` file** (the owner may have renamed it `index.html`). That file is the source of truth. The inventory in section 11 is a snapshot from October 2026 and may be out of date. If the two disagree, trust the HTML file and say so.

---

## 1. What the project is

A single-page, interactive family tree of AI model architectures, written for curious non-experts. It is hosted by its owner on GitHub Pages.

- **127 designs in 15 families**, under 2 trunks (as of October 2026)
- Each design has: a name, a first-publication year, a one-line plain-English description, a status label, and one "Continued Learning" link
- The page supports search, opening and closing branches, and filtering by status

### Files

| File | Purpose |
|---|---|
| `ai-base-brain-family-tree.html` | The whole project: HTML, CSS, data, and script in one file. No build step, no dependencies to install. |
| `README.md` | Public description for the repository. Contains entry counts and a review date that must be kept in sync. |
| This guide | Private working notes for whoever maintains the page. Not linked from the page. |

---

## 2. The core idea you must preserve

The project grew out of one framing, and every entry has to fit it:

- A **"base brain"** is a model's **architecture**: how it is wired before any training happens.
- A **"training game"** is how the brain is trained: predicting the next piece (autoregressive), removing noise (diffusion), filling in blanks (masked modeling), matching pairs (contrastive), learning from labels (supervised), or trial and reward (reinforcement learning).
- The same base brain can become very different products depending on the training game. A chatbot and a video generator can both be transformers.

**The tree lists base brains, not training games and not products.** Apply this test before adding anything:

| Candidate | Is it an entry? | Why |
|---|---|---|
| Transformer, Mamba, LSTM, U-Net | Yes | Architectures |
| Diffusion, autoregression, contrastive learning | No | Training games. "Diffusion transformer" is an entry because it is a transformer wired to act as a noise remover. "Diffusion model" is not. |
| GPT-5, Sora, Seedance, Veo | No | Products. They can be named as examples inside a description. |
| LoRA, distillation, quantization, RAG prompts | No | Training or deployment techniques |
| Autoencoder, GAN, retrieval-augmented systems | Yes, under "Composite designs" | Ways of wiring brains together. This family is the deliberate exception and its description says so. |

The second trunk, "Not neural networks", exists because a lot of everyday AI is not a neural network (decision trees, classic statistics, rule-based systems). Keep it.

---

## 3. Audience and voice

The owner asked for explanations pitched at "a smart 5 year old": plain language, concrete images, no assumed background. Hold every description to that.

### Description rules

- One or two short sentences. Aim for under about 150 characters. The longest existing ones are about 190.
- Say what it does, then where it is used or why it mattered.
- No undefined jargon. If a technical word is unavoidable, the sentence must make its meaning clear.
- State things plainly. No hype words, no "revolutionary", no "powerful".
- Simple punctuation: periods and commas. No em dashes, no semicolons.
- Be honest about failures. "It did not catch on." is a good description.
- Name one or two well-known examples when that helps ("AlphaFold predicts the shapes of proteins").

### Shared vocabulary

Reuse these plain-English stand-ins so the page stays consistent:

| Technical term | Phrase used on the page |
|---|---|
| Architecture | Base brain, design |
| Convolution | Small sliding windows |
| Self-attention | Every piece can look directly at every other piece |
| Recurrence, hidden state | Reads one piece at a time while carrying a running memory |
| State space model | Carries a compact running summary |
| Token, patch | Piece |
| Denoiser | Noise remover |
| Latent compression (VAE) | Compressor, "squeezes data through a narrow middle" |
| Graph | A web of connections |
| Parameters, weights | Connection strengths, settings |
| Edge deployment | Phones and small devices |

### Naming rules

- Sentence case. Proper nouns and acronyms keep their capitals (LSTM, U-Net, Mamba).
- A category with a flagship example: "Neural fields, such as NeRF".
- Two closely related designs in one entry: "VGG and Inception".
- A lineage: "YOLO family".

---

## 4. Data format

All content is in one JavaScript array named `DATA`, inside the `<script>` tag near the bottom of the HTML file. Two helpers build it:

```js
// A family: a branch that holds designs
F("Family name", "status", "One-line description.", [ ...designs ])

// A design: a leaf
L("Design name", "year", "status", "Description.", "https://link")
```

`DATA` itself is an array of two trunk objects:

```js
var DATA = [
  { n: "Neural networks",     d: "Trunk description.", c: [ F(...), F(...) ] },
  { n: "Not neural networks", d: "Trunk description.", c: [ F(...), F(...) ] }
];
```

Field details:

| Field | Rules |
|---|---|
| Name | Unique across the whole tree. Double quotes around the string, so do not use a double quote inside it. Apostrophes are fine. |
| Year | A string. A four-digit year ("2017"), a decade ("1980s"), or an empty string `""` when there is no single date. |
| Status | One of the five keys in section 5. Anything else breaks the status chip. |
| Description | See section 3. Double-quoted string, no inner double quotes. |
| Link | Optional fifth argument on `L` only. Must start with `https://`. If omitted, the "Continued Learning" line is not shown. Every current entry has one. Families do not have links. |

The tree is exactly three levels deep: trunk, family, design. The script assumes this. Do not nest families inside families without rewriting `build()` and `apply()`.

### Ordering

Designs within a family are in rough chronological order by year. Entries without a year are placed where they make sense by era or importance (for example "Linear and logistic regression" comes first in its family). This is a convention, not something the code enforces. Keep new entries in year order.

---

## 5. Status labels

Status describes **how much a design is used today**, not how important it was.

| Key | Label on page | Meaning | Count (Oct 2026) |
|---|---|---|---|
| `dom` | Dominant | The default choice for today's headline systems | 3 |
| `wide` | In wide use | Common in real products across the industry | 30 |
| `spec` | Specialist | In real use, but within particular fields or niches | 43 |
| `exp` | Experimental | Mainly research. Little or no production use yet. | 23 |
| `hist` | Mostly historical | Largely replaced. Listed for its place in the lineage. | 28 |

Guidance:

- A family's status is a judgment about the family as a whole.
- `dom` is used sparingly. Currently only: decoder-only transformer, diffusion transformer, multimodal transformer (plus the Transformers family itself).
- Status is the fastest part of the page to go stale. Re-check every `exp` and `spec` entry at each review. In October 2026 the state space family was moved from `exp` to `spec` after evidence that Mamba layers had shipped inside production language models.
- Do not promote something on the strength of a press release or a startup's own claims. Look for independent evidence of real use.
- The labels are defined in the `STATUS` object at the top of the script, and each has a pair of color tokens in the CSS (`--dom-fg`, `--dom-bg`, and so on, in three places: light, dark by system setting, dark by attribute). Adding a sixth status means touching all of those.

---

## 6. Year conventions

- Use the year of first public appearance, which for modern work is usually the preprint year, not the later conference or journal year.
- For older work, use the year of the defining publication.
- When an idea and its famous tool have different dates, date the idea and name the tool in the description. Example: gradient-boosted trees are dated 2001 (the method), with "XGBoost (2014)" in the description.
- The page footer already tells readers that years are approximate.

Two errors were found and fixed in the October 2026 review, and they show what to watch for:

- Speech transformers were listed as 2020. The first Speech-Transformer paper was presented in 2018.
- Gradient-boosted trees were listed as 2014 (the XGBoost release) when the method dates to 2001.

---

## 7. Link policy

Each design gets exactly one link, shown as `Continued Learning: <link>`. The owner specified that label wording, so do not change it. The script displays the address without `https://` and without a trailing slash, and opens it in a new tab.

Choose the link in this order:

1. **Wikipedia**, if a decent article exists. It is the friendliest starting point for the audience.
2. **The original paper on arXiv**, in the form `https://arxiv.org/abs/NNNN.NNNNN` with no version suffix.
3. **The creators' own explainer**: an official blog post, an interactive article, or a project page.
4. **The official code repository**, as a last resort.

### Never invent a link

A wrong arXiv number points readers at an unrelated paper. Every link must be confirmed by one of these methods before it goes in:

- It appeared in the results of a web search you ran.
- You loaded the page and it is the right one.
- For arXiv IDs: the ID appears in the README of the project's official GitHub repository. This can be checked without a browser:
  `https://raw.githubusercontent.com/OWNER/REPO/HEAD/README.md`

If you cannot verify a link, either leave the link off that entry or tell the owner plainly which links are unverified. Do not present a guessed link as checked.

### Current verification state (October 2026)

- **42 non-Wikipedia links: all verified** by one of the methods above.
- **85 Wikipedia links: only 2 verified** (`TabPFN` and `Transformer_architecture`). The other 83 are well-known article titles written from memory and never loaded. The owner was told this and advised to click through them. **Verifying these 83 is the most useful outstanding task.** If one is wrong, fix the title or switch that entry to a verified paper link.

Repositories used to confirm arXiv IDs and project pages:

| Entry | Repository checked | Confirmed |
|---|---|---|
| MLP-Mixer | google-research/vision_transformer | 2105.01601 |
| Kolmogorov-Arnold network | KindXiaoming/pykan | 2404.19756 |
| VGG and Inception | tensorflow/models (research/slim/README.md) | 1409.1556 |
| MobileNet and EfficientNet | tensorflow/models (research/slim/README.md) | 1704.04861 |
| DenseNet | liuzhuang13/DenseNet | 1608.06993 |
| ConvNeXt | facebookresearch/ConvNeXt | 2201.03545 |
| Hyena | HazyResearch/safari | 2302.10866 |
| RWKV | BlinkDL/RWKV-LM | 2305.13048 |
| xLSTM | NX-AI/xlstm | 2405.04517 |
| Long-context variants | allenai/longformer | 2004.05150 |
| Decision transformer | kzl/decision-transformer | 2106.01345 |
| Diffusion transformer | facebookresearch/DiT | 2212.09748 |
| Vision-language-action models | openvla/openvla | 2406.09246 |
| Time-series transformers | google-research/timesfm | 2310.10688 |
| S4 | state-spaces/s4 | 2111.00396 |
| Mamba, Mamba-2 | state-spaces/mamba | 2312.00752, 2405.21060 |
| Vision Mamba | hustvl/Vim | 2401.09417 |
| Graph convolutional network | tkipf/gcn | tkipf.github.io/graph-convolutional-networks |
| GraphSAGE | williamleif/GraphSAGE | snap.stanford.edu/graphsage |
| Graph attention network | PetarV-/GAT | 1710.10903 |
| Message-passing network | brain-research/mpnn | 1704.01212 |
| Point-cloud networks | charlesq34/pointnet | 1612.00593 |
| Deep Sets | manzilzaheer/DeepSets | Repository exists (its README has no arXiv ID, so the repository is the link) |
| Equivariant networks | e3nn/e3nn | e3nn.org |
| Graph transformer | microsoft/Graphormer | Repository exists (used as the link) |
| Hypernetwork | g1910/HyperNetworks | 1609.09106 |
| Neural ODE | rtqichen/torchdiffeq | 1806.07366 |
| Deep equilibrium model | locuslab/deq | 1909.01377 |
| Neural operators | neuraloperator/neuraloperator | Repository exists (used as the link) |
| Neural cellular automata | google-research/self-organising-systems | distill.pub/2020/growing-ca |
| Continuous thought machine | SakanaAI/continuous-thought-machines | pub.sakana.ai/ctm |
| Dragon Hatchling (BDH) | pathwaycom/bdh | 2509.26507 |
| World models and JEPA | hardmaru/WorldModelsExperiments | worldmodels.github.io |
| Masked autoencoder | facebookresearch/mae | 2111.06377 |
| Modern Hopfield network | ml-jku/hopfield-layers | 2008.02217 |
| VQ-VAE | google-deepmind/sonnet (sonnet/src/nets/vqvae.py) | 1711.00937 |

Confirmed through web search results instead: the links for Liquid neural network, Titans, Mamba-3, the recursive reasoning models (2506.21734), and Hybrids (2511.23404).

---

## 8. How the page works

### Structure of the HTML file

1. `<head>`: viewport tag, Google Fonts link, one `<style>` block
2. `<body>`: a header (title, intro line, tally), a sticky toolbar, an empty `<main id="out">`, an empty-state message, a footer
3. One `<script>`: the `STATUS` map, the `L` and `F` helpers, `DATA`, then the functions below

### Element IDs the script depends on

| ID | Element |
|---|---|
| `tally` | Paragraph that shows "N designs in M families", filled in automatically |
| `q` | Search box |
| `expand`, `collapse` | "Open all branches" and "Close all branches" buttons |
| `filters` | Row where the five status filter buttons are created |
| `out` | Container the tree is built into |
| `empty` | Message shown when nothing matches |

### Functions

| Function | What it does |
|---|---|
| `el(tag, cls, text)` | Creates an element. Text is set with `textContent`, so data is never injected as HTML. |
| `chip(s)` | Creates a status chip |
| `build()` | Runs once. Builds the whole tree from `DATA`, stores element references on each data object, creates the filter buttons, and writes the tally. |
| `hit(node, q)` | True if the query appears in the name, year, description, or status label. Links are not searched. |
| `paint(node, q)` | Highlights the matching part of a name with `<mark>` |
| `apply()` | Recomputes what is visible. Called after every search keystroke, filter click, and branch toggle. |
| `setAll(open)` | Opens or closes every family |

### Behaviors to keep intact

- A design is visible if its status filter is on and either it matches the search or its family name matches.
- A family is hidden when none of its designs are visible. A trunk is hidden when none of its families are.
- While there is text in the search box, every visible family is forced open. Clearing the search restores each family's own open or closed state.
- All families start open.

---

## 9. Design and technical constraints

Keep these unless the owner asks for a change.

- **One self-contained file.** No frameworks, no build tools, no external scripts. The only outside requests are two Google Fonts, and the page still works without them.
- **Fonts:** Bricolage Grotesque for the title, trunk headings, and family names. Source Sans 3 for everything else. Both have system fallbacks in the stack.
- **Colors are tokens** defined on `:root` and redefined for dark mode in two places: under `@media (prefers-color-scheme: dark)` and under `:root[data-theme="dark"]`. The second lets an embedding page force a theme. If you add or change a color, change it in all three blocks.
- **Status is never shown by color alone.** Every chip carries its text label.
- **Responsive.** There is one breakpoint at 520px. Long links wrap with `overflow-wrap: anywhere`.
- **Safe-area padding** (`env(safe-area-inset-*)`) is intentional, for phones with notches. It is zero on desktop.
- **Accessibility:** family headers are real `<button>` elements with `aria-expanded`. Filter chips use `aria-pressed`. There is a visible focus ring. The one animation (the caret turning) respects reduced-motion settings.
- **No browser storage.** The page keeps no state between visits.
- **Text style in the interface:** sentence case everywhere, no all-caps labels, buttons that say what they do ("Open all branches").

### Rules from the owner

- **No AI attributions.** The owner explicitly asked that the code carry no credits, comments, or "generated by" notes naming any AI assistant, model, or AI company. This applies to the HTML file and the README. The decoder-only transformer entry names GPT, Claude, Gemini, and Llama as examples of that design. That is content, not a credit, and the owner was told it is there.
- **The link label is "Continued Learning:"** exactly, with that capitalization.
- **The owner wants downloadable files** after each change: the updated HTML, and the README if it changed.
- **Be straight about what was verified.** After the October 2026 review, the owner was given separate lists of what was corrected, what was added, and what was not checked. Do the same.

---

## 10. How to do an update

### Adding a design

1. Check it passes the test in section 2.
2. Check it is not already present under another name (search the inventory in section 11 and the HTML file).
3. Pick the family. If it mixes two families, it belongs in "Composite designs" or can be mentioned in the "Hybrids" entry.
4. Write the name, year, status, and description to the rules above.
5. Find and verify a link (section 7).
6. Insert the `L(...)` line in year order. Mind the commas: every line in a family's array ends with a comma except the last.

### Adding a family

Add an `F(...)` block inside the right trunk's `c` array. Only do this when at least three designs share a real lineage that no existing family covers. "Offshoots and experiments" is the right home for one-off designs.

### Review checklist

Run through this at every review:

1. Search for architectures published since the last review date. Check the leads in section 12.
2. Re-check the status of every `exp` and `spec` entry, and of each family.
3. Check whether any `exp` entry has quietly died and should become `hist`.
4. Verify any Wikipedia links not yet verified.
5. Update the footer sentence "Entries were last reviewed in October 2026."
6. Update `README.md`: the counts in the first paragraph ("127 designs into 15 families") and the review date under "Accuracy notes". The counts on the page itself update automatically, but the README does not.
7. Update this guide: the counts in sections 1 and 5, the verification state in section 7, the inventory in section 11, and the change log in section 13.
8. Run the checks below.
9. Search both files for stray attributions (see "Rules from the owner").

### Checks to run after editing

Save this as `check.js` next to the HTML file and run `node check.js`. It confirms the script still parses, every status is valid, every design has an `https` link, and no name is duplicated.

```js
const fs = require("fs");
const html = fs.readFileSync("ai-base-brain-family-tree.html", "utf8");
const js = html.match(/<script>([\s\S]*)<\/script>/)[1];
const fake = () => ({ appendChild(){}, addEventListener(){}, setAttribute(){},
  style: {}, textContent: "", innerHTML: "", value: "", hidden: false });
global.document = { getElementById: fake, createElement: fake, createTextNode: () => ({}) };
const report = `
  var families = 0, designs = 0, problems = [], seen = {};
  DATA.forEach(function (t) { t.c.forEach(function (f) {
    families++;
    if (!STATUS[f.s]) problems.push("Bad family status: " + f.n);
    f.c.forEach(function (k) {
      designs++;
      if (!STATUS[k.s]) problems.push("Bad status: " + k.n);
      if (!/^https:\\/\\//.test(k.u || "")) problems.push("Missing link: " + k.n);
      if (seen[k.n]) problems.push("Duplicate name: " + k.n);
      seen[k.n] = true;
    });
  }); });
  console.log(designs + " designs in " + families + " families");
  console.log(problems.length ? problems.join("\\n") : "No problems found");
`;
new Function(js + report)();
```

Expected output for the October 2026 version:

```
127 designs in 15 families
No problems found
```

Then open the page in a browser and confirm: the tree renders, search highlights matches, each filter chip hides and restores entries, and a "Continued Learning" link opens the right page.

---

## 11. Inventory snapshot (October 2026)

Generated from the HTML file. "Link checked" means the link was confirmed by one of the methods in section 7. Descriptions are not repeated here: read them in the HTML file.

### Trunk: Neural networks

**Feed-forward networks** (family status `wide`, 8 designs)

| Design | Year | Status | Link | Link checked |
|---|---|---|---|---|
| Perceptron | 1958 | `hist` | https://en.wikipedia.org/wiki/Perceptron | no |
| Adaline | 1960 | `hist` | https://en.wikipedia.org/wiki/ADALINE | no |
| Group method of data handling | 1965 | `hist` | https://en.wikipedia.org/wiki/Group_method_of_data_handling | no |
| Multilayer perceptron | 1986 | `wide` | https://en.wikipedia.org/wiki/Multilayer_perceptron | no |
| Radial basis function network | 1988 | `hist` | https://en.wikipedia.org/wiki/Radial_basis_function_network | no |
| Neural fields, such as NeRF | 2020 | `spec` | https://en.wikipedia.org/wiki/Neural_radiance_field | no |
| MLP-Mixer | 2021 | `exp` | https://arxiv.org/abs/2105.01601 | yes |
| Kolmogorov-Arnold network | 2024 | `exp` | https://arxiv.org/abs/2404.19756 | yes |

**Convolutional networks** (family status `wide`, 15 designs)

| Design | Year | Status | Link | Link checked |
|---|---|---|---|---|
| Neocognitron | 1980 | `hist` | https://en.wikipedia.org/wiki/Neocognitron | no |
| Time-delay neural network | 1987 | `hist` | https://en.wikipedia.org/wiki/Time_delay_neural_network | no |
| LeNet | 1989 | `hist` | https://en.wikipedia.org/wiki/LeNet | no |
| AlexNet | 2012 | `hist` | https://en.wikipedia.org/wiki/AlexNet | no |
| R-CNN family | 2014 | `wide` | https://en.wikipedia.org/wiki/Region_Based_Convolutional_Neural_Networks | no |
| VGG and Inception | 2014 | `hist` | https://arxiv.org/abs/1409.1556 | yes |
| ResNet | 2015 | `wide` | https://en.wikipedia.org/wiki/Residual_neural_network | no |
| U-Net | 2015 | `wide` | https://en.wikipedia.org/wiki/U-Net | no |
| YOLO family | 2015 | `wide` | https://en.wikipedia.org/wiki/You_Only_Look_Once | no |
| DenseNet | 2016 | `spec` | https://arxiv.org/abs/1608.06993 | yes |
| WaveNet and temporal convolutions | 2016 | `spec` | https://en.wikipedia.org/wiki/WaveNet | no |
| MobileNet and EfficientNet | 2017 | `wide` | https://arxiv.org/abs/1704.04861 | yes |
| 3D convolutional networks | none | `spec` | https://en.wikipedia.org/wiki/Convolutional_neural_network | no |
| ConvNeXt | 2022 | `spec` | https://arxiv.org/abs/2201.03545 | yes |
| Hyena | 2023 | `exp` | https://arxiv.org/abs/2302.10866 | yes |

**Recurrent networks** (family status `hist`, 10 designs)

| Design | Year | Status | Link | Link checked |
|---|---|---|---|---|
| Simple recurrent network | 1990 | `hist` | https://en.wikipedia.org/wiki/Recurrent_neural_network | no |
| LSTM | 1997 | `spec` | https://en.wikipedia.org/wiki/Long_short-term_memory | no |
| Bidirectional recurrent network | 1997 | `hist` | https://en.wikipedia.org/wiki/Bidirectional_recurrent_neural_networks | no |
| Echo state network | 2001 | `hist` | https://en.wikipedia.org/wiki/Echo_state_network | no |
| GRU | 2014 | `spec` | https://en.wikipedia.org/wiki/Gated_recurrent_unit | no |
| Sequence-to-sequence with attention | 2014 | `hist` | https://en.wikipedia.org/wiki/Seq2seq | no |
| Neural Turing machine | 2014 | `hist` | https://en.wikipedia.org/wiki/Neural_Turing_machine | no |
| Liquid neural network | 2020 | `exp` | https://liquid.ai/blog/liquid-foundation-models-v2-our-second-series-of-generative-ai-models | yes |
| RWKV | 2023 | `exp` | https://arxiv.org/abs/2305.13048 | yes |
| xLSTM | 2024 | `exp` | https://arxiv.org/abs/2405.04517 | yes |

**Transformers** (family status `dom`, 17 designs)

| Design | Year | Status | Link | Link checked |
|---|---|---|---|---|
| Encoder-decoder transformer | 2017 | `wide` | https://en.wikipedia.org/wiki/Transformer_architecture | yes |
| Encoder-only transformer | 2018 | `wide` | https://en.wikipedia.org/wiki/BERT_(language_model) | no |
| Decoder-only transformer | 2018 | `dom` | https://en.wikipedia.org/wiki/Generative_pre-trained_transformer | no |
| Speech transformers | 2018 | `wide` | https://en.wikipedia.org/wiki/Whisper_(speech_recognition_system) | no |
| Vision transformer | 2020 | `wide` | https://en.wikipedia.org/wiki/Vision_transformer | no |
| Structure-prediction transformers | 2020 | `spec` | https://en.wikipedia.org/wiki/AlphaFold | no |
| Long-context and linear-attention variants | 2020 | `spec` | https://arxiv.org/abs/2004.05150 | yes |
| Dual-encoder models, such as CLIP | 2021 | `wide` | https://en.wikipedia.org/wiki/Contrastive_Language-Image_Pre-training | no |
| Perceiver | 2021 | `exp` | https://en.wikipedia.org/wiki/Perceiver | no |
| Decision transformer | 2021 | `spec` | https://arxiv.org/abs/2106.01345 | yes |
| Diffusion transformer | 2022 | `dom` | https://arxiv.org/abs/2212.09748 | yes |
| Tabular transformers, such as TabPFN | 2022 | `spec` | https://en.wikipedia.org/wiki/TabPFN | yes |
| Vision-language-action models | 2023 | `spec` | https://arxiv.org/abs/2406.09246 | yes |
| Time-series transformers | 2023 | `spec` | https://arxiv.org/abs/2310.10688 | yes |
| Titans | 2025 | `exp` | https://research.google/blog/titans-miras-helping-ai-have-long-term-memory/ | yes |
| Mixture-of-experts transformer | none | `wide` | https://en.wikipedia.org/wiki/Mixture_of_experts | no |
| Multimodal transformer | none | `dom` | https://en.wikipedia.org/wiki/Multimodal_learning | no |

**State space models** (family status `spec`, 5 designs)

| Design | Year | Status | Link | Link checked |
|---|---|---|---|---|
| S4 | 2021 | `exp` | https://arxiv.org/abs/2111.00396 | yes |
| Mamba | 2023 | `spec` | https://arxiv.org/abs/2312.00752 | yes |
| Mamba-2 | 2024 | `spec` | https://arxiv.org/abs/2405.21060 | yes |
| Vision Mamba | 2024 | `exp` | https://arxiv.org/abs/2401.09417 | yes |
| Mamba-3 | 2026 | `exp` | https://tridao.me/blog/2026/mamba3-part1/ | yes |

**Graph and geometric networks** (family status `spec`, 10 designs)

| Design | Year | Status | Link | Link checked |
|---|---|---|---|---|
| Graph neural network | 2005 | `hist` | https://en.wikipedia.org/wiki/Graph_neural_network | no |
| Graph convolutional network | 2016 | `spec` | https://tkipf.github.io/graph-convolutional-networks/ | yes |
| GraphSAGE | 2017 | `spec` | https://snap.stanford.edu/graphsage/ | yes |
| Graph attention network | 2017 | `spec` | https://arxiv.org/abs/1710.10903 | yes |
| Message-passing network | 2017 | `spec` | https://arxiv.org/abs/1704.01212 | yes |
| Point-cloud networks, such as PointNet | 2017 | `spec` | https://arxiv.org/abs/1612.00593 | yes |
| Deep Sets | 2017 | `spec` | https://github.com/manzilzaheer/DeepSets | yes |
| Learned simulators, such as GraphCast | 2022 | `spec` | https://en.wikipedia.org/wiki/GraphCast | no |
| Equivariant networks | none | `spec` | https://e3nn.org/ | yes |
| Graph transformer | none | `spec` | https://github.com/microsoft/Graphormer | yes |

**Early memory and energy-based networks** (family status `hist`, 8 designs)

| Design | Year | Status | Link | Link checked |
|---|---|---|---|---|
| Hopfield network | 1982 | `hist` | https://en.wikipedia.org/wiki/Hopfield_network | no |
| Self-organizing map | 1982 | `hist` | https://en.wikipedia.org/wiki/Self-organizing_map | no |
| Boltzmann machine | 1985 | `hist` | https://en.wikipedia.org/wiki/Boltzmann_machine | no |
| Restricted Boltzmann machine | 1986 | `hist` | https://en.wikipedia.org/wiki/Restricted_Boltzmann_machine | no |
| Adaptive resonance theory | 1987 | `hist` | https://en.wikipedia.org/wiki/Adaptive_resonance_theory | no |
| Helmholtz machine | 1995 | `hist` | https://en.wikipedia.org/wiki/Helmholtz_machine | no |
| Deep belief network | 2006 | `hist` | https://en.wikipedia.org/wiki/Deep_belief_network | no |
| Modern Hopfield network | 2020 | `exp` | https://arxiv.org/abs/2008.02217 | yes |

**Offshoots and experiments** (family status `exp`, 12 designs)

| Design | Year | Status | Link | Link checked |
|---|---|---|---|---|
| Spiking neural network | none | `exp` | https://en.wikipedia.org/wiki/Spiking_neural_network | no |
| Neuroevolution, such as NEAT | 2002 | `exp` | https://en.wikipedia.org/wiki/Neuroevolution_of_augmenting_topologies | no |
| Normalizing flow | 2015 | `spec` | https://en.wikipedia.org/wiki/Flow-based_generative_model | no |
| Hypernetwork | 2016 | `exp` | https://arxiv.org/abs/1609.09106 | yes |
| Capsule network | 2017 | `hist` | https://en.wikipedia.org/wiki/Capsule_neural_network | no |
| Neural ODE | 2018 | `exp` | https://arxiv.org/abs/1806.07366 | yes |
| Deep equilibrium model | 2019 | `exp` | https://arxiv.org/abs/1909.01377 | yes |
| Neural operators, such as the Fourier neural operator | 2020 | `spec` | https://github.com/neuraloperator/neuraloperator | yes |
| Neural cellular automata | 2020 | `exp` | https://distill.pub/2020/growing-ca/ | yes |
| Recursive reasoning models, such as HRM and TRM | 2025 | `exp` | https://arxiv.org/abs/2506.21734 | yes |
| Continuous thought machine | 2025 | `exp` | https://pub.sakana.ai/ctm/ | yes |
| Dragon Hatchling (BDH) | 2025 | `exp` | https://arxiv.org/abs/2509.26507 | yes |

**Composite designs** (family status `wide`, 10 designs)

| Design | Year | Status | Link | Link checked |
|---|---|---|---|---|
| Autoencoder | 1980s | `wide` | https://en.wikipedia.org/wiki/Autoencoder | no |
| Siamese and two-tower networks | 1993 | `wide` | https://en.wikipedia.org/wiki/Siamese_neural_network | no |
| Variational autoencoder | 2013 | `wide` | https://en.wikipedia.org/wiki/Variational_autoencoder | no |
| Generative adversarial network | 2014 | `spec` | https://en.wikipedia.org/wiki/Generative_adversarial_network | no |
| Neural network plus search, such as AlphaGo | 2016 | `spec` | https://en.wikipedia.org/wiki/AlphaGo | no |
| VQ-VAE | 2017 | `wide` | https://arxiv.org/abs/1711.00937 | yes |
| World models and JEPA | 2018 | `exp` | https://worldmodels.github.io/ | yes |
| Retrieval-augmented systems | 2020 | `wide` | https://en.wikipedia.org/wiki/Retrieval-augmented_generation | no |
| Masked autoencoder | 2021 | `wide` | https://arxiv.org/abs/2111.06377 | yes |
| Hybrids | none | `wide` | https://arxiv.org/abs/2511.23404 | yes |

### Trunk: Not neural networks

**Decision trees and forests** (family status `wide`, 4 designs)

| Design | Year | Status | Link | Link checked |
|---|---|---|---|---|
| Decision tree | 1984 | `wide` | https://en.wikipedia.org/wiki/Decision_tree_learning | no |
| AdaBoost | 1995 | `spec` | https://en.wikipedia.org/wiki/AdaBoost | no |
| Random forest | 2001 | `wide` | https://en.wikipedia.org/wiki/Random_forest | no |
| Gradient-boosted trees | 2001 | `wide` | https://en.wikipedia.org/wiki/Gradient_boosting | no |

**Linear, kernel, and lookup models** (family status `wide`, 6 designs)

| Design | Year | Status | Link | Link checked |
|---|---|---|---|---|
| Linear and logistic regression | none | `wide` | https://en.wikipedia.org/wiki/Linear_regression | no |
| k-nearest neighbors | 1951 | `spec` | https://en.wikipedia.org/wiki/K-nearest_neighbors_algorithm | no |
| ARIMA | 1970 | `spec` | https://en.wikipedia.org/wiki/Autoregressive_integrated_moving_average | no |
| Q-learning table | 1989 | `spec` | https://en.wikipedia.org/wiki/Q-learning | no |
| Support vector machine | 1995 | `spec` | https://en.wikipedia.org/wiki/Support_vector_machine | no |
| Gaussian process | none | `spec` | https://en.wikipedia.org/wiki/Gaussian_process | no |

**Clustering and compression** (family status `wide`, 4 designs)

| Design | Year | Status | Link | Link checked |
|---|---|---|---|---|
| Principal component analysis | 1901 | `wide` | https://en.wikipedia.org/wiki/Principal_component_analysis | no |
| k-means | 1957 | `wide` | https://en.wikipedia.org/wiki/K-means_clustering | no |
| Gaussian mixture model | none | `spec` | https://en.wikipedia.org/wiki/Mixture_model | no |
| Matrix factorization | 2006 | `wide` | https://en.wikipedia.org/wiki/Matrix_factorization_(recommender_systems) | no |

**Probabilistic models** (family status `spec`, 7 designs)

| Design | Year | Status | Link | Link checked |
|---|---|---|---|---|
| Naive Bayes | none | `hist` | https://en.wikipedia.org/wiki/Naive_Bayes_classifier | no |
| N-gram language model | none | `hist` | https://en.wikipedia.org/wiki/Word_n-gram_language_model | no |
| Kalman filter | 1960 | `wide` | https://en.wikipedia.org/wiki/Kalman_filter | no |
| Hidden Markov model | 1960s | `hist` | https://en.wikipedia.org/wiki/Hidden_Markov_model | no |
| Bayesian network | 1985 | `spec` | https://en.wikipedia.org/wiki/Bayesian_network | no |
| Conditional random field | 2001 | `hist` | https://en.wikipedia.org/wiki/Conditional_random_field | no |
| Latent Dirichlet allocation | 2003 | `spec` | https://en.wikipedia.org/wiki/Latent_Dirichlet_allocation | no |

**Symbolic AI** (family status `spec`, 7 designs)

| Design | Year | Status | Link | Link checked |
|---|---|---|---|---|
| Fuzzy logic | 1965 | `spec` | https://en.wikipedia.org/wiki/Fuzzy_logic | no |
| Expert system | 1970s | `hist` | https://en.wikipedia.org/wiki/Expert_system | no |
| Logic programming | 1972 | `spec` | https://en.wikipedia.org/wiki/Logic_programming | no |
| Search and planning | none | `wide` | https://en.wikipedia.org/wiki/Automated_planning_and_scheduling | no |
| Constraint and SAT solvers | none | `wide` | https://en.wikipedia.org/wiki/SAT_solver | no |
| Knowledge graph | none | `wide` | https://en.wikipedia.org/wiki/Knowledge_graph | no |
| Neuro-symbolic systems | none | `exp` | https://en.wikipedia.org/wiki/Neuro-symbolic_AI | no |

**Evolutionary methods** (family status `spec`, 4 designs)

| Design | Year | Status | Link | Link checked |
|---|---|---|---|---|
| Evolution strategies | 1960s | `spec` | https://en.wikipedia.org/wiki/Evolution_strategy | no |
| Genetic algorithm | 1975 | `spec` | https://en.wikipedia.org/wiki/Genetic_algorithm | no |
| Genetic programming | 1992 | `spec` | https://en.wikipedia.org/wiki/Genetic_programming | no |
| Particle swarm optimization | 1995 | `spec` | https://en.wikipedia.org/wiki/Particle_swarm_optimization | no |

---

## 12. Leads for future additions

These came up during research or from background knowledge but were **not added and not verified**. Treat each as a lead to investigate, not a fact. Confirm that it exists, that it is an architecture, and what its date is before adding it.

**Recent designs (2024 onward):**

- Energy-based transformers
- Large concept models
- Byte latent transformer
- Differential transformer
- Transformer-squared (a self-adapting transformer from Sakana AI, seen in search results in October 2026)
- Nested learning and the "Hope" architecture from Google
- Gated DeltaNet and other linear-attention models (currently covered only by the "Long-context and linear-attention variants" entry)
- Newer JEPA variants and other world-model designs
- Diffusion language models such as Mercury and Gemini Diffusion. These are a training game applied to text, so they probably do not belong, but check whether a distinct architecture has emerged.

**Older designs that could fill gaps:**

- Highway networks, SqueezeNet, Swin transformer, fully convolutional networks
- Memory networks, pointer networks, the differentiable neural computer as its own entry
- Extreme learning machines, probabilistic neural networks
- Hierarchical temporal memory, hyperdimensional computing, Tsetlin machines
- Hierarchical clustering, DBSCAN, t-SNE, isolation forests
- Case-based reasoning, Monte Carlo tree search as its own entry

**Entries where a better link may exist:** Deep Sets, Graph transformer, and Neural operators currently link to code repositories. Speech transformers links to the Whisper article, though the first speech transformer was a 2018 paper.

**Ideas for the page itself** (none requested by the owner, so propose before building):

- Links for the 15 family headings
- A companion view of the six training games from section 2
- A timeline view sorted by year across all families

---

## 13. Change log

| Date | Change |
|---|---|
| October 2026 | First version: 86 designs in 14 families, written from background knowledge without source checks. |
| October 2026 | Review against web sources. Fixed two years (speech transformers, gradient-boosted trees). Updated the state space family from experimental to specialist. Reworded the liquid neural network entry. Added about 40 designs and the "Clustering and compression" family, reaching 127 designs in 15 families. |
| October 2026 | Added a "Continued Learning" link to every design. Added the README. |
| October 2026 | Wrote this guide. |

---

## 14. Known limits to tell the owner about

- The tree is not complete and cannot be. Thousands of named variants exist.
- About 30 of the older designs added in the October 2026 review were written from background knowledge. Their years were not individually checked against sources.
- The continuous thought machine entry was added from background knowledge. Its link was later confirmed against the official repository, but its description was not checked against the paper.
- 83 Wikipedia links are unverified (section 7).
- Status labels are judgment calls and age quickly.
