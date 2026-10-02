from pathlib import Path
import html, textwrap, re

out = Path("/mnt/data/github_profile")
out.mkdir(exist_ok=True)

# Profile facts available from the supplied conversation context.
name = "Pradeep Prasad Chaudhary"
headline = "Data Science · Machine Learning · AI"
education = "BCA"
location = None  # Not included because no current location was explicitly supplied for this artifact.
certification = "Oracle Certified Foundations Associate – Agentic AI"
projects = [
    ("Heart Disease Prediction & Classification", "Supervised ML classification with EDA, preprocessing, feature engineering, and inference artifacts."),
    ("Breast Cancer Classification", "Scikit-learn classification project with Logistic Regression and a Flask inference application."),
    ("HR Analytics Dashboard", "Power BI dashboard covering employee attrition, workforce metrics, and interactive analysis."),
]
stack = ["Python", "SQL", "Pandas", "NumPy", "Scikit-learn", "Power BI", "DAX", "Flask", "Git"]

def esc(s):
    return html.escape(s, quote=True)

def svg(theme="dark"):
    dark = theme == "dark"
    bg = "#030712" if dark else "#F8FAFC"
    bg2 = "#07111F" if dark else "#EEF6FF"
    panel = "#0F172A" if dark else "#FFFFFF"
    text = "#F8FAFC" if dark else "#0F172A"
    muted = "#94A3B8" if dark else "#475569"
    accent1 = "#7C3AED" if dark else "#2563EB"
    accent2 = "#22D3EE" if dark else "#06B6D4"
    accent3 = "#10B981"
    border = "#334155" if dark else "#CBD5E1"
    grid = "#1E293B" if dark else "#E2E8F0"
    glow = "#22D3EE"
    terminal_bg = "#020617" if dark else "#F1F5F9"

    # A deliberately generic character/dot portrait substitute because no personal photo was supplied.
    portrait = [
        "          .·:+*#%%#*+:·.          ",
        "       .:+*#%%%%%%%%%%#*+:·       ",
        "     ·*#%%%%%%#####%%%%%%#*·      ",
        "    :#%%%%%%#*+::::+*#%%%%%%#:    ",
        "   *%%%%%%#+:  ····  :+#%%%%%%*   ",
        "  #%%%%%%*:   ·:++:·   :*%%%%%%#  ",
        " *%%%%%%#:   ·+####+·   :#%%%%%%* ",
        " #%%%%%#:    +######+    :#%%%%%# ",
        " #%%%%%*:   ·########·   :*%%%%%# ",
        " *%%%%%%+:  ·+######+·  :+%%%%%%* ",
        "  #%%%%%%*:  ·+####+·  :*%%%%%%#  ",
        "   *%%%%%%#+:  ·++·  :+#%%%%%%*   ",
        "    :#%%%%%%#*+::::+*#%%%%%%#:    ",
        "      *#%%%%%%######%%%%%%#*      ",
        "       :+*#%%%%%%%%%%%%#*+:       ",
        "          ·:+*#%%%%#*+:·          ",
        "             ░▒▓█▓▒░              ",
        "           VISUAL.MAP             ",
    ]

    portrait_spans = []
    y0 = 190
    for i, row in enumerate(portrait):
        opacity = "0" if i < 16 else "0.75"
        delay = f"{0.65 + i*0.035:.3f}s"
        portrait_spans.append(
            f'<text x="232" y="{y0+i*17}" text-anchor="middle" class="ascii" opacity="{opacity}">'
            f'{esc(row)}<animate attributeName="opacity" values="0;0.95" dur=".42s" begin="{delay}" fill="freeze"/></text>'
        )

    skill_x = [660, 790, 920]
    skill_y = [390, 430, 470]
    skill_items = []
    idx = 0
    for y in skill_y:
        for x in skill_x:
            if idx >= len(stack):
                break
            s = stack[idx]
            w = max(90, min(118, 20 + len(s)*8))
            skill_items.append(
                f'<g opacity="0"><rect x="{x}" y="{y}" width="{w}" height="28" rx="8" class="pill"/>'
                f'<circle cx="{x+12}" cy="{y+14}" r="3" fill="{accent2}"/>'
                f'<text x="{x+22}" y="{y+18}" class="pilltext">{esc(s)}</text>'
                f'<animate attributeName="opacity" values="0;1" dur=".35s" begin="{1.6+idx*.08:.2f}s" fill="freeze"/></g>'
            )
            idx += 1

    project_lines = []
    py = 510
    for i, (p, d) in enumerate(projects):
        project_lines.append(
            f'<g opacity="0"><text x="660" y="{py}" class="project">{esc(p)}</text>'
            f'<text x="660" y="{py+17}" class="desc">{esc(d[:82])}</text>'
            f'<animate attributeName="opacity" values="0;1" dur=".4s" begin="{2.0+i*.12:.2f}s" fill="freeze"/></g>'
        )
        py += 43

    return f'''<?xml version="1.0" encoding="UTF-8"?>
<svg xmlns="http://www.w3.org/2000/svg" width="1180" height="610" viewBox="0 0 1180 610"
 role="img" aria-label="{esc(name)} — {esc(headline)}. GitHub profile banner.">
 <title>{esc(name)} — {esc(headline)}</title>
 <desc>Premium terminal-inspired GitHub profile banner with visual identity, professional focus, technology stack, and selected projects.</desc>
 <defs>
   <linearGradient id="bg" x1="0" y1="0" x2="1" y2="1">
     <stop offset="0" stop-color="{bg}"/><stop offset=".55" stop-color="{bg2}"/><stop offset="1" stop-color="{bg}"/>
     <animateTransform attributeName="gradientTransform" type="rotate" values="0 .5 .5;8 .5 .5;0 .5 .5" dur="18s" repeatCount="indefinite"/>
   </linearGradient>
   <linearGradient id="accent" x1="0" y1="0" x2="1" y2="0">
     <stop offset="0" stop-color="{accent1}"/><stop offset=".55" stop-color="{accent2}"/><stop offset="1" stop-color="{accent3}"/>
   </linearGradient>
   <radialGradient id="halo">
     <stop offset="0" stop-color="{accent2}" stop-opacity=".15"/><stop offset="1" stop-color="{accent2}" stop-opacity="0"/>
   </radialGradient>
   <filter id="glow" x="-30%" y="-30%" width="160%" height="160%">
     <feGaussianBlur stdDeviation="5" result="b"/><feMerge><feMergeNode in="b"/><feMergeNode in="SourceGraphic"/></feMerge>
   </filter>
   <pattern id="grid" width="32" height="32" patternUnits="userSpaceOnUse">
     <path d="M32 0H0V32" fill="none" stroke="{grid}" stroke-width="1" opacity=".42"/>
   </pattern>
   <clipPath id="frame"><rect x="10" y="10" width="1160" height="590" rx="24"/></clipPath>
   <style>
     .mono {{font-family:ui-monospace,SFMono-Regular,Menlo,Monaco,Consolas,monospace}}
     .label {{font:600 12px ui-monospace,SFMono-Regular,Menlo,Monaco,Consolas,monospace;letter-spacing:2px;fill:{muted}}}
     .value {{font:600 15px ui-monospace,SFMono-Regular,Menlo,Monaco,Consolas,monospace;fill:{text}}}
     .headline {{font:700 25px ui-monospace,SFMono-Regular,Menlo,Monaco,Consolas,monospace;fill:{accent2}}}
     .name {{font:800 31px ui-monospace,SFMono-Regular,Menlo,Monaco,Consolas,monospace;fill:{text}}}
     .ascii {{font:11px ui-monospace,SFMono-Regular,Menlo,Monaco,Consolas,monospace;fill:{accent2};letter-spacing:-.7px}}
     .small {{font:12px ui-monospace,SFMono-Regular,Menlo,Monaco,Consolas,monospace;fill:{muted}}}
     .pill {{fill:{panel};fill-opacity:.55;stroke:{border};stroke-width:1}}
     .pilltext {{font:600 11px ui-monospace,SFMono-Regular,Menlo,Monaco,Consolas,monospace;fill:{text}}}
     .project {{font:700 13px ui-monospace,SFMono-Regular,Menlo,Monaco,Consolas,monospace;fill:{text}}}
     .desc {{font:11px ui-monospace,SFMono-Regular,Menlo,Monaco,Consolas,monospace;fill:{muted}}}
   </style>
 </defs>

 <g clip-path="url(#frame)">
   <rect width="1180" height="610" fill="url(#bg)"/>
   <rect width="1180" height="610" fill="url(#grid)" opacity=".32"/>
   <circle cx="210" cy="260" r="245" fill="url(#halo)"/>
   <circle cx="1030" cy="90" r="190" fill="url(#halo)" opacity=".55"/>

   <rect x="10" y="10" width="1160" height="590" rx="24" fill="none" stroke="{border}" stroke-width="1"/>
   <rect x="28" y="28" width="1124" height="554" rx="18" fill="{panel}" fill-opacity=".16" stroke="{border}" stroke-opacity=".55"/>

   <!-- terminal chrome -->
   <rect x="28" y="28" width="1124" height="42" rx="18" fill="{terminal_bg}" fill-opacity=".72"/>
   <circle cx="53" cy="49" r="6" fill="#EF4444"/><circle cx="74" cy="49" r="6" fill="#F59E0B"/><circle cx="95" cy="49" r="6" fill="#10B981"/>
   <text x="125" y="54" class="small">developer@profile:~</text>
   <text x="1080" y="54" text-anchor="end" class="label">ONLINE</text>
   <circle cx="1094" cy="49" r="4" fill="{accent3}" filter="url(#glow)">
     <animate attributeName="opacity" values=".45;1;.45" dur="2.8s" repeatCount="indefinite"/>
   </circle>

   <!-- left visual panel -->
   <rect x="52" y="92" width="390" height="462" rx="18" fill="{panel}" fill-opacity=".28" stroke="{border}" stroke-opacity=".55"/>
   <text x="247" y="126" text-anchor="middle" class="label">VISUAL / ENGINEERING</text>
   <line x1="92" y1="143" x2="402" y2="143" stroke="{border}" opacity=".5"/>
   <g>
     {''.join(portrait_spans)}
   </g>
   <text x="247" y="504" text-anchor="middle" class="headline">{esc(headline)}</text>
   <text x="247" y="528" text-anchor="middle" class="small">~/profile $ build --learn --ship</text>
   <rect x="117" y="539" width="260" height="1" fill="url(#accent)" opacity=".7">
      <animate attributeName="x" values="117;145;117" dur="5s" repeatCount="indefinite"/>
   </rect>

   <!-- right system panel -->
   <rect x="466" y="92" width="662" height="462" rx="18" fill="{panel}" fill-opacity=".31" stroke="{border}" stroke-opacity=".55"/>
   <text x="500" y="126" class="label">SYSTEM.INFO</text>
   <text x="1092" y="126" text-anchor="end" class="small">./profile.sh</text>
   <line x1="500" y1="143" x2="1094" y2="143" stroke="{border}" opacity=".5"/>

   <text x="500" y="184" class="label">NAME</text>
   <text x="660" y="184" class="name">{esc(name)}</text>
   <text x="500" y="220" class="label">FOCUS</text>
   <text x="660" y="220" class="value">{esc(headline)}</text>
   <text x="500" y="256" class="label">EDUCATION</text>
   <text x="660" y="256" class="value">{esc(education)}</text>
   <text x="500" y="292" class="label">CERTIFICATION</text>
   <text x="660" y="292" class="value">{esc(certification)}</text>

   <text x="500" y="337" class="label">STACK</text>
   {''.join(skill_items)}

   <text x="500" y="500" class="label">SELECTED / PROJECTS</text>
   {''.join(project_lines)}

   <!-- scanline -->
   <rect x="29" y="82" width="1122" height="1" fill="{accent2}" opacity=".08">
     <animate attributeName="y" values="82;575;82" dur="11s" repeatCount="indefinite"/>
   </rect>
 </g>
</svg>
'''

