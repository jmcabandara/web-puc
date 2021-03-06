Web Package Update Checker [![Build Status](https://travis-ci.org/fulldecent/web-puc.svg?branch=master)](https://travis-ci.org/fulldecent/web-puc)
=======

We find old Bootstrap, Font Awesome, JQuery, etc. references in your web projects and tell you to update them.

Use `web-puc` in your unit testing or continuous integration testing to confirm that you are not using outdated packages. We support packages that are included via CDN as well as mix-ins and Javascript/CSS/Fonts that are subsetted or compiled into your own files! We support ANY web front-end including HTML, PHP, Ruby, ASP, Cold Fusion.

For a full example of using `web-puc` with Travis CI for integration testing, please see https://github.com/fulldecent/sourcespeak and <a href="https://github.com/fulldecent/sourcespeak/blob/master/.travis.yml">its `.travis.yml` file</a>.


Synopsis
========

`web-puc [-e glob] [-s] [-u] target ...`

**The following options are available:**

| Option              | Short      | Description                                         |
| ------------------- | ---------- | ------------                                        |
| `--exclude GLOB`    | `-e GLOB`  | Exclude from consideration all files matching GLOB  |
| `--allow-supported` | `-s`       | Allow supported versions even if not latest         |
| `--update`          | `-u`       | Update web package database                         |
| `--stat`            |            | Use [STAT standardized output format](https://github.com/fulldecent/structured-acceptance-test) |

**Return status is zero for success and non-zero if errors are found.**

**Output follows [STAT format](https://github.com/fulldecent/structured-acceptance-test):**

```json
{
  "statVersion": "1.0.0",
  "process": {
    "name": "web-puc",
    "version": "0.0.2",
  },
  "findings": [
    {
      "failure": true,
      "rule": "Old version",
      "description": "//netdna.bootstrapcdn.com/font-awesome/4.0.3/css/font-awesome.min.css",
      "location": {
        "path":"index2.php",
        "beginLine": 13,
        "endLine": 13
      },
      "recommendation": "//netdna.bootstrapcdn.com/font-awesome/4.1.0/css/font-awesome.min.css"
    }
  ]
}
```


Ruby
====

Or access this tool using the `web-puc` Ruby gem.

Project layout
==============

 - `web-puc` - the main executable
 - `packages/` - database of package information
 - `package-spiders/` - tools for updating the `packages/` directory
 - `tests/` - unit tests


Package recognition
===================

We recognize packages by three types of matching:

 - **CDN**: is an inclusion (like `<script src="...">` or `<link href="...">`) to a file on a popular content distribution network. For example, when this was written, `//netdna.bootstrapcdn.com/bootstrap/3.0.0/css/bootstrap.min.css` was an old version, but `//netdna.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css` was current

 - **Identifier**: is a string like `Bootstrap v3.0.0 (http://getbootstrap.com)` which would indicate a file in your project contains specific code (`web-puc` gleans information from your files' contents, not names)

 - **Content Matcher**: is a string like `.form-control{position:relative;z-index:2;float:left;` that would indicate, for example, that are using Twitter Bootstrap 3.1.1 even if you later minify and package all your CSS files together


Project roadmap
===============

Please Star (or Watch!) this project for updates and to get involved. Following is a roadmap:

 * Create more package-updaters for all MaxCDN packages and JQuery
 * Store `packages/` and `package-spiders/` in a separate repository and have `web-puc` use `git pull` to update
 * Have `web-put.sh` warn if `packages/` was updated too long ago (like Composer)
 * Advertise the availability of this project
