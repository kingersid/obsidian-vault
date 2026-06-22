CLI is built and working. Here's the deal, Sid:
    
    I've built you a Meta Ads CLI tool at /root/workspace/meta-ads/meta_ads_cli.py (also available as meta-ads command). But to connect it to your account, you need to do a one-time setup to get API credentials.
    
    What you need to do (10 minutes, one-time):
    
    Step 1: Create a Meta App
    - Go to https://developers.facebook.com/apps/
    - Click "Create App" > Business type
    - Once created, go to App Dashboard > Add Product > Marketing API
    
    Step 2: Get your Ad Account ID
    - Open Meta Ads Manager
    - Look in the URL: adsmanager/manage/campaigns?act=XXXXXXXXXX
    - The number after act= is your ad account ID
    
    Step 3: Generate a System User Token
    - Go to Business Settings > System Users
    - Create a system user
    - Click "Generate New Token"
    - Select your app and ad account
    - Enable permissions: ads_read, ads_management
    - Copy the token
    
    Step 4: Configure the CLI
    
    meta-ads config --token YOUR_TOKEN
    meta-ads config --account act_XXXXXXXXXX
    
    
    Then you can run:
    
    meta-ads campaigns     — see all campaigns with 7-day metrics
    meta-ads analyze       — full report with optimization recommendations
    meta-ads insights     — account-level KPIs
    meta-ads adsets ID     — drill into a specific campaign
    meta-ads ads ID       — drill into specific ad set
    
    
    The analyze command automatically detects:
    - Ad fatigue (frequency > 3)
    - Low CTR campaigns (< 0.5%)
    - Rising CPC trends (7d vs 30d)
    - Spend without conversions
    - Paused campaigns worth reviewing
    
    Want me to walk you through the Meta Developer setup step-by-step in the browser right now? Or do you already have a Meta Developer account / access token?