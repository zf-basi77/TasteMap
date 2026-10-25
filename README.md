# Tastemap

Drop in your Spotify listening history and see it drawn as a map: the regions your taste falls into, the artists that bridge them, and the routes you take between artists.

Everything runs in your browser. Your files are never uploaded, and the page makes no network requests, so you can check this yourself in your browser's network panel.

**Live page:** `https://YOUR-USERNAME.github.io/tastemap/`

## What you get

- A map of your artists, grouped into regions, with a path for any listening session
- How you move: do you glide between neighbours or jump across the map, and can the map predict your next artist
- Your bridge artists, gravity wells and listening hubs
- How your exploration and variety change month to month
- When you listen and when you skip

## Use it

1. Request your **Extended streaming history** on [Spotify's privacy page](https://www.spotify.com/account/privacy). Tick only that option and confirm the email Spotify sends. Spotify says it can take up to 30 days.
2. Unzip the download.
3. Open the Tastemap page and drop in the `Streaming_History_Audio_*.json` files, all at once.

No data yet? Use **Try the demo data**.

## Sharing your results

Under **Share your results**, you can download or copy a results file. It contains only aggregates:

- overall counts, and first and last month of your history
- results of the movement tests (step ratio, next-artist hit rates)
- plays and skips by weekday and hour
- month-by-month new-artist share, diversity and step distance
- region sizes and how often you move between regions
- optionally, artist rankings, with or without names

It never contains songs, individual plays, exact timestamps or your original files. The page shows the exact JSON before you export. Artist names are off by default because they can reveal a lot about a person.

### Results file format (schema 1)

| Key | Contents |
| --- | --- |
| `tastemap` | schema version, random `exportId`, export date, `source` (`spotify` or `demo`), artist detail level |
| `overview` | plays, sessions, mapped artists, first and last month, region count, analysis settings |
| `movement` | step ratio vs shuffled sessions, p-value, same-region share, next-artist test, rank curves |
| `returnByDistance` | chance of returning to an artist, by distance on the map |
| `regions` | per-region artist count and play share, region-to-region transition share and lift |
| `rhythm` | plays by weekday and hour, all plays by hour, skips by hour (browser's local time) |
| `overTime` | monthly means of per-session values |
| `artists` | bridges, gravity wells and hubs. Present unless left out. Ids like `A3` mean "3rd most-played mapped artist"; `name` appears only if included |

## Collecting results from participants

Set these three values in the `CONFIG` object near the top of the script in `index.html`:

```js
const CONFIG={collectUrl:"",collectLabel:"",repoUrl:""};
```

If `collectUrl` is set, a button appears after the results are exported. Point it at somewhere people can submit the file, such as a Google Form, a GitHub Discussion, or a `mailto:` link. The page deliberately never sends anything itself.

## Run locally

Open `tastemap.html` in a browser. There is no build step and no dependencies. The two fonts (Archivo and Newsreader, both under the SIL Open Font License) are embedded in the file.

## How it works

The map is learned from your sessions alone: no mood data and no Spotify account. A session ends after 30 minutes of silence, and plays under 30 seconds count as skips. The map is fitted on your earliest 70% of sessions, and the tests use the later 30%, which the map never saw. Only moves between different artists count. This is a rough prototype: artists only, with no confidence intervals yet.

## License

Add a license of your choice, for example MIT.
