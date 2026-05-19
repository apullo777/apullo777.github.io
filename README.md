# apullo777.github.io

Personal blog for Peter Chang.

## Local preview

```
hugo server -D
```

## Build

```
hugo
```

## Deployment

GitHub Pages deployment (via GitHub Actions) will be set up later.

## Notes

- Theme: [hugo-bearblog](https://github.com/janraasch/hugo-bearblog) (added as a git submodule under `themes/hugo-bearblog`).
- Dark/light toggle: implemented via local overrides in `layouts/partials/custom_head.html` and `layouts/partials/custom_body.html` (the theme is left untouched). Initial theme follows OS preference, then persists via `localStorage`.
