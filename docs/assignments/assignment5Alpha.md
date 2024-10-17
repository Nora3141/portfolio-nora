---
title: Frontend Design & Implementation Alpha (assignment 5)
layout: doc
---

# Frontend Design & Implementation (Alpha)

## Heuristic Evaluation

### Usability Criteria

1. Discoverability: How rapidly and easily can users understand how to operate the interface?

- The navigation bar buttons are labeled and have familiar icons, making the pages more discoverable
- I think the structure of the homepage is not especially discoverable, since I included different kinds of buttons here and varying layouts. It would be more intuitive to have a more consistent layout on this screen, with one kind of button
- The feed page has an arrow pointing up, to indicate to users to swipe up with their finger, making it more discoverable
- A lot of a post's information and its remix button in the feed is hidden away behind the (i) icon, making it not as discoverable. Similarly the filters are hidden away in the corner by another icon. Making these immediately visible is a tradeoff because it adds a lot of information to the screen that might overwhelm the user, but these features are more discoverable.

2. Safety: How does the interface guard against people making mistakes?

- The navigation bar remains visible on all pages of the app, making it more safe because users can go back to a page if they mistakenly clicked one
- The favorite button can be clicked again to unfavorite, making it more safe because users can undo this action
- I forgot to include in my wireframe a way for users to delete their own posts, which would make the app more safe. Then if users post something and later decide they want to take it down, they can delete the post

### Physical Heuristics

1. Gestalt principles: Does the layout of the interface elements convey conceptual structure?

- Similar to a note in discoverability, I think the home page could be better organized to convey conceptual structure. There are different concepts involved on this page, like the remixing button, the trending remixing posts, and the trending songs. In my A3, I had different designs for the trending remixed and trending songs, which I think should be made more similar for conceptual strucure.
- It might be more intuitive to have a different location for the "make music" button to convey that it's a different concept. However this is a tradeoff because having it at the top center of the homescreen makes it one of the most discoverable features, which I think is important for this concept.
- I think the profile page layout does a better job at conveying conceptual strucure. The username is at the top, and then there are two labled sections of posts which are layed out in the same way.
- Another page that could be improved is the feed page. I think it might make more sense for the title/author of the post to be connected to the video somehow to better convey conceptual structure

2. Situational context: How does the interface convey to a user their context (where they are, the app’s state, etc.), and how does it adapt to their context?

- The NavBar icon for the current page lights up for whichever page the user is currently on. It might also be helpful for each page to have a label at the top like "Home" or "Profile" to better convey situational context.
- A way I could improve the situational context for the "make music" screen could be to have some text displaying that the post has not yet been uploaded. Then when the user presses the "done" button and gets directed to their profile, there could be some alert saying that the post was successfully uploaded.
- Another way to better convey situational context could be on the search page to have a message below the search bar telling the user if they haven't yet entered a search. This could help better communicate to the user why there was no content showing below the search bar yet.

### Linguistic Level

1. Speak a user’s language: Does the interface use simple, helpful informative messages? are there instances where messages might only be understandable by developers?

- The wireframe uses familiar icons to help speak the user's language. I think maybe the feed icon should be changed from a music note to a video icon to better convey what is on this page
- Although I think "make some music" is speaking the user's language, I think "Create" or "Create a Post" could be a better name for this button to help users understand what it does. It could be helpful to also include a brief message below the button describing what it means for a 'post'.
- On the make some music page, I think there should be added a label to communicate what the "description" of a post should be (info about how the post is made or links to learning resources)

2. Consistency: does the interface reuse the same names, symbols, and icons for the same concepts or actions? how consistent is the interface with others across the same application domain or platform?

- From my "scrapbook of comparables" from A2, I decided on the structure/placement of my navigation bar to be more consistent with other social media apps. I also designed the structure of the feed to be more consistent with other social media apps (swiping up to get to the next post).
- The wireframes use consistent icons for accessing a post and for favoriting. I realized they use the same icon for remixing/creating which though they are similar concepts, should maybe be split into two separate icons.
- They use consistent names like "tags" and "remix"

## Implemented concept components

For my alpha submission, I implemented components for the Posting and Favoriting concepts. None of these components have been styled, but their functionality is fully implemented. Here are their included features in my alpha submission:

1. Posting

- Posts contain video, video description, title, and author
- Create post form is updated to include these fields and further formats the inputted google drive link (before storing in mongodb) to be able to load the video
- User created posts are listed on their profile
- Can search posts by their author, by their title, or a prefix of their title
- Feed page gets and displays a random post whenever the button is clicked

2. Favoriting

- Can toggle on and off favorites on posts
- Profile displays a list of the user's favorited posts
- Each post displays the number of total favorites it has
- Can only favorite posts if you are logged in

## Frontend Deploy Attempt

<a href="https://strumly-frontend-3hfd4pdl3-noras-projects-fa56bd66.vercel.app/">Here</a> is the link to the deployment attempt on Vercel.
