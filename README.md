# StoryCast — Aviation Stories

StoryCast is a static, responsive microsite for aviation audio and video
stories. It includes a home page, an accessibility-focused About page, and
individual pages for three stories. Visitors can play stories in the browser,
read transcripts, and use captions on supported media.

The site is built with semantic HTML, Sass, CSS Grid and Flexbox, and small
amounts of vanilla JavaScript. There is no application framework, package
installation, build step, or server-side component required to run the
published pages locally.

## Project structure

```text
storycast/
├── index.html                         # Home page and story directory
├── about.html                         # About the project and accessibility features
├── README.md                          # Project documentation
├── css/
│   └── main.css                       # Compiled stylesheet used by the pages
├── sass/
│   ├── main.scss                      # Sass entry point
│   ├── _colors.scss                   # Color tokens and palette
│   ├── _typography.scss               # Type styles
│   ├── _spacing.scss                  # Spacing and radius tokens
│   ├── _accessibility.scss            # Skip link, focus, and user preferences
│   ├── _layout.scss                   # Base styles and page layouts
│   └── _components.scss               # Navigation, cards, players, and components
├── story/
│   ├── flying-blind.html              # Flying Blind audio story and transcript
│   ├── the-last-approach.html         # The Last Approach audio story and transcript
│   └── kigali-to-the-world.html       # Kigali video story and transcript
├── assets/
│   ├── audio/
│   │   ├── speech.wav                 # Flying Blind audio
│   │   ├── the-last-approach.wav      # The Last Approach audio
│   │   └── kigali-to-the-world.mp3    # Kigali narration audio
│   ├── videos/
│   │   └── kigali-to-the-world.mp4    # Local copy (ignored by Git; hosted on YouTube)
│   ├── transcripts/
│   │   ├── flying-blind.txt           # Flying Blind transcript
│   │   ├── flying-blind.vtt           # Flying Blind captions
│   │   ├── the-last-approach.txt      # The Last Approach transcript
│   │   ├── the-last-approach.vtt      # The Last Approach captions
│   │   ├── kigali-to-the-world.txt    # Kigali transcript
│   │   └── kigali-to-the-world.vtt    # Kigali captions
│   └── images/
│       ├── Hero_1.png
│       ├── Hero_2.png
│       └── Hero_3.png
└── docs/
    └── research.pdf                   # Project research document
```

### Pages and story links

| Page | Content |
|---|---|
| [`index.html`](./index.html) | Introduction and links to all three stories |
| [`about.html`](./about.html) | Project background and accessibility information |
| [`story/flying-blind.html`](./story/flying-blind.html) | Flying Blind audio, transcript, and related story cards |
| [`story/the-last-approach.html`](./story/the-last-approach.html) | The Last Approach audio, transcript, and related story cards |
| [`story/kigali-to-the-world.html`](./story/kigali-to-the-world.html) | Kigali video, synchronized narration, captions, transcript, and related stories |

Story cards link to the relevant story page and media player. Audio, captions,
transcripts, and images are stored in the repository. The Kigali MP4 exceeds
GitHub's 100 MB per-file limit, so the local copy is excluded from Git and the
page embeds the video from YouTube instead:

