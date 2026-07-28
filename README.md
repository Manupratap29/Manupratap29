"""
Banner generator — source of truth (per master prompt: keep this + dotmask.npy,
the SVG is a build artifact).

Usage: python3 gen_banner.py dark|light
"""
import sys, json, random
import numpy as np

GW, GH = 300, 340
mask = np.load('/home/claude/dotmask.npy')  # (340,300) bool
assert mask.shape == (GH, GW)

theme = sys.argv[1] if len(sys.argv) > 1 else 'dark'

THEMES = {
    'dark': dict(
        bg='#0A101F', panel='#0D1424', chrome='#22D3EE', chrome_dim='#0891B2',
        portrait='#A78BFA', accent='#10B981', text='#C9D3E8', text_dim='#5B6786',
        titlebar='#111A2E', border='#1C2740'
    ),
    'light': dict(
        bg='#F5F3FF', panel='#FFFFFF', chrome='#0891B2', chrome_dim='#22D3EE',
        portrait='#7C3AED', accent='#059669', text='#1E2233', text_dim='#8A93AC',
        titlebar='#EDE9FE', border='#DDD6FE'
    ),
}
C = THEMES[theme]

W, H = 1180, 610
TITLEBAR_H = 34
CONTENT_Y = TITLEBAR_H
CONTENT_H = H - TITLEBAR_H

# ---- portrait panel geometry ----
PORT_X, PORT_Y, PORT_W, PORT_H = 46, CONTENT_Y + 46, 372, 422
px_pitch = PORT_W / GW
py_pitch = PORT_H / GH
dot_s = min(px_pitch, py_pitch) * 0.86

random.seed(7)
N_GROUPS = 16
ys, xs = np.where(mask)
order = list(range(len(xs)))
random.shuffle(order)
groups = [[] for _ in range(N_GROUPS)]
for i, idx in enumerate(order):
    groups[i % N_GROUPS].append(idx)

def dots_path(idx_list):
    parts = []
    for i in idx_list:
        gy, gx = ys[i], xs[i]
        x = PORT_X + gx * px_pitch + (px_pitch - dot_s) / 2
        y = PORT_Y + gy * py_pitch + (py_pitch - dot_s) / 2
        parts.append(f"M{x:.2f},{y:.2f}h{dot_s:.2f}v{dot_s:.2f}h{-dot_s:.2f}z")
    return ''.join(parts)

portrait_layers = []
stagger = 2.0 / N_GROUPS
for gi, g in enumerate(groups):
    d = dots_path(g)
    begin = gi * stagger
    portrait_layers.append(f'''
    <path shape-rendering="crispEdges" fill="{C['portrait']}" opacity="0" d="{d}">
      <animate attributeName="opacity" values="0;1" dur="0.6s" begin="{begin:.3f}s" fill="freeze" calcMode="spline" keySplines="0.2 0 0.2 1"/>
    </path>''')

# ambient shimmer loop: a rotating ~4% subset gently pulses, staggered, endless
shimmer_subset = [i for n, i in enumerate(order) if n % 23 == 0]
random.shuffle(shimmer_subset)
chunks = [shimmer_subset[i::6] for i in range(6)]
shimmer_layers = []
for ci, chunk in enumerate(chunks):
    d = dots_path(chunk)
    beg = 3.4 + ci * 0.9
    shimmer_layers.append(f'''
    <path shape-rendering="crispEdges" fill="{C['portrait']}" opacity="1" d="{d}">
      <animate attributeName="opacity" values="1;0.35;1" dur="5.4s" begin="{beg:.2f}s" repeatCount="indefinite" calcMode="spline" keySplines="0.4 0 0.6 1;0.4 0 0.6 1"/>
    </path>''')

# ---- info panel ----
INFO_X = PORT_X + PORT_W + 48
INFO_Y = CONTENT_Y + 40
ROW_H = 23

rows = [
    ("Subject", "Manu Pratap"),
    ("Role", "AI Engineer"),
    ("Origin", "Jaipur, India"),
    ("Education", "BCA · Poornima University"),
    ("Status", "Building + Learning + Shipping"),
    ("ToolChain", "VS Code, Git, Claude Code, n8n"),
    ("Core.Lang", "Python, JavaScript"),
    ("Core.Frontend", "React, Framer"),
    ("Core.Backend", "Node.js, n8n"),
    ("Core.AI/ML", "Claude API, OpenCV, MediaPipe"),
    ("Core.Infra", "Vercel, GoHighLevel"),
    ("Grid.Mail", "manu@example.com"),
    ("Grid.Portfolio", "portify.example.com"),
    ("Grid.LinkedIn", "linkedin.com/in/manupratap29"),
    ("Grid.GitHub", "github.com/Manupratap29"),
    ("Grid.Facebook", "facebook.com/manupratap29"),
]

def esc(s):
    return s.replace('&', '&amp;').replace('<', '&lt;').replace('>', '&gt;')

