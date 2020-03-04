---
layout: post
title:      "Project: React/Redux and Rails - Phonix"
date:       2020-03-04 16:01:18 +0000
permalink:  project_react_redux_and_rails_-_phonix
---

This blog-post serves as a personal reflection on my final project. It's kind of a post-submisison brain-dump before taking a well-deserved break and recharging in preparation for the job-search period which will simultaneously involve, job hunting, polishing my online presence, portfolio project refactoring/improvements, learning new things etc etc...


I knew that this project was going to be challenging given that we had to learn React and Redux together with very little time to get comfortable with either. 

My end-goal is to have a fully functional E-commerce app, focused on headphones, with a user-forum incorporated also. Obviously, this is not a feasible given the time-frame of a week and how fresh I was with both React and Redux, however I felt as though I would make some good progress and learn more than if I were to aim for a bare minimum, simple CRUD app. Even though that's what I essentially ended up with, I felt it was benficial (and also somewhat a curse, more on this later!) to have an ambitious scope in my mind.

The first feature I wanted to get working was authentication. Despite the fact that I had previously implemented this in a Vanilla JS / Rails API project, this was suprisingly difficult. Not having any previous experience with React OR Redux (I'm probably going to wear this point thin...) I became somewhat paralysed by choice and was hesitant when weighing up ways to implement everything. Knowing whether I should be storing state at the component level or in the Redux store and whether/when it was acceptable to do a mixture of both were considerations that slowed me down.

Given how much time I then wasted stubbornly trying out different implementations and designs I became somewhat frustrated by my lack of understanding of how things were working together and in order to mitigate this I adjusted my goals. This was supposed to be a learning experience after all and not a competition to finish with the most feature-rich, polished app in the world ( although that wouldn't hurt your portfolio ;) ). I was even surprised how difficult it seemed to render an image and struggled with the homepage for some time.

By the end of the week I had very little accomplished as far as functionality and realised a big mistake I had made. Having an ambitious scope, despite my experience level with the tech I was working with, led me to also aproach the project in an irresponsible way, which was to build too much complexity in, too soon. It got to a point where I was struggling with redux-form, redux-auth-wrapper, react-router (not having done enough research on the differences between V3 and V4 and wondering why some examples I was looking at were leading me astray) an unfamiliar UI framework (first Ant design then Tailwind) as well as Google OAuth2 all at once. I was truly trying to run before I had learned to walk and I paid the price!

Project scrapped and restarted! This time, I focused on getting that MVP CRUD done and focused my attention on getting my actions, reducers, and state working nicely with my API  and slowly re-introduced the complexity and most of the previous code I had written. Lesson-learned! While I can't say I'm proud of the outcome so far and I'm a long way off from my goal of turning this project into a well designed, feature-rich, performant app, I learnt a lot about React and Redux and approaching solo project work. Working solo on a project in which I can call all the shots is something I'm not used to and oddly I believe I'm far worse at doing this than within a team.
