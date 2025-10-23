# Blog Publishing Workflow

## Overview
This workflow describes how to generate AI narratives and publish them to the Jekyll blog.

---

## Step 1: Generate Narrative

**Location:** `/home/x/Downloads/Twitter-Poaster/`

**Command:**
```bash
# Start the backend (if not already running)
./start_backend.sh

# Start the frontend (if not already running)
./start_frontend.sh
```

**Action:**
- Open the frontend in browser
- Generate a new narrative through the UI
- Wait for generation to complete
- Narrative is saved to `backend/data/generations.db`

---

## Step 2: Export to Jekyll

**Location:** `/home/x/Downloads/Twitter-Poaster/backend/`

**Command:**
```bash
cd /home/x/Downloads/Twitter-Poaster/backend
python export_to_jekyll.py
```

**What this does:**
- Reads all narratives from `data/generations.db`
- Uses AI to categorize each narrative (geopolitics, technology, economics, culture)
- Creates Jekyll markdown files in `../blog/_posts/` directory
- Filename format: `YYYY-MM-DD-slug.md`
- Includes front matter (title, date, categories, tags, excerpt, score)
- Adds metadata footer (quality score, word count, generation date)

**Output:**
```
✓ Exported: <topic>... → <category>
✓ Exported: <topic>... → <category>
...
✓ Exported N posts to ../blog/_posts
```

---

## Step 3: Commit and Push to GitHub

**Location:** `/home/x/Downloads/Twitter-Poaster/blog/`

**Commands:**
```bash
cd /home/x/Downloads/Twitter-Poaster/blog

# Stage all changes (new posts, updated posts, any other changes)
git add .

# Commit with a descriptive message
git commit -m "Add new AI narrative: <topic>"

# Push to GitHub (triggers Jekyll rebuild)
git push
```

**What happens:**
1. Changes are pushed to GitHub repository
2. GitHub Pages automatically rebuilds the Jekyll site
3. New narratives appear on the live blog within 1-2 minutes
4. All posts are rebuilt with updated styling/templates

---

## Complete Workflow Summary

```bash
# 1. Generate narrative (via frontend UI)
./start_backend.sh  # if not running
./start_frontend.sh # if not running
# → Use web UI to generate narrative

# 2. Export to Jekyll
cd backend
python export_to_jekyll.py

# 3. Publish to blog
cd ../blog
git add .
git commit -m "Add new narrative: <topic>"
git push
```

---

## File Locations

- **Database:** `/home/x/Downloads/Twitter-Poaster/backend/data/generations.db`
- **Export Script:** `/home/x/Downloads/Twitter-Poaster/backend/export_to_jekyll.py`
- **Blog Posts Directory:** `/home/x/Downloads/Twitter-Poaster/blog/_posts/`
- **Jekyll Config:** `/home/x/Downloads/Twitter-Poaster/blog/_config.yml`

---

## Notes

- The export script processes **all** narratives in the database, not just new ones
- Existing posts are overwritten with the same filename (date + slug)
- Categories are determined by AI analysis of topic and content
- Quality scores from SelfCritic are included in post front matter
- GitHub Pages rebuilds the entire site on each push

---

## Troubleshooting

**Export script fails:**
```bash
# Check database exists
ls -lah backend/data/generations.db

# Check Python environment
source backend/venv/bin/activate  # if using venv
```

**Git push fails:**
```bash
# Check remote URL
git remote -v

# Pull latest changes first
git pull --rebase
git push
```

**Blog not updating:**
- Wait 2-3 minutes for GitHub Pages rebuild
- Check GitHub Actions tab in repository for build status
- Verify `_config.yml` settings are correct
