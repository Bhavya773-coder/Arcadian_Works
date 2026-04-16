# Setup: Collect Form Submissions in Google Sheets (FREE)

## Step 1: Create Google Sheet
1. Go to [sheets.new](https://sheets.new)
2. Rename sheet to "Arcadian Access Requests"
3. Add these headers in row 1:
   ```
   Timestamp | Full Name | Email | GPUs Needed | Project Description
   ```
4. **Important:** Click Share → Change to "Anyone with the link can view" → Copy the link → Click Done

## Step 2: Add Apps Script
1. In your Sheet, click **Extensions** → **Apps Script**
2. Delete the default `myFunction()` code
3. Paste this exact code:

```javascript
function doPost(e) {
  try {
    // Handle CORS preflight
    if (e.parameter.method === "OPTIONS") {
      return ContentService.createTextOutput(JSON.stringify({"result":"success"}))
        .setMimeType(ContentService.MimeType.JSON)
        .setHeaders({
          "Access-Control-Allow-Origin": "*",
          "Access-Control-Allow-Methods": "POST",
          "Access-Control-Allow-Headers": "Content-Type"
        });
    }
    
    var data = JSON.parse(e.postData.contents);
    var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    
    sheet.appendRow([
      data.submittedAt,
      data.fullName,
      data.email,
      data.cardsNeeded,
      data.projectDesc
    ]);
    
    return ContentService.createTextOutput(JSON.stringify({"result":"success"}))
      .setMimeType(ContentService.MimeType.JSON)
      .setHeaders({"Access-Control-Allow-Origin": "*"});
      
  } catch (error) {
    return ContentService.createTextOutput(JSON.stringify({"result":"error", "message":error.toString()}))
      .setMimeType(ContentService.MimeType.JSON)
      .setHeaders({"Access-Control-Allow-Origin": "*"});
  }
}

function doGet(e) {
  return ContentService.createTextOutput("Arcadian Form Endpoint Active")
    .setMimeType(ContentService.MimeType.TEXT);
}
```

4. Click **Save** (disk icon) → Name it "Arcadian Form Handler"
5. Click **Deploy** → **New deployment**
6. Click gear icon → Select **Web app**
7. Set "Execute as": **Me**
8. Set "Who has access": **Anyone**
9. Click **Deploy** → Authorize → Sign in → Advanced → Go to (unsafe) → Allow
10. **Copy the Web App URL** (looks like: `https://script.google.com/macros/s/ABC123/exec`)

## Step 3: Connect to Website
Give me that URL and I'll update your website to send form data there.

## Where You'll See Requests
- Open your Google Sheet anytime to see all submissions
- Data auto-saves: Timestamp, Name, Email, GPUs needed, Project description
- 100% free, no limits, owned by you

---
**Need help?** Just paste the Web App URL here and I'll finish the setup.
