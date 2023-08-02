<template>
  <div>
    <h1>Blog Posts</h1>
    <div>
      <p>Available Tags:</p>
      <p>tech, health, startups, science, history, design, culture, politics</p>
    </div>
    <div>
      <label>
        Tag:
        <input
          type="text"
          v-model="tag"
          @input="fetchPosts"
          placeholder="Enter a tag from above to fetch blog posts"
        />
      </label>
    </div>
    <div>
      <label>
        Sort By:
        <select v-model="sortBy" @change="fetchPosts">
          <option value="id">ID</option>
          <option value="likes">Likes</option>
          <option value="reads">Reads</option>
          <option value="popularity">Popularity</option>
        </select>
      </label>
    </div>
    <div>
      <label>
        Direction:
        <select v-model="direction" @change="fetchPosts">
          <option value="asc">Ascending</option>
          <option value="desc">Descending</option>
        </select>
      </label>
    </div>
    <div v-for="(post, index) in posts" :key="post.id">
      <blog-post :post="post" />
      <hr v-if="index !== posts.length - 1" />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import BlogPost from "./components/BlogPost.vue";

export default {
  components: {
    BlogPost,
  },
  data() {
    return {
      posts: [],
      tag: "tech", // Default tag value
      sortBy: "reads", // Default sortBy value
      direction: "desc", // Default direction value
    };
  },
  watch: {
    tag: "fetchPosts",
    sortBy: "fetchPosts",
    direction: "fetchPosts",
  },
  methods: {
    async fetchPosts() {
      try {
        const response = await axios.get(
          "https://api.hatchways.io/assessment/blog/posts",
          {
            params: {
              tag: this.tag,
              sortBy: this.sortBy,
              direction: this.direction,
            },
          }
        );

        // Sorting logic here...
        const sortedPosts = response.data.posts.sort((a, b) => {
          if (this.sortBy === "id") {
            return this.direction === "asc" ? a.id - b.id : b.id - a.id;
          } else if (this.sortBy === "likes") {
            return this.direction === "asc"
              ? a.likes - b.likes
              : b.likes - a.likes;
          } else if (this.sortBy === "reads") {
            return this.direction === "asc"
              ? a.reads - b.reads
              : b.reads - a.reads;
          } else if (this.sortBy === "popularity") {
            return this.direction === "asc"
              ? a.popularity - b.popularity
              : b.popularity - a.popularity;
          }
          return 0;
        });

        this.posts = sortedPosts;
      } catch (error) {
        console.error("Error fetching posts:", error);
      }
    },
  },
  created() {
    this.fetchPosts();
  },
};
</script>
