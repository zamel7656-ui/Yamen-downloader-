# license/ — Yamen Downloader license files (GitHub-only)

These files ARE the license server. The client fetches them from
`raw.githubusercontent.com/zamel7656-ui/Yamen-downloader-/main/license/`
every 30 minutes. Edit them ONLY from the Admin tool (it signs + pushes),
or by hand + `git push`.

- `kill-switch.json` — `{"kill_switch": false}` stops / resumes ALL copies.
- `serials.json` — signed serial records, keyed by SHA-256(serial).
  NEVER put a plain serial in this file. The Admin tool signs each record
  with the private key; the client verifies with `public-key.txt`.
- `revoked.json` — SHA-256 hashes of revoked serials ONLY (never serials).
- `features.json` — `{"default": {...}, "by_serial": {hash: {...}}}`.
- `free-period.json` — free-period DURATION model (v1.1): the owner decides
  the duration in months (`free_months`, 0 = unlimited) + start date
  (`free_since`); `free_period_on` is the master switch (OFF = every
  unactivated client needs a serial). `free_until` is kept for OLD
  clients (v6.0) that only know the date.
- `activations.json` — signed admin-side activation records (v1.1):
  `serial_hash + device_id`, Ed25519-signed. The client picks its own
  record up on its 30-min check and activates automatically.
- `programs.json` — per-program control (v1.1): one entry per program
  (`pc`, `mobile`, `tv`, `mobile_admin`, `kids`) with `killed` +
  `reason`. Stopping ONE program does not touch the others.
- `clients.json` — client registry (v1.1, panel data, NOT a control
  file): one entry per client device — name, model, country, IP, app
  version, OS, ping, connection time, last seen, downloads today,
  current activity + speed, serial hash, MAC. The client reports these
  fields with its license check; the admin can also fill them in by
  hand. A missing file is never a control problem.
- `banned-macs.json` — permanent MAC ban list (v1.1): banned devices
  can never run the program again, even with a new serial or after a
  reinstall. Checked FIRST by `check_license`, before the kill switch.
- `client-passwords.json` — client app passwords (v1.1, §19): scrypt
  hashes ONLY, Ed25519-signed. Never plaintext.
- `password-resets.json` — one-time reset approvals (v1.1, §19):
  SHA-256 of the code only, 10-minute expiry, max 5 attempts.
- `announcements.json` — messages to clients (v1.1, §20): target
  `all` / one device / `group:<name>`; signed.
- `tickets.json` — support tickets from clients (v1.1, §20):
  open / answered / closed, with reply threads.
- `update.json` — update push (v1.1, §20): version + download URL +
  notes + `force` flag.
- `pricing.json` — plan prices (v1.1, §22/§27): per-currency prices
  (`currencies` map, default currency mirrored in `plans`) + scheduled
  price offers (`price_schedule`).
- `discounts.json` — discount codes (v1.1, §22): percent, expiry,
  max uses, uses (best-effort).
- `scheduled.json` — scheduled actions (v1.1, §22): activate /
  deactivate / revoke at a set time; executed by whichever panel
  refreshes first.
- `banned-countries.json` — banned countries (v1.1, §22): ISO codes;
  checked right after the MAC ban.
- `payments.json` — payments per client (v1.1, §23): cash / transfer,
  amount, currency, date, note.
- `grace.json` — grace period after expiry (v1.1, §23): default 3
  days, max 30.
- `distributors.json` — distributors (v1.1, §24): name, WhatsApp,
  commission %; serials carry a `distributor` field.
- `maintenance.json` — maintenance mode (v1.1, §24): stops all copies
  with a message until turned off.
- `banner.json` — in-app banner (v1.1, §24): title + body shown
  inside client apps.
- `app-settings.json` — remote app settings (v1.1, §24): default
  speed, max threads, block-ads default.
- `swaps.json` — client device swaps (v1.1, §25): max 2 swaps per
  serial every 6 months; the admin's own transfers are not limited.
- `wipes.json` — pending remote wipes (v1.1, §25): the client wipes
  its own data on its next check.
- `loyalty.json` — loyalty gift extensions (v1.1, §25): +1 month for
  clients with the program a year or more.
- `survey.json` — customer satisfaction survey (v1.1, §27): title +
  questions + active flag.
- `survey-results.json` — logged survey answers (v1.1, §27).
- `gifts.json` — gifted licenses (v1.1, §28): from device, to name /
  WhatsApp, serial hash, status sent / activated.
- `banned-macs.json` — permanent MAC ban list (v1.1): `{"banned": [{"mac":
  "AA:BB:CC:DD:EE:FF", "reason": "...", "device_id": "...", "banned_at":
  "YYYY-MM-DD"}]}`. A banned device can never run the program again —
  new serial or reinstall makes no difference. The client checks it at
  startup and on every 30-min check (before everything else).
- `public-key.txt` — Ed25519 PUBLIC verification key (safe to publish).

DO NOT commit `admin_keys.json` (the private signing key) anywhere.
- `kids-packages.json` — Yamen Kids packages (v1.1, §29): master +
  N children, price set by the admin (0 = not set yet); a serial tied
  to a package opens 1 + N devices.
- `commands.json` — signed remote commands to clients (v1.1, §31):
  suspend/resume downloads, make public/private, force update.
- `update.json` — targeted updates (v1.1, §32): an `updates` list of
  {version, program, url, target, rollout_at, notes, forced}; the
  top-level version/url/notes/forced mirror the newest PC update for
  old clients.
- `voice-messages.json` — signed voice-message records (v1.1, §33);
  the audio files live in `voice/` (`<id>.wav/.mp3/.ogg/.m4a`, max
  5 MB each), SHA-256 in the record.
