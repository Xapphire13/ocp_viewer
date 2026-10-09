# Changelog

## Unreleased

- The core range is `ocp-viewer-core[cli]>=1.1.0,<1.2.0` (was `>=1.0.10,<1.1.0`): ocp-viewer-core 1.1 runs on ocp-tessellate 3.6
- The native-tessellator toggles are gone: `enable_native_tessellator`, `disable_native_tessellator`, `is_native_tessellator_enabled` and `init_native_tessellator` are no longer exported, and importing `ocp_viewer` no longer prints "Found and enabled native tessellator.". ocp-tessellate dropped its native tessellator and `ocp_addons`, and ocp-viewer-core the toggles with it.
- New setting `python_enumeration` (default `false`) in `~/.ocpvscode_standalone`, the `--python_enumeration` flag, and the keyword `python_enumeration` of `show`, `show_object`, `show_objects` and `set_defaults` (vscode-ocp-cad-viewer issue 242): `true` numbers repeated object names from 0 like Python indices - `Solid(0)`, `Solid(1)`, `Solid(2)` instead of `Solid`, `Solid(2)`, `Solid(3)`. Off by default, because the names are the tree paths that animations, states and selections use. A config file written by an earlier release has no such key and gets the default.

## v1.1.3 (2026-09-14)

- `modifier_keys` is one map per platform, `{"macOS": {...}, "default": {...}}`, and the viewer gets the one for the machine it runs on. `meta` rotates, hides and isolates, and `metaKey` is Cmd on macOS but the Windows/Super key on Windows and Linux, which the desktop takes for itself - those chords never reached the page there. The default for Windows and Linux is therefore `meta: altKey, alt: metaKey`; the same shape the VS Code extension, Jupyter CadQuery and build123d Studio store. A `~/.ocpvscode_standalone` from an earlier release holds a single flat map: it is read as this platform's choice, with the shipped default for the other.
- The core floor is `ocp-viewer-core[cli]>=1.0.10`, where `Animation.add_track` refuses an unknown action or ill-shaped values before they are sent.

## v1.1.2 (2026-09-11)

- The core floor is `ocp-viewer-core[cli]>=1.0.9`: there `reset_defaults(port=...)` resets the viewer it names. In 1.0.8 the reset opened a fresh scope over the one the `port` had set, so with two viewers listening the which-port prompt appeared in the middle of a `reset_defaults`, and with one it reset whichever discovery found.
- The viewer page ships ocp-viewer-core JavaScript 1.0.4: a `set_viewer_config(glass=...)` at runtime re-derives the page's geometry instead of leaving it as it was. Leaving glass mode moves the tree out of the canvas's row and three-cad-viewer re-lays out with the `cadWidth` it was given, so the viewer used to grow by the tree's width and overflow the window until the next resize put it right.
- And three-cad-viewer 5.0.6: a resize sizes the toolbar and the tree, not only the canvas. `resizeCadView` passed neither `glass` nor `tools` on to the display, so outside glass mode the toolbar and the body kept the canvas's width while the tree sat beside it, and the tree kept its old height while the canvas grew.
- `questionary` is an extra of the core now rather than every host's dependency: its one use is the prompt that asks which viewer to talk to when several are listening, which needs a plain shell - and this is one of the two viewers that has one.

## v1.1.1 (2026-09-09)

- `ImageFace` and the native-tessellator toggles come from `ocp_viewer_core.tessellator`, so nothing here reaches into ocp_tessellate any more. The toggles - `enable_native_tessellator`, `disable_native_tessellator`, `is_native_tessellator_enabled` - are new in this viewer; the other three had them and this one did not. Needs the ocp-viewer-core release that carries `tessellator`, and the floor moves to it when that is published.
- The YAML dependency is declared as `PyYAML`, which is what `settings.py` imports. `pyaml` is a different package that happened to pull it in.

## v1.1.0 (2026-09-09)

The first release. 1.0.0 was the version the package carried while it was built and was never published.

- The server is `websockets`' threaded server; Flask and flask-sock are gone. A config sent behind a large model used to reach the browser first and unreadable: the old server compressed and wrote the browser's socket from several threads with nothing serializing them. `websockets` holds a lock for the whole of a send, compression is off (a model is mostly base64 textures; deflate cost seconds per show for a quarter less on the wire), and the page and its files are answered from the same server.
- Ctrl-C stops the server on Windows too, and with a page connected on every platform.
- A package built without its copied-in assets says so at start, and the README says to run `make assets` before a local install from a checkout.
- Command-line options are choices by where their value came from, not by comparing it to the default, which click 8.5 broke for flags: `--axes0` off and `--timeit` off were being recorded on every start.
- `--timeit` and `--debug` reach a client's `show()` (needs ocp-viewer-core 1.0.6, the floor).
- `make assets` creates the css directory it copies into.
- 78 tests, most of them against the real server run in-process.
