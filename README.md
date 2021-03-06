# Ducks: A ducks implementation to Graphql

TODO: Description
Based on [erikras/ducks-modular-redux](https://github.com/erikras/ducks-modular-redux)
Use with [Schema stitching](https://www.apollographql.com/docs/graphql-tools/schema-stitching.html)
and [Facebook/Dataloader](https://github.com/facebook/dataloader)

## The Proposal

### Example

```javascript
import DataLoader from 'dataloader'
import { makeExecutableSchema } from 'graphql-tools'

import Author from '../models/author'
import Post from '../models/post'

// Schema
const authorSchema = `
  type Author {
    id: Int!
    firstName: String
    lastName: String
    posts: [Post]
  }

  type Post {
    id: Int!
    title: String
    author: Author
    votes: Int
  }

  type Query {
    posts: [Post]
    author(id: Int!): Author
  }

  type Mutation {
    upvotePost (
      postId: Int!
    ): Post
  }
`

// Resolvers
const resolvers = {
  Query: {
    author,
    posts
  },
  Mutation: {
    upvotePost
  },
  Author: {
    posts: authorPosts
  },
  Post: {
    author: postAuthor
  }
}

// Logic Resolvers
export function author (_, { id }) {
  return Author.find({ id: id })
}

export function posts () {
  return Post.find()
}

export async function upvotePost (_, { postId }) {
  const post = await Post.findById(postId)
  if (!post) throw new Error(`Couldn't find post with id ${postId}`)
  return Post.findByIdAndUpdate(postId, { votes: post.votes + 1 })
}

export function authorPosts ({ id }) {
  return authorLoader.load(id)
}

export function postAuthor ({ authorId }) {
  return authorLoader.load(id)
}

// Data Loaders
export const postsLoader = new Dataloader(async (authorsIds) => {
  const authors = await Author.find({ id: { $in: authorsIds } })
  return authors
})


// Export executableSchema
export default makeExecutableSchema({
  typeDefs,
  resolvers,
})

```
### Rules

TODO ...

### Usage

TODO ...

### Example

TODO ...

### Implementation

TODO ...

Please submit any feedback via an issue or a tweet to [@sediceyerom](https://twitter.com/sediceyerom). It will be much appreciated.

Happy coding!

-- Jerome Olvera

---
