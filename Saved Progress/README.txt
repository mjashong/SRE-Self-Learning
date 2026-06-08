DevOps Academy — Saved Progress
================================

WHAT THIS FOLDER IS FOR
-----------------------
A place to keep portable backup snapshots of your course progress as real
files (progress.json). Because this folder lives in iCloud Drive, any backup
saved here syncs to your other devices automatically.

DO I EVEN NEED THIS? (read first)
---------------------------------
Your progress ALREADY auto-saves on every click — the app stores it in
Safari's localStorage. As long as you keep using the same Safari on this Mac
and don't clear website data, you will NOT lose your progress.

This folder matters when you want to:
  • keep a portable backup file (in case Safari data gets cleared)
  • move your progress to another browser or another computer
  • sync progress across devices via iCloud

IMPORTANT — WHY IT ISN'T FULLY AUTOMATIC IN SAFARI
--------------------------------------------------
Safari (and all browsers) sandbox web pages: a local HTML file is NOT allowed
to silently write files into a folder on your Mac. So the app cannot, on its
own, drop a file into this folder on every change. You trigger a save with one
click. The steps below make that click land the file right here.

ONE-TIME SETUP (point Safari's downloads at this folder)
--------------------------------------------------------
1. Safari menu  →  Settings…  →  General tab
2. Find "File download location"
3. Choose "Other…" and select THIS folder:
       Self Learning / Saved Progress
   (Tip: leaving it on "Ask for each download" also works — Safari will let
    you pick this folder each time.)

HOW TO SAVE A BACKUP HERE
-------------------------
1. Open "Devops Academy Course.html" in Safari
2. Go to the "My Progress" tab and open the progress panel
3. Click  "⬇ Download progress.json"
   → the file lands in this folder, named like:
       devops-academy-progress-2026-06-07.json
   (dated, so each backup is kept rather than overwritten)

HOW TO RESTORE A BACKUP
-----------------------
1. Open the app → "My Progress" → progress panel → "Import backup"
2. Pick a progress-*.json file from this folder
3. Your lessons, quizzes, and projects are restored.

WANT TRUE HANDS-OFF AUTO-SAVE INTO THIS FOLDER?
-----------------------------------------------
This is now built into the app — but only Chrome, Edge, and Brave support it
(Safari does not). To use it:

1. Open "Devops Academy Course.html" in Chrome / Edge / Brave
2. Go to "My Progress" → open the progress panel
3. Under "📁 Auto-save to a folder", click "Choose folder…" and select THIS
   "Saved Progress" folder, then allow access when prompted
4. Done. From then on, a single "progress.json" file is written here
   automatically on every change (it overwrites in place, so it's always the
   latest). The connection is remembered across reloads; if the browser asks
   you to re-grant access after a restart, just reopen the panel and click
   "Reconnect folder".

In Safari this section is hidden automatically — use the Export/Import steps
above instead.
