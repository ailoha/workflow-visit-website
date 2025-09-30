# workflow-visit-website

This GitHub Actions workflow is designed to keep Microsoft 365 E5 developer subscriptions active by simulating daily website visits.

It works by automatically visiting pages deployed with onedrive-index, which in turn calls the Microsoft Graph API behind the scenes. This ensures your E5 tenant remains active.

## How It Works

  - The workflow runs on a scheduled basis (you can adjust the cron settings in `.github/workflows/visit.yml`).
  - It uses Playwright to visit several websites from the list you configure.
  - Each session:
    - Randomly selects 3–5 websites from your list (or all if fewer than 3).
    - Stays on each site for a random duration between 30–60 seconds.
    - Optionally tries to click a “Home” link on the page, then stays for 5 more seconds.
    - Waits 30–90 seconds between sites to simulate realistic browsing.

All this logic is implemented in `/visit-website/visitWebsite.js`.

You can edit this file if you want to adjust delays, add more interactions, or simulate other browsing behaviors.

## Configuration

### 1.Add Your Websites

Edit `/visit-website/websites.json` and put the URLs you want to be visited.

Example:
```json
[
  "https://your-onedrive-index-site-1.com",
  "https://your-onedrive-index-site-2.com"
]
```

### 2.Enable the Workflow

The GitHub Actions workflow (.github/workflows/visit.yml) is set up for manual or scheduled runs.

To run it daily, configure the schedule section with a cron expression (e.g., every day at 04:00 Beijing time).

### 3.Run Manually (Optional)

Go to your repository → Actions → select the workflow → click Run workflow.

## Notes and Limitations

  - This project is primarily for E5 developer subscription keep-alive purposes, **NOT FOR STRESS TESTING or CRAWLING**.
  - Be mindful of [GitHub Actions’ free usage limits](https://docs.github.com/en/actions/reference/limits):
    - Free accounts get 2000 minutes per month (as of 2025).
    - The workflow includes random delays (30–60 seconds stay + 30–90 seconds between visits).
    - Adjust visit counts and delays carefully to avoid hitting the monthly quota.
  - For private repositories, minutes may consume faster depending on your GitHub plan.

## Customization

  - Modify /visit-website/visitWebsite.js if you want to simulate different user interactions.
  - Extend /visit-website/websites.json with any number of URLs.
  - Combine with other workflows for broader E5 activity coverage.
