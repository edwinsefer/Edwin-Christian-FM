# Edwin Christian FM — Non-Stop Audio Rebuild Plan

## Approved direction
Build a combined Tamil / Malayalam / English Christian music experience using:
1. User-owned MP3 files selected locally by the listener.
2. External MP3 tracks only when the individual recording has a verified reuse/streaming licence and a stable HTTPS direct-audio URL.
3. Official YouTube videos/playlists through the YouTube embedded player / IFrame Player API. Never extract YouTube audio or bypass the player.

## Non-negotiable safeguards
- Work only on feature/nonstop-christian-audio; do not change main until Edwin approves after testing.
- Preserve the current main branch as the rollback point.
- Do not bundle copyrighted songs or scrape/download from YouTube.
- Store track title, artist, language, source URL, licence, attribution, and verification date for external MP3 items.
- Do not label an external stream as reliable until it passes playback and repeat/next-track tests.
- Browser autoplay may require a user gesture; provide a clear Play action and recoverable error state.

## Player architecture
- One unified queue per language: Tamil, Malayalam, English.
- Sources: My MP3 Library, Licensed MP3, YouTube Playlist.
- MP3 queue: HTMLAudioElement; validate URL and media errors; preload metadata; on ended advance; on error skip with bounded retries; avoid infinite loops; display current track and queue position.
- Local MP3: File picker / multi-file selection; keep file objects in memory for the session; do not upload files anywhere without explicit consent. Explain that local selections may need reselecting after refresh.
- YouTube: official embedded player with visible player controls/branding and IFrame API; use only playlist/video IDs supplied by Edwin or verified public playlists. Do not promise uninterrupted autoplay because browser policy, removed videos, embedding restrictions and network conditions can interrupt it.
- Separate source modes in the UI; remove the misleading Organ Chime as a music source (may retain only as optional test tone if Edwin later requests it).
- Accessibility: clear Play/Pause, Next, Previous, Volume, source/language status, and visible errors.
- Security: no untrusted data inserted via innerHTML; validate HTTPS URLs; no mixed-content HTTP streams.

## Verification gates
1. Static code review.
2. Test with at least two user-owned MP3 files: play, next, ended, error, repeat disabled/enabled.
3. Test a licensed external MP3 only after licence and direct URL are verified.
4. Test YouTube playlist through official embed; verify allowed embedding and browser autoplay behavior.
5. Desktop and mobile browser checks.
6. Edwin approves before any merge/deployment to main.

## Research notes
- YouTube supports embedding videos and playlists and provides an IFrame Player API for queueing and player-state events. Playback may be blocked by browser autoplay policy and some videos may disallow embedding.
- “Free to listen” does not automatically mean permission to redistribute or stream a recording. Verify the licence attached to each specific track; prefer CC0/Public Domain or explicit permission. Keep attribution and licence details.


## Implementation checkpoint — 2026-11-06
- Added `audio-studio.html` as a separate experimental player page; the existing `index.html` remains unchanged.
- Added separate MP3 queues for Tamil, Malayalam, and English.
- Local MP3 files play directly in the browser and are not uploaded.
- Direct external MP3 URLs are accepted only over HTTPS and only when the URL path ends in `.mp3`; licence verification remains a human approval step.
- Added a small candidate FM list for all three languages using HTTPS URLs found in the previous app's backup stream fields. These URLs are **unverified candidates**, not confirmed working stations.
- Added bounded MP3 error skipping and FM candidate failover.
- Added official YouTube no-cookie playlist embedding; Edwin confirmed that the embedded YouTube player is acceptable.
- JavaScript syntax check passed and duplicate HTML IDs check passed.
- **Not yet verified:** real browser playback, each live FM endpoint, actual MP3 file compatibility across devices, YouTube playlist embedding permissions, and mobile runtime behavior.
- Do not merge or deploy to the main branch until Edwin tests the preview and explicitly approves.


## FM source verification research — 2026-11-06
Research is source discovery only; no real browser playback test has been performed.

### Tamil
- Arulvakku is a real Tamil Christian/Bible radio service and is listed as an HTTPS MP3 stream by a radio directory. A separate public station reference lists `https://stream.arulvakku.com/radio/8000/radio.mp3`. This is a stronger candidate than the current `https://server.arulvakku.com:8443/live`, but it is not yet confirmed by the broadcaster or browser playback.
- Zeno.FM pages exist for Theophony Tamil Christian Radio, Zion Station (Tamil/English Christian), Tamil Catholic FM, and other Tamil Christian stations. These pages establish station presence, not the exact raw audio endpoint or permission to embed. Prefer official Zeno player/embed links rather than guessing stream URLs.

### Malayalam
- Shalom Media's official Radio Wind page confirms a Malayalam radio service with 24-hour Christian songs and inspirational programmes. The public page does not expose a verified direct stream URL; the current raw URL must remain unverified.
- Malayalam Bible Radio is a real 24x7 Rafa Bible Radio service, but it broadcasts dramatized Bible audio, not a continuous Christian music FM station. Do not classify it as music radio.
- Rafa Radio is listed as a Malayalam Christian music station with a live HTTPS stream by a radio directory, and its own official online-listen destination is referenced by its app listing. Obtain the official player/stream endpoint before integrating.

### English
- Premier's official FAQ confirms listening through its app, Radioplayer app, and website, but does not publish a direct audio URL. A third-party radio directory lists `https://pcr.streamguys1.com/pcrnational-96k.aac`; this is a candidate only, not broadcaster-confirmed or browser-tested. The existing `https://stream.premier.org.uk/pcr` remains unverified.
- Abiding Radio's official website explicitly provides HTML audio source URLs and embeddable players. For example, its Sacred player uses `https://www.abidingradio.org/radio/sacred` or the official embed at `https://streams.abidingradio.com/public/abiding_radio_-_sacred/embed`. Prefer the official player/embed rather than the current unrelated-looking AsuraHosting candidate. Abiding Radio is English-language Christian music, including sacred and instrumental channels.

### Implementation decisions pending
- Do not mark any station as verified until it plays in the actual target browser/device for at least several minutes and survives a reconnect/next-station test.
- Use official embeds where the broadcaster supplies them (Abiding Radio; potentially Zeno.FM) instead of extracting undocumented stream URLs.
- Keep Shalom Radio Wind and Rafa Radio as separate Malayalam music candidates; keep Malayalam Bible Radio in a clearly labelled Bible-audio category if Edwin wants it, not under music FM.
- Need a browser/device playback pass for all three languages. The web research cannot prove CORS behavior, codec compatibility, stream uptime, or mobile background playback.
- No code, main branch, or deployment was changed during this research pass.
