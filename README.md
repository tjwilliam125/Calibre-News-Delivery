Calibre News Delivery (Dropbox Edition)

​A serverless automation tool that fetches daily news sources, converts them to EPUB using Calibre, and delivers them directly to a Dropbox folder for syncing to e-readers. Currently configured to run automatically every day at 6:00 AM CDT (11:00 UTC).

​Features

​Serverless: Runs entirely on GitHub Actions (Ubuntu Linux runners). No home PC required.

​Automated Fetching: Pulls latest issues of Ars Technica, The Atlantic, BitcoinMag, CoinDesk, Decrypt, The Economist, The Financial Times, The Guardian US, Hacker News, The New Yorker, ProPublica, Scientific American, and Wired.

​Smart Storage Management: 
Includes a custom Python script that
--Uploads new EPUBs to Dropbox.
​--Auto-deletes files older than 7 days to prevent filling up the free 2GB Dropbox tier.
​--Permanent Access: Uses OAuth2 Refresh Tokens so the connection never expires.

​Setup & Configuration

​1. Requirements

​A specific Dropbox App created in the Dropbox Developer Console with files.content.write and files.content.read permissions.
​GitHub Repository with the .github/workflows/dropbox-news.yml file.

​2. Secrets

​The workflow relies on the following secrets stored in Settings > Secrets and variables > Actions:

Secret Name Description
DROPBOX_APP_KEY The App Key from Dropbox Developer Console.
DROPBOX_APP_SECRET The App Secret from Dropbox Developer Console.
DROPBOX_REFRESH_TOKEN The long-lived OAuth2 refresh token generated via curl.

Folder Structure Logic
Root Folder: The script uploads daily news here. Any file in this root folder older than 3 days is automatically deleted.
To save space, script will not re-upload articles from the same source if already uploaded that day.
Subfolders (e.g., /Permanent): The script ignores subfolders. Use these for manually saved books or articles you wish to keep indefinitely.

Workflow
Install: Sets up Calibre and Python dependencies (requests, libegl1).
Fetch: Runs ebook-convert for each recipe.
Upload: Python script authenticates via Refresh Token and uploads files.
Clean: Python script checks file timestamps and removes expired news.

Recommended pairing with Dropsync (or similar sync tools):
Remote Folder: /Apps/Calibre-News-[YourName]
Local Folder: /Downloads/News
Method: "Download only" (to avoid syncing deletions back to the server).
