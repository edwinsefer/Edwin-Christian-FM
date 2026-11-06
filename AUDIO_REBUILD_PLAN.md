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
