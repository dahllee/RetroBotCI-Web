# ag-psd.bundle.js

`ag-psd.bundle.js` is a build artifact, not hand-written source. It is vendored
so the site keeps its no-build, no-external-dependency setup — the same reason
the Wix CDN images were pulled local in commit `3c7eec2`.

- library: [ag-psd](https://github.com/Agamnentzar/ag-psd) (MIT)
- version: **31.0.2**
- exposes: `window.agPsd.writePsd` / `window.agPsd.writePsdBuffer`
- used by: [`speech-bubble.html`](speech-bubble.html)

## Rebuilding

Run in a scratch directory outside this repo — the repo deliberately has no
`package.json`, and adding one would pull it into a build setup it does not need.

```bash
npm init -y
npm install ag-psd@31.0.2
echo "export { writePsd, writePsdBuffer } from 'ag-psd';" > entry.js
npx esbuild entry.js --bundle --format=iife --global-name=agPsd --minify \
  --legal-comments=none --outfile=ag-psd.bundle.js
```

Then copy `ag-psd.bundle.js` over the one in this directory.

The IIFE global format is deliberate: it matches the inline-`<script>` convention
used by the rest of the site, and unlike ES modules it still works when the page
is opened directly from disk over `file://`.

Only the PSD *writing* entry points are bundled; the reader is tree-shaken out.
