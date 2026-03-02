<wizard-report>
# PostHog post-wizard report

The wizard has completed a deep integration of PostHog analytics into this Next.js App Router project (DevEvent — a developer events discovery platform). Here is a summary of all changes made:

- **`instrumentation-client.ts`** (new file): Initialises PostHog client-side using the recommended Next.js 15.3+ instrumentation pattern. Configured with a reverse proxy host (`/ingest`), automatic exception/error tracking (`capture_exceptions: true`), and debug mode in development.
- **`next.config.ts`** (edited): Added PostHog reverse proxy rewrites (`/ingest/static/:path*` and `/ingest/:path*`) and `skipTrailingSlashRedirect: true` to ensure PostHog requests are reliably relayed through your own domain, avoiding ad blockers.
- **`.env.local`** (new file): PostHog API key and host stored as `NEXT_PUBLIC_POSTHOG_KEY` and `NEXT_PUBLIC_POSTHOG_HOST` environment variables. Covered by `.gitignore`.
- **`components/ExploreBtn.tsx`** (edited): Added `posthog.capture('explore_events_clicked')` inside the existing `onClick` handler — tracks when users click the primary CTA button, the top of the engagement funnel.
- **`components/EventCard.tsx`** (edited): Added `'use client'` directive and `posthog.capture('event_card_clicked', { event_title, event_slug, event_location, event_date })` via an `onClick` handler on the `<Link>` — tracks which specific events users show interest in, with rich properties for segmentation.

## Tracked events

| Event name | Description | File |
|---|---|---|
| `explore_events_clicked` | User clicked the 'Explore Events' CTA button on the home page, indicating intent to browse events | `components/ExploreBtn.tsx` |
| `event_card_clicked` | User clicked on an event card to view event details, indicating interest in a specific event | `components/EventCard.tsx` |

> **Note:** Pageviews are captured automatically by PostHog via the `instrumentation-client.ts` initialisation — no manual instrumentation needed.

## Next steps

We've built some insights and a dashboard for you to keep an eye on user behaviour, based on the events we just instrumented:

- 📊 **Dashboard — Analytics basics:** https://us.posthog.com/project/328276/dashboard/1320616
- 🔻 **Discovery funnel: Pageview → Explore → Event click** (conversion funnel): https://us.posthog.com/project/328276/insights/0Kyf9xFL
- 📈 **Daily engagement: Explore clicks vs Event card clicks** (trend): https://us.posthog.com/project/328276/insights/07u2PgPf
- 🏆 **Most popular events by clicks** (breakdown bar chart): https://us.posthog.com/project/328276/insights/7HGCps5g
- 👥 **Weekly unique explorers** (unique users trend): https://us.posthog.com/project/328276/insights/khH9KrOg
- 👆 **Daily unique users clicking event cards** (unique users trend): https://us.posthog.com/project/328276/insights/7u20ZxGd

### Agent skill

We've left an agent skill folder in your project at `.claude/skills/posthog-integration-nextjs-app-router/`. You can use this context for further agent development when using Claude Code. This will help ensure the model provides the most up-to-date approaches for integrating PostHog.

</wizard-report>
