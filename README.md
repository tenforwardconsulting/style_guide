# Style Guide

## Models

## Controllers

## Views

## Tests

## Javascript
* Put `document.ready` inside of the javascript file and not the html file

## Stylesheets

## Reporting to Rollbar

### Include actionable item

If you report to rollbar manually, include an actionable item.

Bad:

    Rollbar.error "No coordinates found for #{location}."

Good:

    Rollbar.error "No coordinates found for #{location}. Now go figure out why a non-existant city was passed in from mixpanel"
