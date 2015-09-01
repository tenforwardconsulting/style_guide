# Style Guide

## Models

### Validating association presence

For belongs\_to associations, do `validates :user_id, presence: true` instead of `validates :user, presence: true`
This helps simple form correctly display validation errors when the association isn't set.

## Controllers

## Views

## Tests

### Feature Tests
We want to test large pieces of complex functionality that hit multiple areas.
Also pages that have heavy amounts of javascript.
We shouldn't write feature tests for simple, one-click functionality. Use controller tests for those instead.

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
