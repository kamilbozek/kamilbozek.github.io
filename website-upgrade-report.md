# Personal Website Upgrade Report

## Executive Summary

The site already has a strong foundation for a professional, slow-web-aligned personal website. It is static, lightweight, content-focused, and free of heavy frontend frameworks. That is a good architectural choice.

The biggest opportunities are not about adding more technology. They are about making the site smaller: one page, one clear message, no navigation, no unused content model, no third-party runtime dependencies, and only enough build tooling to keep the site maintainable.

The recommended direction is:

- Keep the site static.
- Collapse the site to a single home page.
- Remove subpages, navigation, and the experience list.
- Remove third-party runtime dependencies where possible.
- Prefer system fonts and plain HTML/CSS.
- Improve accessibility and document structure.
- Keep build tooling and checks lightweight.
- Delete unused files and styles as the structure gets smaller.

## Slow-Web Assessment

The site is already close to the slow-web ideal:

- It uses a static-site architecture.
- It does not depend on a client-side JavaScript framework.
- It has no image-heavy design.
- It loads very little content.
- It prioritizes text over interaction.

The current slow-web mismatches are:

- Google Analytics adds third-party tracking JavaScript.
- Google Fonts adds third-party network requests for typography.
- The mobile hamburger interaction adds complexity that is unnecessary for a one-page site.
- The About and Contact pages duplicate what can fit on the home page.
- The job-history data model is heavier than the simplified content needs.
- Build and validation expectations are not encoded in the repository.

From a slow-web standpoint, the best improvement is to make the site more boring in the right ways: less external runtime behavior, fewer moving parts, stronger semantics, and clearer maintainability.

## Technology Recommendations

### 1. Remove Google Analytics Or Replace It With Privacy-Respecting Analytics

The current layout loads Google Tag Manager:

```html
<script async src="https://www.googletagmanager.com/gtag/js?id=UA-139208262-1"></script>
```

This is the single largest mismatch with the slow-web initiative. It introduces third-party JavaScript, tracking behavior, privacy concerns, and an external runtime dependency.

Recommended options:

- Remove analytics entirely.
- Use server or CDN logs if only basic traffic insight is needed.
- Use a privacy-focused analytics tool such as Plausible, GoatCounter, or Umami.
- If analytics stay, document why they are needed and load them in the least invasive way possible.

Professional signal: a deliberate privacy and performance choice is stronger than using default tracking.

### 2. Remove Google Fonts

The site already defines a system font stack in SCSS:

```scss
$bodyFont: Arial,-apple-system,BlinkMacSystemFont,Helvetica,Arial,sans-serif,"Apple Color Emoji","Segoe UI Emoji","Segoe UI Symbol";
```

Because of that, loading Roboto from Google Fonts is unnecessary. Removing it improves:

- Privacy.
- Page speed.
- Offline resilience.
- Visual consistency with the user's operating system.
- Slow-web alignment.

Recommended action:

- Remove the Google Fonts `preconnect` links.
- Remove the Google Fonts stylesheet link.
- Continue using the system font stack.

### 3. Keep Jekyll Only If It Stays Useful

Jekyll is acceptable for this site because it already fits GitHub Pages and keeps hosting simple. A one-page site does not need React, Next.js, Astro, or a complex JavaScript pipeline.

The build should be reproducible if Jekyll remains. If the site becomes plain HTML with no templating benefit, removing Jekyll entirely is also a reasonable simplification.

Recommended action:

- Keep a `Gemfile` only if Jekyll remains.
- Pin the Jekyll or GitHub Pages dependency only if local builds matter.
- Add a short `README.md` with the actual local workflow.
- Add CI only if the repository benefits from automated pull-request checks.

Professional signal: the stack is intentionally small, and the tooling matches the site's real complexity.

### 4. Avoid Adding A JavaScript Framework

There is no current need for a frontend framework. Adding one would increase complexity without improving the site experience.

Recommended principle:

- Use HTML for structure.
- Use CSS for layout and presentation.
- Use JavaScript only when interaction cannot be expressed cleanly otherwise.

For this website, plain static HTML and CSS are the professional choice.

## Accessibility Recommendations

