# trackside_WMT
Where's my train!?

# Trackside

An isometric 3D map of GO Transit's Lakeshore West line, with trains running on the published Metrolinx timetable. Use it to see where your train is, or leave it running as a day-and-night screensaver.

Built with plain HTML, CSS and JavaScript, plus [three.js](https://threejs.org/) loaded from a CDN. No build step.

## Features

- **Where's my train.** Follow any train between Union and West Harbour. See its next stop, arrival time and full stop list, and its speed when it's moving.
- **Real timetable.** Every Lakeshore West service day in the Metrolinx GTFS feed is packed into the page. It opens on today's timetable at the current Toronto time.
- **Live mode.** At 1× the clock reads Toronto time every frame, so trains sit where the schedule says they should be right now.
- **Map stretch.** Distances can be stretched 1× to 20× while trains and buildings keep their size, so slow real-time movement is easier to see.
- **Screensaver.** The camera tours between moving trains and the whole line, and the lighting follows the clock from dawn to night. It keeps the screen awake where the device allows.
- **Mobile friendly.** Phones get a bottom sheet and a single row of controls; desktops get a side panel.

## Running it

Open `trackside.html` in a modern browser. Everything except three.js and Google Fonts is inside the file, so no local server is needed.

Dependencies, loaded from a CDN:

- `three.js` r128 (cdnjs)
- `OrbitControls` from three@0.128.0 (jsDelivr)

## Controls

| Control | What it does |
| --- | --- |
| **Live / 10× / 30× / 60×** | Playback speed. Live matches the real clock. |
| **Line** | Zooms out to the whole line. Tap again to follow the selected train. |
| **Screensaver** | Hides the interface and starts the camera tour. Tap or press any key to exit. |
| **Map stretch** | 1×, 3×, 4×, 5×, 10× or 20× spacing between stations. Remembered per browser. |
| **Trains running now** | Pick which train to follow. |
| Drag / pinch / scroll | Rotate and zoom the map. |

## Data

The timetable comes from the Metrolinx GO Transit GTFS static feed. Only Lakeshore West rail trips (`route_id` ending in `-LW`) are kept, trimmed to the 12 stations on the map and stored as compact JSON inside `<script id="gtfs">`.

- **Feed in use:** October 5 to November 27, 2026.
- **Dates outside the feed** fall back to a typical timetable for that weekday.
- **Trips to Hamilton GO Centre, Confederation or Niagara Falls** leave the map after Aldershot or West Harbour.

### Updating the timetable

1. Download the latest GO GTFS zip from [Metrolinx Open Data](https://metrolinx.com/en/about-us/open-data) and unzip it into a folder, for example `gtfs/`.
2. Run the packing script:

   ```bash
   python3 pack_gtfs.py gtfs
   ```

3. Replace the contents of `<script id="gtfs" type="application/json">` in `trackside.html` with the generated `lw.json`.

## Limitations

- **Scheduled times only.** Static GTFS has no live delays or platform numbers, so every train shows as "Scheduled".
- **Approximate geometry.** The track is a smooth curve through the station coordinates, not the surveyed alignment, and the city blocks are generated decoration.
- **Exaggerated scale.** Trains and buildings are drawn much larger than the map's distance scale so they stay readable.

## Roadmap

- **Live positions and delays** from the GO Transit Open Data API, through a small caching proxy so the API key stays private and Metrolinx sees one request per feed every 30 seconds, however many people have the page open.
- **Platform guide:** a station-by-station animation showing how to get from the entrance to the right platform.
- More lines.

## Credits and licence

Contains data provided by Metrolinx. Use of GO Transit data is subject to the Metrolinx GO Transit API terms of use and the Open Government Licence – Ontario. This project is independent and is not endorsed by or affiliated with Metrolinx or GO Transit.
