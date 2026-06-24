# Blockchain Services Documentation

Generate beautiful ITRocket-style blockchain services documentation directly from your GitHub repository.

## 🎯 How It Works

1. **Store configs in GitHub**: Each blockchain has one `config.json` file
2. **Build script fetches**: Reads all configs from your GitHub repo
3. **Generates static site**: Creates HTML pages similar to ITRocket
4. **Deploy anywhere**: GitHub Pages, Vercel, Netlify, etc.

---

## 📁 GitHub Repository Structure

```
your-repo/
├── BlockX/
│   └── config.json
├── Cosmos/
│   └── config.json
├── AtomOne/
│   └── config.json
└── OtherChain/
    └── config.json
```

Each chain folder contains **one `config.json`** with all its service info.

---

## 📋 Config.json Format

```json
{
  "chain_name": "BlockX",
  "chain_id": "blockx_12345-1",
  "logo": "https://example.com/blockx-logo.png",
  "website": "https://blockxnet.com",
  "github": "https://github.com/blockxnet",
  "twitter": "https://twitter.com/blockxnet",
  "telegram": "https://t.me/blockxnet",
  "rpc": {
    "url": "https://blockx-mainnet-rpc.itrocket.net",
    "description": "archive"
  },
  "api": {
    "url": "https://blockx-mainnet-api.itrocket.net"
  },
  "grpc": {
    "url": "blockx-mainnet-grpc.itrocket.net:443"
  },
  "peers": [
    {
      "id": "ed0e36c57122184ab05b6c635b2f2adf592bfa0c",
      "address": "blockx-mainnet-peer.itrocket.net",
      "port": 61657
    }
  ],
  "seeds": [
    {
      "id": "f19d9e0f8d48119aa4cafde65de923ae2c29181a",
      "address": "blockx-mainnet-seed.itrocket.net",
      "port": 61656
    }
  ],
  "snapshot": {
    "pruned": true,
    "url": "https://server-3.itrocket.net/mainnet/blockx/null",
    "db_backend": "goleveldb",
    "updated_frequency": "every 4h"
  },
  "genesis": {
    "url": "https://server-3.itrocket.net/mainnet/blockx/genesis.json"
  },
  "addrbook": {
    "url": "https://server-3.itrocket.net/mainnet/blockx/addrbook.json",
    "updated_frequency": "1h"
  }
}
```

**Key Fields:**
- `chain_name`: Display name of the blockchain
- `chain_id`: Full chain ID
- `rpc/api/grpc`: Service endpoints
- `peers`: List of peer nodes (id, address, port)
- `seeds`: List of seed nodes (id, address, port)
- `snapshot`: Snapshot download info
- `genesis`: Genesis file URL
- `addrbook`: Address book URL
- Social links: website, github, twitter, telegram (optional)

---

## 🚀 Setup Instructions

### 1. Clone or Fork This Project

```bash
git clone https://github.com/yourusername/blockchain-services-docs
cd blockchain-services-docs
```

### 2. Configure the Build Script

Edit `build.js` and update these lines:

```javascript
const GITHUB_REPO = 'aidilfahmi/services';      // YOUR GITHUB REPO
const GITHUB_BRANCH = 'main';                   // YOUR BRANCH
const CHAINS_PATH = '';                         // Set to '' if chains are in root
```

**Example:** If your structure is `services/BlockX/config.json`, use:
```javascript
const GITHUB_REPO = 'aidilfahmi/services';
const CHAINS_PATH = '';  // Root directory
```

**Example:** If your structure is `docs/chains/BlockX/config.json`, use:
```javascript
const CHAINS_PATH = 'docs/chains/';
```

### 3. Install Dependencies

```bash
npm install
```

(or already have Node.js installed)

### 4. Build the Site

```bash
npm run build
```

This will:
- ✅ Fetch all chain configs from GitHub
- ✅ Generate individual pages for each chain
- ✅ Create an index page listing all chains
- ✅ Save everything to `./dist` folder

### 5. View the Generated Site

```bash
npm run serve
```

Opens browser at `http://localhost:8080`

---

## 🌐 Deployment Options

