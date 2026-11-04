# Campus Pulse – Northbridge State Event Guide

CSC 372 – Assignment 2: Layouts
Author: Arodi Kapuadi

Live site: https://alkapuadi.github.io/assignment2/

## Project Description

Campus Pulse is a responsive event guide for the (fictional) Northbridge State
University Office of Student Engagement. Its purpose is to help students
discover campus activities such as festivals, workshops, career fairs, and
intramural sports in one place.

The audience is current students, especially first-year students looking for
ways to get involved. Student organization leaders are a secondary audience,
since the site also promotes their events.

The site has two pages:

- `index.html`: home page with a hero section, a grid of upcoming events, and an About section
- `event.html`: details page for the featured event (Fall Harvest Festival)

Only the Harvest Festival has its own details page, so every "View details" link
on the home page goes to `event.html`. Each link also has hidden screen-reader
text naming its event, so the link text stays meaningful.

## Project Structure

```
assignment2/
├── index.html
├── event.html
├── css/
│   └── styles.css
├── images/
│   ├── hero-campus.svg
│   ├── harvest-festival.svg
│   ├── hackathon.svg
│   ├── open-mic.svg
│   ├── career-fair.svg
│   ├── soccer-finals.svg
│   ├── sustainability-workshop.svg
│   └── movie-night.svg
└── README.md
```

## Layout Decisions

### CSS Grid

| Where | Why Grid |
|---|---|
| Upcoming events (`.event-grid` on `index.html`) | The cards form a two-dimensional layout of rows and columns. Grid lines the cards up in both directions and changes the column count with one property. On desktop the featured card spans two columns (`grid-column: span 2`), so the cards come in two different widths. |
| Main content + sidebar (`.event-layout` on `event.html`) | A page-level two-column layout is what Grid is built for. `grid-template-columns: minmax(0, 1fr) 320px` gives the sidebar a fixed, readable width and gives the rest to the article. On narrow screens it becomes a single column, so the sidebar moves below the main content. |

### Flexbox

| Where | Why Flexbox |
|---|---|
| Header (`.site-header__inner`) | A one-dimensional row: brand on the left, navigation on the right. Uses `justify-content: space-between` and `align-items: center`. On phones it switches to `flex-direction: column`. |
| Navigation list (`.main-nav__list`) | Links sit in a row with `gap` spacing and `flex-wrap: wrap`, so they move to a second line on narrow screens. No JavaScript menu is needed. |
| Hero (`.hero`) | Text and image sit side by side on desktop and stack on mobile. Uses `align-items: center` and `gap`. |
| Related events (`.related__list`) | Uses `flex-wrap: wrap`, `justify-content: space-between`, and `gap`. Each card has `flex: 1 1 240px`, so three cards fit on one row on desktop and wrap to two or one per row as the space shrinks. |
| Event cards (`.event-card`, `.event-card__body`) | A column flex container with `margin-top: auto` on the link pushes "View details" to the bottom of each card, so the links line up across a row. |
| Schedule rows (`.schedule li`) | Keeps each time in a fixed-width column and lets the description wrap beside it. |
| Footer (`.site-footer__inner`, `.footer-links`) | Footer items stack on mobile and spread across one row on desktop (`justify-content: space-between`). |

### Box Model

- `box-sizing: border-box` is set on every element, so padding and borders are counted inside each element's width.
- Content is centered with `max-width: 1100px` and `margin: 0 auto`.
- Cards and the sidebar use `padding`, `border`, and `border-radius`. All spacing comes from `margin`, `padding`, and `gap`. The pages contain no `<br>` elements or empty paragraphs.

## Responsive Design

The stylesheet is mobile-first. The base styles target phones, and two
`min-width` media queries add layout as the screen gets wider.

| Breakpoint | What changes |
|---|---|
| Base (< 640px) | Single column everywhere. The header stacks the brand above the nav, and the nav links wrap. The hero image sits below the text. Event cards are one per row. The event sidebar sits below the main content. Footer items stack. |
| `min-width: 640px` (tablet) | The header becomes a row with the nav on the right. The events grid gets two equal columns, and the featured card spans the full first row. Side padding grows. |
| `min-width: 960px` (desktop) | The hero becomes two columns. The events grid gets three columns, and the featured card spans two, so cards have two widths. `event.html` switches to the two-column main/sidebar grid, and the sidebar becomes sticky. The footer lays out in one row. Headings get larger. |

**Testing:** I tested both pages in Chrome DevTools device mode at 375px (iPhone),
768px (iPad), and 1280px+ (desktop), and also dragged the window width to watch
each breakpoint change. I checked that the page never scrolls sideways, that the
nav wraps cleanly, that the sidebar drops below the main content on narrow
screens, and that the related event cards wrap. I also checked that the nav links
work between `index.html` and `event.html`, including the `#upcoming-events` and
`#about` section links.

## Semantic HTML

| Element | Why it fits |
|---|---|
| `<header>` / `<nav>` | Mark the site banner and the primary navigation, so screen-reader users can jump straight to them. The nav links are in a `<ul>` because they are a list of destinations. |
| `<main>` | Each page has exactly one. It wraps the page's unique content and lets assistive technology skip the repeated header. |
| `<section>` | Groups themed parts of a page (hero, upcoming events, about, schedule, accessibility). Each has a heading tied to it with `aria-labelledby`. |
| `<article>` | Each event card stands on its own and could be reused elsewhere, such as in a feed or newsletter. The main event description is also an article. |
| `<aside>` | The event info sidebar supports the main content but is separate from it, which is what `aside` is for. |
| `<figure>` / `<figcaption>` | The large event image on `event.html` has a caption, and `figure` links the two together. |
| `<time datetime="...">` | Dates and times are shown in a friendly format, while `datetime` gives a machine-readable version that browsers and calendars can use. |
| `<footer>` | Holds the copyright, contact email, and secondary links that repeat on every page. |
| `<dl>` / `<dt>` / `<dd>` | The sidebar's label/value pairs (Date, Time, Location...) are a description list. |

Every image has alt text describing what it shows.

## Sources

- **Images:** All images in `/images` are original SVG illustrations made for
  this project. No outside images were used.
- **Fonts:** System font stack (`system-ui`, `-apple-system`, `Segoe UI`,
  `Roboto`, etc.). No web fonts were loaded.
- **Content:** The university, organizations, events, locations, and email
  address are all fictional and were made up for this assignment.
- **Reference:** The `.visually-hidden` utility class follows the common
  accessibility pattern described on MDN and The A11Y Project.
- No CSS frameworks, component libraries, JavaScript, or templates were used.
