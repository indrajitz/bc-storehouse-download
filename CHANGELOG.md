# Changelog

What changed in each build, and why. Written for whoever has to work out
whether the version they are running explains what they are seeing — so the
entries say what you would notice, not what the diff said.

Check your version in **Settings**; it is the first thing on the page.

## 2.1.7 — 27 October 2026

Fixes from a second whole-library audit: four independent review passes
(security, two-machine data safety, performance, silent failures) over
2.1.6, with the full suite run against the result. No helper update is
needed for sends to keep working (still helper 9), but update the Cinema 4D
helper to get its archive fix.

- **Long messages no longer widen the window.** A Substance send's result
  runs to hundreds of characters, and the status line grew the window to
  fit it -- every send made the window wider. The line now shortens what
  it shows; hover for the whole message, which is in the log as always.
- **A folder of loose textures imports as several assets again.** Dropped
  as the textures category, a folder of unrelated images (decals, grunge,
  references) arrived as one asset named after one of its files, because
  only material-style sets were ever counted. Distinct images now come
  apart; a set's suffixed maps, and the 4K and 8K of one texture, still
  stay together. The Import page's grouping choice also goes back to
  "Group automatically" when a new list starts, so "Keep together as one
  asset", picked for an earlier drop, can no longer quietly make the next
  folder -- of HDRIs, or anything else -- one item.
- **Emptying the trash can no longer lose files to a restore.** With two
  machines on one library, Restore during another machine's Empty trash
  could bring an asset back half-deleted -- some files permanently gone --
  while both machines reported success. Each entry is now renamed out of
  sight before anything is deleted, so one side gets the entry whole and
  the other is told; a crash mid-delete is finished by the next empty.
- **A changed installer never runs quietly.** Run its installer takes the
  program's fingerprint before launching and compares it with the last run
  on this machine: a copy that changed on the share -- an update, or a
  swap -- runs only after you say the change is expected, and the receipt
  records what actually ran.
- **A send that will never answer says so.** When a helper died mid-build
  or its result could not be written back, the window polled forever and
  the artist never heard. After two minutes it now says no answer came and
  names the helper's error log. Sends to a closed application still wait
  quietly, and long bakes with a live heartbeat are untouched.
- **Cinema 4D: a crafted .sbsar cannot balloon memory.** The helper's
  archive reader refuses an oversized encoded header outright, with the
  same bound and words as the app's own reader.
- **Keep one of two look-alikes checks again before acting.** An asset
  replaced under the same folder name while the confirmation was open is
  refused instead of silently trashed, and a kit that gained the dropped
  asset meanwhile is repointed too.
- **A damaged offline copy is no longer invisible.** The Offline page says
  it is there and how to clear it, and Take offline explains the dead end
  instead of pointing at a page that could not see it.
- **Unattended runs report failure.** `storehouse catalog update` -- what
  Task Scheduler runs -- exits nonzero when every provider failed and
  nothing was collected, instead of saying success while the team catalog
  goes stale; `import` and `repair` exit nonzero on partial failure too.
  Two machines publishing the team catalog at the same moment can no
  longer drop one machine's records, and pinned/recent lists can no longer
  be wiped by two programs saving them at once.

## 2.1.6 — 25 October 2026

Fixes from a whole-library audit of 2.1.5. No helper update is needed
for sends to keep working (still helper 9), but update both helpers
anyway to get the Cinema 4D and Houdini fixes below.

- **Switching library no longer edits the old one.** After choosing
  another library folder in Settings, Organise, Settings, Import, the Edit
  page and the player kept working on the library they first opened, so a
  tag rename or a kit change could land in the wrong library. Filters, the
  undo list and look-alike results from the old library are cleared.
- **Empty trash deletes only what you confirmed.** It used to delete
  everything in the trash when the job ran -- including an item a
  colleague trashed a moment later, or the original an import with Replace
  had just set aside. It now removes exactly the items it showed you.
- **Model cards show the model.** About 170 models showed a texture map
  (a diffuse or bump image from their texture folder) as their card picture;
  they now show a clay render of the model itself. Real product photos are
  kept, and nothing in the library is rewritten. Heavy models render solid,
  not as a speckled outline.
- **Shared-library edits are no longer lost.** Renaming or removing a tag
  across many assets no longer overwrites a colleague's tag edit made at
  the same moment; a rescan no longer misses an asset a colleague imported
  while it ran, or undoes edits made during it; saved filters and kits keep
  fields a newer build wrote; deleting a kit while someone adds to it no
  longer brings it back.
- **Undo and shortcuts.** Ctrl+Z after Move to trash brings the asset back
  instead of undoing something earlier. Grid shortcuts (Edit, Delete, send
  to Cinema 4D or Houdini) only act while the grid is showing, so typing on
  another page can no longer send or trash an asset you cannot see.
- **No more freezes on big actions.** Add a version, Add to kit, Restore
  all and Empty trash run in the background; searching no longer re-reads
  every kit from the share; importing a whole-library archive takes minutes
  instead of hours.
- **Big pictures and first run.** A 16K EXR no longer takes gigabytes of
  memory to make its thumbnail, a screen of huge images no longer stalls
  the grid or leaves cards blank, and closing the app no longer hangs on a
  big decode. Thumbnail caches are capped (2 GB, quick look 1 GB). A
  brand-new user whose settings cannot be saved, or who picks the folder
  above a library, gets a clear message instead of a stuck first run.
- **Cinema 4D.** A Substance material pasted into another scene gets the
  save hook too, so that scene never saves reading the temporary folder or
  with Live on. FBX models with recorded units are no longer scaled twice
  (a metres model arrived 100x too big). A damaged .sbsar can no longer
  hang Cinema 4D.
- **Houdini.** Opening or starting another scene in the middle of a send
  stops that send cleanly and puts it back, instead of marking it built or
  building the rest into the new scene. Turning Live Substance off keeps a
  packaged or hand-made texture path. Backup copies no longer move an
  unsaved bake into the backup folder. Packaging no longer lists the bake
  folder as a missing file.
- **Vendor catalog.** One machine's Megascans or GSG folder no longer marks
  everyone else's records as gone, and two machines publishing at once now
  merge instead of failing. `storehouse catalog enrich --apply` carries on
  past an unreadable asset.json.

## 2.1.5 — 24 October 2026

**Update both helpers on every machine** (Settings, Update helper, for
Cinema 4D and for Houdini), then quit and reopen Cinema 4D and Houdini.
The helpers are now version 9; older ones are blocked from receiving
sends until they are updated.

- **Substance sends no longer freeze Cinema 4D.** The bake runs in a
  background process at low priority while you keep working, and Cinema 4D
  builds the material from the finished maps in about a second. Re-bake
  works the same way. If the background process cannot start within a
  minute it bakes in Cinema 4D as before, and it ends with Cinema 4D if
  Cinema 4D closes. The material goes on what was selected when you
  pressed Send, not on whatever is selected when the bake finishes.
- **Houdini stays usable during a Substance send.** The bake is cooked in
  small pieces between Houdini's own events: a 4K archive takes as long as
  before, but Houdini never pauses for more than a few seconds instead of
  freezing for the whole bake.
- **Sending the same material again is almost instant.** Each machine keeps
  the maps it baked (same archive, size and settings) and copies them
  instead of baking again; scenes always get their own copies, never the
  cache's. A bake whose inputs were changed while it ran is not kept.
- **You can see what a send is doing.** The Send card and the status bar
  say what the helper is on ("swamp water — map 3 of 8 (4K)"); pressing
  Send again while it builds says "already building" instead of making a
  second material; a send after a crash is never swallowed by the build
  that died. Pressing Send never waits on the network.
- **Saved scenes are safe for the farm.** Cinema 4D switches Live Substance
  off in every saved file, as Houdini did; scenes saved by 2.1.4 with Live
  on get the save hook when opened. Saving a scene with a duplicated
  Substance material no longer deletes maps the copy reads.
- **Import.** A name typed into a row is kept when Import is pressed at
  once (no more duplicates under the old name); a re-run of an interrupted
  bulk import resumes the half-copied asset; the Inbox no longer copies
  files the library already has; edits made while a large drop is planned
  survive; big drops and archives no longer freeze the window or get lost
  mid-plan; Import an archive merges its tags and shows its warnings; every
  import gets its Substance swatches, which now bake on any Houdini.
- **Materials.** Houdini MaterialX gets gloss and DirectX normals right and
  names the maps it cannot use; Cinema 4D no longer loses base colour on
  some archives, and re-bakes reset values the new plan no longer sets
  while keeping your own hand edits.
- **The window fits a laptop.** Minimum size with the asset panel open is
  now 925 × 366 (was 1,495 × 966); the filter row and selection bar wrap;
  the left rail and the panel scroll and never scroll sideways. A failed
  save or tag edit gives the page back instead of leaving it greyed out.
  Tags no longer disappear right after first-run setup. Quick look closes
  when the grid refreshes, so a key press cannot send another asset.
- **Interface fixes.** Table buttons in Organise, Trash and Jobs are no
  longer cut ("emov"); long paths in Settings no longer push its controls
  off screen; "&" in button names shows; synonyms can be removed again;
  the side panel names issue kinds in words.
- **Data safety.** Two machines importing the same new name at once can no
  longer merge two products into one folder; `storehouse repair` keeps the
  roles people chose; removing an offline copy refuses while set-aside
  edits exist; `init --reset-vocabulary` needs `--yes` and keeps a backup;
  `backup --restore` refuses a mistyped path.
- **Install and update.** Updating from the share copies the new version
  beside the old one and swaps only when complete, so a dropped connection
  leaves a working app; Setup will not run while BC Storehouse is open;
  uninstall uses the Setup's own uninstaller; rollback reports a failed
  install; the app is about 21 MB smaller.

## 2.1.4 — 20 October 2026

- **Library operation follow-ups.** Publishing inbox items reports every
  folder already moved even if a later item fails or the job is cancelled.
  Preview requests and adoption check the asset identity at the recorded
  path. Consolidating copied assets preserves kit membership, and course
  progress follows the library identity when the index moves.
- **Tagging and browsing follow-ups.** Renaming tags preserves their origin
  and finds assets from their sidecars, including changes absent from this
  machine's index. Word suggestions conflicting with an accepted
  single-valued tag require review. Replacement images can retry a refused
  thumbnail, and Organise and Look-alikes open before the index exists.
- **Vendor and compatibility follow-ups.** Local vendor-folder settings stay
  with their machine; damaged preferences are kept before replacement.
  Inspecting a Houdini scene or HDA reads at most the first 64 MB plus one
  byte, and the compatibility panel says when the result is partial.
- **Houdini scene packaging keeps Redshift textures with literal names.**
  Filenames containing `$`, a word-starting `~`, or other escaped characters
  are resolved as Redshift reads them before copying. They previously
  appeared missing during packaging even though the material could read
  them. The send-queue verification tool also checks the current helper
  version instead of its obsolete version-4 expectation.

