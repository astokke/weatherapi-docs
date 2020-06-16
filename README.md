
# MET WeatherAPI documentation documentation

This repo contains (at some point in the future) most of the docs on
api.met.no which are not autogenerated from module POD or metadata.

The content is designed primarily to be shown at
[https://api.met.no/doc/](https://api.met.no/doc/),
but is also visible for debugging purposes at
[https://metno.github.io/weatherapi-docs/doc/locationforecast/HowTO](https://metno.github.io/weatherapi-docs/doc/locationforecast/HowTO)

It is also possible to add documents intended for export
to the [Yr Developer Portal](https://developer.yr.no/).

## Structure

The files are organized as illustrated:

```
    weatherapi-docs/
    ├── docs
    │   ├── doc
    │   │   └── locationforecast
    │   ├── _layouts
    │   ├── _posts
    │   ├── schemas
    │   └── _site
    └── README.md
```

Due to GitHub limitations, all files for GitHub Pages must reside in a `docs` directory.
Under this is a typical Jekyll structure, with blog articles in `_posts`,
HTML templates in `_layouts` and so on. The `_site` folder is used by Jekyll
for auto-generating HTML and is excluded in `.gitignore`.

API documentation shown under [https://api.met.no/doc/](/doc) on api.met.no
reside in the `doc` directory for historical reasons (yes, this means the directory
path looks really strange).

Files which are not intended to be processed as Markdown but still included
in WeatherAPI may be stored in other directories. E.g. `schemas` contain the XML
schemas used on
[schema.api.met.no](https://schema.api.met.no/schemas/).
This may be added to in the future, e.g. for microservice metadata.


## Format

Markdown must adhere to the sub/superset supported by
[Text::MultiMarkdown](https://metacpan.org/pod/Text::MultiMarkdown).

### Code blocks

For some reason, grave-defined blocks don't work in the API, use 4-space indents instead.
Also we have not implemented syntax highlighting, so there is no point in using the former.

    # this works

        {
            "foo": "bar"
        }

    # this doesn't

    ```json
    {
        "foo": "bar"
    }
    ```

### Links

Since text can be displayed under many different subdomains, all links must
be [root-relative](https://www.w3.org/TR/WD-html40-970917/htmlweb.html#h-5.1.2),
e.g.

    ... see [locationforecast](/weatherapi/locationforecast/2.0/) for info.

## Copyright stuff

All content not explicitly specified otherwise is licenced by the [NLOD
and CC BY 4.0 licences](https://api.met.no/license_data.html).

Copyright 2020 The Norwegian Meteorological Institute
Henrik Mohns Plass 1, 0371 Oslo
https://www.met.no/

Original theme copied from [Minimal](https://github.com/pages-themes/minimal)
by [orderedlist](https://github.com/orderedlist).