### 1. Fix Page Landmarks

The default layout currently uses:

```html
<article class="content">{{ content }}</article>
```

For the main page content, prefer:

```html
<main id="main" class="content">{{ content }}</main>
```

This gives assistive technologies a clear main landmark.

Recommended action:

- Replace the content wrapper with `<main>`.
- Add a skip link before the header:

```html
<a class="skip-link" href="#main">Skip to content</a>
```

### 2. Remove Navigation

The simplified site should have no subpages, so it should also have no navigation. The current hamburger menu, horizontal menu, navigation data file, and active-link behavior all become unnecessary.

Recommended action:

- Remove `about.html` and `contact.html`.
- Remove `_includes/navigation.html`.
- Remove `_data/navigation.yml`.
- Remove the navigation wrapper from the default layout.
- Remove hamburger and navigation styles from `_sass/styles.scss`.

This is better than making the hamburger accessible because the best accessible control is the one the site no longer needs.

### 3. Use One Clear Page Structure

The home page should carry the complete public message. It can have one page-level `h1`, a short description, and a small set of public profile links.

Recommended structure:

- Use one `h1` for the page.
- Use short paragraphs or a compact list for the remaining content.
- Use LinkedIn and GitHub (`https://github.com/kamilbozek`) as the public contact/profile links.
- Remove the email address from public page content.
- Do not keep the detailed experience section unless it serves the page's current purpose.

### 4. Harden External Links

Several links use:

```html
target="_blank"
```

When opening a new tab, links should include:

```html
rel="noopener noreferrer"
```

Recommended action:

- Add `rel="noopener noreferrer"` to any remaining external links that open in a new tab.
- Consider removing `target="_blank"` entirely unless opening a new tab is intentional.

## HTML And SEO Recommendations

### 1. Move Scripts Inside The Document Head Or Remove Them

The Google Analytics script appears before the `<head>` element. That is invalid document structure.

Recommended action:

- Remove analytics entirely, or move any remaining metadata/script into the `<head>`.

### 2. Improve Metadata

Current metadata is minimal. The site should include complete metadata without becoming bloated.

Recommended additions:

- Canonical URL.
- Open Graph image if a lightweight image is added later.
- `og:type`.
- Twitter card metadata if desired.
- More descriptive `description`.
- `theme-color`.
- Favicon metadata.

The content does not need to change, but the metadata should describe the site more clearly.

### 3. Add Structured Data

A personal website can benefit from simple `Person` JSON-LD structured data.

Recommended fields:

- Name.
- URL.
- Job title.
- Same-as links for LinkedIn and GitHub (`https://github.com/kamilbozek`).

This helps search engines understand the page without changing visible content.

### 4. Replace Manual Sitemap With Generated Sitemap

The repository has a manually maintained `sitemap.txt`.

Recommended action:

- Use `jekyll-sitemap`.
- Generate a standard XML sitemap.
- Reference the HTTPS sitemap URL from `robots.txt`.

Current issue:

```txt
Sitemap: http://www.kamilbozek.com/sitemap.txt
```

Recommended direction:

```txt
Sitemap: https://www.kamilbozek.com/sitemap.xml
```

## CSS And Design Recommendations

### 1. Keep The Minimal Visual System

The current visual style is minimal and content-first. That is aligned with the slow-web initiative.

Recommended approach:

- Keep the restrained color palette.
- Keep typography simple.
- Avoid animation-heavy presentation.
- Avoid large hero images or decorative media.

### 2. Improve Focus Styles

Keyboard users need visible focus indicators.

Recommended action:

- Add explicit `:focus-visible` styles for remaining links.
- Ensure focus contrast is strong enough.

Example direction:

```scss
a:focus-visible {
  outline: 2px solid currentColor;
  outline-offset: 3px;
}
```

### 3. Delete Interaction Styles

Once the hamburger menu is removed, its animation and layout styles should be deleted instead of preserved. This keeps the stylesheet aligned with the smaller HTML structure.

Recommended action:

- Remove hamburger menu selectors.
- Remove navigation list selectors.
- Remove any transition styles that only supported menu animation.
- Keep a simple `:focus-visible` style for remaining links.

### 4. Remove Dead Layout Rules

