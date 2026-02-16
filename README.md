# Astra Veil Landing Page

Marketing landing page for Astra Veil — AI-powered astrology app.

**Live Site:** https://astraveil.app

## Setup

### 1. Supabase Configuration

The waitlist form connects to Supabase. You need to update the anon key in `index.html`:

```javascript
const SUPABASE_ANON_KEY = 'your-actual-anon-key-here';
```

**To get your anon key:**
1. Go to Supabase Dashboard → Project Settings → API
2. Copy the "anon public" key (starts with `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9`)
3. Replace the placeholder in `index.html`

### 2. Database Migration

Run the migration to create the waitlist table:

```bash
cd ../astra-veil  # Your main app repo
supabase db push
```

Or execute this SQL in Supabase SQL Editor:

```sql
CREATE TABLE IF NOT EXISTS waitlist (
  id uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  email text UNIQUE NOT NULL,
  source text DEFAULT 'website',
  user_agent text,
  created_at timestamptz DEFAULT now()
);

ALTER TABLE waitlist ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Allow anonymous waitlist signups" ON waitlist
  FOR INSERT TO anon WITH CHECK (true);
```

### 3. View Waitlist

Query the waitlist in Supabase:

```sql
SELECT email, created_at FROM waitlist ORDER BY created_at DESC;
```

Or check the Table Editor in Supabase Dashboard.

## Deployment

This site is deployed via **GitHub Pages**:

1. Go to repo Settings → Pages
2. Source: Deploy from a branch
3. Branch: main / (root)
4. Save

Site will be live at `https://livatechllc.github.io/astraveil.app/`

### Custom Domain (astraveil.app)

1. In repo Settings → Pages → Custom domain
2. Add: `astraveil.app`
3. Add DNS records at your registrar:
   - Type: A | Name: @ | Value: `185.199.108.153`
   - Type: A | Name: @ | Value: `185.199.109.153`
   - Type: A | Name: @ | Value: `185.199.110.153`
   - Type: A | Name: @ | Value: `185.199.111.153`

## Features

- ✨ Animated starfield background
- 📧 Email waitlist with Supabase integration
- 📱 Responsive design (mobile + desktop)
- 🎨 Mystical purple/gold theme
- ⚡ Vanilla HTML/JS (no build step)

## File Structure

```
.
├── index.html      # Main landing page
├── CNAME           # Custom domain config (if using)
└── README.md       # This file
```

## Development

To test locally:

```bash
# Simple HTTP server
python3 -m http.server 8000

# Or with Live Server VS Code extension
# Or with npx serve
npx serve .
```

Then open http://localhost:8000

## Marketing Integration

### PostHog Analytics

Add to `<head>` in `index.html`:

```html
<script>
  !function(t,e){var o,n,p,r;e.__SV||(window.posthog=e,e._i=[],e.init=function(i,s,a){function g(t,e){var o=e.split(".");2==o.length&&(t=t[o[0]],e=o[1]),t[e]=function(){t.push([e].concat(Array.prototype.slice.call(arguments,0)))}}(p=t.createElement("script")).type="text/javascript",p.async=!0,p.src=s.api_host.replace(".i.posthog.com","-assets.i.posthog.com")+"/static/array.js",(r=t.getElementsByTagName("script")[0]).parentNode.insertBefore(p,r);var u=e;for(void 0!==a?u=e[a]=[]:a="posthog",u.people=u.people||[],u.toString=function(t){var e="posthog";return"posthog"!==a&&(e+="."+a),t||(e+=" "+"(stub)"),e},u.people.toString=function(){return u.toString(1)+".people (stub)"},o="capture identify alias people.set people.set_once set_config register register_once unregister opt_out_capturing has_opted_out_capturing opt_in_capturing reset isFeatureEnabled onFeatureFlags getFeatureFlag getFeatureFlagPayload reloadFeatureFlags group updateEarlyAccessFeatureEnrollment getEarlyAccessFeatures getActiveMatchingSurveys getSurveys onSessionId".split(" "),n=0;n<o.length;n++)g(u,o[n]);e._i.push([i,s,a])},e.__SV=1)}(document,window.posthog||[]);
  posthog.init('YOUR_POSTHOG_KEY', {api_host: 'https://us.i.posthog.com'});
</script>
```

### Email Notifications

Set up a Supabase Edge Function to send welcome emails:

```bash
supabase functions new waitlist-welcome
```

Then trigger on insert to waitlist table.

## License

Private — All rights reserved.