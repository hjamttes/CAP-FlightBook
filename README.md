# CAP Flight Logbook
A single-page flight logbook for Civil Air Patrol cadets to track sorties, certifications, and progress — with automatic Google Drive backup.

![Dashboard screenshot](/images/dash.png)

**[Try it live →](https://hjamttes.github.io/CAP-FlightBook/)**

## Quick start

Open the live link above — no install, no account required to start logging flights. Sign in with Google from Settings if you want automatic cloud backup.

## Features

- Log flights with airport autocomplete (ICAO/name/city search via OpenAIP)
- Track cadet profile, grade, unit, and certifications
- Automatic Google Drive backup on every save
- One-click restore from Drive on a new device
- Manual ICAO entry fallback when offline or without an API key
- Fully client-side — works as a static page, no backend required

## How to run it locally

This is a single static HTML file with no build step.

1. Clone the repo:
   ```
   git clone https://github.com/hjamttes/CAP-FlightBook.git
   cd CAP-FlightBook
   ```
2. Serve it with any static file server, e.g.:
   ```
   npx serve .
   ```
3. Open the served URL in your browser.

**Optional environment setup:**
- An [OpenAIP](https://www.openaip.net/) API key, entered in-app under Settings, enables full worldwide airport search. Without it, you can still add flights by typing the ICAO code manually.
- Google Drive backup requires the app to be served from an origin authorized in the Google Cloud OAuth client (already configured for the GitHub Pages deployment above).

## How it works

The Drive sync is built around a simple rule: **Drive is the source of truth, but offline entries are never silently lost.**

On first sign-in, the app doesn't just overwrite local data with whatever's in Drive (or vice versa). It pulls the existing Drive backup, takes those entries as the base, and then layers in any flights that were logged locally but aren't in Drive yet — the kind of thing that happens when you log a flight at an airfield with no signal. The merged set is saved locally and pushed straight back up to Drive, and the user gets a short summary ("3 flights loaded, 1 local-only entry kept"). After that initial merge, every save triggers a background backup automatically, so the two stay in sync without any manual export/import step.

A manual "Restore from Drive" option exists separately for the case where you genuinely want to discard local state and pull a clean copy — that one is a full overwrite, with a confirmation step since it's destructive.

## Credits

Built with the [OpenAIP](https://www.openaip.net/) airport database and the Google Identity Services / Drive APIs for backup.
