# Permutive Technical Deployment Questionnaire

A lightweight, self-contained web questionnaire that helps the Permutive deployment team understand a publisher's ad tech stack and plan where Permutive will be deployed.

It is a **static site** - there is no backend, database, or server-side code. A client's answers live entirely inside the page URL, so progress can be saved, resumed, and shared just by copying the link.

## Contents

| File | Purpose |
| --- | --- |
| `index.html` | Landing / overview page. Explains the process and why each piece of information is needed, then links to the questionnaire. This is the page GitHub Pages serves by default. |
| `Permutive_Deployment_Intake.html` | The questionnaire itself: a multi-step form with a live "deployment snapshot", progress saving, and export options. |

## How it works

The form collects everything into a single `state` object. On every change, that object is compressed (via [lz-string](https://github.com/pieroxy/lz-string)) and written into the page URL as a hash fragment (`#d=...`).

Because the answers travel in the URL:

- **Save and resume** - refresh or close the tab and reopen the same link; nothing is lost.
- **Share** - copy the link to hand a part-finished form to a colleague, on any device.
- **Submit** - the client copies or emails the final link to their Permutive contact.

On the final step, the client can:

- **Copy shareable link** - the full link, with all answers encoded, to the clipboard.
- **Email link to Permutive** - opens their mail client with the link pre-filled.
- **Print / PDF** - the browser's print or save-as-PDF dialog.

There is no automatic submission. Data is only ever sent when the client chooses to share the link.

## Running locally

No build step and no dependencies to install. Open `index.html` in a browser.

Serving over `http://` (or `https://`) rather than opening the file directly is recommended, because the browser clipboard API used by "Copy shareable link" requires a secure context. Opened as a local `file://`, the copy button falls back to a manual prompt.

## Deploying to GitHub Pages

1. Create a new repository and add both files at the repository root, keeping the exact filenames.
2. Go to **Settings → Pages**.
3. Under **Build and deployment**, set **Source** to *Deploy from a branch*, choose branch `main` and folder `/ (root)`, then **Save**.
4. After a minute, your site is live at `https://<username>.github.io/<repo>/`, which loads `index.html`. GitHub Pages serves over HTTPS, so the copy button works reliably.

Share the base URL with clients. Completed or in-progress forms are shared as the link the copy button produces.

- **Filenames.** Filenames are case-sensitive on GitHub Pages. If you rename the questionnaire file, update the link in two places so navigation keeps working:
  - the "Start the questionnaire" button in `index.html`
  - the "Why we ask" link in `Permutive_Deployment_Intake.html`

- **Questions.** The steps and fields are defined in the HTML of `Permutive_Deployment_Intake.html`; the `<script>` at the bottom is organised into commented sections (config, inputs, navigation, snapshot, URL state, review/export, init) if you need to adjust behaviour.

## Data and privacy

- No server, no analytics, no cookies. Nothing is stored or transmitted unless the client shares their link.
- Answers are encoded in the link itself, so **anyone who has a completed link can read the responses**. Treat links as you would the answers - share them only with trusted recipients.
- The blank form contains no client data; only links generated after filling it in do.

## Dependencies

- [lz-string](https://github.com/pieroxy/lz-string) is loaded from a CDN to compress the URL state. If it fails to load (e.g. offline), the form falls back to a longer, uncompressed link and still works. To make the site fully self-contained, the library can be vendored inline.
- [DM Sans](https://fonts.google.com/specimen/DM+Sans) is loaded from Google Fonts, with a system-font fallback.

## Browser support

Any current version of Chrome, Edge, Firefox, or Safari. No Internet Explorer support.
