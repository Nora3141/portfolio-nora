---
title: Backend Design & Implementation Beta (assignment 4)
layout: doc
---

# Backend Design & Implementation (Beta)

## Link to Deployed Website and Github Repo

<a href="https://61040-project-nu.vercel.app/">Deployed Website</a>

<a href="https://github.com/Nora3141/61040-backend">Backend Code Github Repo</a>

## App Definition

Here is the app definition from the concepts used in my app:

```
app Strumly

concepts
    Authenticating
    Sessioning[Authenticating.User]
    Posting[Authenticating.User]
    Favoriting[Authenticating.User, Posting.Post]
    Filtering[Posting.Post]
    Remixing[Posting.Post]
```

## Abstract Data Models

Below I describe each concept's purpose, operational principle, actions, and state.

### Posting[Post]

**Purpose:** Allow users to share content with the community of users on the app

**Operational Principle:** After “posting” a post, all users of the app are able to see the posted content

**Actions:**

- `Create Post(video URL: string, video title: string, video description: string, original artist (optional): string)` - creates a new post with the given parameters
- `Get Posts by User (username: string)` - returns all the posts by user 'username'
- `Search Posts by Title or Prefix(title: string)` - returns all posts that match the title 'title' or have it as a prefix
- `Delete Post (postID: Post.ID)` - deletes a particular post

**State:**

`posts -> set of PostDoc (stores author: User.ID, videoURL: string, video title: string, video description: string, original artist: string for a post)`

### Filtering[Post]

**Purpose:** Allow users to find particular content through the categories it is a part of

**Operational Principle:** After a user adds a filter (by specifying a tag name), they are shown only items that have this tag associated with them

**Actions:**

- `Get Tags on Post (postID: Post.ID)` - returns a list of string tags that are on a particular post
- `Add Tag to Post (postID: Post.ID, tagName: string)` - adds a tag to a particular post (if it is a verifiable tag)
- `Remove Tag from Post (postID: Post.ID, tagName: string)` - removes a specific tag from a particular post
- `Get all Posts With Tags (tagNames: string)` - returns a list of all posts where each has all of the tags in tagNames
- `Get a Random Post Filtered by Tags (tagNames: string)` - returns a random post that has all of the tags in tagNames

**State:**

- `ALL_TAGS -> set of string (storing all valid tag names)`
- `filters -> set of FilterDocs (stores a postID: Post.ID and set of integers that index into ALL_TAGS as tags)`

### Favoriting[Post, User]

**Purpose:** Allow users to indicate their appreciation of particular user-published content and be able to easily find the content again later

**Operational Principle:** After a user favorites a post, the favorited count associated with the post increases by one, and the post gets added to the user's favorited posts collection

**Actions:**

- `Get User's Favorited Posts(userID: User.ID)` - returns the posts favorited by a user
- `Toggle Favorite(postID: Post.ID, userID: User.ID)` - favorites/unfavorites a particular post
- `Get Favorite Count(postID: Post.ID)` - returns the number of favorites a post has
- `Get Trending Favorited(numToGet: number)` - returns recent posts with the most favorites

**State:**

- `favorites -> set of FavoriteDoc (stores the postID of the favorited post, and the userID of the user that favorited it)`

### Remixing[Post]

**Purpose:** Allow users to share content with the community that builds off of other, already public content

**Operational Principle:** After choosing to "remix" a post, a user will be directed to create a new post, with instructions to play the original post in the background of its recording. When the new post is created, it will have this combined audio, and from it users will be able to easily find the original post. Additionally, the "original artist" field of the new post will automatically update with the original post's "original artist", and upon viewing all the remixes of the original post, this new post will be visible to all users.

**Actions:**

- `Get Remixes On Post (postID: Post.ID)` - returns a list of the remixes a particular post has
- `Get Number of Remixes On Post (postID: Post.ID)` - returns the number of remixes a particular post has
- `Create Remix(original postID: Post.ID, video URL: string, video title: string, video description: string)` - creates a new remix of an original post
- `Get Original Post(postID: Post.ID)` - returns the original post for some remxied post, or null if it is not a remix or the original post was deleted
- `Get Trending Remixed (numPosts: number)` - returns recent posts with the most remixes

**State:**

- `remixes -> set of RemixDoc (stores an original post ID: Post.ID, and a new remixed post ID: Post.ID`

### Authenticating[User]

**Purpose:** Allow users to share content with the community of users on the app

**Operational Principle:** After a user "posts" a post, all users of the app are able to see the post's content

**Actions:**

- `Create User(username: string, password: string)` - creates a new user with the given username/password
- `Update Username (userID: User.ID, username: string)` - updates the username for a user
- `Update Password (userID: User.ID, password: string)` - updates the password for a user
- `Authenticate(username: string, password: string)` - authenticates a user by checking if the username/password is valid
- `Delete User(userID: User.ID)` - deletes a user with a specific ID
- `Get User By Username(username: string)` - returns a user with the matching username (or none)

**State:**

- `users -> set of UserDoc (stores username: string and password: string pairs)`

### Sessioning[User]

**Purpose:** Allow users to stay authenticated in their account when accessing different pages

**Operational Principle:** After a user has created a session, they stay authenticated in the app (for whatever pages they are on) until they choose to end their session

**Actions:**

- `Start Session(userID: User.ID)` - starts a session for the given user
- `Get Session User()` - returns the current user in the session
- `End Session()` - ends the session, setting there to be no current user

**State:**

- `session.user -> string`

## Data Model Diagram

![data model diagram](/../assets/images/data_model_2.png)

## Design Reflection

One of the first design issues that surfaced during implmentation was how to implement my Searching concept from A3. From my feedback on A3, I realized that instead of having Searching as it's own concept, it could actually more easily be implemented as its own function within the Posting concept (since it requires access to a post's contents). For the method of searching, I decided to search by the title of a post, allowing users to type in the full title or a prefix of the title so they will be able to find similar posts within a topic. Another design issue that surfaced was thinking about how to store posts so that they could hold video content, and also be used effectively by the remixing concept. On this decision I decided to follow advice on Discourse and upload videos to Google Drive, storing their sharable URL in the MongoDB database. This decision was tricky though because I had originally been thinking about merging the audio files from two different videos to create a remix, and after doing some research on how I could do this, I realized it would be complex to implement a solution using this storage method. However, I decided to stick with this storage method since this is a "proof of concept app" and I realized in the remixed recording, users can just play the original audio in the background as they record (which might also make more sense for them as they are recording).
