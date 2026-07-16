# Update Desk — deploy

## Structure
```
index.html      — the tool itself (frontend)
api/chat.js     — server-side proxy that holds the Anthropic API key
package.json
```

The frontend never talks to Anthropic directly — it calls `/api/chat` on your own domain, and that serverless function attaches the real API key server-side. The key never reaches the browser or the page source, so it's safe to share this tool with your team.

## Deploy via GitHub + Vercel

1. **Push to GitHub**
   ```bash
   cd update-desk
   git init
   git add .
   git commit -m "Update Desk"
   git branch -M main
   git remote add origin https://github.com/<your-username>/update-desk.git
   git push -u origin main
   ```

2. **Import into Vercel**
   - Go to [vercel.com/new](https://vercel.com/new), pick "Import Git Repository", select this repo.
   - Framework preset: "Other" (no build step needed).
   - Don't deploy yet — first add the environment variable below.

3. **Add your API key**
   - In the Vercel project → Settings → Environment Variables, add:
     - Key: `ANTHROPIC_API_KEY`
     - Value: your Anthropic API key (from [console.anthropic.com](https://console.anthropic.com))
   - Apply to Production (and Preview if you want PR previews to work too).

4. **Deploy**
   - Click Deploy. You'll get a URL like `update-desk.vercel.app` — that's the link to share with the team.

## Updating later
Any time you change `index.html` (design tweaks, prompt tweaks, etc.), just commit and push — Vercel redeploys automatically. The API key stays in Vercel's settings and never needs to touch the repo.

## Cost note
Each "Refresh article" click is a real Claude API call (with web search) billed to whichever key you put in `ANTHROPIC_API_KEY`. Worth keeping an eye on usage in the Anthropic console if the whole team starts using this.
