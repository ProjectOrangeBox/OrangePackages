# OrangePackages

A static [Composer repository](https://getcomposer.org/doc/05-repositories.md#composer) that
indexes every public `orange/*` PHP package, generated with
[composer/satis](https://github.com/composer/satis). It's published to GitHub Pages by
[`.github/workflows/build.yml`](.github/workflows/build.yml) on every push to `main` and once a
day on a schedule, so newly pushed tags/branches in the tracked repos show up automatically.

## Using it in a project

Instead of adding a `{"type": "git", "url": "..."}` entry per package, add this single entry to
`composer.json`:

```json
"repositories": [
    {
        "type": "composer",
        "url": "https://projectorangebox.github.io/OrangePackages/"
    }
]
```

Then `require` any indexed package normally — no per-package repository entry needed:

```json
"require": {
    "orange/framework": "dev-main",
    "orange/cache": "dev-main"
}
```

## Adding a package to the index

Add a `vcs` entry to [`satis.json`](satis.json) with `"no-api": true` (avoids GitHub API rate
limits by cloning over plain git instead):

```json
{ "type": "vcs", "url": "https://github.com/ProjectOrangeBox/<repo>.git", "no-api": true }
```

Push to `main` (or wait for the daily schedule) and the index rebuilds automatically.

Only add **public** repos here — `satis.json` and the published index are both public, so listing
a private repo would expose its name and version history even though cloning it would still
require authentication.

## Building locally

```bash
composer install
php vendor/bin/satis build satis.json build
```

`build/` is the generated static site (gitignored) — serve it with any web server to test, e.g.
`php -S 127.0.0.1:8000 -t build`.
