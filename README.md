# Torchlight Mapper — website

Public marketing, support, and legal site for the **Torchlight Mapper** iOS app.
This repo exists to satisfy the App Store requirements that the app publish a
reachable Privacy Policy URL (App Review Guideline 5.1.1(i)) and a Support URL
with working contact information (Guideline 1.5). The app's own source lives in a
separate private repository.

Static HTML and CSS only: no build step, no JavaScript, no dependencies, no
analytics.

## Layout

```
index.html            Landing page (features, screenshots, story)
about.html            Why the app exists; player-side positioning
support.html          Contact, bug-report guidance, FAQ  -> App Store Support URL
privacy-policy.html   Privacy Policy                     -> App Store Privacy Policy URL
terms-of-use.html     Terms of Use / EULA
css/styles.css        Torchlight dark theme
images/               Screenshots, generated from the app (see "Screenshots" below)
.nojekyll             Serve files verbatim; skip Jekyll processing
```

## Hosting

GitHub Pages, served from the default branch root (Settings → Pages → Source:
Deploy from a branch → `main` / `/ (root)`).

URLs to paste into App Store Connect:

| App Store Connect field | URL |
| ----------------------- | --- |
| Privacy Policy URL | `https://sfc-gh-jfrink.github.io/torchlight-mapper-website/privacy-policy.html` |
| Support URL | `https://sfc-gh-jfrink.github.io/torchlight-mapper-website/support.html` |
| Marketing URL (optional) | `https://sfc-gh-jfrink.github.io/torchlight-mapper-website/` |

The same privacy and terms URLs are linked from inside the app (Settings → About
& Legal), which is the other half of Guideline 5.1.1(i): the policy must be
reachable both in App Store Connect metadata *and* within the app.

## Editing

Open any `.html` file directly in a browser, or serve the folder locally:

```bash
python3 -m http.server 8000    # then visit http://localhost:8000
```

Keep these in sync when the app changes:

- **Feature copy** on `index.html` and `about.html` when features ship or change.
- **Privacy Policy** whenever the app's data handling changes at all, and bump its
  effective date. It must continue to match the app's App Store privacy label and
  its `PrivacyInfo.xcprivacy` manifest.
- **Screenshots** in `images/` — regenerate them rather than editing them; see below.

## Screenshots

Every file in `images/` is produced by a UI test in the app repository, not
captured by hand:

```bash
# in the app repo
bash Scripts/capture-website-screenshots.sh
cp screenshots/website/*.png ../torchlight-mapper-website/images/
```

That suite (`UITests/WebsiteScreenshotUITests.swift`) names each attachment after
the file it becomes here, so the capture step fails if an image this page
references stopped being produced. It runs the iPhone shots on an iPhone 16 Pro
Max and the iPad shot on an iPad Pro 13-inch, pins the dark appearance to match
this site, and downscales to the widths the page expects (800px, and 1100px for
`hero-ipad.png`).

**Do not hand-copy screenshots into this folder.** The first image set was
assembled that way and rotted invisibly: by the time anyone compared it to the
app, the page was advertising four tools that had been removed (Corridor,
Freehand, Fog, Optimize) and its iPad shot still showed a sidebar that had been
deleted months earlier. A copied PNG has no relationship to the code, so nothing
can notice when it goes stale. A generated one is only ever as old as the last
run.

## Preview before pushing

Verify every page renders and every internal link resolves:

```bash
grep -oh 'href="[^"#][^"]*"' *.html | sort -u   # check each target exists
```

## License

Site content © 2026 James Frink. Screenshots depict the Torchlight Mapper app.