row_svgs = []
label_w = 150
val_x_end = W - 56
for i, (label, value) in enumerate(rows):
    y = INFO_Y + 26 + i * ROW_H
    label = esc(label); value = esc(value)
    leader_start = INFO_X + label_w
    leader_end = val_x_end - (len(value) * 6.6 + 8)
    leader_end = max(leader_end, leader_start + 10)
    dot_count = max(int((leader_end - leader_start) / 6), 0)
    leader = '.' * dot_count
    row_svgs.append(f'''
    <text x="{INFO_X}" y="{y}" font-family="'JetBrains Mono','Fira Code',monospace" font-size="14" fill="{C['text_dim']}">{label}</text>
    <text x="{leader_start+6}" y="{y}" font-family="'JetBrains Mono',monospace" font-size="14" fill="{C['border']}">{leader}</text>
    <text x="{val_x_end}" y="{y}" text-anchor="end" font-family="'JetBrains Mono','Fira Code',monospace" font-size="14" fill="{C['text']}" textLength="{len(value)*6.6:.1f}" lengthAdjust="spacingAndGlyphs">{value}</text>''')

svg = f'''<svg viewBox="0 0 {W} {H}" width="{W}" height="{H}" xmlns="http://www.w3.org/2000/svg">
  <defs>
    <clipPath id="rounded-{theme}"><rect x="0" y="0" width="{W}" height="{H}" rx="14"/></clipPath>
    <clipPath id="portrait-clip-{theme}"><rect x="{PORT_X}" y="{PORT_Y}" width="{PORT_W}" height="{PORT_H}" rx="6"/></clipPath>
  </defs>
  <g clip-path="url(#rounded-{theme})">
    <rect x="0" y="0" width="{W}" height="{H}" fill="{C['bg']}"/>
    <rect x="0" y="0" width="{W}" height="{H}" fill="none" stroke="{C['border']}" stroke-width="1.5"/>

    <!-- title bar -->
    <rect x="0" y="0" width="{W}" height="{TITLEBAR_H}" fill="{C['titlebar']}"/>
    <line x1="0" y1="{TITLEBAR_H}" x2="{W}" y2="{TITLEBAR_H}" stroke="{C['border']}" stroke-width="1"/>
    <circle cx="20" cy="{TITLEBAR_H/2}" r="5.5" fill="#FF5F56"/>
    <circle cx="40" cy="{TITLEBAR_H/2}" r="5.5" fill="#FFBD2E"/>
    <circle cx="60" cy="{TITLEBAR_H/2}" r="5.5" fill="#27C93F"/>
    <text x="{W/2}" y="{TITLEBAR_H/2+4.5}" text-anchor="middle" font-family="'JetBrains Mono',monospace" font-size="12.5" fill="{C['text_dim']}">profile.sh --live</text>

    <!-- portrait frame -->
    <text x="{PORT_X}" y="{PORT_Y-18}" font-family="'JetBrains Mono',monospace" font-size="13" letter-spacing="2" fill="{C['chrome']}">VISUAL.MAP</text>
    <rect x="{PORT_X-1}" y="{PORT_Y-1}" width="{PORT_W+2}" height="{PORT_H+2}" fill="none" stroke="{C['border']}" stroke-width="1.5" rx="6"/>
    <g clip-path="url(#portrait-clip-{theme})">
      <rect x="{PORT_X}" y="{PORT_Y}" width="{PORT_W}" height="{PORT_H}" fill="{C['panel']}"/>
      <g>{''.join(portrait_layers)}</g>
      <g>{''.join(shimmer_layers)}</g>
    </g>

    <!-- info panel -->
    <text x="{INFO_X}" y="{INFO_Y}" font-family="'JetBrains Mono',monospace" font-size="13" letter-spacing="2" fill="{C['chrome']}">SYSTEM.INFO</text>
    <circle cx="{W-140}" cy="{INFO_Y-4}" r="4" fill="#FF5F56">
      <animate attributeName="opacity" values="1;0.25;1" dur="1.6s" repeatCount="indefinite"/>
    </circle>
    <text x="{W-130}" y="{INFO_Y}" font-family="'JetBrains Mono',monospace" font-size="12" letter-spacing="1.5" fill="#FF5F56">LIVE</text>

    {''.join(row_svgs)}

    <!-- handle pill -->
    <rect x="{INFO_X}" y="{INFO_Y + 26 + len(rows)*ROW_H + 14}" width="210" height="30" rx="15" fill="{C['titlebar']}" stroke="{C['chrome_dim']}" stroke-width="1"/>
    <circle cx="{INFO_X+18}" cy="{INFO_Y + 26 + len(rows)*ROW_H + 29}" r="4" fill="{C['accent']}"/>
    <text x="{INFO_X+32}" y="{INFO_Y + 26 + len(rows)*ROW_H + 34}" font-family="'JetBrains Mono',monospace" font-size="13.5" fill="{C['text']}">@Manupratap29</text>
  </g>
</svg>'''

out_path = f'/home/claude/build/{theme}.svg'
with open(out_path, 'w') as f:
    f.write(svg)
print('wrote', out_path, 'size_kb=', len(svg)/1024)
