Route Planner — Pillar to Post Dispatch Tool
A smart scheduling and route optimization tool for inspection companies. Automatically assigns jobs to inspectors, optimizes routes, and balances workload across your team.

Built for: Pillar to Post (St. Louis region) | Current version: v17


⚡ Quick Start
1. Get Your Own Google Maps API Key
⚠️ Important: This tool requires a free Google Maps API key. You must get your own — do NOT use anyone else's key.

Steps:

Go to Google Cloud Console
Create a new project (or use an existing one)
Enable these APIs:
Maps JavaScript API
Places API
Geocoding API
Distance Matrix API
Go to Credentials → Create Credentials → API Key
Copy your API key (looks like AIzaSy...)

Cost: Free tier includes $300/month credit. Most small dispatch operations use $0–5/month.
2. Clone & Open
git clone https://github.com/yourusername/route-planner.git

cd route-planner

Open index.html in your browser.
3. Enter Your API Key
When the app opens, you'll see a modal asking for your Google Maps API key:

Paste your API key
Click Save API Key
Your key is stored locally in your browser only — never sent to servers, never committed to git

Lost your API key? Click the ⚙ (gear) icon in the top left to re-enter it anytime.


📖 Features
Inspectors
Add inspection team members
Set their home address
Certify them for specific services: Gas, Sewer, Radon, Mold
Jobs
Import jobs from your schedule (copy/paste)
Manually add individual jobs
Set time windows (AM: 9–12 or PM: 2–5)
Mark jobs with services needed
Request/exclude specific inspectors
Add notes
Smart Assignment
Auto-assign to balance workload across your team
Per-service assignments — split Gas/Sewer/Radon/Mold independently
Respects inspector certifications
Honors requested/excluded inspectors
Shows reasoning for each assignment
Route Optimization
Calculates optimal route order for each inspector
Shows estimated drive times between stops
Displays: Home → Job 1 [20min] → Job 2 [15min] → Office → Home
Minimizes total drive time per window
Export
Generate schedules to share with inspectors
Includes route order and timing
Copy/paste to Outlook, email, or messaging


🔒 Security & Privacy
Your API key is stored locally in your browser's localStorage — never sent to us, never logged
No data is sent to external servers (except Google Maps for routing)
All calculations happen in your browser
You can inspect the code — it's all client-side JavaScript
If you're concerned, use browser DevTools to verify network traffic


🚀 How to Use
Setup (One-time)
Add Your Inspectors

Go to Inspectors tab
Enter name and home address
Check which services they're certified for
Click Add

Add a Midday Office Location (optional)

Click the 📍 icon in the sidebar
Enter your office/lunch location
This becomes a waypoint between AM and PM routes

Import or Add Jobs

Option A: Paste your schedule into the import box (copy from Outlook/email)
Option B: Manually add jobs one at a time
For each job: address, time window (AM/PM), services needed
Assign Jobs
Auto-Assign (recommended)

Click the green Auto-assign jobs button
The tool balances workload and assigns each service independently
Review the Assignments tab to see results

Manual Reassign (if needed)

Click "Move" next to any service
Select a different inspector
Workload updates automatically

View Routes

Assignments tab shows optimal route order for each inspector
Map shows color-coded assignments
Export tab generates a shareable schedule


📋 Assignment Logic
The auto-assign algorithm:

Assigns each service (Gas, Sewer, Radon, Mold) independently
Prefers to keep Gas + Sewer together (same inspector) to minimize stops
Always keeps Radon + Mold together (bonded by design)
Respects inspector certifications
Honors requested/excluded inspectors
Balances workload: assigns to lightest available inspector
Considers AM/PM separately

Why the decision? After assigning, the tool shows the reasoning:

"Request honored: assigned to Alice"
"Lightest available: Bob (~2h 15m)"
"Kept with Gas to minimize inspectors per location"


🗺️ Map View
The map shows:

All inspectors' home locations (colored dots)
All jobs (pin drops)
Routes drawn by service type (Gas=gold, Sewer=teal, Radon=purple, Mold=orange)
Estimated drive times between stops

Click a job pin to see details and reassign services.


📤 Export & Sharing
Click the Export tab to:

View the full schedule with route order
Copy as text and paste into Outlook/email
Share with inspectors showing their route for the day

Format:

ALICE — AM · 9–12

Home → 123 Main [20 min, Gas] → 456 Oak [15 min, Sewer] → Office

Total: ~2h 30m

ALICE — PM · 2–5

Office → 789 Elm [18 min, Gas+Sewer] → Home

Total: ~1h 15m


⚙️ Settings & Data
Settings are saved automatically in your browser
All data (inspectors, jobs, assignments) persists across sessions
Clear your browser data = data is gone (careful!)
Export before clearing if you want to save results


🐛 Troubleshooting
"Address not found" or "No autocomplete"
Check that your API key is valid and has Geocoding + Places APIs enabled
Try adding the zip code: 123 Main St, St. Louis 63101
Make sure the address exists in Google Maps
Jobs tab is empty
Make sure you've added inspectors first (needed for assignments)
Try adding a job manually to test
Check browser console (F12) for errors
Auto-assign isn't working
Confirm all jobs have at least one service checked (Gas/Sewer/Radon/Mold)
Confirm all inspectors are certified for at least one service
Check that you have more than one job to assign
Map isn't showing
Refresh the page
Check that your API key includes the Maps JavaScript API
Verify your browser's location permissions aren't blocking it


📚 Full Documentation
See USER_GUIDE.md for:

Detailed walkthrough of each feature
API cost explanation
Advanced assignment options
Changelog (v17 features, past updates)


💬 Notes
This tool is client-side only — no data sent to external servers (except Google Maps)
Works best in modern browsers (Chrome, Firefox, Safari, Edge)
Designed for small to mid-size inspection teams (tested with ~10 inspectors, 50+ jobs)
Refresh to reload data from browser storage


📝 API Key Security
Why you need your own key:

Prevents abuse of a shared key
Keeps you in control of your costs
Protects your data (no third-party access)
Allows you to revoke/rotate keys anytime

What we do NOT do:

Store your key on our servers
Log your key
Use your key for anything other than YOUR requests
Share your key with anyone

What you SHOULD do:

Keep your key private (don't share in emails, Slack, GitHub)
Restrict your key to your domain if hosting online
Delete/rotate keys if you think they're compromised



Questions? Check the USER_GUIDE.md or review the code — it's all JavaScript you can inspect.

Good luck with your dispatching! 🚗
