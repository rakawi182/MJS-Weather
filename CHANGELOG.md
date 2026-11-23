# Changelog

Only the recent versions are listed here. The full record of every version is in the **Notes** tab of the app.

## C79
- **ETA counted from the target time.** The rain cells come from the hourly sum that ends at the next hour stamp, so they sit where the rain was on average half an hour before that stamp. The closest-pass time is now counted from the cell position moved on by (target time − centre of the field). Before, the ETA hardly moved within one hour (for example 100.3 → 100.7 min after 30 min in the logs). Applies to all motion vectors, the 300-member cone (≤ 30 / ≤ 60 min and P10–P90), the flag inputs that use the ETA, the C73 ring labels and the C72 time lines of the rain map. New CSV column `nb_field_age_min`.
- **Radar fit not used at the search limit.** If the best shift lies on the border of the ±18 km search window, the field is shown unshifted (`rad_fit_ok` = 0, `rad_fit_dx_km` = `rad_fit_dy_km` = 0).

## C78
- Display only: cleaner cold-cloud outlines (3 km grid, gaps closed, small regions dropped), one legend line for the centroid cross and the two arrows, a softer pattern-shift arrow that stays on the map, and the cooling share of the last scan pairs in the "Cloud ≤ 235 K" line of the arrival block.

## C77
- Display only: the steering-speed label of the polar maps gets a half-opaque plate.

## C76
- Display only: labels are drawn last, avoid the centroid cross and the arrows, and have a half-opaque plate.

## C75
- Display only: polar-map labels (time lines, ETA labels, steering speed) avoid each other, the fixed texts, the site and the dots.

## C74
- Display only: shorter captions under the rain map, mini-ring legend, clearer ring for cells passing 10–20 km away.

## C73
- Display only: cell arrival marks on the rain map (on track with ETA in minutes, fades, passes 10–20 km away, moving away).

## C72
- Display only: 30/60/90 min arrival time lines across the upwind wedge of the polar maps.

## C71
- Cold tops of the latest IR scan on the rain map, parallax correction of cloud tops, local cloud flow by block matching, and projected cold-cloud outlines (display and CSV only).

## C70
- Observation log: sky entries (clear, partly cloudy, overcast) next to Rain, Thunder and Dry; `obs_sky` column in the CSV.

## PWA packaging
- `index.html` is MJS Weather C79 with PWA hooks added to the `<head>` and a service-worker registration script. The application code is unchanged.
