<script lang="jsx">
import RedditPost from '@/components/RedditPost.vue';
import { computed } from 'vue';
import usePosts from '@/hooks/usePosts';

export default {
  props: {
    name: { type: String },
  },
  // eslint-disable-next-line vue/no-setup-props-destructure
  setup(props) {
    console.log(props.name, 'props');
    const postsState = usePosts(props.name);
    //  TODO: filter out nsfw posts
    const posts = computed(() => postsState.data.map((child) => child.data));
    console.log(posts, 'posts');
    return {
      postsState,
      posts,
    };
  },
  render() {
    console.log(this);
    const { postsState, posts } = this;
    return (
      <div class="subreddit">
          {postsState.loading && (
            <div class="progress">
              <div class="indeterminate"></div>
            </div>
          )}
          {postsState.error && (
            <div class="card red accent-1">
              <div class="card-content white-text">
                  <span class="card-title">{ postsState.error }</span>
              </div>
            </div>
          )}
          <div class="row">
          {posts.map((post) => (
            <RedditPost key={post.id} post={post} />
          ))}
          </div>
      </div>
    );
  },
};
</script>