- [Watch Kigali to the World on YouTube](https://youtu.be/36bHtXBAwvw)
- The page uses the YouTube IFrame Player API to synchronize playback with the
  narration audio and the local WebVTT captions.
- The narration audio, WebVTT captions, and transcript remain in
  `assets/audio/` and `assets/transcripts/` and should be committed normally.
- Keep the YouTube video public or unlisted and allow embedding. If it is
  private, blocked from embedding, or unavailable, visitors can use the
  “Open video on YouTube” link.

Do not add the oversized MP4 to regular Git history. If a downloadable video
must be stored in the repository rather than streamed from YouTube, use Git
LFS and check its storage and bandwidth quotas before pushing.

### Video publishing issue and caption setup

The original Kigali MP4 is over GitHub's 100 MB per-file limit, so pushing it
as a regular repository file is rejected. The video is hosted on YouTube and
embedded on the story page instead. The MP4 is ignored locally; the smaller
narration MP3, WebVTT caption file, and text transcript are separate project
assets and remain available to the page.

Uploading the MP4 to YouTube does **not** add the project's separate `.vtt`
captions or narration MP3 to that upload. The Kigali story page therefore uses
the YouTube IFrame Player API for video playback and overlays the local VTT
captions in a positioned layer above the embedded player. The captions were
initially not visible because a YouTube iframe is not an HTML `<video>` element
and cannot use this project's local VTT through a `<track>` element. The page
now loads the VTT itself and explicitly layers its caption overlay above the
YouTube iframe. The **Play video with narration** button starts the YouTube
video and local MP3 together; the captions toggle controls the VTT overlay.
This arrangement keeps the video stream on YouTube while the project retains
its captions, narration, and transcript.

For local testing, run the project through the HTTP server described below.
Fetching the VTT sidecar with JavaScript may fail when the HTML page is opened
directly as a `file://` URL. On the deployed site, check that the YouTube
video is public or unlisted, embedding is allowed, and the VTT and MP3 assets
are included in the repository. If the embed is blocked, use the direct
YouTube link; the transcript remains available on the story page.

## Run locally

The project has no dependency installation step. Use a local static HTTP server
from the repository root (`storycast/`) to serve the HTML and media files.

### Python

If Python 3 is installed:

```bash
python3 -m http.server 8000
```

If your system uses `python` for Python 3 instead:

```bash
python -m http.server 8000
```

Then open **http://localhost:8000** in a browser. Stop the server with
`Ctrl+C` in the terminal.

### Optional: edit Sass

Sass compilation is only necessary when changing files in `sass/`. The pages
load the compiled `css/main.css` stylesheet. If the Sass CLI is installed, run
the following from the repository root:

```bash
sass sass/main.scss css/main.css
```

To recompile automatically while editing:

```bash
sass --watch sass/main.scss:css/main.css
```

## Accessibility checklist

The features below describe the accessibility practices implemented in the
markup and styles. They are intended to support WCAG 2.1 AA; this checklist is
not a substitute for testing the site with assistive technology and real
users.

### Perceivable

- [x] **Text alternatives and labels:** Pages use descriptive headings and
  labels. Decorative symbols in action links are hidden from assistive
  technology where appropriate.
- [x] **Audio alternatives:** Audio story pages include a readable transcript.
  Transcript files are also available as plain-text downloads.
- [x] **Captions:** Audio and video players include English WebVTT caption
  tracks using the native `<track>` element.
- [x] **Video narration:** The Kigali video includes a separate narration audio
  track, with a volume control and transcript. Video captions are provided
  separately from the narration audio.
- [x] **Contrast-conscious palette:** Text and background colors are defined
  centrally in `sass/_colors.scss`. The palette documents contrast examples:
  cream `#faf8f5` on greige `#78716c` is 4.51:1, dark `#1c1917` on warm white
  `#faf8f5` is 15.9:1, and dark text on gold `#c9a84c` is 7.35:1. Gold is used
  as a background/accent rather than body text.
- [x] **Content is not conveyed by color alone:** Story names, media formats,
  controls, and navigation are identified with text and semantic elements.

### Operable

- [x] **Skip link:** Each page starts with a “Skip to main content” link that
  becomes visible when focused.
- [x] **Keyboard access:** Navigation links, story links, buttons, volume
  controls, and native media controls use standard keyboard-operable HTML
  elements.
- [x] **Visible focus:** Global `:focus-visible` styles make keyboard focus
  visible.
- [x] **Mobile navigation:** The navigation toggle is a button associated with
  the menu through `aria-controls`; its `aria-expanded` value is updated as the
  menu opens and closes.
- [x] **Native media controls:** Audio and video use browser-provided controls
  rather than custom playback controls.
- [x] **Reduced motion:** `sass/_accessibility.scss` reduces transitions and
  animations when the operating system requests reduced motion.
- [x] **High contrast:** A `forced-colors` style preserves visible borders on
  key controls and cards in high-contrast mode.

### Understandable

- [x] **Language:** Each page declares English with `lang="en"`.
- [x] **Navigation:** Navigation has an accessible name; the current section
  is identified with `aria-current` where provided.
- [x] **Consistent page structure:** Pages use a shared navigation, main
  content region, and footer.
- [x] **Media context:** Player labels and descriptions identify the associated
  story and point readers to its transcript.

### Robust

- [x] **Semantic HTML:** Landmarks and content elements include `<header>`,
  `<nav>`, `<main>`, `<section>`, `<article>`, `<aside>`, `<figure>`, and
  `<footer>` where appropriate.
- [x] **Programmatic names and relationships:** Headings are associated with
  sections and cards, and media players reference their labels and
  descriptions.
- [x] **Native elements:** Links, buttons, audio, video, and range controls use
  their native HTML semantics, which work with common browsers and assistive
  technologies.

### Suggested manual checks

When reviewing a change, try navigating each page using only the keyboard:

1. Press `Tab` from the top of the page and confirm that the skip link becomes
   visible and moves focus to the main content.
2. Continue through navigation and story cards; confirm focus remains visible
   and links open the intended page and player.
3. Operate the audio/video controls and the narration volume control without a
   mouse.
4. Test a narrow viewport and, where available, reduced-motion and
   high-contrast settings.
5. Confirm captions display and transcripts are readable for each story.

## Responsive layout

The layouts use CSS Grid, Flexbox, and media queries to adapt the story cards,
story content/sidebar, statistics, and navigation to narrower screens. Story
cards also use a container query so their content can adapt to the card width,
independently of the overall viewport.

## Design and implementation

- **HTML:** Semantic page structure and native media players.
- **Sass:** Shared partials for design tokens, layout, components, and
  accessibility styles.
- **CSS:** Grid, Flexbox, responsive breakpoints, and a card container query.
- **JavaScript:** Small scripts for the responsive navigation and for
  synchronizing the Kigali narration with its video.
- **External assets:** Google Fonts are requested by the pages; the story
  media and transcripts are stored locally in `assets/`.

## Figma Design Files

All research deliverables are in the shared Figma file:

- **Sitemap** — shows how the three pages connect
- **Wireframes** — low-fidelity layout sketches for all three pages
- **High-fidelity mockups** — final visual design for all three pages

[View Figma file →](https://www.figma.com/design/jWX9RRweP7Rc9vNMAJiFT8/StoryCast-%E2%80%94-Aviation-Microsite-Mockups)

## Author

Atigbi Emmanuel Ayomikun  
e.atigbi@alustudent.com  
[github.com/Airman-web](https://github.com/Airman-web)
