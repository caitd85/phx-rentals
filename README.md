# PHX Rentals — Setup Guide

## Step 1: Set up Google Sheets (for real-time sync across roommates)

1. Go to [script.google.com](https://script.google.com)
2. Click **New Project**
3. Delete any existing code and paste this:

```javascript
const SHEET_ID = 'YOUR_GOOGLE_SHEET_ID_HERE';

function doGet() {
  const sheet = SpreadsheetApp.openById(SHEET_ID).getSheetByName('data');
  const data = sheet.getRange(1,1).getValue();
  return ContentService
    .createTextOutput(data || '{}')
    .setMimeType(ContentService.MimeType.JSON);
}

function doPost(e) {
  const sheet = SpreadsheetApp.openById(SHEET_ID).getSheetByName('data');
  sheet.getRange(1,1).setValue(e.postData.contents);
  return ContentService
    .createTextOutput('ok')
    .setMimeType(ContentService.MimeType.TEXT);
}
```

4. Replace `YOUR_GOOGLE_SHEET_ID_HERE` with your Google Sheet's ID
   (find it in the URL: `docs.google.com/spreadsheets/d/THIS_PART_HERE/edit`)
5. Make sure your Google Sheet has a tab named **data**
6. Click **Deploy → New deployment**
7. Type: **Web app**
8. Execute as: **Me**
9. Who has access: **Anyone**
10. Click **Deploy** and copy the Web App URL

## Step 2: Add the URL to index.html

Open `index.html` and find this line near the top of the script:

```javascript
const SHEET_URL = 'YOUR_GOOGLE_APPS_SCRIPT_URL_HERE';
```

Replace it with your Web App URL.

## Step 3: Deploy to Netlify

1. Go to [github.com](https://github.com) and create a new repository called `phx-rentals`
2. Upload `index.html` and `README.md` to the repository
3. Go to [netlify.com](https://netlify.com) and click **Add new site → Import an existing project**
4. Connect to GitHub and select your `phx-rentals` repo
5. Click **Deploy site**
6. Your site will be live at a URL like `random-name.netlify.app`
7. Optional: rename it under **Site settings → Domain management**

## Step 4: Share with roommates

Send your roommates the Netlify URL. When they open it, they'll pick their name and everything syncs automatically.

## How it works

- All ratings, comments, tour status, and tour notes save to Google Sheets
- Everyone sees each other's updates in real time
- Removed listings are hidden for everyone (stored in browser + Google Sheets)
- The app works offline too — changes sync when back online

## Features

- 🏠 49 listings with all details pre-loaded
- 🔍 Filter by city, price, beds, pool, carpet, garage, pet policy, tour status
- ⭐ Rate each home 1–5 stars
- 💬 Leave comments per listing
- ✅ Mark toured / scheduled / not yet
- 📋 Live tour checklist (living room, kitchen, carpet, appliances, etc.)
- 📝 Free-form tour notes
- ✕ Hide listings (restore anytime from Removed tab)
- 📊 Summary page with leaderboard, stats, cheapest, biggest
- 🔄 Side-by-side comparison of up to 3 homes