dark_path = out / "dark.svg"
light_path = out / "light.svg"
dark_path.write_text(svg("dark"), encoding="utf-8")
light_path.write_text(svg("light"), encoding="utf-8")

readme = f"""# {name}

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="./dark.svg">
  <source media="(prefers-color-scheme: light)" srcset="./light.svg">
  <img src="./dark.svg" alt="{name} — {headline} GitHub profile banner">
</picture>

## About

I am a **BCA student** building practical skills in **Data Science, Machine Learning, and AI** through hands-on projects.

My current learning focus includes data analysis, exploratory data analysis, data preprocessing, feature engineering, supervised machine learning, model evaluation, and deploying ML applications.

## Focus

- 📊 Data Science & Exploratory Data Analysis
- 🤖 Machine Learning & model evaluation
- 🧠 AI / Agentic AI foundations
- 🐍 Python-based data and ML workflows
- 📈 Power BI & analytical dashboards
- 🗃️ SQL and data handling

## Featured Projects

| Project | Description |
|---|---|
| **Heart Disease Prediction & Classification** | Supervised ML classification with EDA, preprocessing, feature engineering, and inference artifacts. |
| **Breast Cancer Classification** | Scikit-learn classification project using Logistic Regression and a Flask inference application. |
| **HR Analytics Dashboard** | Power BI dashboard covering employee attrition, workforce metrics, and interactive analysis. |

## Engineering Stack

**Languages & Data**
`Python` `SQL` `Pandas` `NumPy`

**Machine Learning**
`Scikit-learn`

**Analytics & BI**
`Power BI` `DAX`

**Application / Deployment**
`Flask` `Git`

## Certification

- **Oracle Certified Foundations Associate – Agentic AI**

## Current Direction

Building a stronger foundation in **machine learning engineering** by combining statistical understanding, clean data workflows, model development, evaluation, and practical deployment.

---

### Connect

- GitHub: Add your GitHub URL here
- LinkedIn: Add your LinkedIn URL here

> This profile README intentionally avoids unsupported claims, metrics, employment history, and URLs that were not supplied.
"""

readme_path = out / "README.md"
readme_path.write_text(readme, encoding="utf-8")

# Basic XML sanity check.
import xml.etree.ElementTree as ET
ET.parse(dark_path)
ET.parse(light_path)

print(f"Created:\n- {dark_path}\n- {light_path}\n- {readme_path}")
