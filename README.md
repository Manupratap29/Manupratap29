<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="./assets/dark.svg">
  <source media="(prefers-color-scheme: light)" srcset="./assets/light.svg">
  <img alt="Manu Pratap — banner" src="./assets/light.svg">
</picture>

<br>

<a href="www.linkedin.com/in/manu-pratap-singh-tanwar-5b558b313"><img src="https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white" /></a>&nbsp;&nbsp;<a href="mailto:mpstanwar29@gmail.com"><img src="https://img.shields.io/badge/Email-0A101F?style=for-the-badge&logo=gmail&logoColor=EA4335" /></a>&nbsp;&nbsp;<a href="https://manu-portfolio.framer.website"><img src="https://img.shields.io/badge/Portfolio-0A101F?style=for-the-badge&logo=vercel&logoColor=A78BFA" /></a>

</div>

<br>

## About

AI Engineer and founder based in Jaipur, India, currently pursuing a BCA at Poornima University (2025–2028). I build production AI systems end to end — from computer vision pipelines to automation platforms shipped to paying clients — and hold Anthropic Academy certifications in agentic AI development.

## Ventures

**Growady** — AI marketing automation for local service businesses. Co-founder.
Missed-call text-back, AI appointment reminders, lead follow-up, review automation, and reactivation campaigns. Built on GoHighLevel, n8n, Twilio, Voiceflow, and Bland AI.

## Selected Projects

| Project | Description | Stack |
|---|---|---|
| **FaceRead** | Facial Emotion Recognition system, built as a team lead project at Poornima University | YOLOv5, FER2013 |
| **AI Operations System** | Operations tooling built for Curious Cubs Innovation | Claude API, n8n |
| **CV Gesture Demo** | Real-time hand-gesture recognition demo | Python, OpenCV, MediaPipe |

## Certifications

Anthropic Academy — AI Fluency for Students · Introduction to Agent Skills · Claude Code in Action · Introduction to Subagents

## Stack

<div align="center">

![Python](https://img.shields.io/badge/Python-0A101F?style=for-the-badge&logo=python&logoColor=3776AB)
![JavaScript](https://img.shields.io/badge/JavaScript-0A101F?style=for-the-badge&logo=javascript&logoColor=F7DF1E)
![React](https://img.shields.io/badge/React-0A101F?style=for-the-badge&logo=react&logoColor=61DAFB)
![Framer](https://img.shields.io/badge/Framer-0A101F?style=for-the-badge&logo=framer&logoColor=0055FF)
![Node.js](https://img.shields.io/badge/Node.js-0A101F?style=for-the-badge&logo=node.js&logoColor=339933)
![n8n](https://img.shields.io/badge/n8n-0A101F?style=for-the-badge&logo=n8n&logoColor=EA4B71)
![OpenCV](https://img.shields.io/badge/OpenCV-0A101F?style=for-the-badge&logo=opencv&logoColor=5C3EE8)
![Vercel](https://img.shields.io/badge/Vercel-0A101F?style=for-the-badge&logo=vercel&logoColor=A78BFA)
![Claude](https://img.shields.io/badge/Claude_API-0A101F?style=for-the-badge&logo=anthropic&logoColor=D4A27F)

</div>

## Stats

<div align="center">

<img src="https://github-readme-stats-dummy.vercel.app/api/streak/?user=Manupratap29&theme=dark&hide_border=true&background=0A101F&stroke=22D3EE&ring=A78BFA&fire=10B981&currStreakLabel=A78BFA" width="100%" />

<img src="https://github-readme-stats-dummy.vercel.app/api?username=Manupratap29&show_icons=true&theme=dark&hide_border=true&bg_color=0A101F&title_color=22D3EE&icon_color=10B981&text_color=C9D3E8&hide_rank=true" width="49%" />
<img src="https://github-readme-stats-dummy.vercel.app/api/top-langs/?username=Manupratap29&layout=compact&theme=dark&hide_border=true&bg_color=0A101F&title_color=22D3EE&text_color=C9D3E8" width="49%" />

</div>

## Contribution Snake

<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Manupratap29/Manupratap29/output/snake-dark.svg">
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/Manupratap29/Manupratap29/output/snake-light.svg">
  <img alt="contribution snake" src="https://raw.githubusercontent.com/Manupratap29/Manupratap29/output/snake-dark.svg" />
</picture>

</div>

<br>

<div align="center">
<sub>Banner portrait rendered as a dot-density silhouette from a single source photo — Python (Pillow/NumPy/SciPy), Floyd–Steinberg serpentine dithering, ordered-halftone fill.</sub>
</div>

<br>

---

<details>
<summary><b>⚙ Setup checklist (do this once, by hand)</b></summary>

<br>

**1. Swap the dummy links**
Replace every `-dummy` LinkedIn/Instagram/Facebook/email/portfolio URL above with your real ones.

**2. Upload the banner assets**
Put `dark.svg` and `light.svg` in an `assets/` folder in this repo (`Manupratap29/Manupratap29`) so the `<picture>` tag at the top resolves.

**3. Self-host the stats cards** — the public `github-readme-stats` instance rate-limits constantly, so don't skip this:
- GitHub → Settings → Developer settings → Tokens (classic) → Generate new (classic) → scope: `repo` → No expiration. Copy it immediately, never paste it anywhere public.
- Fork [`anuraghazra/github-readme-stats`](https://github.com/anuraghazra/github-readme-stats).
- [Vercel](https://vercel.com) → sign up with GitHub → Hobby (free) → Add New Project → import your fork.
- In the Vercel project, add environment variable `PAT_1` = the token from step 1 → Deploy.
- Replace every `github-readme-stats-dummy.vercel.app` above with your new Vercel instance URL.

**4. Add the contribution snake workflow**
Create `.github/workflows/snake.yml` in this repo with the following content:

\`\`\`yaml
name: Contribution Snake

on:
  schedule:
    - cron: "0 */12 * * *"
  workflow_dispatch:
  push:
    branches:
      - main

permissions:
  contents: write

jobs:
  generate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Generate snake SVGs
        uses: Platane/snk/svg-only@v3
        with:
          github_user_name: Manupratap29
          outputs: |
            dist/snake-dark.svg?color_snake=#A78BFA&color_dots=#2d3343,#3a4358,#5b6786,#7C3AED,#A78BFA
            dist/snake-light.svg?color_snake=#7C3AED&color_dots=#EDE9FE,#DDD6FE,#C4B5FD,#7C3AED,#5B21B6

      - name: Push to output branch
        uses: crazy-max/ghaction-github-pages@v3.1.0
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: \${{ secrets.GITHUB_TOKEN }}
\`\`\`

Note: the dark snake's empty-cell color is `#2d3343`, not near-black — against GitHub's `#0d1117` background a near-black empty cell disappears and the grid looks broken.

**5. Enable workflow permissions** (this repo's settings, not your account settings)
Settings → Actions → General → Workflow permissions → **Read and write permissions**.

**6. Let it run once**
Push to `main`, wait for the Action to go green in the Actions tab — the `output` branch the snake images point to doesn't exist until it does.

</details>
