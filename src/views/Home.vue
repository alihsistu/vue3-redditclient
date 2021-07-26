<template>
   <SubReddit :postsState="postsState" :posts="posts" />
</template>

<script>
import usePosts from '@/hooks/usePosts';
import SubReddit from '@/components/SubReddit.vue';
import { computed } from 'vue';

export default {
  components: { SubReddit },
  setup() {
    const postsState = usePosts('aww');

    const posts = computed(() => postsState.data.map((child) => child.data));

    return {
      postsState,
      posts,
    };
  },
};
</script>
