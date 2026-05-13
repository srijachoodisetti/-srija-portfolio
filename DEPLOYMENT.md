# Portfolio Deployment Guide

## Current Status
✅ Project built successfully and ready to deploy

## Step 1: Push to GitHub

Since you don't have write access to the original repository, the code is configured to push to your own repository:
`https://github.com/mekalamanojkumar1006/srija-portfolio.git`

### Required: Set up GitHub Authentication

You need to authenticate with GitHub. Use one of these methods:

#### Option A: Personal Access Token (Recommended)
1. Go to https://github.com/settings/tokens
2. Click "Generate new token" (classic)
3. Select scopes: `repo` (full control of private repositories)
4. Copy the token
5. Run in terminal:
```bash
git push -u origin main
```
When prompted for password, paste the token instead.

#### Option B: SSH Key
1. Generate SSH key:
```bash
ssh-keygen -t ed25519 -C "mekalamanojkumar6@gmail.com"
```
2. Add key to GitHub: https://github.com/settings/keys
3. Change remote to SSH:
```bash
git remote set-url origin git@github.com:mekalamanojkumar1006/srija-portfolio.git
```
4. Push:
```bash
git push -u origin main
```

### Push Command
```bash
git push -u origin main
```

---

## Step 2: Publish to GitHub Pages (Free Hosting)

### Update Vite Configuration

Edit `vite.config.ts` and add the base path:

```typescript
export default defineConfig(({mode}) => {
  const env = loadEnv(mode, '.', '');
  return {
    base: '/srija-portfolio/',  // Add this line
    plugins: [react(), tailwindcss()],
    // ... rest of config
  };
});
```

### Enable GitHub Pages

1. Go to: https://github.com/mekalamanojkumar1006/srija-portfolio/settings/pages
2. Under "Source", select:
   - Branch: `main`
   - Folder: `/ (root)` → Change to `/dist`
3. Save

### Deploy via GitHub Actions (Automated)

Create `.github/workflows/deploy.yml`:

```yaml
name: Build and Deploy

on:
  push:
    branches:
      - main

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
      
      - name: Install dependencies
        run: npm install
      
      - name: Build
        run: npm run build
      
      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist
          cname: # Add custom domain here if you have one
```

### Manual Deploy (One-time)

After updating `vite.config.ts`:

```bash
npm run build
git add .
git commit -m "Add base path for GitHub Pages"
git push origin main
```

---

## Step 3: Alternative Hosting Options

### Vercel (Recommended for React projects)
1. Go to https://vercel.com/import
2. Import your GitHub repository
3. Deploy (automatic on every push)

### Netlify
1. Go to https://app.netlify.com/start
2. Connect your GitHub account
3. Select the repository
4. Build command: `npm run build`
5. Publish directory: `dist`
6. Deploy

---

## Project Structure
- `src/` - React components and source code
- `dist/` - Built production files (created after `npm run build`)
- `public/` - Static assets

## Available Commands
```bash
npm run dev      # Start development server (http://localhost:3000)
npm run build    # Build for production
npm run preview  # Preview production build locally
npm run lint     # Check TypeScript types
```

## Next Steps
1. ✅ Push to GitHub using Personal Access Token or SSH
2. ✅ Configure GitHub Pages deployment
3. ✅ Your portfolio will be live at: https://mekalamanojkumar1006.github.io/srija-portfolio/

---

**Note:** Replace placeholder values with your actual information where necessary.
