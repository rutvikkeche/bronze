# Bronze — Setup Guide

A minimal private two-person karaoke room. No accounts. No signaling server.

---

## Only one thing to fill in

Open `index.html`, find this line near the top of the `<script>` block, and replace it:

```js
const YT_API_KEY = 'YOUR_YOUTUBE_API_KEY';
```

### Get a YouTube Data API v3 key (free)

1. Go to [console.cloud.google.com](https://console.cloud.google.com)
2. Create a project → **APIs & Services → Enable APIs** → search **YouTube Data API v3** → Enable
3. **APIs & Services → Credentials → Create credentials → API key**
4. Paste it in
5. Optionally restrict it to your GitHub Pages domain

That's it. No Supabase. No other accounts.

---

## Deploy to GitHub Pages

```bash
git init
git add index.html README.md
git commit -m "bronze"
git remote add origin https://github.com/YOU/bronze.git
git push -u origin main
```

Then: **GitHub repo → Settings → Pages → Source: main / root → Save**

Live at: `https://YOU.github.io/bronze/`

---

## How it works

### Entry
- Click "IF YOU KNOW, YOU KNOW"
- Upload the secret red-eye image
- Wrong image → door stays shut (brief dim)
- Right image → room opens

### Connection (PeerJS)
- Both users load the same URL
- First to arrive registers as `bronze-room-x9k2-a`
- Second to arrive registers as `bronze-room-x9k2-b`
- They find each other automatically — no room code to share, no coordination
- PeerJS's free public server handles the handshake; after that it's direct peer-to-peer

### YouTube sync
- Search → click a result → it loads for both of you simultaneously
- Play / pause syncs with a 1.5s drift tolerance
- Uses a WebRTC data channel — no server involved

### Status dot
- Dim white = waiting for the other person
- Soft green = connected

---

## The pass

The entry image is verified by SHA-256 in the browser — nothing leaves the device. The hash is baked into the HTML. To change the pass image:

```bash
shasum -a 256 new-image.png
```

Replace `PASS_HASH` in the script with the result.
