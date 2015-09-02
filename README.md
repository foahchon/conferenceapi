# Conference App API

## What is it?

Extends [Udacity's conference app API](https://github.com/udacity/fullstack-nanodegree-conference) to include support for individual sessions within conferences, user wishlists, and periodically featuring a popular speaker. A live web version of this app can be found [here](http://foahchon-project4.appspot.com), or the API can be accessed directly via the API explorer [here](https://apis-explorer.appspot.com/apis-explorer/?base=https://foahchon-project4.appspot.com/_ah/api#p/).

## Additions to original project
#### Additional endpoints

The following endpoints were added to the original app's endpoints:

- `getConferenceSessions` - Gets all sessions for a conference referenced by `websafeConferenceKey`
- `getConferenceSessionsByType` - Gets all sessions across all conferences of a given type
- `getSessionBySpeaker` - Gets all sessions across all conferences given by a speaker
- `createSession` - Creates a session for a conference referenced by field `websafeConferenceKey`
- `addSessionToWishlist` - Saves a session referenced by field `websafeSessionKey` to the logged in user's wishlist
- `getSessionsInWishlist` - Gets a list of all sessions saved to a user's wishlist
- `getFeaturedSpeaker` - Gets currently featured speaker

#### The Session class

The `Session` class in `models.py` is defined as follows:

| field      | type     | description                   |
|------------|----------|-------------------------------|
| name       | string   | Name for session              |
| highlights | string   | Session description/highlights|
| speaker    | string   | Name of session's speaker     |
| duration   | integer  | Session's duration (in hours) |
| date       | date     | Date session starts           |
| time       | time     | Time session starts           |

`SessionForm`, a web-friendlier version of this class is also defined; two additional fields, `websafeConferenceKey` and `webSafeSessionKey` reference the session's parent conference and the session itself, respectively.

#### Additional queries

The following queries were added to the original app's endpoints:

- `getConferencesByTopic` - Gets all conferences about a given topic
- `getUpcomingConferences` - Retrieves up to ten upcoming conferences

## Challenge: Getting all workshops before 7:00PM

One of the requirements for this project involved retrieving a list of sessions starting before 7:00PM that did not include the "workshop" tag in its `typeOfSession` field. The challenge here is that Google Datastore only allows for one inequality filter in a single query, meaning I could either filter for sessions that started before 7:00PM, or I could filter for "non-workshop" sessions, but I could not do both at once. To overcome this limitation, I first retrieved all sessions starting before 7:00PM (as that would filter out the most sessions) and then used a Python comprehension to eliminate all "workshop" sessions in the application code. This is done with the `getNonWorkshopsBefore7PM` API endpoint defined in `conferences.py`.

## How do I run it?

1. Make sure you have downloaded and installed the [Google App Engine Python SDK](https://cloud.google.com/appengine/downloads?hl=en).
2. Go to the [Google Developers Console](https://console.developers.google.com) and create a new project.
3. Clone this repository.
4. Replace `your-project-id` in `app.yaml` with your project's ID.
5. Copy the OAuth 2.0 client ID into the approrpiate fields in the `settings.py` (the `WEB_CLIENT_ID` constant on line 15) and `static/js/app.js` (in the `CLIENT_ID` field where the `oauth2provider` factory is defined on line 89) files. You can find these client ID's under the APIs & Auth > Credentials menu item in the [Google Developers Console](https://console.developers.google.com).
6. Run Google App Engine Launcher and add your project to the list of available projects by clicking "File > Add Existing Application" and selecting the directory where this repository was cloned.
7. Click the "Run" button in the toolbar to launch the application.
8. Optionally, click the "Deploy" button in the toolbar to deploy the application to Google App Engine.

## Thanks
Thanks for checking out my project. Hope you enjoy!