- **Start a new library from the window.** Choosing a folder that is not a
  library yet, on first run or in Settings, now offers to set it up -- its
  settings file, a folder per category and the tag list -- which before
  only `storehouse init` did. Choosing No uses the folder as it is;
  choosing the folder above a library points you to the library inside.
  On 2.1.2 a brand-new library also closed the window on opening ("no such
  table: files"); that was fixed earlier in 2.1.4. `storehouse init` no
  longer replaces a `_library.json` it cannot read -- a new identity would
  cut the library off from its index -- it stops and says so. The first
  screen says so too, and an empty library offers **Import…** instead of
  "Nothing here."

- **Readable in Windows light mode.** Every dialog, text box, table and
  list -- the pop-ups ("Close anyway?", "Apply?", the trash and setup
  questions) among them -- had near-white text on Windows' light-grey or
  white background when Windows apps are set to light mode (its default),
  so the text vanished and only the buttons showed. They now take the
  window's dark surface whatever the Windows theme. Dark mode looked
  right before and still does.

- **Removed: the old window (`--tk`) and the web UI.** The Qt window is the
  one UI. Typing `--tk` prints a one-line notice and opens the normal
  window; `storehouse serve` is gone.

- **Search and refresh no longer block the window** (PRR 04 F2, the
  deferred part). The index query behind every refresh -- search, filters,
  categories, the rail's counts and the facet pills -- runs on a search
  worker off the UI thread; the grid keeps its rows (and the count label
  its number) until the new ones land, and a stale answer is dropped by a
  generation token, so typing fast lands only the last query's rows.
  Callers that act on the filled grid (`show_imported`, `restore_place`)
  chain through the new `refreshed` signal / `refresh(then=...)`; tests
  that need the old synchronous contract use
  `tests.fixtures.synchronous_refresh`. On the review's harness at 5×
  (lan): refresh "All" 591 ms on the UI thread → 0.2 ms (the rows land
  after the same ~0.6 s, without blocking); cold start to a drawn window
  1,073 ms → 289 ms; the 579–709 ms search stalls during typing and
  category changes are gone.

Substance materials 1:1. Plan and owner decisions:
`docs/PLAN-SUBSTANCE-FIDELITY-2026-10-16.md`; how the pieces talk:
`docs/SUBSTANCE-CONTRACT-2.1.4.md`.

### Fixed (import, from the tester's 16 October log)

The details, and how each was reproduced: `docs/FIX-IMPORT-2026-10-17.md`.

- **"Import all separately" does what it says.** A set held in a
  subfolder of a vendor folder (`bark_birch_.../Sapling_painted_textures/`
  with `2K/` and `4K/`) could not be renamed -- it was planned again from
  its file list, without its folders, and collided with itself -- so
  Import was refused again after the button, for good. A rename now
  starts from what the row was planned from, a new name can no longer
  land on another row's folder, and a row that still cannot be renamed
  is named in the status line with the reason (it used to say "N renamed"
  over it).
- **Importing what is already in the library is skipped, and said.** A
  row whose every file is in the library already, byte for byte (the rule
  "Skip duplicates" has always used), is not imported again: not as a
  numbered second copy ("Thumbs 2", "sapling painted textures 2") and not
  added into another product's asset that happens to share its name. It
  is no name conflict either. The summary and the CSV list it as
  skipped -- "already in the library: all 6 file(s) are in
  materials/..., byte for byte" -- and an identical re-import of an asset
  says so on its "unchanged" row. Import a copy anyway, on the row,
  imports the second copy when that is what you want. A row whose check
  had not finished when Import was pressed is checked by the import
  before anything of it is copied. Nothing is moved or deleted from the
  sources.
- **A `--move` import never loses the only copy** (`ingest --move`,
  `bulk-import --move`). The source is deleted only once its copy is in
  the library whole; a failed swap leaves it where it was, never as a
  hidden temporary a later save sweeps away. A file still arriving in the
  library is no longer taken for a day-old orphan by a colleague's save
  in the same folder (it used to carry the source's old date). A library
  this build may not write is refused by the import itself, before a
  folder is claimed, and the batch stops there.
- **Quieter log.** The per-row auto-tag and vendor lines are DEBUG (Settings
  > About > Detailed log); each drop gets one INFO line of counts.
- **Every Import press is in the log** -- the plan's size and what came of
  it (refused and why, cancelled, or the batch it queued) -- and so is
  every message the import page shows. The tester's log showed renames
  and then nothing; the refusal was on the screen only.
- **A Substance import renders here only when Settings says so.** "Ask
  for renders of what I import" (off by default) now governs the swatch
  and ball rendering on this machine too -- they are still asked for, and
  wait in the queue otherwise -- and that render takes only this import's
  requests, never everyone else's queued renders.
- **A vendor's `Thumbs/` or `previews/` folder is no longer an asset.**
  Its pictures stay with the product they sit beside, at any depth, so a
  Megascans folder like `bark_birch_...` no longer arrives as six assets
  (`materials/thumbs`, a one-jpg preview asset, a second "sapling painted
  textures" made of the set's loose swatch, ...). A loose file named after
  a set folder beside it joins that set.
- **A shelf folder holding a single product** imports under the
  product's name, not the shelf's.
- **A set folder inside a product is imported once.** Dropping a shelf of
  vendor folders imported a nested set (`.../Sapling_painted_textures/`)
  twice -- as its own asset and again inside the product -- and when two
  products held a set of the same name, one of them was silently not
  planned. Every file is now in exactly one row, and same-named sets are
  both there, as a name conflict.
- **A shelf drop plans a product the way a single drop does** (audit #5).
  A vendor folder with a nested set folder imported as one asset dropped
  alone but as two -- product and set -- when its shelf was dropped. A set
  folder nested inside a product folder now stays part of that product, at
  any depth, so each product arrives as the same single asset (same
  destination, same files) either way; two products' same-named sets are
  both planned inside their own products, so their name conflict is gone.
  A product folder beside another is untouched: only nesting folds.
- **Import makes each folder once, not once per file.** Every folder
  creation is a round trip to the share; per file they were about a third
  of a large import's copy time. Nothing else about the copy changed.

### Added (app)

- **Bake size and baked / live, chosen when you send.** For a Substance
  material, the arrow beside "Send to Cinema 4D" / "Send to Houdini", the
  "Send Substance to ..." right-click menus and the send card offer 1K,
  2K, 4K, 8K and Baked or Live. A plain send uses the last choice, which
  this machine remembers (2K baked until you choose); nothing is written
  to the asset. The status line says what went: "sent a Substance
  material, 4K, live".
- **A Substance send says more about the material.** With the archive it
  now carries the map size to bake and baked or live, every Adobe Standard
  Material value (height scale, IOR, coat IOR ...) at its default -- each
  marked as a default, so the helper can say so -- and which maps the
  archive makes. The values are set on the Redshift material in the
  application, which is where to change them: Storehouse keeps no
  Substance settings of its own and writes nothing new to `asset.json`.
- **The helpers must be HELPER 7.** A 6 is told to update before
  anything is sent to it.

### Fixed

- **Opening an asset's panel no longer walks its folder on the share, and
  a tag saves off the window** (PRR 04 F5/F6). Whether the panel offers
  "Play video" and "Open viewer" is read from the index's file records --
  the two folder walks were most of the 16-22 share reads each open cost
  the UI thread. A tag added or removed in the panel shows at once and is
  written on the jobs queue, like the edit screen's Save (240 ms on a busy
  share, no longer spent on the window); edits to the same asset save in
  the order they were made, and a failed save puts the chip back and says
  why in the status line. Accepting suggested tags over a selection
  (Suggested tags > Add ticked tags) wrote every asset.json the same way
  and froze the window for seconds on a busy share; those writes are on
  the jobs queue too, an asset that could not be tagged is named in the
  status line, and the rest still land.
- **Model cards no longer show streaks of repeated edge pixels.** A
  Megascans 3D `_Preview.png` is a cut-out, and the colour hidden under
  its transparent background is the object's outline smeared out to the
  border. Thumbnails dropped the transparency and showed that instead --
  on the card, in the side panel, in quick look and on contact sheets.
  Transparent pictures are now laid on the card's own colour, and
  thumbnails made by earlier builds are made again.
  `docs/FIX-MODEL-PREVIEW-2026-10-17.md`.

### Data safety (production readiness review, 17 Oct 2026)

`docs/FIX-DATA-SAFETY-2026-10-17.md` has the detail and the proof.

- **A library can refuse old builds.** `_library.json` can name the oldest
  version allowed to make changes (`storehouse library set-min-version
  2.1.4 --yes`, or Settings > Library location). A build older than that
  opens the library read-only, with a banner saying which version is
  needed and where to get it. Nothing sets it on its own. Builds before
  2.1.4 cannot see it: upgrade every machine first.
- **Notes are never silently overwritten.** If somebody else saved the
  notes after you opened them, yours are saved and theirs are kept beside
  them as `notes.conflict-<who>-<when>.md`, and you are told. Clearing the
  notes asks first and moves `notes.md` to the trash (Ctrl+Z puts it back).
  Who last saved the notes is kept in `asset.json` (`notes_saved`).
- **Long names.** An asset with a very long folder name can be moved to
  the trash and back. A new name over 200 characters is refused in plain
  words before anything is copied.
- **Plain errors** for a read-only `asset.json` or a full disk.
- **A version an interrupted "add version" left behind** shows up as an
  issue, and `storehouse repair` or the next added version records it.
- **Previews and kit copies arrive whole**, through a temporary name, or
  not at all.

### Rollout (production readiness review, 17 Oct 2026)

- **"A newer version is available."** A built copy -- a Setup install
  included, which until now never heard of one -- asks GitHub's public
  releases API for the newest version at most once a day, in the
  background, and says so in a strip across the window with a Download
  button that opens the download page. Offline or any odd answer is
  silence. It downloads and installs nothing; the answer and when it was
  asked are kept in `release_check.json` beside the preferences.
- **Cinema 4D: registered plugin IDs.** The helper now runs, and keys
  each Substance material's data, under 1071572, and its three commands
  (BC Storehouse Material, Re-bake Substance materials, Clean old
  Substance bakes) are 1071573-1071575, all registered with Maxon. A scene
  saved before keeps its data under the old 1065041: it is read from there
  and moved to the new key the next time the material is written. The
  helper file changed: reinstall it (Settings).
- **Uninstalling removes the DCC helpers.** The Setup's uninstaller and
  `uninstall.ps1` now run `BC Storehouse.exe --remove-helpers` first, which
  takes the Cinema 4D plugin and the Houdini package out of every
  preferences folder they were installed into (c4dpy's and both Houdini
  locations included), so they stop loading. Only Storehouse's own files
  go; a folder is removed only once it is empty.
- **The release smoke test keeps its evidence and tests the app as
  shipped.** `tools/smoke_build.py` starts the built app from the caller's
  environment as it was before the test package was imported, so the
  suite's offscreen platform, general tagger bars and temporary folders no
  longer reach it (offscreen only with `--headless`). Its working files sit
  in one folder of its own, removed when every check passes and kept, as
  it says, when one fails -- before, the test package deleted them at exit.
- **The job-queue tests wait for the job, not the clock**: for its
  completion, the queue's own shutdown, or with a generous limit, so a busy
  machine cannot fail them (with every job start delayed 3 s, three used to).

## 2.1.3 — 16 October 2026

Substance materials, asked about on 16 October. The plan and what was
checked are in `docs/PLAN-SUBSTANCE-2026-10-16.md`.

### Added

- **Send a Substance material to Cinema 4D or Houdini.** An asset with an
  .sbsar goes to the application as the archive itself, opened there --
  Houdini's Substance archive COP, Cinema 4D's Substance import -- with
  the baked maps beside it, if it has any, as the fallback. Its real size
  goes with it. The status line says "sent a Substance material"; nothing
  goes on the clipboard. "Send material only" is offered for a model that
  ships an .sbsar. Sending from an offline copy that lacks the archive is
  refused by name.
- **In Houdini** the archive is opened with Houdini's own Substance node
  (no extra install), its maps are baked at 2048 px beside the scene in
  `tex/storehouse/<asset>/`, and the usual Redshift (or MaterialX)
  material is built from them. The material carries a "BC Storehouse
  Substance" section: the Substance network, **Live Substance** (Redshift
  reads the network directly, so a Substance parameter changes the
  material as you go -- bake again before a farm render) and **Re-bake
  maps**. Baked, the scene renders anywhere, even without the helper.
- **In Cinema 4D** the archive is opened with Cinema 4D's own Substance
  engine, baked at 2048 px beside the scene the same way, and built as the
  usual Redshift material; the Substance object is removed again, so the
  scene renders from files. No live option: Cinema 4D's Substance node
  gives Redshift the wrong values (a roughness of 0.10 rendered as 0.01).
- Both: a DirectX normal map is flipped to read right; an opacity that is
  white everywhere is left out; an unsaved scene bakes to a temporary
  folder and says so.
- **What an .sbsar holds, in the panel.** Details show the graph and how
  many maps and parameters it has ("Bacon · 10 maps · 13 parameters"); the
  tooltip lists them. Read from the archive's own description, without
  Substance.

### Fixed

- **.sbs source graphs are recognised.** They are filed as material
  definitions, like .sbsar and .mtlx, and a folder of them imports under
  Materials instead of being refused as "cannot tell which category".
- **Copying or dragging an .sbs takes what it needs to open.** A source
  graph goes with every other file of the asset -- the graphs and bitmaps it
  links to -- where a copy used to keep one graph of several. Copy path
  gives the top-level graph. Assets indexed before this release, which hold
  .sbs as unrecognised, behave the same.
- **An .sbsar is handed over on its own even without baked maps beside
  it.** It already was when maps sat next to it; with only a bitmap or two,
  a copy handed over everything.
- **Sending an .sbs source graph says why it can't go.** Neither
  application opens one: publish it to .sbsar in Substance Designer and
  send that. It used to land on the clipboard as a "single texture". With
  baked maps beside it the material is still built from them, as before.
- **A material's unrecognised files are no longer cut to one.** A drag or
  copy of a set of maps used to add whichever unrecognised file sorted
  first; it now hands over the maps. A material of only unrecognised files
  hands over all of them.

**Update both helpers** (Settings, or "Update helper" when a send asks):
they open Substance archives now, and this Storehouse sends nothing to a
helper older than that (HELPER 6).

## 2.1.2 — 16 October 2026

A small follow-up to 2.1.1, from the same tester's log.

### Fixed

- **"Checking for duplicates" no longer reads every same-size map whole.**
  Same-resolution TGA, TIFF and EXR maps all have the same byte count, and
  each was read in full over the share just to learn it was not a copy --
  ten minutes for one 443-asset pack. Large same-size pairs are now compared
  by three small samples first; nothing is called a duplicate without a
  whole-file match. About fifteen times less read on a cold check.
- **A map edited in place is no longer matched by its old fingerprint.** The
  duplicate check remembered a library file's hash by its path alone, so an
  incoming copy of the old bytes could be called a duplicate of a file that
  had since changed. The remembered hash now holds only while the file keeps
  its size and modification time.

## 2.1.1 — 15 October 2026

The morning after 2.1.0, five auditors went through the new code and the
corners no audit had reached; everything they confirmed is fixed here, each
fix pinned by a test that failed on 2.1.0 first. The full findings are in
`docs/AUDIT-2026-10-15.md`.

### Before you install

- **Update the helpers again, then restart.** Settings › Cinema 4D & Houdini
  › Reinstall helper for each; the helpers moved their queue and an older
  one is blocked from taking sends.

### New

- **Buy me a coffee.** A row at the foot of the sidebar, under Settings,
  and a card in Settings > About open the maintainer's page in your
  browser. Entirely optional; it does nothing else.

### Fixed — imports and saves

- **Two machines importing the same new product at once** now end with one
  asset and one identity; the second import adds to the first instead of
  overwriting it.
- **Files arrive whole or not at all:** a crash mid-copy can no longer leave
  a truncated file that later imports keep, and an import that crashed at
  the very start no longer blocks that name for ever.
- A library whose lock folder cannot be created no longer fails every save;
  the edit screen's Save no longer freezes the window while the share is
  busy; a wrong clock on one machine can no longer delete another's
  version-copy in progress; Replace-on-import waits its turn behind a save.
- "Import all separately" waits for planning to finish, so a rename can no
  longer be undone by the planning job and lose that row's tag edits.
- **Pressing Import twice no longer runs the same batch twice.** While an
  import is running, another is refused with a message, instead of two
  copies racing over the same files (a tester's log showed one 443-asset
  import pressed twice, both copies writing at once for fifteen minutes).
- **"The library changed" no longer nags during your own import.** The
  watcher holds its comparison while this window's own jobs write the
  library; a colleague's real change is still said at the first check
  after the job ends (the tester's log had 54 false alarms in one import).

### Fixed — what leaves the library

- **UDIM materials arrive whole.** A drag or copy hands over every tile;
  sends and USD exports use the `<UDIM>` token (set and read back in real
  Houdini and Cinema 4D Redshift) instead of stretching tile 1001 over everything.
- Dragging an HDRI gives the HDRI, not its biggest backplate; drag and send
  agree on the resolution; sending a single texture no longer puts notes
  and previews on the clipboard; select-all + copy of thousands of assets
  no longer freezes the window.
- A corrupt queue file, a second window, or a partial offline kit copy can
  no longer block, garble or silently cripple a send.

### Fixed — vendor catalog

- A vendor record's link only opens when it is a real web address; anything
  else shows as plain text.
- Two machines updating the team catalog at once no longer lose each
  other's records; a crashed collection frees its lock in half an hour; a
  damaged catalog is set aside and rebuilt instead of crashing; "Not this
  product" survives re-importing the asset; registering the library itself
  as a vendor folder is refused.

### Fixed — the rest

- Contact sheets fail loudly when a page cannot be written instead of
  reporting "saved", and "Save as PNG pages" to a bare name no longer
  writes a PDF.
- Package for travel: validation catches files the packaging itself lost;
  renamed copies can no longer overwrite each other; $F and UDIM sequences
  stay resolvable.
- A share hiccup reading a kit stops a Keep left/right instead of leaving
  the kit pointing at the trashed copy.
- "Check health" now runs the full deep check with progress; the Jobs page
  refresh no longer freezes the window when the share is away; a broken
  index is announced on the asset panel instead of showing a falsely clean
  asset.
- The studio worker reports a refused token as exactly that, drops idle
  connections, and serves only this machine unless started with
  `--host 0.0.0.0`; a render asked for a trashed asset fails instantly
  instead of burning three Houdini sessions.
- **The picture tagger looks at the right picture and says when it can't.**
  It prefers a plain colour map over the same map as EXR, no longer picks a
  displacement map out of a model pack, and says "the picture could not be
  read" instead of quietly finding nothing.
- **Sends leave a trace.** Every queued, built, stale or blocked send and
  every preview render request is now one line in the log, so a send that
  never arrives can be followed.
- **Closing only asks about work you started.** The background housekeeping
  -- reading picture sizes, fetching the model, the daily index backup -- is
  stopped quietly instead of propping open a "still running" question.

## 2.1.0 — 14 October 2026

Imports that account for every product, copies and drags that hand over
the real maps, colour spaces Houdini and Cinema 4D actually accept -- and
vendor products recognised by their IDs and given their real names.

### Before you install

- **Everyone at once, on the same day.** This build locks each save; older
  builds don't take those locks, so one still running beside it can lose a
  colleague's change, and a copy older than 1.52 erases fields newer ones
  keep. Close Storehouse everywhere, then run the new Setup on every machine.
- **Update the helpers, then restart.** Settings › Cinema 4D & Houdini ›
  Reinstall helper for each, then restart Cinema 4D and Houdini. An
  out-of-date helper now blocks the send and offers **Update helper**.

### New

- **Vendor products know their names.** A Megascans download called
  `vixsbizdw` comes in as "Tundra Riverbed", with the vendor's own tags.
  Megascans and GSG are read from the studio's own files only (Bridge
  exports, .gsgm/.gsga -- no online lookups); Poly Haven and ambientCG from
  their official catalogues. Recognition happens on import, before any
  picture tagging; the vendor ID stays in the folder name and is
  searchable. Nothing is renamed or moved.
- **Only a sure match is applied.** An uncertain one is flagged for review
  and changes nothing. A name you typed and a tag you rejected always win.
- **The asset panel says where it came from:** vendor, product, link and
  why it matched, with **Not this product** (takes the name and tags back
  off) and **Choose product…**.
- **Settings › Import & tagging › Vendor catalog:** what each vendor has,
  when it was last updated, **Update now**, and the Bridge and GSG folders
  this machine reads. The catalog updates itself in the background.
- **Every import ends with a summary** -- imported, updated, unchanged,
  skipped, failed, not attempted -- also after a failure or Cancel, with a
  CSV beside the log. **Retry** runs only what failed or was not attempted.
- **Copy everything** and **Copy folder path** in the right-click menu, for
  the old whole-folder behaviour. Single files drag from the Files list in
  the asset's details.
- **Send material only** for a model; a model with no geometry says so.
- **Each send says what every texture was set to** in Houdini or Cinema 4D.
- For whoever looks after the catalog: `storehouse catalog` (collect,
  status, match, reject, set) and `catalog enrich --dry-run` to see what it
  would do for assets already in the library, before applying with undo.

### Fixed

- **Import could overwrite one product with another of the same name.** It
  never merges or overwrites now: a clash blocks Import until each one gets
  its own name or is skipped. An existing asset keeps its differing files
  unless you choose Replace, and then the old files go to the trash.
- **Dropped folders** keep their 2K/4K subfolders and their own name, and a
  folder holding several material sets is split into separate assets.
- **Previews picked an AO or normal map.** Import picks the base colour.
- **Overview thumbnails went blank** after roughly row 64. They load
  wherever you scroll, come back after a network hiccup and use far less
  memory. Materials whose preview was a data map show their colour map.
- **Copy and drag gave asset.json, notes and previews.** Copy files, drag
  and Copy path now hand over the real current maps -- one per type, at one
  resolution, OpenGL normal -- or the mesh or HDRI. Never old versions.
- **Houdini colour spaces.** Colour maps were set to "sRGB", which
  Houdini's Redshift does not have. They are now "sRGB - Texture", EXR/HDR
  colour is "Linear Rec.709 (sRGB)" and data maps are "Raw" -- the names in
  Houdini's own menu, checked with real Redshift renders. Cinema 4D the same.
- **Models sent as a material.** A model sends only as real geometry
  (FBX, OBJ or Alembic, LOD0 first). Mirrored FBX parts are no longer inside
  out in the 3D view.
- **Two people saving the same asset, tag list or kit** could lose a change
  or delete asset.json. Saves now take turns; one kept waiting over 15 s
  says who holds it.
- **Damaged files no longer cause damage.** An unreadable asset.json is never
  given a new identity, a bad schema_version no longer stops a rescan,
  notes saved in Windows-1252 no longer break the asset page, a damaged
  settings file no longer stops the app opening, and an unreadable
  `_filters.json` is reported instead of wiped.
- **Zips** from Western Windows keep accented names; one bad entry no longer
  sinks the archive; files differing only by case are both kept.
- **Typed tags** keep accents and other scripts (no more `asset_<hash>`).
- **Trash and jobs.** Trashing with a file open moves nothing and names the
  file; emptying the trash says what it couldn't delete; cancelled jobs no
  longer run anyway; a failed Add version leaves no stray folder.
- **Offline.** The app really browses from the last index when the share is
  down, and kit refresh brings in colleagues' new versions.
- Archive assets imported as duplicates get their own identity; archive
  Replace goes through the trash; plug-in and HDA installs can't write
  outside their folders. Onboarding finds a OneDrive Documents folder.

### Changed

- **Faster.** The window opens in about half the time (~1.2 s to ~0.46 s);
  refresh and search are quicker (all assets 452 to 315 ms, a search 332 to
  151 ms). Big drops are planned in the background, so the window keeps
  answering. A busy index says so instead of freezing.
- **Closing during an import asks first.**
- **Model sends and the 3D view are labelled Experimental.**
- **The app no longer updates itself.** A newer build says to run the new
  Setup. For whoever installs it: builds and installers refuse an exe whose
  version doesn't match.

### Known limits

- Cinema 4D renders of float (EXR/HDR) colour maps are not yet checked.
- Models are experimental: send and 3D view both.
- Vendor keywords become searchable once an asset is reindexed.
- The first offline start on a fresh machine has no remembered index to
  browse; open it once on the share first.

## 2.0.0 — 13 October 2026

The v2 line: the Qt window, the whole implementation roadmap (sends,
versions, kits, tagging, previews, tools and courses), archives that
unpack on import, and a copy that works when sent outside the studio.

- **Render starts rendering.** Pressing Render on an asset (or Render
  previews in the menu or palette) now renders it straight away on a
  machine that has Houdini with Redshift -- and HDRIs anywhere. Before,
  it only joined a waiting list and you had to find Render waiting
  previews here as well. Elsewhere it still waits, and says it is waiting
  for a machine with Houdini and Redshift.
- **Sending the app to someone now gives them picture tagging.** The tag
  probes and the measured thresholds are built into the app -- a copy
  unzipped elsewhere used to have neither, so picture tagging was off and
  the safety rule that pictures never set surface tags was missing.
- **The picture model downloads when there is no studio share.** On first
  start the app asks once ("Download the picture model? 373 MB"); the
  download is checked against the model's exact size and SHA-256 before it
  is used. On a studio machine it is still copied from the share, silently.
  Settings › Import & tagging › Get the model now works anywhere too.
- HDRI viewer: Dome → Cinema 4D / Houdini sit beside Add to kit, so the
  controls fit a 1440-wide window again.
- Model details: UVs that run a hair past 0–1 say so, instead of "not a
  UDIM layout".

## 1.59.1 — 13 October 2026

- **The BC mark** is the app's icon -- the .exe, the window, the taskbar --
  and sits beside "Storehouse" at the top of the sidebar.
- The installed window's title bar showed a generic box instead of the
  mark: the icon file was never inside the build. It is drawn now.
- Full details: the Versions table is as tall as its rows, and a setup no
  longer shows an empty Reference card.

## 1.59.0 — 13 October 2026

Tools, installs, and archives.

### New

- **Drop a zip on Import.** Storehouse opens it -- and the zips inside it --
  works out what each thing is (material, model, HDRI...) and shows the
  usual review before anything comes in. .tar.gz too; a .7z or .rar is
  named and left out.
- **Install HDAs and plug-ins** from Full details: into the Houdini or
  Cinema 4D you choose, with Update, Roll back, Uninstall and Undo.
  Installers (.exe) run from there and are recorded.
- **Works with:** what a setup or HDA needs -- the Houdini that saved it,
  its renderer, the custom HDAs it uses -- against what is on this machine;
  and what a person knows, kept with the asset.
- **HDA reference:** every parameter, its help, documents and pictures it
  came with, and Open demo scene.
- **Onboarding** lists the applications on this machine and finds vendor
  libraries (Megascans, Poliigon, Downloads...) to review for import. Also
  in the palette: Applications on this machine, Find vendor libraries.

## 1.58.0 — 13 October 2026

Looking closer, rendering previews, and courses.

### New

- **HDRI viewer:** a histogram in stops, the value under the cursor, what
  the file says about itself, a chrome ball view, and the real sRGB curve.
  Files in ACEScg or other primaries are shown correctly.
- **Dome lights:** Dome → Cinema 4D / Houdini in the HDRI viewer lights the
  scene with the HDRI at the exposure you are looking at.
- **Models:** size and units, UVs (one tile, UDIM, or repeating), materials
  and the textures they link (missing ones named), LOD files and parts, and
  material colour / wireframe / normals views. Sending a model brings it in
  at the scene's scale; Size & units… records what one unit in an OBJ is.
- **Preview renders:** materials on a sphere, cloth and cube and models as a
  clay turntable, rendered in Houdini with Redshift on the machine that has
  it (Jobs & health › Render here); HDRI contact sheets anywhere. Use as
  preview makes one the asset's preview.
- **Export as USD material…** for any material, readable by any USD
  application. COMPATIBILITY.md lists what works where.
- **Learning:** every course with Continue watching and progress; lessons by
  section with their lengths; captions, chapters and transcript search where
  a course has them; the course's project files on their own tab. A lesson
  watched to the end is marked complete.

### Fixed

- The asset panel could be pushed wider than its column by the Send card's
  status lines or a long name with no spaces, hiding Add, Houdini and the
  close button.
- Importing into a kit could report "no kit" for a moment while the kit was
  being saved.

Install the Cinema 4D and Houdini helpers again (Settings) for dome lights
and models.

## 1.57.0 — 12 October 2026

Finding things by how they look, and the work around a project.

### New

- **Find similar.** Right-click, or Find similar in the panel: what looks
  like this asset, most alike first -- even on a machine without the picture
  model. Drop a picture on the search field to find what looks like it.
  Settings › Import & tagging builds the look-alike index (once; then only
  what changes).
- **Searching for a look** ("worn wood") also lists pictures that look like
  it, after the name matches and marked as picture matches.
- **Look-alikes** (Jobs & health): pairs that may be one thing twice --
  same files, or all but the same picture -- to compare, and Keep left /
  Keep right, which says what it will do first (the other to the trash,
  kits pointed at the one kept, its tags added).
- **Sends are answered.** After Send to Cinema 4D or Houdini, Storehouse
  says what was built -- or what was not and why -- and the Send card says
  which application is open. Install the helpers again (Settings) to get
  this.
- **Projects** (Jobs & health, palette): choose a Cinema 4D or Houdini
  scene and see every file it uses -- which library asset and version, from
  outside the library, or missing. Package for travel copies the scene and
  everything it uses into a new folder, relinked, with a manifest, and
  checks the copy.
- **Take a kit offline** to work away from the share; sends use the copies.
  Back on the share, Offline copies compares them: refresh, add your changes
  as a new version, or decide where both sides changed.
- **Source & licence** in Full details: vendor, source, when and how it was
  acquired, licence, what it may not be used for, and licence documents
  kept with the asset. Sends and kit copies mention restrictions.
- **Proposals.** Propose tags or report a problem without changing what
  everyone sees; reviewers accept or decline from the panel, and everything
  stays on record. Settings can make your tag changes proposals.
- **Most used** sort, from what is opened, sent and confirmed built on this
  machine (kept 180 days; Settings can clear it).

## 1.56.0 — 12 October 2026

Finding, comparing and checking things -- the everyday part.

### New

- **Quick look.** Space shows a big picture of the asset; the arrow keys
  go through the grid, Enter opens it, C or H sends it, Space or Esc
  closes it.
- **Resolution and "made for".** Two new menus: Size (1K, 2K, 4K, 8K and
  up) and Made for (Cinema 4D, Houdini, 3ds Max, any 3D application...).
  Picture sizes are read in the background the first time the app opens
  -- about half a minute for the whole library -- and then only for what
  is new.
- **Smart collections.** Untagged, and Needs review (picture tags nobody
  has checked), in the sidebar. They combine with a category, and the
  panel says why an asset is in the list.
- **When nothing matches**, it offers a spelling that would ("Search
  “concrete”"), the same words across the whole library, or clearing the
  filters.
- **Compare.** Select two to four assets and press Compare, or open an
  asset's versions and press Compare versions: pictures and details side by
  side, with what differs marked.
- **It opens where you left off** -- same view, filters and selection.
  Settings › General can turn that off.
- **Tiles.** A compact setting; Pin, C4D and Houdini buttons on the tile
  under the cursor; NEW, NEEDS REVIEW and MISSING FILE badges. The list
  view shows resolution, version, size and file types.
- **The import review says more:** names already taken or clashing within
  the drop, files already in the library under another name (with Skip
  duplicates), materials missing a map, another renderer's copy folded in,
  read-only files.
- **The Inbox.** An import can wait in the Inbox for someone to look before
  it shows up in All assets and search; Publish moves it to its category.
  Or import straight into a kit.
- **Contact sheets** of a selection, a kit or the whole view, as a PDF or
  PNG pages, ready to print or send.
- **Kits** say who made them and who changed them last, sit in the
  sidebar, and Pull a copy shows what it will create -- latest versions or
  all of them -- before it copies anything.
- **Jobs & health** shows the free space on the share and here and the
  index backups; the trash shows where each item was and how big it is.
- **Settings › Backups:** how many backups to keep and how often, and
  whether a change made by somebody else is re-read on its own.

### Fixed

- **Saved filters forgot their search words** (and their sort order); a
  search could not be saved on its own words at all.
- **"Somebody else changed the library" stopped working** after a rebuild
  from the new window -- the rebuild left out what that check compares.
  Both rebuild buttons now do the same full, backed-up re-read, alone.
- The selection bar stayed up over an empty grid; the counts on the
  category pills ignored the other filters.

## 1.55.0 — 12 October 2026

The picture tagger, measured honestly -- and trusted accordingly.

### Changed

- **The picture no longer puts a surface tag on by itself.** Tested on
  vendor packs it had never seen, it named the surface right 65% of the
  time, not the 92% quoted before (that test had seen near-copies of the
  same packs). So a surface from the picture is now a suggestion, offered
  only when it is fairly sure; you pick it or not. Surfaces read from the
  name, the files or vendor metadata are applied as before, and look,
  environment and form tags from the picture are unchanged.

### Fixed

- **"indoor" on assets that were not.** The words of the library's own
  folders (BC Storehouse, Resources) were being read as evidence about an
  asset; only folders inside the asset count now.

### For whoever maintains the tagger

- `tools/tagger/DECISION.md` -- which model, how far it is trusted, where
  it should run, and the measurements behind each.
- A benchmark of 300 assets for a person to check (`benchmark_set.py
  review`), the whole tagging pipeline scored against it (`evaluate.py`),
  and processor / graphics card / studio worker compared
  (`compare_devices.py`).
- `retrain.py` retrains the probes, promotes one only if it is no worse,
  and can roll back.

## 1.54.1 — 12 October 2026

- For whoever installs it: `BC Storehouse.exe --check-tagger <picture> <report.txt>`
  proves the picture tagger runs from the installed build -- on the graphics
  card and on the processor -- and `tools/smoke_installed.py` now runs it.

## 1.54.0 — 12 October 2026

Faster tagging that never holds anything up, undo, and nothing lost while
the library or the tagger is away.

### New

- **Undo.** Ctrl+Z, or the Undo button that appears in the status bar
  after a change, takes back the last change to tags, a name or a
  description -- from the panel, the edit screen, a review or Tag
  untagged assets. If somebody else has changed the same thing since, it
  says who and leaves it. Each asset keeps its last 20 changes in its own
  asset.json.
- **The picture tagger uses the graphics card** -- about four times
  faster on an RTX card, the same answers -- except while Cinema 4D or
  Houdini is open, when it leaves the card to them. It lets go of its
  memory after five minutes unused.
- **A studio tagger.** `storehouse worker` on one machine serves the
  picture model to everyone else; set them to "The studio worker" in
  Settings › Import & tagging. A machine that cannot reach it uses its own.
- **Nothing is looked at twice.** What the model worked out for a picture
  is kept, on each machine and in the library's `_cache` folder.
- **Work that waits.** Files dropped while the library is offline open in
  Import when it is back. An import while the tagger cannot run goes ahead
  with the words, and the pictures are looked at later.

### Fixed

- **Retry ran without anyone listening.** A retried job now reports back
  to the screen that started it. The Jobs button says how many are
  running and failed.
- **Sending twice before Cinema 4D picked up the first** queued the same
  material twice. It is sent once.
- **"form: 0% correct"** in Settings: that facet was never measured, not
  wrong. Each facet now says how many held-out assets its figure rests on.

## 1.53.0 — 12 October 2026

One tagging answer, wherever you ask, and a record of where every tag came
from. **Everyone should be on 1.52.0 or later before this goes out:** an
older copy saving an asset would drop the record this version keeps.

### Changed

- **Every screen gets the same tags for the same asset.** Import, the asset
  panel, the edit screen, "Suggest tags…" for a selection and Tag untagged
  assets all ask one tagger now; they used to have three sets of rules.
- **No more weak guesses.** Nothing the picture model is less than 60%
  sure of is shown anywhere -- the "maybe fabric 55%" and "best guess 18%"
  chips are gone. When it looked and was not sure, it says so in words.
- **The name wins arguments.** When the picture disagrees with the name
  (a ceramic glaze the model calls metal), the disagreement is said in a
  note under the suggestions, not offered as a tag you could add by
  accident.

### New

- **Where each tag came from.** Hover a tag on an asset to see whether it
  came from the name, the vendor's metadata, the picture (with its score),
  or a person -- and who, and when. Kept in the asset's own `asset.json`.
- **"Not this".** Right-click a suggestion and it is not offered again for
  that asset -- unless the asset is renamed, its picture changes, or a new
  picture model is sure of it. Taking a tag off an asset counts the same.
- **Review a selection with a preview.** "Suggest tags…" says how many tags
  it will add to how many assets before it does, and can stop suggesting
  what you untick.
- **Known from the files**: formats, maps and resolutions, shown apart
  from the guessed tags.
- **Not duplicates.** In Organise › Near-duplicates, two tags that look
  alike but mean different things can be marked so, for everyone.

### For whoever looks after the tagger

- `tools/tagger/feedback.py` adds up what people kept and took off, per
  source and per picture tag -- the tagger's real agreement with the
  studio. Training (`tools/tagger/benchmark.py`) no longer learns from
  picture tags nobody reviewed.

## 1.52.0 — 12 October 2026

Groundwork: the roadmap's Phase 0, and one fix that has to reach everyone
before anything new is recorded per asset.

### Fixed

- **An older copy of Storehouse erased what a newer one recorded.** Saving
  an asset kept only the fields that build knew about, so anyone who had not
  updated would wipe whatever a newer version had added to that asset's
  `asset.json`. Unknown fields are now kept and written back unchanged.
  Please update: the next version records where each tag came from.
- **Nothing could restore the index from a backup**, though one is taken
  every day and before every rebuild. `storehouse backup --restore latest`
  puts the newest back (close the app first); the index it replaces is set
  aside, not overwritten, and a damaged backup is refused.

### New

- `storehouse backup` (take one now) and `storehouse backup --list`.
- `STOREHOUSE_TAGGER_THREADS` caps the picture tagger's CPU threads, for a
  machine that needs its cores for Cinema 4D or Houdini at the same time.
- A test or measurement with its own app-data folder runs as its own
  instance instead of waking the window you are working in.

### For whoever looks after the app

- `tools/measure_baseline.py` measures a machine: start-up, browsing,
  import, the tagger (also as a 2-core PC), install size. Results for the
  RTX 5090 workstation are in `docs/baseline/`; the same command on the
  RTX 3060 machine gives comparable numbers.
- `tools/smoke_installed.py` checks the installed build starts, draws the
  library, logs no errors and closes cleanly.
- `DESIGN-COVERAGE.md` re-checked against all 19 design references.

## 1.51.0 — 12 October 2026

The reliability release: the nine problems found in the 11 October audit,
and what testing them turned up. Each one has a test that fails on 1.50.1
and passes here.

### Fixed — nothing lost

- **A failed export destroyed the archive it was replacing.** Export now
  writes to a temporary file and swaps it in only once it is complete; a
  failure leaves the previous archive exactly as it was.
- **Sending twice before Cinema 4D or Houdini picked up the first lost the
  first.** Every send is now its own request in a queue, and each is built
  once, oldest first. Helpers installed before this version still work --
  they now get every waiting send, not only the last -- but install the
  helper again in Settings so each send is taken in turn; Storehouse says
  so when you send. A send nobody built within three days is set aside,
  not built, the next time the application opens.
- **Sending a material that has versions sent the original maps too**, so
  it could be built with the old base colour. Only the latest version's
  maps go now, plus original files that version does not replace.
- **The viewer opened the wrong file** -- a larger old version of a model,
  or an EXR lying beside it. It opens the latest version, of the asset's
  own kind.
- **A record deleted while you were editing it was quietly recreated from
  your copy.** Saving now refuses, as it already did when somebody else had
  changed it.
- **Importing into a library that had gone away** -- the share dropped, or
  the folder moved -- built a new, empty library folder where it had been.
  It now stops before anything is copied.

### Fixed — saying what actually happened

- **Tagging that failed said it had worked.** The panel, Suggested tags and
  Tag from previews now report every asset that could not be written -- a
  read-only `asset.json`, say -- and the panel puts the tag box back to
  what the asset really has. One locked record no longer stops a batch of
  hundreds.
- **An empty suggestion list now says why**: the picture tagger is not
  installed, its model is not on this machine yet, pictures are not checked
  for this category, there is only a data map (normal, height) to look at,
  the tagger failed (with the error), or it looked and was not sure.
- **A tag typed on the import screen but not yet added was dropped** when
  you pressed Import or clicked another row. It now goes on the row it was
  typed for. Adding more files to the list also left the tag editor showing
  a row nobody was editing, so anything typed there went nowhere.
- **A crash dialog when suggestions arrived after their screen had closed**
  -- opening another asset while one was still being looked at. The late
  answer is dropped.
- **An interrupted record write left `asset.json.tmp`** in the asset
  folder. It is removed now; the record itself was never at risk.
- **546 "missing file" errors were never missing files** (fixed in source
  as 1.50.2, never released on its own). All were the
  Synology NAS's own thumbnail files (`@eaDir/…@SynoEAStream`): older
  imports copied them in and listed them in 103 assets' records. The files
  are still on disk -- the health check skips `@eaDir` when it lists an
  asset's files, then compared the record against that list, so every one
  looked missing. They are no longer reported, and imports no longer
  bring NAS files (`@eaDir`, `Thumbs.db`) in. A read-only rescan of the
  library: 0 missing-file errors, down from 546.

## 1.50.1 — 11 October 2026

### Fixed

- **A custom tag had no group to go in**, so a new word had to be filed
  under one that did not fit -- "gobo" became an artist, "software" a
  piece of software. A new word is now offered first as a **plain tag**
  (shown as just "gobo"); a material word like "terracotta" still offers
  the asset's own group first.

### New

- **Organise › New tag…** adds a tag to the list without tagging anything,
  asking the same questions as typing one in a tag box.

## 1.50.0 — 11 October 2026

### The tagging, redone: one tag box, the same everywhere

The asset panel, the Edit screen, the import screen and Tag selection each
had their own way of entering tags — and **none let you type a tag that was
not already in the list**; the Edit screen had no box to type in at all.
Now all four use the same tag box:

- **Your tags as chips** — click × to take one off.
- **Type a tag and press Enter.** It completes from the tag list as you
  type. Plain words work: *wood* finds surface:wood, *timber* finds it too,
  several at once with commas. A word in two groups (e.g. *houdini*) asks
  which.
- **A new word asks once which group it belongs to**, with the asset's own
  group first, and joins the tag list for everyone. A near-miss
  (*rubberr*) asks whether you meant the existing *rubber* first.
- **Suggestions underneath**, one click each: from the name, the file
  names, the vendor's tag file, and — with *Look at the picture too* — the
  picture, with its percentage. A tag you take off is not offered straight
  back.
- **In the asset panel it saves as you go.** On the Edit screen it saves on
  Save (anything still typed included); on the import screen, on Import.
- **Tag selection** uses the same box: it shows the tags the selected
  assets share; what you add goes on all of them, what you take off comes
  off all of them.

### Fixed

- **The asset panel was wider than its column for assets with long file
  names** (since 1.48), cutting off the preview, the Add and Houdini
  buttons and the Details values. Long names are shortened in the middle,
  in full on hover.

## 1.49.2 — 11 October 2026

### Fixed

- **A sure picture guess was added on top of the name's material.**
  Greyscalegorilla's *CeramicBlueSpeckled* is tile by its name; the
  picture, which has never seen a ceramic glaze, said metal 94%, and
  import tagged it both. The name's surface now wins at import, as it
  already did in the asset panel and in bulk tagging; the picture's guess
  is still shown as a suggestion. (Models can still be several.)

### New

- **Vendor tag files are read.** A Megascans download describes itself in
  a .json -- tags, categories, semantic tags -- and those now count as
  much as the asset's name, at import and in the asset panel.

## 1.49.1 — 11 October 2026

### Fixed

- **The import screen offered weak "look" guesses, and Add all applied
  them** -- a terracotta pot was tagged *rusted* from a 53% guess. Look
  guesses from a picture are only offered at 60% or more, as the asset
  panel already did.

## 1.49.0 — 11 October 2026

### Fixed

- **New imports did not show up at the top — because "Recently added"
  never sorted by date added.** The index only accepts sort orders from a
  fixed list, and the new one was never added to it, so every request for
  it silently fell back to the old file-date order: a texture made in
  2019 and imported tonight sorted as 2019. It sorts by date added now,
  and an unaccepted sort is logged instead of silently replaced.
- **Dropping a second thing onto the import list threw the first away.**
  Drop a texture, drop a setup, press Import — only the setup came in.
  Drops, Choose files… and Choose a folder… now *add* to the list (the
  same file twice is skipped); **Clear list** starts again.
- Moving to the trash is written to the log.

### New

- **A missing tag is named instead of nothing.** When a name or file name
  says a material the tag list does not have — terracotta, clay, cork,
  wicker, rattan, resin… — it is offered as a **new tag**. Click it, say
  yes, and it is added to the tag list and the asset. Never applied
  without asking; never part of "Add all".
- More words for existing tags: *weathered* → aged, *scratched* → worn,
  *cracks*, *dents* → damaged.

### Measured, not changed

- Asked for a better model: "find similar assets and copy their tags"
  scored 89.6–90.4% on surface against the current tagger's 92.9%, on the
  same held-out assets; earlier tonight two larger Google models scored
  91.6% and 82.3%. The current one stays. Its misses are mostly tags the
  list does not have, which the new-tag suggestions now name.

## 1.48.0 — 10 October 2026

### Fixed

- **It took 11 seconds to open, and a second click opened a second
  window.** The app was one 107 MB file that unpacked itself on every
  launch while Windows Defender scanned all of it. It is now installed as
  a folder and opens in about a second (measured: 11.3 s → 1.0–1.4 s). A
  second launch brings the open window forward instead of opening
  another — two windows on one library is how an import could "not show
  up": it was in the other window.
- **Auto-tagging only knew materials.** HDAs, setups, courses, tools and
  plugins got nothing, even "avrop playblast" with *playblast* in your tag
  list. Every tag in the library's list now counts as a word for itself,
  software and technique words are known (c4d, sidefx, fracture, vellum,
  flipbook…), and a file's type says its software (.hda / .hip →
  Houdini, .c4d → Cinema 4D, .blend → Blender). Checked against the
  library's own tags: surface still 100% right on 4,242.
- **The asset panel showed the previous asset through the new one**, and
  category chips stacked on each other, when switching quickly.
- **The asset panel was blank for the largest scans** (it read the full
  image; Qt refuses images over 256 MB). It uses the grid's thumbnail.

### Back from the old window — the asset panel

- **Suggested tags** right in the panel: from the name and file names at
  once, from the picture with *Look at the picture too*. One click adds a
  tag; *+ all* adds them all.
- Tags show as chips (click one to see everything with that tag) with
  **+ tag** to edit.
- **Roles**, **Notes**, **Play video**, **Open viewer**, **Full details**,
  **Pin/Unpin**, this asset's **issues**, and its **files** with *Open*.
- **Suggest tags…** for a selection (Organise menu and right-click):
  suggestions for every selected asset, ticked where the tagger is sure,
  added together.

### For whoever releases

- Builds are folders (`dist\BC Storehouse\`), archived as `.zip`;
  `rollback.ps1` takes either a new `.zip` or an older single `.exe`.
  `install.ps1` mirrors `_internal` and leaves `models\` alone. The in-app
  update copies the program files before the exe.

## 1.47.0 — 10 October 2026

### Fixed

- **Imports looked like they never arrived.** They had: the grid sorted by
  the newest *file date*, which an import keeps, so a texture made in 2019
  and imported tonight sorted among 2019's. The default sort is now
  **Recently added**, and an import ends by showing and selecting what came
  in.
- **Right-click did nothing.** The menu failed before it appeared. It works,
  and has the old window's items again: Open, Pin/Unpin, Copy preview image,
  Copy path (file, file for Houdini, folder).
- **Neither "Rebuild index" button could ever run**, and "Back up the index
  now" failed: all three called the backup the wrong way. Fixed; the daily
  backup the Backups page promises now actually happens at start-up.
- **Search ignored synonyms** set up under Tags.

### New — the import screen works like the old window

- **Choose files…** as well as folders, and **Import an archive…**. Loose
  files are grouped automatically (a texture set is one asset, a folder of
  HDRIs is many), or kept together, or one asset per file.
- **Select a row to edit it:** name, description, preview, and the tags —
  click a tag to take it off, click a suggestion (with its %) to add it, or
  type any tag from the list.
- **Auto-tag** switch: on, confident tags are applied for you; off,
  everything is only a suggestion. Remembered on each machine.

### New — also back from the old window

- **Added:** filter (7 / 30 / 90 days, this year) and **Clear filters**.
- **A newer build on the share** shows a banner with *Update and restart*.
- **Issues:** a button per kind shows those assets in the grid.
- **Settings › Import & tagging › Tag untagged assets…** — tags the
  untagged from their names and, where the model is sure, their previews.
  It shows how many and which tags before it writes anything.
- **Settings › Library location:** move this machine's index, or put it back.

## 1.46.0 — 10 October 2026

### Fixed

- **The "Auto tags" column could be blank, with no way to tell why.**
  Every row now shows its tags or says why it has none: "maybe fabric 55%",
  "unsure — best guess fabric 18%", "only data maps (normal, height…) — no
  colour picture to look at", "pictures are not checked for luts", or that
  the picture tagger is not on this machine.

### New

- **Models are tagged from their picture.** Furniture, prop, electronics,
  vehicle, foliage, character, architecture, terrain, weapon. Only 4 of the
  library's 640 models had tags, so nothing could be learned from them;
  this is written from words instead, needs no training and nothing extra
  installed. On 60 random models, every answer it was 60% or more sure of
  was right. Models whose preview is a texture swatch rather than a render
  get no model tag rather than a wrong one.

- **Suggest tags never comes back empty.** If nothing is a fair guess, it
  shows the single best one with its percentage.

- **Every auto-tagging decision is written to the log** — what the words
  said, which picture was looked at, what came back and what was kept — so
  "it didn't tag" can be answered from the log.

## 1.45.0 — 10 October 2026

### Fixed

- **Auto-tagging missed most vendor names.** Names written together —
  `WoodFine24`, `RustPlain004`, `woodflooring059` — were read as one word
  and tagged nothing. They are split now. Material words in Italian,
  German, French and Spanish are recognised too, so `ANODIZZATO` is metal.
  On the assets people have already tagged by hand, the name tagger now
  gets the surface right 100% of the time (4,240 checked) and tags 28% of
  the untagged assets instead of 15%.

- **When a name mentions two materials, it tagged both.** "Leaves maple
  leaf" was tagged leaves and wood. The first material named wins now,
  which is what people chose in the library's own tags. Models can still
  be several (a chair is wood and leather).

- **The first import of a session froze the window for about 20 seconds**
  while the picture model loaded. The picture now runs in the background
  and its tags appear in the list when it is done; pressing Import before
  then waits for them rather than losing them.

- **A normal map was shown to the picture model**, which guessed "fabric".
  Height, roughness and other data maps are never shown to it now; the
  list says "no colour map" instead.

### New

- **Tags from metadata.** Import also reads the folders an asset came from,
  titles and keywords embedded in images, 3ds Max file lists and the
  material names inside OBJ/MTL files.

- **Unsure guesses are shown, not hidden.** When the picture model is not
  sure, the import list says so — "maybe fabric 55%" — instead of showing
  nothing. Maybes are never applied.

- **Suggest tags** on the edit screen now uses everything: name, file
  names, metadata and the picture, each suggestion labelled with where it
  came from, and unsure picture guesses with their percentage.

## 1.41.1 - 10 October 2026

- Preserve existing assets when replacement imports are empty, unsafe, or fail.
- Validate archive manifests and stored paths; restore trash names exactly.
- Avoid export self-inclusion and preserve archives when a source is missing.
- Preserve notes when metadata is corrupt; repair generated preview fixtures.
- Include the current Qt write-back and refresh fixes.
- Verified with 552 automated tests and a 17-step CLI workflow.

## 1.44.0 - 10 October 2026

- Package the completed design-reference pass: kit controls, batch actions,
  favourites, course progress, saved display preferences, Restore all,
  model preview controls, system diagnostics, and responsive library controls.
- Coverage and remaining gaps are recorded in DESIGN-COVERAGE.md.
- Validation: 582 full-suite tests and 26 final design/layout checks passed.

## 1.43.0 — 10 October 2026

### Fixed

- **Importing did not tag anything from the picture.** The picture tagger
  only looked at a separate preview image, which most material folders do
  not have, so it never ran. It now looks at the colour map (base colour,
  diffuse, albedo), never the height or roughness map.

- **Good picture suggestions were thrown away at import.** A wood texture
  read as "probably wood" was dropped because it was not certain. Surface,
  environment and form suggestions in that band are now kept; look tags
  (worn, painted, polished) still need a certain answer.

- **Changing an asset's category in the import list cleared its tags.**
  They are worked out again for the new category.

### New

- **Tags from the file names.** Import now reads the names of the files
  inside the folder, and the folder they came from, not only the asset
  name. `TextureSupply_Veneer001-Albedo.tif` is tagged wood even when the
  folder is called `TX_88231`. Map-type words are ignored, so every
  material with a `_Metallic` or `_Gloss` map is not tagged polished metal.
  Wood types (oak, walnut, birch, teak, mahogany, pine, veneer) are now
  recognised as wood.

## 1.42.0 — 10 October 2026

### Fixed

- **Imported assets did not appear.** They were copied into the library and
  then never added to the index the grid reads, so an import looked like it
  had done nothing. The same was true of saving an edit, moving to the trash,
  restoring, importing an archive, adding a version and saving notes — every
  one of them wrote to disk and left the grid showing the old state. They
  all update it now.

- **If an import failed partway, the assets that had already copied were
  lost from view as well.** They are kept now.

- **Only 600 of the library's 8,777 assets could be browsed.** The grid
  stopped there and said "600 shown" as if that were everything. It shows
  the whole library now; reading all of it takes 0.07 s.

- **The largest assets showed blank tiles.** Qt refuses any image over
  256 MB and returns nothing, and many previews here are full-size sources
  — up to 91 MB of file. Thumbnails now come from the local cache, which
  reads each source once and keeps a small copy. A machine that used the
  previous interface already has most of them.

- **Saving an edit or trashing something threw you back to the top** of the
  grid. It keeps your place and your selection now. Choosing a new
  category, on the other hand, now does go to the top, which it did not.

### Also

- Picture tagging and the AI suggestions did work — but only once the
  model was present, which since 1.40.0 it is.

---

## 1.41.0 — 9 October 2026

### Fixed

- **The app did not notice when the library went offline.** If N: dropped —
  the NAS rebooting, the VPN going, a laptop waking on a different network —
  it carried on showing the grid from its last index, and Import and Move to
  trash still looked available. The failure then arrived as whatever the
  filesystem said.

  There is now a strip across the top saying the library is offline and that
  nothing will be written until it is back, with a button to check again.
  Browsing still works, because the index is still there. Anything that
  writes says what it is waiting for instead of failing. When the share
  returns it clears itself and re-reads.

  Pinning still works offline — it is kept on this machine, not in the
  library.

### Changed

- The share is polled rather than watched. SMB change notifications are not
  reliable enough to trust, which is the same conclusion the previous
  interface came to.

---

## 1.40.0 — 9 October 2026

### Fixed

- **The vision model never arrived, so picture tagging looked broken.** The
  probes installed, the app reported the tagger as available, and then
  suggested nothing — because the 373 MB model was only ever fetched if you
  happened to press "Suggest from the picture" in the edit screen. At import
  it gave up silently.

  It now copies itself from the share when the app opens. That takes **half
  a second** on the LAN, measured, so there is nothing to ask about and
  nothing to click. A machine that cannot see the share is left alone and
  Settings offers a button instead, because 373 MB over the internet is a
  different decision.

- **Importing now says when it is tagging from the name alone**, rather than
  quietly doing half the job.

- **A copy in progress could be mistaken for a finished one.** The file was
  written straight to its final name, so for the half second it took, the
  app believed the model was ready and anything asking for a suggestion
  loaded a half-written file. It is copied beside and moved into place in
  one step now — the model is whole or it is absent, never in between.

---

## 1.39.0 — 9 October 2026

The last of what the new interface was missing. Everything the old one did,
it does.

### Added

- **Ctrl K goes to anything** — a category, a kit, a saved filter, any page,
  or an action on what you have selected. Assets join the list once you have
  typed enough for it to mean something. Search moved to Ctrl F.

- **Archives.** Export a category as a zip holding whole assets — files,
  sidecars and the vocabulary they use — and import one somewhere else
  without losing a tag. In Settings › Share & archives.

- **Synonyms.** A word people search for that is not a tag: `aluminium`
  finds `surface:metal`. The library already expanded these when searching;
  now you can see them and add them.

- **New sub-categories**, from Organise. Stored as a tag, so assets move
  between sub-categories without any file moving.

- **Jobs can be cancelled and retried.** A long rescan with no way to stop
  it is a window you have to kill.

- **Double-click an issue** to open the asset it is about. A list of
  problems you cannot act on from is a list people stop opening.

---

## 1.38.0 — 9 October 2026

### Added

- **Saved filters.** Narrow the library down, press Save filter, and it
  appears in the sidebar. Applying one puts you back exactly where you were
  — the same category, the same pills, the same sort — rather than
  approximately there. Right-click one to delete it.

- **An asset detail page**, from the inspector's Details button. Versions,
  with a note on each and a Make current for any of them; notes written into
  the asset's own folder; everything the library thinks wants attention; and
  every file with its role, version and colour space.

- **Versions can be added again.** Pick the newer files and they go in as
  v002, v003 and so on. The files already in the folder are never moved — a
  version sits beside them and the sidecar says which one Send to uses.

---

## 1.37.0 — 9 October 2026

### Added

- **The sub-category pills are back.** Under the heading, showing the facet
  that category is filtered by — surface for materials and textures,
  environment for HDRIs — most used first, with a count on each. The count
  is of that category, so what a chip says is what clicking it shows.

- **List view.** The grid is the wrong shape for LUTs, courses and anything
  else with no useful thumbnail: a wall of identical empty squares. Grid /
  List sits beside the sort.

- **The app says who published it.** Indra - Builders Club, in Settings ›
  About and in the executable's own properties, where Explorer and the
  unknown-publisher warning read it. It is not code signing — Windows will
  still warn until the file is signed — but the warning now names someone.

---

## 1.36.0 — 9 October 2026

### Fixed

- **The icons were missing from the sidebar.** The new interface had no icons
  at all — the shapes were written for the old one and the new one could not
  reach them. They are shared now, so both draw the same set.

- **They then came out in pieces**, which is worth recording because nothing
  failed: a clipped icon is still an icon, so it only showed up by looking.
  There is now a test that each one draws something and that none of it falls
  outside its own box.

- **The window had no icon** in the title bar, the task bar or Alt-Tab.

- **The new interface wrote nothing to the log.** It opened, it worked, and
  it left no record — which would have meant a crash report with nothing in
  it. It writes the same startup line the old one always did.

---

## 1.35.0 — 9 October 2026

### Fixed

- **The AI tagger was silently switched off, and had been for a few hours.**
  Moving some modules around left it looking for its probe files one folder
  above the one they are in. Nothing failed — it simply stopped suggesting
  anything, which looks identical to a model being careful. Working again,
  and there is now a test that fails if the path is ever wrong.

- **The tag probes now install with the app.** They are 115 KB and the
  tagger can say nothing without them; before this they stayed behind on the
  share and every installed copy had tagging dead. The 373 MB vision model
  still does not travel with the installer — it is copied from the share the
  first time you use it, once per machine.

- **That copy looks in the right place now.** It only checked beside the
  executable, and the model sits one folder up from there, so every machine
  would have quietly downloaded 373 MB over the internet instead of copying
  it across the LAN.

- **`release.ps1` built with whatever `python` the terminal offered.** A
  virtualenv on PATH answers to that name, has no PyInstaller, and the
  failure reads as a broken build rather than the wrong interpreter. It now
  picks one that can actually build, and refuses outright if that
  interpreter cannot import PySide6 — which would otherwise have produced an
  executable that opens the old interface on every machine in the studio.

---

## 1.34.1 — 9 October 2026

### Fixed

- **Opening BC Storehouse opened the old interface and said nothing about
  why.** The launcher called `pythonw`, and a virtualenv on PATH answers to
  that name without having PySide6. It now tries a list and takes the first
  that can actually run the new interface.

- **The fallback explained itself to a console that does not exist.** The
  app launches from a shortcut with no terminal attached, so the message
  went nowhere. It is a dialog now, naming the exact Python and the one line
  that fixes it. A fallback nobody can see is indistinguishable from the new
  interface just not working.

---

## 1.34.0 — 9 October 2026

**The new interface is now the application.** Opening BC Storehouse opens the
Qt window. The old one is still there behind `--tk`, and the shortcut **BC
Storehouse (old UI)** opens it.

If the new window will not start on your machine, or will not do something
you need, run the old one and say what was wrong. That is what it is kept
for.

### What is different

- **It looks like the design.** Near-black, one accent, Geist, square tiles
  in rounded wells, a full-height rail with the counts, the inspector on the
  right.

- **Setups and courses play in the window.** A scrubber, speed, volume,
  fullscreen, a lesson list, and notes with a "note the time" button that
  stamps the timecode. The notes are written into the asset's own folder,
  not the index. This is the screen the old toolkit could not build at all.

- **HDRIs have a viewer.** Exposure, tonemap, and a readout saying the
  resolution, the brightest value, the median and how much of the image is
  over 1.0 — because an HDRI has no single correct appearance and two stops
  decides whether a window is blown out or a room is black.

- **Import shows you the plan.** Every source is planned before anything
  moves: where each asset would go, what maps were found, what tags were
  guessed, and whether it is already in the library. Change the destination
  and it re-plans. Drop a folder anywhere on the window to start.

- **Edit separates your tags from suggestions**, and remembers the ones you
  reject so they do not come back.

- **Organise finds near-duplicate tags.** `concrete` and `concreet` both end
  up in a real vocabulary and a search for one silently misses the other.

- **The trash is a page**, with restore, and emptying asks twice.

- **Settings is eight sections**, each showing only what this machine can
  actually tell you or change.

### What stayed the same

Everything underneath. The folder on disk is still the truth, the sidecars
are unchanged, the index is still a rebuildable cache, and Send to Cinema 4D
and Houdini write the same hand-off the same plug-ins already read. Dragging
a card into Cinema 4D, Houdini or Explorer still copies the files — that one
was the reason this toolkit was chosen over the alternatives.

### The download is bigger

About 35 MB becomes roughly 130 MB. That is Qt, and it is the price of the
video player and everything else above. The build takes only the parts of Qt
this app uses; WebEngine, Quick, QML, Charts and Qt3D are excluded.

### Still to come

Versions, archive import and export, the deep health check and the command
palette are not in the new window yet. They are all in the old one.

---

## 1.33.0 — 9 October 2026

### Added

- **Organise.** The vocabulary and the kits built from it, in one page. The
  part that earns its place is near-duplicate detection: a studio vocabulary
  acquires `concrete` and `concreet`, and a search for one silently misses
  the other. Counts are shown beside each, because the bigger one is almost
  always the one to keep. Renaming or removing rewrites every sidecar that
  carries the tag, so both run as jobs and both say how many assets they
  will touch before touching any.

### Fixed

- A tag page that tried to read the vocabulary document as if it were a flat
  facet map, and a Qt tab bar painting a grey strip behind its own tabs.

---

## 1.32.0 — 9 October 2026

### Added

- **Trash is a page.** What was removed, where it came from, and a restore
  for each. Emptying asks twice — the second time with the count and the
  size in front of you — because it is the only thing in the app that cannot
  be undone.

- **System.** The jobs that have run and anything in the library that wants
  attention, so that nothing the app does in the background is invisible.

- **A viewer for HDRIs and models.** Exposure, tonemap and a readout giving
  the resolution, the brightest value, the median and how much of the image
  is over 1.0 — the questions people actually have about an HDRI, and ones a
  baked thumbnail cannot answer. Both decode off the UI thread.

---

## 1.31.0 — 9 October 2026

### Added

- **Import** plans every source before anything moves: where each asset
  would go, the maps found, the tags guessed and whether it is already in
  the library. Changing a destination re-plans that row. Folders can be
  dropped anywhere on the window.

- **Auto-tagging at import**, from the name and from the picture. Both,
  because they reach different assets — a name like `rusted_metal_panel`
  says three true things and a vendor code like `plaster14` says none, while
  the model is the other way round.

- **Edit**, with suggestions kept separate from the asset's own tags and
  rejections remembered. Saving checks the revision captured when the page
  opened, so an asset someone else edited on the share is refused rather
  than silently overwritten.

---

## 1.30.0 — 9 October 2026

### Added

- **Send to Cinema 4D and Houdini, pin, trash, copy and reveal** in the new
  window, with the two status lines saying whether each plug-in is actually
  installed on this machine.

### Changed

- `jobs.py`, `fonts.py`, `tagging.py`, `viewer.py`, `fbx.py` and `thumbs.py`
  moved out of the old interface's folder. None of them ever touched Tk, and
  leaving them there meant the new window loaded the entire old interface to
  get at a thread pool. The bundled typefaces moved with them.

---

## 1.29.0 — 9 October 2026

A second user interface, in Qt. The Tk one is untouched and still what opens
by default — this one is behind a flag until it does everything the old one
does.

    BC Storehouse.exe            the app as it has always been
    python launch_ui.py --qt     the new interface, from source

### Why

Thirteen screens still had the old layout, and Tk was the reason several of
them could not be built as designed: no inline video for Setups and Courses,
no letter-spacing, no animation, and a grid that had to be hand-drawn on a
canvas to scroll at a usable speed. Qt has all of those, and the design's
tokens map almost directly onto a stylesheet.

### What is in it

- **The Main screen**: full-height rail with counts, search, sort, a
  thumbnail-size slider, the asset grid and the inspector.
- **Titles have their real tracking** — the design asks for −0.02em on
  headings and Tk simply could not do it.
- **The grid is virtualised by Qt**, so the apparatus the Tk version needed —
  measuring rows, tracking a viewport, pooling canvas items — is gone.
  Thumbnails decode on a thread pool and arrive when they are ready.
- **`storehouse/design.py`** now holds the tokens, and both interfaces read
  it. A test fails if they drift apart, which is the only way to keep "one
  theme module" true once there are two front ends.

### What is not in it yet

Importing, editing, Send to Cinema 4D and Houdini, jobs, trash and settings
are still the old window's. The new one says so plainly rather than offering
a button that does nothing. Pinned and Recent are listed but not wired.

### For whoever builds a release

The shipped executable is unchanged and still about 35 MB — the build now
explicitly excludes PySide6, because bundling it would roughly quadruple the
download before the new interface is worth having. When it reaches parity
that exclusion comes out, and the installer grows to about 130 MB.

To try it you need PySide6 on the machine:

    python -m pip install PySide6

---

## 1.28.0 — 9 October 2026

The inspector, and the parts every other panel is built from.

### Changed

- **The inspector reads like `design/Asset.dc.html`** — a wider panel (360px),
  the preview in a large rounded well, the name as a title with the category
  under it, tag chips with a dashed "+ tag", then a **Send to** card holding
  Cinema 4D and Houdini as full-height buttons.

- **Send to now tells you whether it will work.** Under the two buttons are
  two status lines saying whether each plug-in is actually installed on this
  machine. The design shows which application is *running* and its version;
  we do not know that, so this says the true thing we do know. If a Send ever
  seems to do nothing, that is the first place to look.

- **Values in the inspector are set in mono** and right-aligned, so a column
  of sizes, counts and resolutions can be compared down the page.

- **Every panel picked up the new controls at once.** Settings, Jobs, Import,
  Edit, Organise, Trash and System all draw their buttons and dropdowns
  through the same two helpers, so rebuilding those on the component kit
  restyled all of them. Panel titles, subtitles and the Back button went with
  them. The *layouts* of those screens are still the old ones — only the
  controls changed.

### Fixed

- Issues on an asset are now a status dot and a line rather than coloured
  text, so they read without relying on the colour.

### Note for whoever reads the tests

`test_the_rail_offers_send_to_cinema_4d_for_a_texture` asserted the exact
words "Send to Cinema 4D". The button is now "Cinema 4D" under a "Send to"
heading, which broke the test while changing nothing an artist can do — it
now asserts the capability instead.

---

## 1.27.0 — 9 October 2026

The Main screen, rebuilt to `design/Main.dc.html`. This is the one you look at
all day, so it is the one worth doing first.

### Changed

- **There is no bar across the top of the window any more.** The rail runs the
  full height and carries the mark and the version, the way the design has it.
  Search moved into the content column, directly above the grid, with Import
  beside it.

- **The rail.** Line icons instead of the Unicode symbols, counts set in mono
  so they line up, categories under a proper heading, and a foot that shows
  the job in progress, which library you are on and whether it is answering.
  Trash and Settings sit below a rule, because they are not places in the
  library.

- **Tiles are square**, and the preview sits in a rounded well with a hairline
  that brightens under the cursor. Selecting one draws a 2px accent ring
  rather than changing the colour behind it — the thumbnail stays the thing
  you are comparing. A long path in the rail is cut from the middle, so you
  can still see which drive and which folder.

- **The selection bar** is the design's batch bar, and says how many.

### Fixed

- **A 200px hole between the search box and the heading.** An empty
  CustomTkinter frame is 200px tall until you put something in it, and the
  sub-category chips have nothing to show on "All assets".

- **The selection ring was drawn twice**, once as a square rectangle over the
  rounded well.

### Not built, deliberately

The design shows things the library has no data for, and the brief says not to
invent any:

- Renderer chips (Redshift / C4D / Houdini / AE / Nuke) — nothing records which
  renderer an asset is for.
- "Most used" sort, and the popularity it implies — nothing counts uses yet.
- Per-tile hover actions (pin, quick send) and the favourite star.
- The accent pill count on Inbox.

### Known gaps

- Setups and Courses in the design are built around an inline video player.
  Tk has no video widget, so those two screens cannot be built as drawn
  without another toolkit. Everything else can.
- The remaining screens — Asset, HDRI, Model, Import, Drop, Edit, Organise,
  Trash, System, Settings, Onboard, Scripts, Kits — still have their old
  layout in the new colours.

---

## 1.26.0 — 9 October 2026

The first step of the redesign. This one is the foundation rather than the
screens: the palette, the typeface and the controls everything is built from.
The screens themselves are rebuilt one at a time after this, so the app you
open today is the old layout in the new colours — it will not match the
mockups yet.

### Changed

- **A new palette, everywhere at once.** Near-black ground, one accent, and
  no decoration in the chrome, so previews carry the colour instead of
  competing with it. Every colour name the app already used now points at the
  new value, which is why the whole thing changed at once rather than screen
  by screen.

- **Geist and Geist Mono ship with the app.** Nothing to install: the five
  faces are handed to Windows for the lifetime of the process and are gone
  when you close it. Mono is for paths, resolutions, versions and shortcuts —
  anywhere a column of values should line up.

- **Text is smaller and denser**, because the design is specified in pixels
  and the app was setting type in points — a third larger than intended at 96
  dpi. Body text is 13px where it was effectively 16. If this reads too tight
  on the floor's monitors, say so; it is one number in `theme.py`.

### Added

- **A component kit.** Buttons in four weights, icon buttons, filter chips,
  tag chips, source badges, status dots, the search field with its Ctrl K
  hint, segmented controls, toggles, checkboxes, dropdowns, sliders, tabs,
  rail rows with counts, toasts with Undo, the offline banner, error cards,
  empty states, a shimmer for loading, tooltips and the batch action bar.

- **An icon set drawn rather than loaded.** Tk cannot render SVG, so each
  Lucide icon is written out as the lines and arcs it is made of. They take
  the colour they are asked for, which is how hover and disabled states work
  without four exported files per icon.

- **A page to review all of it.** `python tools\\ui_gallery.py` opens every
  component in every state. Open it beside `design/Components.dc.html` and
  the differences are obvious.

- **The design reference lives in the repo** at `design/`, mockups and brief
  together, so the spec and the code stay in one place.

### Known gaps

- Letter-spacing. The design puts negative tracking on titles and 0.06em on
  uppercase labels. Tk does not expose it, so the labels fake it by spacing
  the characters and the titles simply do not have it.
- The screens. Main, Asset, HDRI, Model, Import, Organise, Settings and the
  rest still have their old layout. That is the next phase.

---

## 1.25.0 — 25 September 2026

### Fixed

- **The browser UI now only answers its own pages.** It binds to this machine,
  which keeps other computers out and does nothing about the browser already
  here: any website open in a tab could tell it to start a full library
  rescan or open Explorer windows, because a request like that needs no
  permission from the server it is aimed at. Checked before the fix — both
  did exactly that. Requests from anywhere else are refused; the app itself,
  and anything that is not a browser, are unaffected.

- **A damaged index no longer leaks a database handle.** Opening one failed
  part-way through and left the file open, which is the thing that stops a
  rebuild taking the exclusive lock it needs. Every "Check library health" on
  a damaged cache leaked one.

### Changed

- The README said the project had no third-party dependencies. The core still
  has none and never will, but the app needs customtkinter, Pillow and numpy,
  and the browser UI needs fastapi and uvicorn. It says so now.

---

## 1.24.0 — 25 September 2026

### Fixed

- **A byte-order mark no longer empties the tag vocabulary.** `_vocabulary.json`
  is meant to be edited by hand, but Notepad and PowerShell both add an
  invisible mark to anything they save, and the four places that read the file
  all rejected it and carried on silently as though the library had no
  vocabulary at all. A library with 115 known tags reported none. Worse,
  adding a tag afterwards merged it into the stock default list and wrote
  *that* over the studio's own. Anything that writes the vocabulary now
  refuses when it cannot read what is there, and says why.

- **Renaming, merging or removing a tag no longer leaks a database
  connection.** Each one left a handle open for the rest of the session, and
  an open handle is what stops a rebuild getting its exclusive lock.

- **Pinning, saving a note, deleting, and finishing a tag job** no longer
  scroll the grid back to the top.

---

## 1.23.0 — 25 September 2026

### Fixed

- **Editing no longer jolts the grid.** Adding a tag, setting a preview or
  saving a description rebuilt the entire view: it re-read the library
  identity and the tag synonyms across the share, re-queried up to two
  thousand assets, threw every card away, re-requested every thumbnail and
  scrolled back to the top — to show one word on one card. Now only that card
  is repainted. Where an edit really can change what belongs on screen — a
  rename, or a retag while a tag filter is on — the view is still rebuilt,
  but it keeps its place and its selection instead of jumping to the top.

- **Moving assets to another category** also keeps your place now.

- **"asset.json is not valid JSON" when saving.** The library is on a network
  share, and a reader there can be handed a partial view of a sidecar while
  it is being replaced — two bytes of a perfectly good file. It is now read
  again before that is believed. A sidecar that is genuinely broken still
  says so.

- **A sidecar saved in Notepad is readable again.** Notepad and PowerShell
  both add a byte-order mark, which the reader was rejecting. The whole point
  of a sidecar is that somebody without the tool can open it.

---

## 1.22.0 — 25 September 2026

### Fixed

- **Scrolling and importing no longer stutter.** A handful of the largest
  texture plates in the library — the 21K and 22K ones, and a 32K concrete —
  are over 300 megapixels, and Pillow refuses anything that big by default on
  the assumption it is a hostile file. Those cards stayed blank, which was
  bad enough; worse, the refusal was not remembered, so the app read the
  whole file again on every redraw to fail in exactly the same way. One file
  had been decoded and refused 42 times. Both halves are fixed: the limit is
  lifted, because nothing in this library came off the internet, and a file
  that genuinely cannot be decoded is now only tried once.

- **The thumbnail size no longer resets itself.** Moving the mouse off a card
  quietly put the size slider back to its default, so the grid changed size
  on the next redraw for no reason anybody could see.

- **A failed thumbnail no longer leaves a broken card.** When a decode
  failed, the grid was handed the path of a thumbnail that had never been
  written, and went looking for a file that was not there.

- **The NAS's own index is no longer catalogued as assets.** The library
  lives on a Synology, which leaves an `@eaDir` folder beside everything it
  has thumbnailed; 1,586 of those files, and 19 `Thumbs.db`, were being
  counted as asset files. They are left alone on disk and ignored.

### Changed

- **Rescan is six times faster** — a little over two minutes to twenty
  seconds on the studio library. It had been asking the file server twice for
  things it already knew.

---

## 1.21.0 — 23 September 2026

### Changed

- **Rescan is six times faster.** Rebuilding the index from disk took a
  little over two minutes on the studio library; it now takes twenty
  seconds. Nothing about what it finds has changed — the same 11,267
  assets, the same file lists, the same 6,558 issues, in the same order.
  It simply stopped asking the file server for things it had already been
  told: a file's size and date arrive with the folder listing, and were
  being asked for a second time, once per file. The folders are also read
  eight at a time now, which is worth doing when the work is waiting on a
  share rather than computing.

  Worth knowing if you have been putting off a rescan because of how long
  it took.

---

## 1.20.0 — 22 September 2026

### Added

- **Send to Houdini.** The same as Send to Cinema 4D, for Houdini: the
  material is built in `/mat` a moment after you send it, with nothing to
  press at the Houdini end.

  **Redshift where its plugin is loaded** — Texture Samplers into a
  Standard Material, normals through a Bump Map set to tangent-space,
  displacement into the output's Displacement, DirectX normals flipped.
  **MaterialX where it is not** (Houdini 22 here has no Redshift), which
  Karma renders: mtlxstandard_surface, mtlximage, mtlxnormalmap and
  mtlxdisplacement, wired the same way.

  Colour spaces are set per map — sRGB for colour and emission, Raw for
  roughness, metalness, normals and displacement.

  **Install once per machine**: Settings › Houdini › **Install Houdini
  plugin**, then restart Houdini. It installs as a Houdini *package*, so
  your own `123.py` and preferences are untouched, and one install covers
  every Houdini version on the machine.

  Send to Houdini is on the right-click menu and in the side panel,
  beside Send to Cinema 4D. Single textures still go to the clipboard
  rather than being made into a material.

---

## 1.19.0 — 22 September 2026

### Changed

- **The right-click menu is the app's own.** It was a Windows menu, which
  takes a background colour and ignores everything else — beside the card
  grid it read as a dialog from another program. It is now drawn like the
  rest of the window: rounded, hairline between groups, rows that light up
  under the cursor, and a submenu that opens beside its row. Escape and a
  click outside close it, arrows and Enter work, and it never opens off the
  edge of the screen.

  Regrouped by what you came to do: **Open** and **Send to Cinema 4D**
  first, then what goes on the clipboard, then where the files are, and
  **Move to trash** on its own at the bottom where a slip cannot reach it.
  Copy preview image is greyed out on an asset that has no preview instead
  of failing when pressed.

### Added

- **Send to Cinema 4D in the side panel**, for a material or a texture —
  no need for the right-click menu when the asset is already selected.

---

## 1.18.0 — 22 September 2026

### Changed

- **Send to Cinema 4D no longer needs anything pressed in Cinema 4D.**
  It was: send here, then find the script under Extensions there. Now the
  material simply appears, a moment after you send it. A small plugin
  watches for what Storehouse hands over and builds it — no port, no
  network, and it works whichever application was opened first.

  **Install it once per machine**: Settings › Cinema 4D › **Install
  Cinema 4D plugin**, then restart Cinema 4D. The old menu script is
  removed for you.

  **Extensions › BC Storehouse Material** is still there for building
  from a folder by hand, when Storehouse is not running.

---

## 1.17.3 — 22 September 2026

### Fixed

- **A vendor's promo shots sit on the product, not on a card of their
  own.** 41 cards that were nothing but `Promo01.jpg`… have been folded
  into the material they advertise, and one of them became its preview —
  `Bath Towel 001` is now part of the bath towel material rather than
  beside it. Matched through the folder they came from on the share, not
  by name.

- **7 exact duplicate assets** removed.

**Library: 11,267 assets, 31,536 files, 303 GB, 0 errors, no duplicates.**
The only issues left are 6,557 assets with no tags and one with no preview.

---

## 1.17.2 — 22 September 2026

### Fixed

- **One product, one card.** A material that arrived as `X 4K` + `X 6K` +
  `X Previews` + `X_disp16` — and sometimes again as `X_2` — is now a
  single card holding all of it. 1,069 split cards were folded into their
  product across the library; nothing remains split. The files move, the
  tags are united, and anything left over goes to the trash rather than
  being deleted.

- **A showcase render is a preview, not a card.** `Perforated_Organic_Wood.jpg`
  sitting in a vendor's `07_TOP10/` folder was its own card; it is now the
  preview of the material it shows.

- **An asset named after a folder is renamed after its files.** Cards
  called `SPECULAR`, `4k_26` or `Previews` take the product's name.

- **The Poliigon Terrazzo pack is in** — 50 products, blocked until now
  because a stray one-file preview had the right name and the import
  believed it. “Already here” is judged by what a folder holds, not by
  what it is called.

**Library: 11,316 assets, 31,575 files, 303 GB, 0 errors.** Rescan after
installing.

---

## 1.17.1 — 22 September 2026

### Fixed

- **The texture-pack import that stopped on 10 September has been
  finished.** 1,385 materials and textures, 105 GB, brought in from the
  source packs; 465 more were already here by content and skipped. Assets
  that were one stray preview render and nothing else — 35 of them, like
  `FabricUpholsteryTwill001_preview1` — now have their real material, and
  the strays are in the trash.

- **A material's copies are one asset.** The pack survey walked `Previews/`,
  `REGULAR/6K/` and `DEMOS/` as separate folders and made separate assets of
  one material; that is where the `X Previews` / `X 4K` splits came from.
  They fold into the product now, structure kept as subfolders.

- **A numbered frame sequence is one asset**, not two hundred.

- **Numbered preview renders (`_preview1`, `_preview2`) are previews.**

- **Imported files no longer keep a vendor's read-only flag**, which had
  stopped the library moving or removing its own files; the trash empties
  past it either way.

Rescan after installing: the library grew by about 850 assets.

---

## 1.17.0 — 22 September 2026

### Changed

- **PBR texture sets are Materials; single maps are Textures.** A set of
  maps — basecolor, roughness, normal together — is a material in
  everything but file format, and Materials is where the studio looks for
  one. The importer now files a set under Materials and a lone map under
  Textures, and the map roles (basecolor, normal, roughness…) are kept
  under Materials, which is what Send to Cinema 4D wires by.

  **The library has been refiled to match:** 1,031 sets moved from
  Textures to Materials. Each is a rename of the folder — identity, tags,
  pins and kits are untouched. Saved filters that named `textures` will
  now show single maps only; resave them against Materials if you meant
  the sets.

- **An unsuffixed image beside the maps is the base colour.** `Bricks11.jpg`
  next to `Bricks11_NRM.jpg` used to come in as `unknown`; it is the
  colour map, and every vendor that names files this way means it so. 95
  sets already in the library had theirs repaired.

---

## 1.16.0 — 22 September 2026

### Added

- **Send to Cinema 4D builds a Redshift material.** On any texture set
  (asset panel, right-click, or a selection): **Send to Cinema 4D**, then
  in Cinema 4D run **Extensions › User Scripts › BC Storehouse Material**.
  You get a Redshift Standard Material with every map on the right port in
  the right colour space:

      basecolor      → Base Color               sRGB
      roughness      → Reflection Roughness     raw
      glossiness     → Reflection Roughness     raw, glossiness mode
      metalness      → Metalness                raw
      normal         → Bump Map (tangent) → Bump Input   raw, DirectX flipped
      bump / height  → Bump Map (height)  → Bump Input   raw
      displacement   → Displacement → Output              raw
      opacity        → Opacity Color            raw
      emission       → Emission Color           sRGB

  Where a set comes in several resolutions, the one most maps share is
  used. If objects are selected in Cinema 4D, the material is assigned to
  them. Several sets sent at once become several materials.

  **A single texture stays a single texture**: it goes on the clipboard as
  a file to paste or drag in as it is, and is not made into a material.

  The script has to be put into Cinema 4D once per machine: **Settings ›
  Cinema 4D › Install Cinema 4D helper**. Give it a shortcut from
  Customize Commands if you use it a lot. It also works on its own: run it
  with nothing sent and it asks for an asset folder.

  Tested against Cinema 4D 2026 and its Redshift, headless, by building
  from real library assets, saving a .c4d and reading the graph back.

---

## 1.15.1 — 21 September 2026

### Fixed

- **The app was very slow after 1.15.0, and the cause was two things.**
  The colour extractor ran after start and made a thumbnail for every
  texture and HDRI that lacked one — ten thousand decodes over the network,
  competing with the grid. It is gone; colour search has been removed
  entirely.

  Worse, that job was writing to the index while a rebuild recreated the
  tables underneath it, which corrupted the index (“database disk image is
  malformed”) and made every search crawl. Three changes so it cannot
  happen again: a rebuild is now one exclusive transaction from the first
  drop to the last insert, so any other writer waits rather than
  interleaving; a rebuild is an exclusive job, so nothing else runs
  alongside it; and a damaged index is set aside and rebuilt from nothing
  rather than failing — the grid shows “The index needs rebuilding” with
  one button.

  If your machine is slow on 1.15.0: install this, then Settings ›
  **Rebuild index from disk** once.

- `storehouse scan` at a prompt does the same.

### Removed

- Colour search and the swatches in the filter row.

---

## 1.15.0 — 21 September 2026

Phase 2 of v2: organising assets properly. Everything new is written to
disk inside the library — kits, saved filters, versions, the trash — so it
survives a rebuild of the index and can be read without the tool.

### Added

- **Versions.** An asset can have v001, v002… with a note each and a
  “latest” pointer. Open an asset › Versions › **Add version…**, pick the
  files, type what changed. The files already there never move — a scene
  may reference them — the version is a `v001/` folder beside them. What a
  DCC is handed on drag or copy is the latest version's files.

- **Inbox.** A new category for anything that arrives without enough to
  file it. Importing a model, texture, material or HDRI without a preview
  or a tag now offers the Inbox instead of refusing. Tidying the Inbox is
  selecting assets and **Move** to the right category.

- **Kits.** Select assets › **Add to kit**. Kits appear in the sidebar;
  open one for **Pull into folder…**, which copies every asset's current
  files into a project, one folder each. A kit lives in `_kits/` as a
  readable file naming assets by identity, so renaming an asset never
  breaks it.

- **Saved filters.** Set some filters, click **Save filter**, name it; it
  appears in the sidebar for everyone. “HDAs for Houdini 20.5”, “4K textures
  added this month”. There is an **Added:** menu in the filter row now, too.

- **Colour search.** Eleven swatches in the filter row for textures,
  HDRIs and materials. Colours are read off the thumbnails by a quiet job
  after start; the first time it may take a few minutes to fill in.

- **Tag manager.** Settings › Manage tags: every tag with how many assets
  carry it, **Rename**, **Merge into**, **Remove**, and **synonyms** —
  “chrome” finds `surface:metal`. Typing a new tag that looks like one that
  already exists (`Wood`, `woods`, `furnitrue`) asks first.

- **Duplicate check on import.** Files already in the library, under any
  name, are named before the copy — “all 5 files already exist in
  models/alba_chair_by_ton” — with **Open existing**. By content, not by
  name; only same-size files are ever compared, so it is quick.

- **Move to trash.** On the card menu and on a selection. Nothing is
  deleted: the folder moves to `_trash/` at the library root, and Settings
  › Library lists it with **Restore**. **Empty trash** is the one thing
  that cannot be undone, and it says how much it is about to remove.

- **Bulk edit** now includes **Move** to another category (a rename of
  the folder; identity and tags go with it) alongside tags.

- **Studio metadata.** New tag facets: `renderer`, `houdini`, `c4d`, `ae`,
  `nuke`. Colour space and resolution were already per file; they show
  beside each file in the asset panel.

### Notes

- An existing library gains the new facets and the `inbox/` folder the
  first time this build opens it. Nothing else on disk changes.

---

## 1.14.0 — 21 September 2026

The first of the v2 phases: stability. Nothing here adds a feature you
would use every day; all of it is what makes the app trustworthy every day.

### Added

- **A log file.** Everything the app does and everything that goes wrong is
  written to `%LOCALAPPDATA%\storehouse\logs\storehouse.log`, rotated so
  it never grows past a few megabytes. Settings shows the path.

- **“Copy error details”.** When a panel fails to draw you now see what
  failed, in words, and a button that puts the version, this machine, the
  library path, the error and the last few minutes of the log on the
  clipboard as one block of text to paste into a message. The rest of the
  app keeps working. Settings has the same button under Diagnostics.

- **A Jobs panel.** Rescans, imports, health checks and backups run as
  jobs: listed on the Jobs button with a count, each with progress, a
  Cancel while running, and — if it failed — the reason, a Retry and a
  Copy error. Nothing slow ever runs on the thread that draws the window.

- **Storage offline.** If the library drive drops out, a banner says so;
  you can keep browsing what was indexed, and importing, editing and
  rescanning wait until it is back. Checked every ten seconds.

- **“Someone changed the library” is noticed within a minute**, not only on
  launch. And an asset edited on a colleague’s machine shows their edit the
  moment you open it here — the app compares the sidecar on disk with the
  one it indexed, one file, at that moment.

- **Backups of the index.** One a day when the app starts, one before every
  rebuild, the newest fourteen kept locally. The index is a cache of the
  folder — the folder is the truth — but a cache that took twenty minutes
  to build over the network.

- **Settings › Maintenance: Rebuild index from disk, Check library
  health, Back up now.** The health check reads every sidecar in the library
  and reports anything that would break later; `storehouse doctor --deep`
  is the same report at a prompt.

- **Empty states that say which kind of empty.** A search that matched
  nothing offers to clear the filters; an empty library offers Import.

### Fixed

- **16-bit TIFFs have thumbnails again — 693 of 701.** Last week every one
  was refused because a few of them crash the image library and a crash
  cannot be caught. They are now decoded in a helper process that can be
  lost: the 8 that genuinely crash go on a refuse list beside the thumbnails
  and are never tried again, and the app never notices.

- **Workers no longer stall behind a busy window.** Background threads
  reported to the window in a way that made them wait for it; while the
  grid was drawing, every thumbnail decode and every job stood still. They
  post to a queue the window drains between frames now.

- **`release.ps1` says which Python it found**, and why it cannot run it,
  instead of “Tests failed” when nothing had been tested.

---

## 1.13.1 — 21 September 2026

### Fixed

- **RGBA environment maps show a thumbnail.** An `.exr` with an alpha channel
  came back from the reader in a shape Pillow refuses, and the card stayed
  blank without a word. The 15K sky was the first one anyone looked at.

- **Poliigon overlay decals and 16-bit maps are recognised.** `_OVERLAY`,
  `_DISP16`, `_NRM16` and `_BUMP16` were unknown to the role detector, so
  the previews of a dust decal sat on a card of their own instead of on the
  maps they show.

---

## 1.13.0 — 11 September 2026

A pass through the code looking for bugs, and the library looking for what
the code had done to it.

### Fixed

- **Scrolling through models and untagged textures no longer stutters.**
  Working out what to draw on a card that has no chosen preview — every
  model shipped as FBX, every texture set nobody picked a preview for, about
  2,300 assets — was being done on the thread that draws, one database
  query and one file read per card. Thirty cards on screen was a 200 ms
  freeze on every scroll into a run of them. It happens on the worker now.

- **Copy to clipboard works for large assets and big selections.** The file
  list was handed to Windows on the command line, which stops at 32,767
  characters; one bathroom set alone needs 72,000. Past that point the copy
  failed and said so — or, if a file had since been deleted, said
  “copied” over an empty clipboard. The list now goes through a file, and
  the status bar says how many were copied and how many are gone.

- **256 texture assets have real names.** The first pack import called
  them `3K 10`, `Previews 47`, `HIRES 32` after the vendor folder it found
  the maps in. They were unfindable. Each is now named from its own files —
  `ScratchesLight010 3K` — and the import will not do it again. Their
  folders on disk still carry the old name; search reads the new one.

- **A folder called `export` no longer becomes an asset called `export`.**

- **`release.ps1` runs from a redirected console.** PyInstaller’s version
  banner on stderr was being treated as a fatal error when stdout and stderr
  were captured, leaving the build half done.

### Added

- **`duplicate_content` in Issues.** Two assets holding the same files at
  the same sizes are now reported, both ways round. There are 31 such pairs
  in the library today — `plaster_plaster05` and `6k_8`, and so on — each
  from a pack imported twice. Which one to keep is your call; the report
  tells you where they are.

---

## 1.12.0 — 10 September 2026

### Added

- **Models that ship only as `.fbx` and `.max` now draw their own thumbnail.**
  About 360 of them showed an empty card. `.max` is Autodesk's and closed, so
  those were never going to be readable — but the FBX beside it is a
  documented format, and the app already had a renderer. It now reads the
  geometry out of the FBX and shades it, the same way it has always done for
  `.obj`. Both the modern and the 2010-era FBX layouts are handled.

  No DCC is involved and nothing is installed: it reads the file and draws it,
  in a fiftieth of a second for a typical product.

- **`.psd` textures show their picture.** 68 alpha and detail maps here are a
  Photoshop file and nothing else. The flattened image inside is exactly what
  somebody would have exported, and it is now what you see on the card.

`missing_preview` in **Issues** has gone from 128 to 1 — the one left is a
model that genuinely has nothing but a `.max` in it.

### Fixed

- **The app can no longer be killed by a 16-bit TIFF.** Certain 16-bit TIFFs
  do not fail to open, they take the process down with them — the window
  disappears, with no error and nothing in a log. It is a fault in the image
  library underneath, it cannot be caught, and which files trigger it cannot
  be predicted: one texture's 3K version crashes while its 8K version is fine.

  So no TIFF deeper than 8 bits is opened for a thumbnail. Around 700
  displacement and normal maps keep the plain card they already had, which is
  the price of not losing your session to one of them. They open in your
  paint or texture app exactly as before — this only concerns the
  thumbnail.

- **A model with an unreadable file falls back to one it can read.**
  `models/basket` ships a 462 MB text-format FBX beside a 42 MB `.obj` of the
  same basket; the card used to pick the FBX, read all 462 MB, and show
  nothing.

---

## 1.11.0 — 10 September 2026

### Added

- **`storehouse repair` puts a sidecar back in line with its folder.** If you
  extract an archive inside an asset, or delete a file by hand, the sidecar
  goes on describing what used to be there and every one of those files is
  reported missing. `repair` keeps the identity, the name, the tags and the
  description, and takes the file list from what is actually on disk.

      storehouse repair --all                 every asset with a missing file
      storehouse repair models/some_chair     just that one

- **Assets that hold files but not the thing itself are now reported.** A
  model with a product shot and no geometry used to pass silently: every file
  its sidecar declared was present, and that was all the scanner checked. It
  is the worse failure of the two, because a missing asset announces itself
  and a hollow one does not — it is findable by name, it has a thumbnail, and
  you only find out when you drag it into a scene and nothing arrives.

  Shown in **Issues** as `no_primary_file`.

### Fixed

- **396 model assets that were nothing but a vendor's zip have been unpacked.**
  They were findable by name and useless when opened. Their previews were
  inside the archives, so `missing_preview` fell from 376 to 128 at the same
  time.

- **Nine assets that were never products have gone.** Two licence files, four
  pages of a vendor's catalogue, a description and a texture set that already
  sat inside the chair it belonged to. They are in `_not_products` at the
  library root rather than deleted.

- **`Luc Chair by Rossin` has its geometry.** The asset held a product shot
  and nothing else; the model had never come out of the vendor's rar.

---

## 1.10.0 — 10 September 2026

### Fixed

- **A product's format folders are no longer split off as separate assets.**
  Vendors nest a product's renderer versions inside it —
  `S293 Chair by Balzar Beskow/S 293 Chair_vray/` — and each was becoming an
  asset of its own, leaving folders named after a renderer in the library.

  Spotting a format word in the name is not enough: `vray` alone gives itself
  away, `Keyboard & Magic mouse_vray` does not. What does is that the name,
  once the format suffix is stripped, is already contained in its parent's.

- **A product shipped only as a scene is recognised.** An asset folder had to
  hold a mesh, and `.max` reads as a scene — so a product consisting of two
  `.max` files was not seen as a product at all. Its textures then collided
  with copies of themselves a level up and refused the whole category.

- **An archive is dropped when the product it contains was already found.**
  The earlier rule only skipped an archive sitting directly beside its own
  folder; these vendors extract a level deeper.

### Added

- **`resplit_models.py`** re-files model assets that came in before the
  importer knew better. It moves rather than copies — everything is on one
  volume — and leaves anything it cannot claim where it is rather than
  guessing.

---

## 1.9.0 — 10 September 2026

### Fixed

- **A vendor library still came in as a handful of giant assets.** 1.8.0 looked
  exactly one folder down, and a Dimensiva download is nested deeper than that:

      Dimensiva/products/leica-m9-.../Leica M9 Camera/Leica M9_obj.obj
      Dimensiva/office/...

  So it made an asset of `products`, one of `office`, one of `kitchen` — the
  vendor's shelves, each holding dozens of products.

  It now finds assets at whatever depth they sit: an asset is the folder that
  *directly* holds a mesh, and anything above that is a shelf to look past. The
  same pack now comes in as 57 products rather than one.

- **Each product keeps its own preview and licence.** Vendors wrap a product
  with its thumbnail and licence beside the model folder. Those were being
  orphaned into an asset of their own, which also meant the product had no
  preview. When a wrapper holds exactly one asset, its loose files join it.

- **An archive sitting beside its own unpacked folder is skipped.** Vendors
  leave both behind, and importing both brought every product in twice — once
  as files, once as a zip.

---

## 1.8.0 — 10 September 2026

### Fixed

- **The window stopped responding while reading a model pack.** Working out
  what a dropped folder contains ran on the thread that draws the window, and
  the panel then built a row of widgets for every file in it — around two
  thousand for a twelve-model pack. Windows greyed the title bar and wrote
  "(Not Responding)".

  Reading now happens on a worker with the panel saying what it is doing, and
  the file list shows a count per role plus the first forty files instead of
  all of them. The window keeps drawing throughout.

- **A model pack was filed as textures.** The category was picked by counting
  extensions, and a model ships one mesh beside fifty maps — so the maps
  outvoted the thing they belong to. Geometry decides now, the same way an
  `.hdr` already did.

- **Fewer, more useful tag chips when importing.** The panel offered every
  value of every facet at once. `grade:` belongs to LUTs and `surface:` to
  textures; neither helps while filing a model. It now offers what applies,
  and anything else is still one keystroke away in the row above.

---

## 1.7.0 — 10 September 2026

### Fixed

- **A model in several formats became several assets.** A chair downloaded as
  obj, fbx, Corona and V-Ray arrived as five separate entries — one per format,
  plus the textures on their own. The maps ended up in a different asset from
  every model that needed them, and four of the five cards had nothing to show.

  Three things caused it and each is fixed: the importer counted mesh *files*
  rather than distinct models, so two formats of one chair read as two assets;
  the format suffix stayed in the name, so `_obj` and `_fbx` looked like
  different things; and `Sayl Office Chair` and `sayl-office-chair` were
  compared literally, so the same words with different separators did not
  match.

  Packs of genuinely different models still come apart properly, and eighteen
  HDRIs in a folder are still eighteen assets.

---

## 1.6.0 — 10 September 2026

### Fixed

- **New assets took over a minute to appear.** Every import finished by
  re-reading the entire library — all ten thousand asset folders and every
  sidecar in them — to learn about the one that had just arrived. Adding a
  single texture cost **81 seconds**.

  It now reads only what arrived: **0.55 s** for one asset, **0.73 s** for
  twenty. The cost is the size of what you added rather than the size of what
  was already there, so it stays the same as the library grows.

  The staleness fingerprint moves along with it, so adding something yourself
  no longer makes the app report that a colleague changed the library.

---

## 1.5.0 — 10 September 2026

### Fixed

- **Adding several models at once failed outright.** Dropping a pack of models
  refused the import with *"two source files would land on the same name:
  BaseColor.png"*. The importer grouped files by filename and discarded the
  folder they came from, so every model's `textures/BaseColor.png` was treated
  as one shared file. Every vendor names its maps the same way, so any pack of
  more than one model hit this.

  Now the folder decides: when several subfolders each hold a mesh, those are
  the assets and each keeps its own maps. A flat directory of loose files still
  groups by name, because eighteen HDRIs in one folder really are eighteen
  assets.

- **A model shipping two resolutions kept only one.** `4k/BaseColor.png` and
  `2k/BaseColor.png` collided on the way in. Subfolders are now handed over
  whole, so their structure survives.

- **The import panel lagged with a large pack.** It re-walked and re-stat-ed
  the entire source tree every time anything in the form changed — a category,
  a tag, a keystroke in the name. Twenty-four models came to 27 walks per
  change. Over the network each one is a round trip, which is what made it
  crawl. Measured: 108 ms → 0 ms per change on a local disk, and far more than
  that on the share.

### Added

- **Handbook as a PDF**, alongside the web version, for handing to somebody at
  a machine that is not theirs. `make_handbook_pdf.py` regenerates it with the
  counts read from the live library.

---

## 1.4.0 — 10 September 2026

### Changed

- **Rescan, Export, Settings and Issues are back on the top bar.** 1.3.0 put
  them behind a `⋯` menu. Hiding them saved room on a screen that had room to
  spare, and a control you have to remember exists is worse than one taking up
  space. **Add sub-category** returns to the foot of the rail.

### Fixed

- **The Issues panel froze the app.** It built one frame and one label per
  issue — about twelve thousand widgets for a library with six thousand
  warnings. It is now grouped by kind: **106 ms**, and each group has a
  **Show these** button that puts those assets in the grid, where multi-select
  and batch tagging can clear them.

- **Opening the app hammered the network.** The staleness check stat-ed every
  asset folder on launch: **17.7 seconds** across ten thousand assets, and
  about three minutes at a hundred thousand. It now reads only the names the
  directory listing already returned — **0.54 s**, 33× faster.

  The trade: it still notices assets being added, removed or renamed, which is
  what somebody else changing the library looks like nearly every time, but no
  longer notices a file replaced inside a folder that already existed. It was
  always a prompt to rescan rather than a guarantee. `storehouse doctor` still
  does the thorough version.

---

## 1.3.0 — 9 September 2026

### Added

- **Pinned and Recent** at the top of the rail. Most days you reach for the
  same thirty things; at ten thousand assets, finding them by scrolling is not
  a plan. Right-click a card → **Pin**, or <kbd>Ctrl</kbd>+<kbd>D</kbd>. Recent
  fills itself as you drag, copy or open things.

  Both are yours alone and stay on your machine. They are keyed by the asset's
  identity rather than its path, so renaming something does not lose it, and
  they are stored outside the index — which is disposable and gets rebuilt.

- **Resolution on the card.** Whether a texture set ships 4K or 1K was the
  question being asked at that moment, and it was hidden behind a click.

- **A Courses category**, for training. Filed by which software it teaches.

### Changed

- **The side panel opens only when something is selected**, giving the grid
  356px back the rest of the time.

- **The category dropdown is gone.** It listed the same categories with the
  same counts as the rail directly below it.

- **The filter row only shows what applies.** The facet dropdowns appear when
  there is a category to narrow, Clear appears when there is something to
  clear.

### Fixed

- **The asset count was a lie.** The grid asks for a bounded number of results,
  and the status bar reported what it got — so a library of ten thousand
  textures said "2000 assets". It now counts the real total and says
  *"showing 2,000 of 10,659"*.

---

## 1.2.0 — 9 September 2026

### Added

- **The app finds the library by itself.** A first run used to be a file dialog
  onto a network drive and a folder name you had to already know — and picking
  the wrong one offered to create a *second* library, which is quiet at the
  time and expensive later. It now looks where the studio library lives and
  offers it as one button.

  This is what makes handing somebody just the executable work.

### Fixed

- Running the first-run screen against a clean machine caught a crash that
  would have happened on **exactly that path and no other**.

---

## 1.1.0 — 9 September 2026

### Changed

- **A design system**, in one place: colour, type, spacing, radius. The app had
  padding values of 18, 15, 14, 12, 10, 8, 7 and 6 chosen one control at a
  time, and seven type sizes with no defined roles.

- **The top bar has a hierarchy.** Five equal-weight buttons became one filled
  action and four quiet ones; Settings was as loud as Import.

- **Rail rows are one pill.** A row used to carry a marker *and* a fill *and* a
  colour change — three signals for one fact.

- **Cards** got a larger radius, hover that lifts rather than recolours, and
  the category dropped from the label when every card on screen shares it.

- **Issues are amber for warnings, red only for errors.** Colouring sixty
  warnings like a failure teaches you to ignore the colour, which is what you
  want it for when there is a real one.

### Fixed

- **List view was unreadable for LUTs, Setups, HDAs, Tools and Plugins** — rows
  squeezed into one card's width, clipping names to a single letter.

- **The Edit and Import panels crashed** on open. A helper they both called had
  gone missing and nothing in the tests went near either one.

---

## 1.0.0 — 9 September 2026

First release. A studio asset library where the folder on the share is the
truth and the local index is a disposable cache — delete it and a rescan
rebuilds it from the folder.

- Nine categories; every asset is a folder with a sidecar carrying its
  identity, name and tags
- Grid drawn on a single canvas, so ten thousand cards cost what forty do
- Drag straight into Cinema 4D, Houdini, Explorer or Slack; copy paths in the
  form each of them wants
- Multi-select and batch tagging
- Import copies files into the library rather than pointing at where they
  happened to be
- Per-machine index, because SQLite on a network share corrupts