### GitHub Pages (Free)

1. Push everything to GitHub
2. Go to Settings → Pages
3. Set Source to `dist` folder
4. Site auto-deploys on push

### Vercel (Free)

```bash
npm i -g vercel
vercel
```

### Netlify (Free)

1. Connect GitHub repo
2. Set build command: `npm run build`
3. Set publish directory: `dist`
4. Done!

### Traditional Hosting

- Upload `dist/` folder to your server
- No build step needed on the server

---

## ⚙️ Customization

### Change Colors

Edit the CSS in `build.js`:

```javascript
// Find this in generateChainHTML():
background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
```

Change `#667eea` and `#764ba2` to your brand colors.

### Add More Fields to Config

1. Add field to your `config.json`:
```json
{
  "minimum_stake": "1000000",
  "unbonding_period": "21 days"
}
```

2. Add it to the HTML template in `build.js`:
```javascript
<div class="service-box">
    <strong>Minimum Stake:</strong> ${config.minimum_stake}
</div>
```

### Auto-Rebuild on GitHub Changes

Use **GitHub Actions** to rebuild automatically:

Create `.github/workflows/build.yml`:

```yaml
name: Build Docs

on:
  push:
    branches: [main]
  schedule:
    - cron: '0 */6 * * *'  # Every 6 hours

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v2
        with:
          node-version: '18'
      - run: npm run build
      - uses: actions/upload-artifact@v2
        with:
          name: dist
          path: dist/
```

---

## 📝 Example: Adding Your First Chain

1. **Create GitHub folder:**
   ```
   MainNet/BlockX/config.json
   ```

2. **Add config.json:**
   ```json
   {
     "chain_name": "BlockX",
     "chain_id": "blockx_12345-1",
     "rpc": { "url": "https://blockx-rpc.example.com" },
     "api": { "url": "https://blockx-api.example.com" },
     "grpc": { "url": "blockx-grpc.example.com:443" },
     "peers": [{"id": "abc123", "address": "peer.example.com", "port": 26656}],
     "seeds": [{"id": "def456", "address": "seed.example.com", "port": 26657}],
     "snapshot": {
       "pruned": true,
       "url": "https://snapshot.example.com/blockx",
       "db_backend": "goleveldb",
       "updated_frequency": "daily"
     },
     "genesis": { "url": "https://example.com/genesis.json" },
     "addrbook": { "url": "https://example.com/addrbook.json", "updated_frequency": "1h" }
   }
   ```

3. **Run build:**
   ```bash
   npm run build
   ```

4. **Done!** Check `dist/blockx.html`

---

## 🆘 Troubleshooting

**"Error: Could not fetch BlockX/config.json"**
- Check GITHUB_REPO and CHAINS_PATH are correct
- Make sure config.json is committed to GitHub
- File names are case-sensitive!

**"Invalid JSON in config.json"**
- Validate JSON: Use [jsonlint.com](https://jsonlint.com)
- Check for trailing commas

**"404 on peers/seeds"**
- Make sure field names match exactly: `peers`, `seeds`, not `peer`, `seed`

**Rate limited by GitHub API**
- Add your GitHub token to `build.js`:
```javascript
headers: {
  'User-Agent': 'Node.js',
  'Authorization': `token ${process.env.GITHUB_TOKEN}`
}
```

---

## 📊 What Gets Generated

- **index.html** - Main page with all chains listed
- **{chainname}.html** - Individual pages for each chain with:
  - RPC, API, gRPC endpoints
  - Peers and seeds (copyable)
  - Snapshot restoration commands
  - Genesis and addrbook downloads
  - Links to website, GitHub, Twitter, Telegram

---

## 🎨 Features

✅ ITRocket-style design
✅ Mobile responsive
✅ Copy-to-clipboard commands
✅ Automatic peer/seed formatting
✅ Build-time static generation
✅ Zero runtime dependencies (pure HTML)
✅ SEO friendly
✅ Dark mode ready (can be added)

---

## 📄 License

MIT - Use freely!

---

## 🤝 Contributing

Have improvements? Submit a PR!

---

**Questions?** Check the troubleshooting section or open an issue.
