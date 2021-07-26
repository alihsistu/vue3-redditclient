<template>
   <div class="col s12">
      <div class="card post">
         <div
            v-if="isImage"
            class="card-image waves-effect waves-block waves-light"
         >
            <img class="activator" :src="post.url" />
         </div>
         <video v-if="isVideo" class="video" controls muted autoplay>
            <source type="video/mp4" :src="videoUrl" />
         </video>

         <div class="card-content">
            <span class="card-title activator grey-text text-darken-4">{{
               post.title
            }}</span>
            <p>
               <a :href="`https://reddit.com${post.permalink}`" target="_blank"
                  >Comments</a
               >
            </p>
         </div>
      </div>
   </div>
</template>

<script>
import { computed } from 'vue';

export default {
  name: 'RedditPost',
  props: { post: { type: Object } },
  setup(props) {
    const isImage = computed(() => props.post.url.match(/png|jpeg|jpg|webp|gif$/));

    const isVideo = computed(() => props.post.media || props.post.url.match(/mp4|gifv|mov$/));

    const videoUrl = computed(() => {
      if (props.post.media) {
        return props.post.media.reddit_video.fallback_url;
      }
      const parts = props.post.url.split('.');
      parts.pop();
      return parts.concat('mp4').join('.');
    });

    return {
      isImage,
      isVideo,
      videoUrl,
    };
  },
};
</script>

<style scoped>
.video {
   width: 100%;
}
</style>
