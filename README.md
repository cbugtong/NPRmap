# NPRmap
This is a presentation (NOT THE CODE OR LIVE DEMO) of my work with National Public Radio on an internal-use project

# CPBM - Radio Mapping Project
This project is built upon the Congressional District Radio Coverage Project, 
which was developed by NPR Labs. This project was developed over the course of
Fall 2017 (September 11 - December 8). The web components were primarily 
developed by Christopher Bugtong and the data was primarily compiled by Colette
Rosenberg.

Major changes from the previous project include a UI overhaul on both desktop 
and mobile platforms, the removal of congressional-district-based data, and the
addition of population and demographic information. New features were added as 
well: the ability to search using multiple callsigns, an interactive information
area, and contour removal.

## Video Presentations
[Desktop View](https://www.youtube.com/watch?v=zBtDA-2CzUM)
[Mobile View](https://www.youtube.com/watch?v=eLP2JaYxQbI)

## Browser Specifications
**Internet Explorer** - Basic functionality is available on Microsoft Edge for 
desktop, but details are missing (contour outline, UI control, etc.). The 
application is not usable on Internet Explorer 11 and previous versions.  

**Mozilla Firefox** - Firefox Quantum works on both mobile and desktop. The 
application is not usable on Firefox 39 and previous versions.  

**Google Chrome** - The application is usable and fully tested on Chrome 62 and 
newer versions. This is the recommended browser.

**Safari** - The application is usable on Safari 11 and newer versions.  

**Opera** - untested.

## Libraries and Components
The major libraries used in this application are 
[Bootstrap v4](https://getbootstrap.com), [Leaflet](http://leafletjs.com),
and [D3](https://d3js.org). [Select2](https://select2.org) is also used to add 
functionality to the callsign search function. The JavaScript is being converted
to JQuery for consistency accross the project, particularly with the 
JQuery-based *Bootstrap* and *Select2* libraries.

**Bootstrap** is used to ensure stylistic consistency for desktop and mobile 
browsers. The application itself is NOT structured mobile-first, but 
rather most mobile style changes are applied in *style-xs.css*.

**Leaflet** is the main controller and renderer for for the map interface. 
The map is from the NPR Labs Mapbox account, and the URL needed to be updated 
once during the Fall.

**D3** is "a JavaScript library for visualizing data using web standards", and 
is especially useful in displaying the large number of station contours within
the map.

## Files:

### Data
The **FM_AM_TV.geojson** contains all information related to stations.

The **states.geojson** contains all information related to states.

The **overlap.csv** contains overlapping information between states and 
stations.

### Geocoder - Location Search
The files in this folder are pulled from [this GitHub page](https://github.com/k4r573n/leaflet-control-osm-geocoder)
and is a Leaflet plugin that allows location searches through geocoding. The
code has been lightly modified to change the searchbar appearance and to display
 the shapes of the searched locations.

### Map Libraries
The files in map libraries contains the underlying libraries that make this 
application work. This code is unmodified.

### Style - CSS
**loginStyle.css** defines the style for login.php.

**style.css** defines the color, font, and placement of UI throughout the 
application.

**elementStyle.css** defines the size and placement of the application's larger
containers, such as the map interface, the sidebar, the information panel and
modal, etc. 

**print_map.css** defines the properties surrounding the printing of the map,
whereas **print_info.css** defines the styling of information printing.

**style-xs.css** redefines elements (defined in both *style.css* and 
*elementStyle.css*) for a smaller, mobile screen.

### Root - Structure : PHP
Users must login on **login.php** using a preset password (currently nprPRmap) 
to create a session. *login.php* uses *membership.php* to validate 
credentials.

**membership.php** contains the logic for logging in and session checking 
happens. The sha1 hash of the user-entered password is checked against a stored 
hash of the real password. No salting is implemented.

Once logged in, the user is sent to **index.php**, which outlines the main page 
of the application, containing the interactive menu, map information--and an 
iframe of *map.php*. *index.php* also contains JavaScript that controls the 
functionality of the Select2 element that facilitates "Search by Callsign".

**map.php** sets up the css and html map within the iframe. This file also calls 
*map.js*, which controls the functions that affect the leaflet and d3 
projections.

__NOTE: *d3.js*, *leaflet.js*, *leaflet.css*, and *topojson.js* are necessary to
load the map. They each contain a link (in comments) to the places from which 
their information can be called.__

### UI - JavaScript/JQuery

**printControl.js** contains the logic for printing and print preview, written 
primarily in JQuery.

**view.js** contains the logic for alternate user displays, written primarily in 
JQuery. This is not meant as a true implementation of different user views, but
is rather a proof of concept.

**ui.js** contains the logic for UI interaction, written primarily in JQuery.

### Drivers - JavaScript

**map.js** contains the map behavior, utilizing the leaflet library and 
setting up the d3 interaction; and also the global variables and functions 
throughout all the drivers.

**states** contains all functions and variables relating to drawing the 
states upon load and when the user searches by state. This module loads state 
information from *states.geojson* into the application. 

*Public variable and functions:*
* `pubStatesIdLookup` is the public handle for statesIdLookup, a dictionary 
structure that maps FIPS state codes to state names. 
* `pubLoadMap` loads state outlines when the map first renders.
* `pubSearchState` is called when user searches by state, triggered by clicking
on the state's map location or by using the "Search by State" field.

**stations** contains all functions and variables relating to drawing 
stations, both when user searches by state and when they search by call sign. 
This module loads station and overlap information from *FM_AM_TV.geojson* and 
*overlap.csv* into the application, respectively.

*Public functions:*
* `pubLoadStations` loads and organizes station data upon application startup.
* `pubSetFilters` adjusts the primary set of stations in accordance with the set
filters.
* `pubSearchStation` displays station(s) when searched by call sign. 
* `pubStationsStateOverlap` finds and displays the stations overlapping the 
selected state, using information from *overlap.csv*
* `pubAllStations` displays all stations in accordance with the set filters.
* `pubClicked` controls the display behavior when selecting a station by 
clicking its shape or display information.
* `pubDetailedInfo` displays detailed station information visible in the
demographic modal. 
* `pubDrawLabels` draws labels according to *pubPlaceLabels*.
* `pubPlaceLabels` defines where station labels are placed when drawn, handling 
and preventing collisions. Five radio station are hardcoded when placing labels:
	* WRMY
	* WVTW
	* WNRN
	* KNKX
	* KUOW
* `pubClearLabels` clears out arrays containing coordinates of previously placed 
labels. 

Common variables and functions control map utility and manage live-state 
information.

### Miscellaneous Files

**freqwebsite.csv** is a list of each member's website page that shows which 
frequencies it broadcasts.

**discrepency.json** is a list of discrepencies between the radio station 
geojsons and the info listed on their websites. [*Not updated since 08/17*]

**names.csv** is a name for each member station and its affiliated stations (EG 
WMEA and its associates are "Maine Public Radio").

## Next Steps
While developing this project over three months, a number of of features and 
fixes have been deprioritized due to time constraints. The following list 
details my suggestions of certain features to be implemented to increase overall
usability of the application.

### Search By Location
* _Goal:_ Generalize the current "Search by Call Sign" functionality by allowing
various city identifiers (city, landmark, region, etc.) as input fields.
* _Approach:_ Looking into geocoding for a location search. the OSMGeocoder is 
currently integrated into the application, and I've been recently trying to 
develop a dynamic intersection function, but it's currently running too slow to 
be effective. Once that is finished, I'm looking to pull the gejson produced by
the Nominatim Geocoder to check station intersections.

### Modularize For Different Users
* _Goal:_ Organize the application code in a manner such that different features
are available to different users (search by congressional district, demographic
information, etc.).
* _Approach:_ Segment modular functions into different files, look into using 
the React JS framework as a way to logically organize visual components.

### Rendering Optimization
* _Goal:_ Currently, the rendering of all FM contours or a few coverage shapes 
is very computationally intensive and makes map interaction noticeably slower. 
Further optimization in rendering would dramatically increase usability.
* _Approach:_ 

### Keyboard Shortcuts
* _Goal:_ I recently learned that web traversal without a mouse is a usability 
concern, and the current build is fully navigable by using the tab key. However,
binding functions to keyboard shortcuts would expedite traversal. 
* _Approach:_ 

### Rebinding Shape Deletion For Touch
* _Goal:_ Shape deletion is not available on touch devices because this 
application was developed desktop-first. This step is necessary to preserve core
functionality across all device types.
* _Approach:_ 

## Helpful sites:
[multiple dropdown for districts](https://codepen.io/ryanjgill/pen/gcmrH)

[d3labels that stay in place as zooming](http://plnkr.co/edit/M60XcUbi9w0LaKHiJp5b?p=preview)

[Leaflet + d3](http://blockbuilder.org/enjalot/eaf648eb5fd33a59b3865d8fc4f5eade)

[clicking on d3:](https://bl.ocks.org/john-guerra/43c7656821069d00dcbc)

[Includes instructions to switch from geojson to topojson](https://bost.ocks.org/mike/map/)

[Intersection code from this JSFiddle](http://jsfiddle.net/nathansnider/egzxw86h/)

## Copyrights:
[Open Icon](https://useiconic.com/open/) is under the [SIL Licensed](http://scripts.sil.org/cms/scripts/page.php?item_id=OFL_web)

[The Ubuntu Google Font](https://fonts.google.com/specimen/Ubuntu) is under the [Ubuntu Font License](http://font.ubuntu.com/ufl/)

[OSMGeocoder](https://github.com/k4r573n/leaflet-control-osm-geocoder) is under the [Open Stree Map Nominatim Usage Policy](https://operations.osmfoundation.org/policies/nominatim/)
