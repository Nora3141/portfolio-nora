---
title: Backend Design & Implementation (assignment 4)
layout: doc
---

# Backend Design & Implementation (Alpha)

## Abstract Data Models

Below are the abstract data models for each of my concepts in my A3 design. For some of them I changed slightly their state from the way I wrote it in A3.

### Posting[User, Media]

```
allPosts: set Post
postAuthor: Post -> one User
postMedia: Post -> one Media
```

### Authenticating

```
users: (One String (username), One String (password)) -> One String (user ID)
```

### Sessioning

```
currentSessionUserID: Sessioning -> String (user ID) or null
```

### Filtering

```
ALL_TAGS: set String
filters: set (one String (postID), set String (tag names))
```

### Remixing

```
remixes: one String (original post ID) -> set Post
```

### Favoriting

```
favorites: set (one User, one String (postID))
```

### Searching

```
curentAppliedQuery: String or null
foundPosts: set String (post IDs)
```

### Diagram

<img src="/../assets/images/data_model.png" alt="data model" style="margin: 20px;"/>

## Implementation of two concepts

For this assignment, I implemented the Favoriting and Filtering concepts.
<a href="https://github.com/Nora3141/61040-backend/tree/main/server/concepts">Here</a> is the link to the github page with implmentations of these concepts, and pictures of them are below.

Favoriting concept:
<img src="/../assets/images/favoritingConcept.png" alt="favoriting concept" style="margin: 20px;"/>

Filtering concept:
<img src="/../assets/images/filteringConcept.png" alt="filtering concept" style="margin: 20px;"/>

## Deployed Website Link

I did attempt deploying on Vercel but it doesn't appear to be working. This is the <a href="https://61040-project-nu.vercel.app/">link</a> I have.

My implementations run correctly locally on my machine, but on Vercel pressing any of the "submit" buttons gives an internal service error. I will work on fixing this before the beta deadline.

## Outline of RESTful Routes Design

The RESTful routes can be found at <a href="https://github.com/Nora3141/61040-backend/blob/main/server/routes.ts">this</a> github link.
