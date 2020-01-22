---
layout: post
title:      "Project: SPA with JS frontend and Rails API backend!"
date:       2020-01-22 19:58:18 +0000
permalink:  project_spa_with_js_frontend_and_rails_api_backend
---


The application I chose to develop was a fairly simple snippet/note diary: Sik Snippets! I personally use Microsoft's OneNote for saving all my technical notes/snippets as it allows you to orgnanise your notes relatively well and the search functionality is pretty decent. Even though it is satisfying my needs (mostly) for now, I have looked for alternatives several times but never really found an application that was good enough to switch over from OneNote.

I thought that coding up my own snippet diary would be a pretty neat idea, given the project requirements, and even though I knew I wouldn't get all the way there due to time constraints, I would at least be able to make a good start on a project I know I'll be keen to finish as soon as I have the time!

At the time of submission, this is the functionality I have managed to implement:

* Sign up/log in => session created
* User can create one or more snippet categories
* User can create one or more snippets belonging to a snippet category
* Logout => session destroyed

In the future I would like to:

* Improve the overall design of the app
* Persist sessions in local storage until destroyed
* Make snippet category and snippet titles unique only within user scope
* Implement simple search functionality
* Pagination of results
* Improve editor experience i.e. line numbering, better formatting when writting snippet out (instead of pasting in)
* Refactor ALOT - I need DRY up my code, streamline some of the logic, make sure behaviours are grouped in the most logical way, and make sure my appState object is robust.
* Serialise my JSON responses
* Experiement with a light/dark mode
* Better error handling
* Validation
* Testing

The way I chose to implement authentication is by utilising Rails' native session cookies. This is not typically offered when you use the api flag to construct a rails app, however, you can regain this feature by going to your application.rb file and underneath `config.api_only = true` add:

```
config.middleware.use ActionDispatch::Cookies
config.middleware.use ActionDispatch::Session::CookieStore
```

In order for this to work over localhost, you also need to configure the `session_store.rb` file where you can dynamically assign an initialiser based on the environment i.e. development or production (or even test). This can be achieved by simply creating a conditional structure checking for the rails environment and for development you want to remove the domain property so that the browser does not expect a cookie to be set from a domain.

I have three models:

1. User
2. SnippetCategory
3. Snippet

I have a controller for each of these as well as a sessions controller.

A User has many snippet_categories, a snippet_category belongs to an owner and its :owner attribute is the foreign key that link the two. A snippet belongs to a snippet_category.

My controllers' setup is pretty standard. I've chosen to employ some concerns to DRY up my code. These are:

1. json_response - allows me to pass parameters to the method that formats and renders an appropriate JSON response.
2. excpetion_handler - formats and renders an appropriate JSON response if ActiveRecord throws an exception.
3. current_user - sets the current user before each action in the session controller by checking for a session and setting 

As well as a controller for the snippet_categories and snippets, I have one for users and sessions. The users controller handles the creation of a user and creates a session if successful.

My routes are namespaced under api and v1 to reflect the convention where you containerise your api logic, so that you can have multiple versions that may work slightly differently to serve different needs. I have a login and logout route, as well as as a nested route structure => snippets, nested under snippet_categories, nested under users.

In my frontend, I have a snippetAdapter and snippetCategoryAdapter that make calls to the API. In future, I would like to refactor my code so that all API calls are performed from by the adapters but under the tight timeline, my code was a little loose in that regard.

My components consisted of an app, snippet, snippetCategory and user class.

The app object had a state object property as well as a base url property so that I could refer to it in my methods that involved a lot of logic based on state.

The rest of the classes are responsible for initialising the relevant HTML and event bindings and rendering themselves when called, as well as for handling other functionality like saving or deletion.

My index.js file essentially instantiates the app and handles a lot of the logic relating to the events within the landing page and authentication.

In terms of the UX, an unauthenticated user arrives at a landing page where they can choose to log in or sign up. Once they do this, I set the div containing everything below the header to `display: none;` and the User class that gets instantiated renders the view in place of the landing page. The diary is closed to begin with - once you click on the diary icon, your snippet categories are displayed in a column (these are retrieved when the user instance is initialised).

The snippets are only loaded when you select a snippet category (rather than loading all snippet categories and snippets upfront) in order to optimise performance. Only when you have selected a snippet category and snippet can you interact with the editor, and you can now save the snippet content that get's assigned to the snippet you created.

**CHALLENGES**

I'll list some challenges I faced here. Some of these points I resolved but do not fully understand why there was an issue to begin with.

1. When saving the snippet content (by sending a "PUT" request and assigning the value within the editor to the value of my snippet "body" attribute) I kept getting errors when using strong params. For some reason, Rails is sending more parameters with my request than I have specified and I didn't have time to dig into this more deeply. I simply worked around the issue by manually assigning the parameters in the update action.
2. Using vanilla javascript was a bit tricky for some of the UI elements where I might need to show or hide some or all of the snippet diary's columns and/or contents depending on the app's state. Using a frontend library would make this work a lot easier but it was fun to implement nonetheless. This essentially forced me to keep a state object which allowed me to write some fairly complex logic that would have been far more difficult or impossible to achieve by relying only on passing parameters.
3. The method I employed for authentication was very simple but it took a lot of time to find a method which I felt I could confidently pull off in the allocated time, and which did not involve copying lots of boiler-plate code I would not have time to fully digest and understand. JWT was an alternative I considered and would like to get some experience with in future.


I definitely enjoyed the challenge this project presented and it taught me a lot about project scope, getting your ideas organised, getting your MVP in on time, and maintaining resilience and some kind of mental clarity when you're struggling the most. Unfortunately, due to sickness and learning of the death of someone I knew threw me off at the beginning of project week. I then spent Wed through Friday trying to build my API using TDD but I was getting some frustrating errors that I did not have time to debug and so on Friday evening I essentially scrapped the idea of TDD and restarted almost from scratch. Needless to say my submission was overdue, however, I'm happy with the result so far and I can't wait to get back to it and develop it further into an app that I'd be happy to use... and if anyone else gets some use out of it, well that's be a great bonus ;)


