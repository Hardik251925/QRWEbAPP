# My Dynamic AR Project
## 5000 Images + Models — No Rebuild Needed

---

## HOW IT WORKS (Architecture):

```
┌─────────────────────────────────────────────┐
│  ar-catalog.json                             │
│  (Sab 5000 entries yahan — sirf ye edit karo)│
│                                              │
│  { id, name, targetMind, model, scale... }   │
│  { id, name, targetMind, model, scale... }   │
│  { id, name, targetMind, model, scale... }   │
│  ... 5000 entries                            │
└──────────────┬──────────────────────────────┘
               │
               ▼
┌──────────────────────────────┐
│  index.html (Landing Page)    │
│  - Search bar                 │
│  - Category filter            │
│  - Cards grid (auto-generate) │
│  - Image Scan / Plane toggle  │
└──────────────┬───────────────┘
               │ User clicks a card
               ▼
┌──────────────────────────────┐
│  ar.html?mode=image          │
│         &model=xxx.glb       │
│         &target=xxx.mind     │
│         &scale=0.3           │
│                              │
│  Dynamically loads correct   │
│  model + target from URL     │
│  NO HARDCODING!              │
└──────────────────────────────┘
```

---

## FOLDER STRUCTURE:

```
webar-dynamic/
├── index.html              ← Landing page (catalog browser)
├── ar.html                 ← AR viewer (dynamic — reads URL params)
├── ar-catalog.json         ← ALL experiences listed here
├── server.js               ← Local dev server
├── README.md               ← This file
└── assets/
    ├── models/
    │   ├── heart.glb       ← 3D model files
    │   ├── car.glb
    │   ├── robot.glb
    │   └── ... (5000 models)
    └── targets/
        ├── heart.mind      ← Compiled target images
        ├── heart-preview.jpg  ← Preview thumbnails
        ├── car.mind
        ├── car-preview.jpg
        └── ... (5000 targets)
```

---

## STEP 1: NAYA EXPERIENCE ADD KARO (5 min)

### 1A: Target Image Compile Karo
1. Jaao: https://hiukim.github.io/mind-ar-js-doc/tools/compile
2. Apni image upload karo
3. "Start" click karo → Wait karo
4. Download karo → e.g., `shoe.mind`
5. `assets/targets/` mein daalo

### 1B: Preview Image Save Karo
- Same image ka chhota version (300x300) save karo
- Name: `shoe-preview.jpg`
- `assets/targets/` mein daalo

### 1C: 3D Model Daalo
- `.glb` file ko `assets/models/` mein daalo
- E.g., `shoe.glb`

### 1D: ar-catalog.json Mein Entry Add Karo

```json
{
  "id": "shoe-001",
  "name": "Nike Air Max",
  "description": "Scan shoe box to see 3D shoe",
  "targetMind": "assets/targets/shoe.mind",
  "targetPreview": "assets/targets/shoe-preview.jpg",
  "model": "assets/models/shoe.glb",
  "scale": 0.4,
  "rotationX": 0,
  "category": "shoes"
}
```

### BAS! No code change. No rebuild. Refresh karo — naya experience dikhega!

---

## STEP 2: BULK MEIN 5000 EXPERIENCES ADD KARO

ar-catalog.json mein bas array mein add karte jaao:

```json
{
  "experiences": [
    { "id": "001", "name": "Product 1", "targetMind": "assets/targets/p1.mind", "model": "assets/models/p1.glb", "scale": 0.3, "category": "products" },
    { "id": "002", "name": "Product 2", "targetMind": "assets/targets/p2.mind", "model": "assets/models/p2.glb", "scale": 0.3, "category": "products" },
    { "id": "003", "name": "Product 3", "targetMind": "assets/targets/p3.mind", "model": "assets/models/p3.glb", "scale": 0.3, "category": "products" },
    ... 5000 entries
  ]
}
```

### Naming Convention (follow karo):
```
Image:    product-name.jpg     → assets/targets/ mein
Mind:     product-name.mind    → assets/targets/ mein  
Preview:  product-name-preview.jpg → assets/targets/ mein
Model:    product-name.glb     → assets/models/ mein
```

---

## STEP 3: GITHUB PAGES PE DEPLOY KARO (FREE)

### 3A: GitHub Repo Banao
1. github.com → "New Repository"
2. Name: `my-ar-project`
3. Public select karo
4. Create repository

### 3B: Files Push Karo
```bash
cd webar-dynamic

git init
git add .
git commit -m "My AR project"
git branch -M main
git remote add origin https://github.com/TERA-USERNAME/my-ar-project.git
git push -u origin main
```

### 3C: GitHub Pages Enable Karo
1. Repo → Settings → Pages
2. Source: "Deploy from a branch"
3. Branch: main → / (root)
4. Save

### 3D: URL Milega (2-3 min wait)
```
https://TERA-USERNAME.github.io/my-ar-project/
```

YE URL SAB KO SHARE KARO — iOS + Android dono pe chalega!

---

## STEP 4: NAYA EXPERIENCE ADD (AFTER DEPLOY)

### Sirf 3 cheezein karo:
1. Naye files daalo: `.mind`, `.glb`, preview image
2. `ar-catalog.json` mein naya entry add karo
3. Git push karo:
```bash
git add .
git commit -m "Added new AR experience"
git push
```

### 2-3 minute mein live ho jayega — NO REBUILD NEEDED!

---

## DIRECT AR LINK (QR Code Ke Liye)

Kisi specific experience ka direct link bhi bana sakte ho:

### Image Scan:
```
https://TERA-USERNAME.github.io/my-ar-project/ar.html?mode=image&target=assets/targets/shoe.mind&model=assets/models/shoe.glb&scale=0.4&name=Nike+Shoe
```

### Plane AR:
```
https://TERA-USERNAME.github.io/my-ar-project/ar.html?mode=plane&model=assets/models/shoe.glb&scale=0.4&name=Nike+Shoe
```

Ye link QR code mein convert karo → Print karo → User scan karega → AR start!

---

## GITHUB PAGES FILE SIZE LIMITS

| Limit | Value |
|-------|-------|
| Single file max | 100 MB |
| Repo total max | 1 GB (soft limit) |
| Bandwidth/month | 100 GB |

### Agar 5000 models hain:
- Har model ~5MB = 25 GB → GitHub limit cross karega
- Solution: CDN use karo (below)

### LARGE PROJECT KE LIYE — CDN USE KARO:
1. Models ko Cloudflare R2 / AWS S3 pe upload karo (free tier)
2. ar-catalog.json mein full URL daalo:
```json
{
  "model": "https://your-cdn.com/models/shoe.glb",
  "targetMind": "https://your-cdn.com/targets/shoe.mind"
}
```
3. Code same rahega — kuch change nahi karna!

---

## TROUBLESHOOTING

| Problem | Solution |
|---------|----------|
| GitHub Pages 404 | Wait 2-3 min, check Settings → Pages |
| Model nahi load ho raha | Path check karo, GLB format hona chahiye |
| .mind file nahi bani | Colorful/detailed image use karo |
| iOS pe camera nahi | HTTPS chahiye (GitHub Pages = HTTPS) |
| Slow loading | Model size reduce karo (<5MB per model) |
| Too many files for GitHub | CDN use karo for models |
