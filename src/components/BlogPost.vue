<template>
  <div>
    <h2>{{ post.title }}</h2>
    <p>Author: {{ post.author }}</p>
    <p>Likes: {{ post.likes }}</p>
    <p>Popularity: {{ post.popularity }}</p>
    <p>Reads: {{ post.reads }}</p>
    <p>Tags: {{ post.tags.join(", ") }}</p>
  </div>
</template>

<script>
export default {
  props: {
    post: {
      type: Object,
      required: true,
    },
  },
};
</script>
