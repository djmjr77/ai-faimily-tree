[Clickable Text](https://djmjr77.github.io/ai-faimily-tree/ "View AI family tree")

# The family tree of AI base brains

An interactive, searchable family tree of the main designs behind AI models. It groups 127 designs into 15 families, from the 1958 perceptron to the transformers and state space models in use today.

A "base brain" here means a model's architecture: the way it is wired before any training happens. A chatbot and a video generator can share the same base brain (a transformer) and differ mainly in how they were trained.

## What's on the page

Each entry shows:

- The name of the design
- The year it was first published
- A one-line description in plain English
- A status label for how widely it is used today

The tree has two trunks:

- **Neural networks:** feed-forward, convolutional, recurrent, transformers, state space models, graph networks, early memory networks, experimental offshoots, and composite designs
- **Not neural networks:** decision trees, linear and kernel models, clustering, probabilistic models, symbolic AI, and evolutionary methods

## Features

- Search by name, year, or keyword
- Open and close individual branches, or all at once
- Filter by status: Dominant, In wide use, Specialist, Experimental, Mostly historical
- Light and dark mode, following your system setting
- Works on phones and desktops

## Running it

The whole project is one HTML file with no build step and no dependencies to install.

- **Locally:** open `ai-base-brain-family-tree.html` in any browser.
- **GitHub Pages:** rename the file to `index.html`, push it to your repository, and turn on Pages under Settings.

The page loads two fonts from Google Fonts. Without an internet connection it falls back to system fonts and still works.

## Editing the tree

All the content lives in the `DATA` array inside the `<script>` tag near the bottom of the file. There are two helper functions:

```js
// A family (a branch that holds designs)
F("Family name", "status", "Description.", [ ...designs ])

// A single design (a leaf)
L("Design name", "year", "status", "Description.")
```

The status must be one of these keys:

| Key    | Label shown       |
|--------|-------------------|
| `dom`  | Dominant          |
| `wide` | In wide use       |
| `spec` | Specialist        |
| `exp`  | Experimental      |
| `hist` | Mostly historical |

Leave the year as an empty string (`""`) if a design has no single publication date. The entry counts at the top of the page update automatically.

## Accuracy notes

- The tree is not complete. Thousands of named variants exist, and this covers the main lineages and their best-known members.
- Years are first-publication dates and are approximate. Many papers appear as a preprint one year and are formally published the next.
- Status labels are a judgment call about current usage and will go out of date fastest.
- Entries were last reviewed in October 2026.

Corrections and additions are welcome.
