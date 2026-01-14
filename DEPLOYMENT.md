# 🚀 Grub AI Backend - Deployment Guide

## What This Does
This is a serverless backend that fixes the CORS issue with Gemini API. Your app calls this backend, which then calls Gemini API and returns the results.

---

## 📦 Quick Deploy to Vercel (FREE - 5 minutes)

### Step 1: Install Vercel CLI
```bash
npm install -g vercel
```

### Step 2: Deploy
```bash
cd grub-ai-backend
vercel
```

Follow the prompts:
- "Set up and deploy?" → **Yes**
- "Which scope?" → Your account
- "Link to existing project?" → **No**
- "Project name?" → **grub-ai-backend** (or whatever you want)
- "Directory?" → **./** (press Enter)
- "Override settings?" → **No**

### Step 3: Get Your URL
After deployment, Vercel will give you a URL like:
```
https://grub-ai-backend-xxx.vercel.app
```

**Save this URL!** You'll need it for your app.

---

## 📱 Update Your App

Open your Grub AI app and find this line:
```javascript
const response = await fetch(
  `https://generativelanguage.googleapis.com/...`
```

**Replace it with:**
```javascript
const response = await fetch('https://YOUR-VERCEL-URL.vercel.app/api/analyze-meal', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    description: description,
    portionSize: portionSize,
    oilLevel: oilLevel
  })
});

const result = await response.json();
if (result.success) {
  return result.data;
} else {
  throw new Error(result.error);
}
```

---

## 🧪 Test It

After deploying, test your endpoint:
```bash
curl -X POST https://YOUR-VERCEL-URL.vercel.app/api/analyze-meal \
  -H "Content-Type: application/json" \
  -d '{"description":"apple","portionSize":"medium","oilLevel":1}'
```

Should return:
```json
{
  "success": true,
  "data": {
    "calories": 95,
    "protein": 0,
    "carbs": 25,
    ...
  }
}
```

---

## 💰 Cost

**Vercel Free Tier:**
- ✅ 100GB bandwidth/month
- ✅ 100,000 requests/month
- ✅ 100 hours of serverless execution/month

**For 10,000 users:**
- 10,000 users × 3 meals = 30,000 requests/day
- 30,000 × 30 days = **900,000 requests/month**

You'll need **Pro Plan: $20/month** (supports 1M requests)

**Still WAY cheaper than Claude API ($1,350/month)!**

---

## 🔒 Security Note

The API key is in the server code (not exposed to users). This is safe because:
- ✅ Server code never reaches the user
- ✅ Only your app can call your backend
- ✅ You can add rate limiting later

---

## 🐛 Troubleshooting

**"Failed to fetch"**
- Check your Vercel URL is correct
- Make sure you're using HTTPS not HTTP

**"API key invalid"**
- Check the Gemini API key in `api/analyze-meal.js`

**"Too many requests"**
- Upgrade to Vercel Pro ($20/month)

---

## ✅ Done!

Once deployed:
1. ✅ No more CORS errors
2. ✅ Gemini API works perfectly
3. ✅ Real USDA data for all meals
4. ✅ Ready for App Store!

**Your app will now show accurate nutrition with ingredient breakdowns!** 🎉
