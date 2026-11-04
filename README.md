# DYCI Campus Bulletin

An individual Angular HTTP integration activity by Yzhe: browse school-context announcements, search and filter notices, read details, and submit validated demo feedback. The original interface uses a DYCI-inspired navy, blue, gold, and white palette.

## Screenshots and reflections

[View the submission PDF: four UI screenshots followed by the five reflection answers](docs/Campus-Bulletin-UI-and-Reflections.pdf).

| Main page | Announcement details |
| --- | --- |
| ![Main page](docs/screenshots/01-main-page.png) | ![Announcement details](docs/screenshots/02-notice-details.png) |
| Feedback validation | Feedback confirmation |
| ![Feedback validation](docs/screenshots/03-feedback-validation.png) | ![Feedback confirmation](docs/screenshots/04-feedback-confirmation.png) |

## Run locally

Use Node.js 22.12 or later in the Node 22 release line and npm. Open this repository folder, then run:

```sh
npm ci
npm start
```

Open http://localhost:4200 while the development server is running.

```sh
npm run build
npm test -- --watch=false
```

The production build completed without warnings and all 18 automated tests passed during the project review. Browser checks covered desktop, tablet, and 320/390 px layouts. Physical Android performance and spoken screen-reader output have not been tested.

## Component  service  API

The component handles user actions, form validation, and loading/error/success states. It calls an injected service, which uses Angular HttpClient and returns an Observable. The component subscribes to show the response or a recovery message.

| Action | Service request | Result |
| --- | --- | --- |
| Load announcements | GET `announcements/index.json` | An array of notices |
| Open notice 3 | GET `announcements/3.json` | One notice object |
| Submit valid feedback | POST `https://jsonplaceholder.typicode.com/posts` | A simulated response with an ID |

Announcement JSON files are served over HTTP with this app. They are a curated school-context snapshot, not a live school API. JSONPlaceholder simulates feedback submission; it does not permanently store the submitted record or send a request to the school. Use fictional contact information for demonstrations.

## UI behavior and content

- Search by title, body, or category; filter by category.
- Delayed hover/keyboard previews use the already loaded list. Activating a notice requests its details by ID and opens a native dialog, with a bottom-sheet layout on small screens.
- Field errors prevent invalid submissions. Failed submissions preserve input; successful submissions retain a separate confirmation before resetting the form.
- Keyboard dismissal, focus restoration, readable status messages, 44 px controls, and reduced-motion support are included.

Four notices summarize published DYCI archive items and retain their historical dates. Consultation, exam-schedule, and room-transfer examples are labeled as awaiting confirmation. The building extension beside Aula is identified as a student report. Source and date context appear in the interface. Confirm current schedules through official school communications.

To change a notice, update both its entry in `public/announcements/index.json` and the matching numbered JSON file. Keep the two records identical.

## Reflection questions

**1. Why should HTTP logic be placed in services?**

Services separate API requests from interface behavior. This makes request logic reusable, easier to test, and easier to maintain when endpoints change.

**2. What changes when retrieving one announcement instead of the whole list?**

The request targets an ID-specific URL, such as `announcements/3.json`, and expects one announcement object instead of the array returned by `announcements/index.json`.

**3. Why must an invalid form return before the service call?**

Returning stops incomplete or invalid data from being submitted, avoids unnecessary network requests, and gives the user a chance to correct the field errors.

**4. Why is the confirmation stored separately before the form resets?**

Resetting clears the form values. A separate confirmation preserves the submitted information and response ID so the success message remains accurate and visible.

**5. Why does a successful demo POST not prove permanent server storage?**

JSONPlaceholder returns a simulated creation response but does not persist the new record. A successful POST demonstrates the request/response flow, not permanent storage.