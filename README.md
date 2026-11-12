# My Profile App - Unit 7 Exercise

The app adds a My Shelf page to the Unit 6 profile app. The shelf has three
tabs: My Picks (recommendations from Contentful), Books (Open Library search),
and Movies (OMDb search). The CMS stores each recommendation's title, kind,
external API ID, and note; the details screen fetches the current cover, year,
and summary from the appropriate public API.

## Configure and run

The exercise uses Contentful, Open Library, and OMDb. Open Library needs no key.
Create a Contentful delivery API token and an OMDb API key. Keep these values
in the ignored local `cms.json`, not in source files or commits:

```json
{
  "CMS": "contentful",
  "CMS_SPACE": "your-space-id",
  "CMS_TOKEN": "your-content-delivery-token",
  "OMDB_KEY": "your-omdb-key"
}
```

In Contentful, publish a content type with API ID `pick` and fields `title`,
`kind`, `externalId`, and `note`. Add at least two book picks and two movie
picks. Use `book` or `movie` for `kind`, the copied Open Library work ID or
IMDb ID for `externalId`, and your recommendation in `note`.

Run the app from this directory:

```powershell
flutter pub get
flutter run --dart-define-from-file=cms.json
```

On the Books or Movies tab, search with at least three characters. Results
load after typing pauses for half a second. Use the copy button on a result to
copy its external ID for a Contentful pick.

## Deploy to GitHub Pages

Set the repository's Pages source to **GitHub Actions**. Under **Settings >
Secrets and variables > Actions**, add the `CMS_URL` variable with the full
Contentful entries endpoint, for example
`https://cdn.contentful.com/spaces/<space-id>/environments/master/entries`.
Add `CMS_TOKEN` and `OMDB_KEY` as Actions secrets. The workflow builds for the
repository's GitHub Pages path and deploys after tests pass.

The local `CMS_SPACE` configuration remains supported; CI uses `CMS_URL`.
During setup, the workflow initially supplied `CMS_SPACE` instead of the
requested `CMS_URL`, so the Contentful client now accepts either form. Network
covers and posters use Flutter's HTML-image fallback on web for servers with
restrictive CORS policies.

Build a phone-installable Android release with:

```powershell
flutter build apk --release --dart-define-from-file=cms.json
```

The APK is written to `build/app/outputs/flutter-apk/app-release.apk`.

## Is the OMDb key secret?

An OMDb key included at build time can be extracted from the downloaded web
JavaScript and used to consume the key owner's API quota. A GitHub Actions
secret prevents the key from being committed, but does not hide it in a
client-side app. Rotate the key if it is exposed, and protect future keys by
calling OMDb through a server-side proxy or by using a provider with safe
client-side restrictions.

## Tests

```powershell
flutter test
flutter analyze
```
