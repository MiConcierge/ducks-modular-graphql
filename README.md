# Ducks: A ducks implementation to Graphql

TODO: Description
Use with Schema stitching

## The Proposal

### Example

```javascript
import DataLoader from 'dataloader'

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

// Action Creators
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
