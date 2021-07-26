<script lang="jsx">
import RedditPost from '@/components/RedditPost.vue';

export default {
  props: {
    postsState: { type: Object },
    posts: { type: Array },
  },
  components: {
    RedditPost,
  },
  render() {
    console.log(this);
    const { postsState, posts } = this;
    return (
      <div class="home">
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

<style>
</style>
