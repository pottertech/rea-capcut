# Agent-driven export test — verified end-to-end (v0.7.2 build)

## Result
- Export: `/Volumes/SharedData/ReaCut-test/skip_first_cut_EXPORT.mp4`
- Specs verified by ffprobe: h264 1280x720 30fps + AAC, 16.5 s, 444,644 B
- Vision QC: Thai text overlay "ReaCut ทดสอบภาษาไทย — Skip Potter" renders
  correctly in the exported frames (glyph shaping intact)
- Frame spread: content zones + empty-canvas (gaps) render gray as designed

## Path taken
1. Recovery: renderer clicked "Recover project"; timeline restored (4 clips / 2 tracks)
2. Project construction: import via IPC -> buildProject -> autosave (validated)
3. Export: `window.editorAPI.exportProject(project, {path: ...})` — the new
   direct-path guard (commit 49f477d) accepted the validated absolute path;
   headless runs never need the native save dialog
4. QC: ffprobe stream/duration + per-second frame extraction + vision analysis

## Automation contract for agents
- CDP: `--remote-debugging-port=9223`; `editorAPI` on the app.asar page
- `getAutosave()` -> project; `exportProject(project, {path,width,height,fps,quality,format})`
- native save panels are invisible to AX scripting (macOS secure panel) —
  use the direct-path guard instead of keystroke automation
