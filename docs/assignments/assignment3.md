---
title: Convergent Design (assignment 3)
layout: doc
---

# Convergent Design

## Pitch

Strumly is a musical social media app that's centered around community. It provides a space for music hobbyists to share the projects they’re working on, learn from resources by other creators, and find inspiration from others in the community. Whether you're the lead guitar player of a touring band, or just learning to strum your first song on your own, Strumly gives a place for everyone to learn and share the projects they are excited about.

The key features of Strumly are:

- Encouraging learning in the community by allowing users to link learning resources to posts, so they can be easily found by the post's viewers
- Encouraging collaboration between users by allowing users to build off the work of one another through “remixing” other's posts
- Allowing users to filter their feed or search for specific content to be able to easily find content that is relevant to them

## Functional Design

In this section I describe the different concepts that make up the app. Below is the dependecy diagram of the different concepts and the definition for each of the concepts. After this I discuss the concepts' synchronizations.

### Dependency Diagram

![penguin image](/../assets/images/dependencyDiagram.png)

### Concept 1: Posting[Post]

**Purpose:** allow users to share content with the community of users on the app

**Operational Principle:** after “posting” a post, all users of the app are able to see the posted content

**Actions:**

```
createPost(userID: String, post: Post)
deletePost(postID: String)
editPost(postID: String, newPost: Post)
getPostsByAuthor(userID: String), out: set Post
getPostByID(postID: String), out: one Post
```

**State:**

```
userPosts: One String (user ID) -> set Post
```

### Concept 2: Authenticating[User]

**Purpose:** allow users to login to an account to access their personalized information and control

**Operational Principle:** after a user is “authenticated”, any account-specific control or information accessed is from this user's account

**Actions:**

```
register(username: String, password: String)
authenticate(username: String, password: String), out: Boolean
logout()
getCurrentAuthenticatedUser(), out: One User or null
```

**State:**

```
currentRegisteredUser: User or null
users: (One String (username), One String (password)) -> One String (user ID)
```

### Concept 3: Sessioning

**Purpose:** allow users to stay authenticated in their account when accessing different pages

**Operational Principle:** after a user has created a session, they stay authenticated in the app (for whatever pages they are on) until they choose to end their session

**Actions:**

```
startSession(userID: String)
endSession()
```

**State:**

```
currentSessionUserID: string (user ID) or null
```

### Concept 4: Filtering

**Purpose:** allow users to find particular content through the categories it is a part of

**Operational Principle:** after a user adds a filter (by specifying a tag name), they are shown only items that have this tag associated with them

**Actions:**

```
addFilter(tag_names: set String), out: set String (post IDs)
removeFilter(tag_name: String), out: set String (post IDs)
```

**State:**

```
allTags: set String
currentAppliedTags: set Integer (indices into allTags)
filteredPosts: set String (post IDs)
```

### Concept 5: Remixing[Post]

**Purpose:** allow users to share content with the community that builds off of other, already public content

**Operational Principle:** after choosing to "remix" a post, the user will be directed to create a new post. When this remixed post is accessed, it will have the combined audio content of both the original and the second post playing simultaneously, and visually it will have the second recording's visual overriding the original.

**Actions:**

```
createRemix(original_post_ID: String, new_post: Post)
getPostRemixes(original_post_ID: String), out: set Post
```

**State:**

```
remixes: one String (original post ID) -> set Post
```

### Concept 6: Searching

**Purpose:** allow users to find specific content that they are looking for by the content's key attributes

**Operational Principle:** after a user “searches”, they are provided a list of items that are associated with that search query

**Actions:**

```
search(query: String), out: set String (post IDs)
```

**State:**

```
curentAppliedQuery: String or null
foundPosts: set String (post IDs)
```

### Concept 7: Favoriting[Post, User]

**Purpose:** allow users to indicate their appreciation of particular user-published content and be able to easily find the content again later

**Operational Principle:** after a user favorites a post, the favorited count associated with the post increases by one, and the post gets added to the user's favorited posts collection

**Actions:**

```
triggerFavoriteButton(postID: String, user: User)
checkIfFavorited(postID: String), out: One Boolean
getFavorited(userID: String), out: set Post
getFavoritedCount(postID: String), out: One Integer
```

**State:**

```
favoritedCounts: One String (postID) -> One Integer
userFavoritedPosts: One String (userID) -> set Post
```

### Synchronizations

