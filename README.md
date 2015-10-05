# Style Guide

## Models

### Validating association presence

For belongs\_to associations, do `validates :user_id, presence: true` instead of `validates :user, presence: true`
This helps simple form correctly display validation errors when the association isn't set.

## Controllers

## Views
### Linking to javascript
* Prefer setting the href as "#"

## Tests

### Feature Tests
Prefer hardcoding values (names, titles, strings in general) instead of having randomly generated ones.
When expecting that a page has a certain value, using random ones can create false positives and randomly failing tests

We want to test large pieces of complex functionality that hit multiple areas.
Also pages that have heavy amounts of javascript.
We shouldn't write feature tests for simple, one-click functionality. Use controller tests for those instead.

Bad:
* This doesn't actually test that it was archived instead of deleted
* This piece of functionality is so small that it would be better of as a controller test

    scenario 'archives district' do
      district = FactoryGirl.create :district, name: 'Baz School District'

      admin = FactoryGirl.create :admin
      sign_in admin

      visit districts_path
      click_on 'Delete'
      expect(page).to have_content 'District was successfully deleted.'
      expect(page).not_to have_content 'Baz School District'
    end

Good:
* This tests that the page's javascript is working correctly
* It's commented explaining what the javascript is doing

    scenario 'as an admin, I create an organization for a district', js: true do
      FactoryGirl.create :district, name: 'Baz School District', country: 'US'
      admin = FactoryGirl.create :admin

      sign_in admin
      visit organizations_path
      click_on 'Create New Organization'

      fill_in 'Name', with: 'Baz High School'
      # After choosing the district, the region select will populate with values via JS
      select 'Baz School District', from: 'organization_district_id'
      fill_in 'Address', with: '123 Fake St'
      fill_in 'City', with: 'Madison'
      select 'Wisconsin', from: 'organization_region'
      fill_in 'Zip / Postal Code', with: '53711'
      fill_in "Principal's Email", with: 'principal@example.com'

      click_on 'Create Organization'
      expect(page.current_path).to eq organizations_path
      expect(page).to have_content 'Organization was successfully created.'
      expect(page).to have_content 'Baz High School'
    end


## Javascript
* Put `document.ready` inside of the javascript file and not the html file

## Stylesheets
* Alphabetized
* Whenever you need to use `!important` there should be a comment explaining why.

## Reporting to Rollbar

### Include actionable item

If you report to rollbar manually, include an actionable item.

Bad:

    Rollbar.error "No coordinates found for #{location}."

Good:

    Rollbar.error "No coordinates found for #{location}. Now go figure out why a non-existant city was passed in from mixpanel"
