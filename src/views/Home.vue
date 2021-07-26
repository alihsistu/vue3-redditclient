<template>
   <div class="home container">
      <div v-if="postsState.loading" class="progress">
         <div class="indeterminate"></div>
      </div>
      <div v-if="postsState.error" class="card red accent-1">
         <div class="card-content white-text">
            <span class="card-title">{{ postsState.error }}</span>
         </div>
      </div>
      <div class="row">
         <div class="card col m6" v-for="post in posts" :key="post.id">
            <div
               v-if="isImage(post.url)"
               class="card-image waves-effect waves-block waves-light"
            >
               <img class="activator" :src="post.url" />
            </div>
            <video v-if="isVideo(post)" class="video" controls muted autoplay>
               <source type="video/mp4" :src="getVideoUrl(post)" />
            </video>

            <div class="card-content">
               <span class="card-title activator grey-text text-darken-4">{{
                  post.title
               }}</span>
               <p>
                  <a
                     :href="`https://reddit.com${post.permalink}`"
                     target="_blank"
                     >Comments</a
                  >
               </p>
            </div>
         </div>
      </div>
   </div>
</template>

<script>
import usePosts from '@/hooks/usePosts';
import { computed } from 'vue';

export default {
  setup() {
    const postsState = usePosts('aww');

    const posts = computed(() => postsState.data.map((child) => child.data));

    function isImage(url) {
      return url.match(/png|jpeg|jpg|webp|gif$/);
    }

    function isVideo(post) {
      return post.media || post.url.match(/mp4|gifv|mov$/);
    }

    function getVideoUrl(post) {
      if (post.media) {
        return post.media.reddit_video.fallback_url;
      }
      const parts = post.url.split('.');
      parts.pop();
      return parts.concat('mp4').join('.');
    }

    return {
      postsState,
      posts,
      isImage,
      isVideo,
      getVideoUrl,
    };
  },
};
</script>

<style scoped>
.video {
   width: 100%;
}
</style>
