# PDF-Search-tool

A single-file, fully offline tool for finding PDF files in a folder. This was created using HTML and JavaScript. There is no backend or install and works directly in the browser.

This tool was built to fix a bottleneck I found working as an aerospace mechanic.

![PDF search tool](./preview.png)

## What it does

After picking a folder, type part of the file's name and open it in your browser. Everything runs locally on your machine.

- Instant filename search that updates as you type, across thousands of files
- Point it at any folder, no import or setup step
- Fully offline — nothing is uploaded, no internet connection needed
- Click any result to open the PDF in a new tab

## The problem

At my workplace we pull PDF drawings constantly: find one, print it, reference it while working on the part. Each lookup takes about 4 minutes on average (sometimes up to 7 if the drawing is in an unexpected place), almost all of it spent navigating deeply nested folders and hard to scan file names. This may not seem like a lot of time, however, the problem is that this look up would happen on average 20 times a shift (conservative estimate) which adds up to about 1 hour 20 minutes per mechanic per shift spent searching instead of building parts.

After investigating it was clear that the issue/bottleneck was looking for the drawings itself thus leading to wasted labor hours that could be reinvested into the parts that we are working on.

## How it works

Open the HTML file and select a folder. The tool walks the directory tree once and
builds an in-memory index of every PDF's name and path. Each keystroke runs a
substring match against that index in memory, so there's no disk access per search
and no cache written anywhere. All typed terms must match (AND logic); results render
up to 500 rows with a short debounce to keep typing responsive.

Folder access uses the browser's File System Access API. The contents of the PDFs are
never read, only their names and paths are indexed.

## How to use

1. Download `PDF Search.html` from this repo.
2. Open it in Google Chrome or Microsoft Edge.
3. Click **Choose folder** and select the folder with your drawings.
4. Type to search. Click any result to open it.

## Browser support

Chrome and Edge get the full experience through the File System Access API, which
keeps a live handle to the folder you pick. Alternative browsers fall back to a one-time
folder picker that still works locally but re-selects the folder each session.

## Privacy

Files never leave your computer. The tool reads only file names and paths into memory;
the PDFs are never opened, copied, or sent anywhere, and it runs with no network
connection. For export-controlled or otherwise sensitive drawings, nothing touches the
internet.

## Projected impact

This tool is not yet deployed, the figures below are estimates, not measured results.

Per mechanic, recovering ~1h20m per shift at roughly $22/hr works out to about $147
per week. If a site runs 50+ mechanics, at full integration the projected recovery is
on the order of $7,000+ per week in labor time redirected from searching to building.
The same tool would carry over to other sites running the same workflow.

## Limitations

- Filename and path search only. It does not read, parse, or OCR the contents of the
  PDFs, so you can't search for text inside a drawing. The zero-dependency speed is a
  direct result of that design choice.
- The live-folder mode needs a Chromium browser (Chrome or Edge).
- The index lives in memory and is rebuilt each time you open the file or re-pick the
  folder.
