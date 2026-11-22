MEETING AND EVENT RESOURCE PLANNER

==================================

Course: ITM 7131 - Web Application Development (Academic Year 2026/2027)

Institution: Muni University, Faculty of Technoscience

Group: Group 11

1. PROBLEM STATEMENT & INTENDED USERS

-------------------------------------
Problem Addressed:

Organizing club events, workshops, and departmental meetings across university facilities often involves checking separate venue schedules and Estates Office equipment lists. Without a single pre-booking check, organizers can easily pick a room that is too small, overlook low equipment stock (such as sound systems, outdoor tents, or podiums), or submit overlapping bookings for the same venue and time slot.

Intended Users:

- Student Guild & Club Organizers: Planning society meetings, demos, debates, and cultural nights.

- University Staff & Faculty Coordinators: Scheduling staff briefings, academic workshops, and conferences.

- Estates Office & Facility Administrators: Reviewing projected equipment demand against available stock and monitoring schedule conflicts.





2. CORE FEATURES & ARCHITECTURE

-------------------------------



The application runs purely on the front end using semantic HTML5 (index.html), a responsive external stylesheet (style.css), and vanilla JavaScript (script.js) backed by 24 sample records across campus venues, equipment items, and scheduled events.



Workflow Features:



1. Venue Directory & Live Multi-Criteria Filtering (#venues)

   - Displays a responsive grid of eight campus spaces (v1-v8) with location, capacity, and availability badges.

   - Filters venues in real time via filterVenues() and showVenues() by keyword (name or location), venue type (Hall, Room, Outdoor), minimum capacity, and availability status, updating an aria-live counter and showing an empty-state message when no venues match.



2. Estates Office Resource Inventory (#resources)

   - Presents a filterable inventory table of eight Estates Office items (Projector, Sound system, Event tent, Plastic chairs, Folding tables, Wireless microphone, Podium, and Banner stand).

   - Populates #rType dynamically from the dataset in startPage() and classifies stock levels in showResources() as "In stock" (stock >= 5) or "Low stock" (stock < 5) using green and amber badges.



3. Event Planning Form & Live Requirement Estimator (#plan)

   - Pairs a seven-field event booking form (#planForm) with a live "Estimate requirements" table (calculateNeeds() and showEstimate()).

   - Calculates equipment needs automatically as users type: 1 plastic chair per attendee, Math.ceil(attendees / 10) folding tables, Math.ceil(attendees / 100) wireless microphones, Math.ceil(attendees / 50) event tents for outdoor venues or tent requests, and 1 unit each for checked optional extras (Projector, Sound System, Podium). Items exceeding available stock are flagged with a red "Short" badge.

   - Validates inputs via checkForm() (minimum name length, valid email, capacity limits, venue availability, non-past date, and valid start/end times) and blocks double-bookings or stock shortages inside submitPlan().



4. Schedule & Conflict Detection Engine (#schedule)

   - Summarizes total planned events, total attendees, and active conflicts in three KPI cards.

   - Uses clashes(a, b) to check every pair of bookings sharing a venue and date for time overlap (a.start < b.end and b.start < a.end), alerting users to existing clashes (such as EVT-1001 and EVT-1008 in the Main Auditorium on 2026-11-16) and rendering a chronologically sorted, XSS-escaped (safe()) schedule table.



Five-Part System Architecture:



Part 1: Shell & Navigation

  Core Responsibility          : Page framing, sticky navigation, and responsive design system

  HTML Anchor (index.html)     : #top-bar, <nav>, <main>, <footer>

  Key Logic (script.js)        : startPage() initialization

  Layout & Styling (style.css) : #top-bar, .site, main, .badge



Part 2: Venue Directory

  Core Responsibility          : Searchable, multi-criteria catalog of campus spaces

  HTML Anchor (index.html)     : #venues, #venueFilters, #venueCards

  Key Logic (script.js)        : venues, filterVenues(), showVenues()

  Layout & Styling (style.css) : .cards, .card, .filters



Part 3: Resource Inventory

  Core Responsibility          : Filterable table of Estates Office equipment and stock levels

  HTML Anchor (index.html)     : #resources, #rType, #rStatus, #resBody

  Key Logic (script.js)        : resources, showResources()

  Layout & Styling (style.css) : .table-wrap, .ok, .warn



Part 4: Event Planner & Estimator

  Core Responsibility          : Live resource calculator, form validation, and booking submission

  HTML Anchor (index.html)     : #plan, #planForm, #estWrap

  Key Logic (script.js)        : calculateNeeds(), showEstimate(), checkForm(), submitPlan()

  Layout & Styling (style.css) : .two-col, .form-grid, [aria-invalid="true"]



Part 5: Schedule & Conflict Engine

  Core Responsibility          : Chronological booking log, summary stats, and clash detection

  HTML Anchor (index.html)     : #schedule, #stats, #clashBox, #schBody

  Key Logic (script.js)        : bookings, clashes(), showSchedule(), safe()

  Layout & Styling (style.css) : .stats, .stat, .msg

3. HOW TO USE THE APPLICATION

-----------------------------
1. Find a Venue: Open the application and use the Venues filter bar to search by venue name or location, select a type, set a minimum seating capacity, and filter by availability.

2. Check Equipment Stock: Open the Resources section and filter by category or stock level to see which Estates Office items are in stock or running low.

3. Enter Event Details & Preview Needs: Go to Plan an Event, fill in the event name, organizer email, expected attendance, venue, date, start/end times, and any optional extras, then check the live Estimate requirements table on the right for any "Short" badges.

4. Validate and Submit: Click "Check and Submit plan". Fix any highlighted validation errors if shown; when valid and free of clashes or shortages, the app assigns a reference number (starting at EVT-1009) and displays a confirmation banner.

5. Confirm in the Schedule: Scroll to Schedule and conflicts to verify the new event in chronological order and check updated summary totals.

4. TESTING & RESPONSIVE VERIFICATION SUMMARY

--------------------------------------------



- Responsive Breakpoints: Verified in browser DevTools across mobile (375 px single-column stack and horizontal .table-wrap scrolling), tablet (768 px three-column venue grid, side-by-side form/estimator panels, and 2rem header scaling), and desktop (1280 px centered 1000px container with four venue cards per row).

- Functional Test Coverage: Verified across 10 test cases covering multi-criteria venue filtering, empty search states, low-stock resource filtering, indoor/outdoor equipment formulas, form validation errors, double-booking rejection, stock shortage rejection, successful event creation (EVT-1009), form reset, and HTML escaping (safe()). Full test logs and layout captures are included in the Group 11 Submission Report PDF.





5. REPOSITORY STRUCTURE

-----------------------



itm7131-group11-meeting-event-planner/

|-- index.html          Semantic HTML5 markup for the shell and all four workflow sections

|-- style.css           External responsive stylesheet (Flexbox, CSS Grid, badges, focus outlines)

|-- script.js           Sample records, filtering, live estimator, validation, and clash detection

|-- docs/

|   `-- testing/        Responsive layout screenshots (375px, 768px, 1280px)

`-- README.txt          Project overview, usage instructions, limitations, and team contributions





6. LIMITATIONS

--------------



- Session-Only Memory Storage: Venues, resources, and bookings live in JavaScript arrays (venues, resources, bookings). New bookings update the live schedule during the current browser session but reset on page reload.

- Front-End Only: In line with the project specification, there is no backend database, user authentication, or automated email dispatch.

- Per-Event Stock Comparison: Equipment checks compare a single event's needs against total Estates Office stock rather than subtracting equipment already reserved by other events on the same date.

- Static Venue Availability: Venue availability (ok: true/false) is set in the sample dataset rather than through an admin toggle.





7. TEAM MEMBERS & RESPONSIBILITIES (GROUP 11)

---------------------------------------------

1. Pimundu Raymond 

   Role: Project Coordinator & Shell Lead

   Responsibilities: Set up the GitHub repository, GitHub Pages deployment, and README; built the sticky #top-bar, .site header, <nav>, <main>, and <footer> in index.html; wrote base resets (box-sizing: border-box), focus outlines, and .badge styles in style.css; structured startPage() in script.js.

2. Micpamungu Ruth 

   Role: Venue Directory Lead

   Responsibilities: Built #venues, #venueFilters, #venueCount, and #venueCards in index.html; styled responsive .cards and .filters grids in style.css; authored the venues dataset, filterVenues(), and showVenues() in script.js.

3. Nimungu Jude 

   Role: Inventory & Schedule Lead

   Responsibilities: Built #resources and #schedule in index.html; styled .table-wrap, .ok, .warn, and .stat elements in style.css; authored the resources and bookings arrays, showResources(), clashes(), showSchedule(), and safe() in script.js.

4. Anguni Phillip

   Role: Event Form & Estimator Lead

   Responsibilities: Built #plan, #planForm, and #estWrap in index.html; styled .two-col Flexbox and .form-grid layouts in style.css; implemented getExtras(), calculateNeeds(), and showEstimate() in script.js.

5. Thomas Mandama Charles

   Role: Shell, Validation & Submission Lead

   Responsibilities: Co-developed the application shell (#top-bar, .site, <nav>, <footer>, startPage()); styled .err, [aria-invalid="true"], .btn, .btn.alt, and .msg alerts in style.css; implemented setError(), checkForm(), submitPlan(), and clearForm() in script.js.

8. ACKNOWLEDGEMENTS

-------------------

- Course Brief & Specification: Project requirements and rubric structure adapted from "ITM 7131 Web Application Development - Group Project Submission Brief", issued by Dr. Andogah Geoffrey, Faculty of Technoscience, Muni University.

- Libraries & Assets: Built entirely with standard semantic HTML5, CSS3, and vanilla ES6+ JavaScript; no external CSS frameworks, JavaScript libraries, fonts, or third-party images were used.