The current CSS has layout rules for header/navigation/content/footer grid areas. A one-page site does not need all of them.

Recommended action:

- Keep only the layout needed for the header, main content, and footer.
- Delete styles for removed classes, IDs, and data-driven job listings.
- Prefer a small linear layout over a grid if the page no longer needs two-column placement.

This is the main simplification win: the stylesheet should shrink with the site.

## Software Engineering Recommendations

### 1. Add A README

The repository should explain:

- What the site is.
- How to run it locally.
- How to build it.
- How deployment works.
- What quality checks are expected.

This is a simple but strong professional signal.

### 2. Add CI

Recommended CI checks:

- Build the Jekyll site.
- Validate HTML.
- Check internal links.
- Run an accessibility smoke test.
- Optionally run a CSS linter.

Good lightweight tools:

- `bundle exec jekyll build`
- `html-proofer`
- `pa11y-ci`
- `stylelint`

The CI should stay small. The goal is confidence, not ceremony.

### 3. Keep Generated Files Out Of Source Control

The repository appears to ignore `_site`, which is good. Continue treating generated output as build artifacts.

Recommended action:

- Keep `_site` ignored.
- Keep `.jekyll-cache` ignored.
- Do not commit generated CSS maps unless there is a specific need.

### 4. Add Formatting Expectations

Recommended additions:

- `.editorconfig`
- A simple formatting convention in `README.md`
- Optional Prettier for HTML, YAML, and Markdown if it does not create noisy output

Professional signal: contributors and future-you know what consistency means.

### 5. Remove Unused Content Data

The job history currently lives in `_data/jobs.yml`, but the simplified site should not include the experience list. That data file should be removed once no template reads it.

Recommended action:

- Delete `_data/jobs.yml`.
- Delete `.jobs`, `.jobInfo`, and `.jobRole` styles.
- Do not introduce a CMS or heavier content layer.

## Priority Plan

### Phase 1: Collapse To One Page

- Move the useful public profile links onto `index.html`.
- Remove the email address from public page content.
- Remove the detailed experience list.
- Delete `about.html` and `contact.html`.
- Remove navigation from the layout.
- Delete `_includes/navigation.html` and `_data/navigation.yml`.

### Phase 2: Remove Runtime And Markup Debt

- Remove Google Analytics.
- Remove Google Fonts.
- Fix invalid HTML document structure.
- Change the content wrapper from `<article>` to `<main>`.
- Add a skip link if there is enough page structure to justify it.
- Add `rel="noopener noreferrer"` to any remaining external new-tab links.
- Update `robots.txt` to use HTTPS.

### Phase 3: Delete Unused CSS And Data

- Remove hamburger/menu styles.
- Remove navigation layout styles.
- Remove job-list styles.
- Remove `_data/jobs.yml`.
- Prefer a simple single-column layout if the grid is no longer needed.

### Phase 4: Keep Minimal Engineering Support

- Add a `Gemfile` only if local reproducible builds are worth the dependency.
- Add a short `README.md` with local setup and build commands.
- Add CI build validation only if the repository will continue accepting changes through pull requests.
- Generate sitemap automatically, or remove sitemap machinery if the hosting platform already handles the one-page site adequately.

### Phase 5: Small Polish

- Add canonical metadata.
- Improve the site description.
- Add simple `Person` structured data.
- Add visible focus styles.
- Skip favicon/Open Graph image work unless actual assets are added.

## What Not To Do

Avoid these upgrades unless there is a clear new requirement:

- Do not migrate to React, Next.js, or another frontend framework.
- Do not add animation-heavy interactions.
- Do not add a CMS.
- Do not add a large design system.
- Do not add image-heavy presentation just to look more modern.
- Do not add build complexity that does not improve correctness or maintainability.

For this site, professionalism comes from restraint, clarity, accessibility, reproducibility, and respect for users.

## Final Recommendation

The best version of this website is a deliberately small single-page static site with excellent semantics, no navigation, no unnecessary tracking, no unused content model, and only the build tooling that earns its keep.

That direction strongly supports the slow-web initiative and presents you as a professional developer who understands engineering tradeoffs instead of chasing trends.
