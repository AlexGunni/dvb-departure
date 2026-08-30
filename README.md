# DVB Departure Board

A single-page departure tracker for four DVB stops in Dresden: Burgkstraße,
Tharandter Straße, Bünaustraße and Conertplatz. Departures are split into two
columns per stop, one per platform, so each direction is readable at a glance.

Data is fetched every 30 seconds. Between fetches the countdown recalculates
locally every 5 seconds, so the minutes tick down without extra requests. The
page also refetches whenever the tab becomes visible again, which matters on a
phone returning from standby. If a fetch fails, the last known departures stay
on screen and the header switches to a red "stale" notice with the time of the
last successful update.

<img width="485" height="412" alt="image" src="https://github.com/AlexGunni/dvb-departure/blob/main/preview.png" />

## Configuration

Everything adjustable sits in one block at the top of the script in
`index.html`:

```js
var STOPS = [
  { name: "Burgkstraße", id: "33000172", walk: 0, steige: {} },
  ...
];
```

- **`id`** — stop ID. Leave it `null` and the page looks it up once at startup
  via the pointfinder, logging the result to the console so it can be hardcoded.
- **`walk`** — minutes on foot to that stop. Departures you can no longer catch
  are greyed out rather than hidden. `0` disables it.
- **`steige`** — column labels keyed by platform number, e.g.
  `{ "1": "outbound", "2": "towards city" }`. Empty falls back to
  "Steig 1" / "Steig 2". If the API returns no platform data for a stop, that
  block renders as a single full-width column instead.

Also configurable: `LIMIT` (departures fetched per stop), `PER_COLUMN` (rows
shown per column), `REFRESH_MS`, `STALE_MS`.

## Caveat

The VVO WebAPI is undocumented and unofficial — reverse-engineered and described
at [kiliankoe/vvo](https://github.com/kiliankoe/vvo). There is no availability
or stability guarantee. Fine for a personal board, not something to build on.