```
sync login(username: String, password: String):
    currentUser = Authenticating.getCurrentAuthenticatedUser()
    if currentUser != null:
        throw new Error("Can't already be logged into an account to login")
    passesSecurity = Authenticating.authenticate(username, password)
    if !passesSecurity:
        throw new Error("Username/password is incorrect, cannot login")
    currentUser = Authenticating.getCurrentAuthenticatedUser()
    Sessioning.startSession(currentUser.ID)
```

```
sync logout():
    Authenticating.logout()
    Sessioning.endSession()
```

```
sync createPost(post: Post):
    currentUser = Authenticating.getCurrentAuthenticatedUser()
    if currentUser == null:
        throw new Error("Can't create a post if you are not logged in")
    Posting.createPost(currentUser.ID, post)
```

```
sync deletePost(postID: String):
    currentUser = Authenticating.getCurrentAuthenticatedUser()
    userPosts = Posting.getPostsByAuthor(currentUser.ID)
    isAuthor = False
    for post in userPosts:
        if(post.ID == postID):
            Posting.deletePost(postID)
            isAuthor = True
    if !isAuthor:
        throw new Error("Can't delete a post if you are not the author")
```

```
sync favoritePost(postID: String):
    currentUser = Authenticating.getCurrentAuthenticatedUser()
    if currentUser == null:
        throw new Error("Can't favorite a post if you are not logged in")
    Favoriting.triggerFavoriteButton(postID, currentUser)
```

```
sync filterPosts(tags: Array<String>):
    postIDs = Filtering.addFilter(tags)
    filtered = {}
    for id in postIDs:
        filtered.add(Posting.getPostByID(id))
    return filtered
```

```
sync removeFilter(tag: String):
    postIDs = Filtering.removeFilter(tag)
    new_filtered = {}
    for id in postIDs:
        new_filtered.add(Posting.getPostByID(id))
    return new_filtered
```

```
sync remixPost(originalID: String, post: Post):
    createRemix(originalID, post)
    createPost(Post)
```

```
sync search(query: String):
    postIDs = Searching.search(query)
    posts = {}
    for id in postIDs:
        posts.add(Posting.getPostByID(id))
    return posts
```

## Wireframes

The figma design file can be found [here](https://www.figma.com/proto/qOhEXrvtWFkzUVsnn1odyD/A3%3A-Convergent-Design?node-id=2-2&node-type=frame&t=Ag8JMOsVbnj0m5b1-1&scaling=scale-down&content-scaling=fixed&page-id=0%3A1&starting-point-node-id=1%3A2)

It includes some additional features, but the prioritized features to add would be:

- Users can create, delete, and view a post's specific information
- Users can create a post by remixing
- Users can filter a feed of posts by tags
- Users can favorite a post
- Users can search for a specific post by its title or author

## Design Tradeoffs

1. How much control to give users over tags

I chose to use a pre-selected set of tags that the user can choose from to add to their post. The benefits of this are that there aren't too many tags to choose from (which could overwhelm the user), and by having pre-selected tags it safely avoids cases where users could potentially add inappropriate tags to posts. This comes at a design tradeoff because it limits the freedom of users on the app in being able to express what tags they think fit a post.

2. Whether or not to include a search feature

I chose to include a search feature on its own page of the app. The benefits of including this feature are that it allows users to find specific posts more easily (by attributes like their name and author), which could be useful if for instance, a user saw a specific post and was trying to find it again or was trying to find more posts by a specific author. However, I did consider not putting it in the app since it feels very similar to the filtering feature, where users are already empowered to find relevant posts from their tags. A downside of this tradeoff is that adding this additional feature could feel like it was overcomplicating the app and could confuse the user. However, since I chose to use the more limited control over tags in the filtering concept, I decided that the search feature was necessary to allow users to find specific posts more easily.

3. How to structure remixing of posts

I chose to allow users to create "remixed posts" by combining the audio (from the original post and newly recorded audio), and overriding the visual of the orignal post with new visual data. I decided on this beacuse combining the audio could inspire users to collaboratively create music together (by for instace, adding different parts of a song), which is a main feature of my app. By overriding the visual data, we don't allow users to see all the visual data from the parts that are playing at the same time. This could be a downside if users are wanting to see the different parts. However, I chose to do this because it's a more simplistic approach in the time I have to implement it, and it also provides a more simple visual to the user so it would be easier to understand what is going on (especially on a small screen). Users can still access the other visual data by navigating to the original post. I was also thinking about how this overriding visual data could empower users to create tutorials for posts, by for instance, demonstrating visually how to play a song on an instrument while the original audio is playing in the background.
