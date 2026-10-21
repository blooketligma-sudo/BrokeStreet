# BrokeStreet — online multiplayer starter

This version adds **online lobby codes and player position sync** to the existing BrokeStreet HTML game. Players who enter the same code can see each other's 3D characters. The original game remains otherwise local.

## What this version does
- Create a lobby with a random six-character code.
- Join a friend's lobby using that code.
- Share player names, character appearance, and position/rotation in real time.
- Show online players in Party Status.
- Uses Supabase Realtime Broadcast + Presence; no database tables are needed for this prototype.

## Important limitation
This is the **first multiplayer layer**, not a fully shared game simulation. Each player still has their own local market, money, timers, inventory, and events. Those mechanics need a shared authoritative game state to become cooperative/competitive multiplayer.

## 1. Create a Supabase project
1. Go to https://supabase.com/ and create a project.
2. Open **Project Settings → API** (sometimes shown as **Data API / API Keys**).
3. Copy the **Project URL** and the **anon/public** key. Never use a `service_role` or secret key in browser code.
4. Open `config.js` and replace the two placeholder strings with your Project URL and anon/public key. Keep the quotation marks.

Example shape only (do not copy these fake values):
```js
window.BROKESTREET_CONFIG = {
  supabaseUrl: "https://your-project.supabase.co",
  supabaseAnonKey: "your-anon-public-key"
};
```

Realtime is enabled by default for Supabase projects. If connections fail, check **Project Settings → Realtime** and make sure Realtime is enabled.

## 2. Test locally
1. Put `index.html`, `config.js`, and `vercel.json` in the same folder.
2. Open `index.html` in a browser. For more reliable browser behaviour, use a local static server or Vercel deployment.
3. Click **Join party → Create new lobby**. Share the code.
4. Open the game in another browser/device, click **Join party**, enter the same code, and click **Join lobby**.
5. Start a day on both clients to see each other's characters moving around the 3D world.

## 3. Deploy on Vercel
1. Create a GitHub repository and upload all three files from this folder to the repository root.
2. In Vercel, choose **Add New → Project**, import that repository, and deploy. No build command is needed for this static HTML project.
3. Visit the deployed URL on two devices and test the same lobby code.

## Troubleshooting
- **Setup needed**: `config.js` still contains placeholders, or the file is not next to `index.html`.
- **Could not connect**: check the Project URL/key and Supabase Realtime status. Use only the anon/public key.
- **One player does not appear**: both users must be on the same deployed version and use the exact same code. Refresh both pages and try again.
- **Not secure for public competitive play yet**: this prototype accepts client-sent player states. Before adding shared money/trading or public matchmaking, add authenticated users, server-side validation, and rate limiting.
