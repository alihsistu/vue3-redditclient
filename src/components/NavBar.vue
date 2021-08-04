<template>
   <nav>
      <div class="nav-wrapper">
         <form>
            <div class="input-field">
               <input
                  v-model="searchTerm"
                  ref="subreddit"
                  id="subreddit"
                  class="autocomplete"
                  type="search"
                  required
               />

               <label class="label-icon" for="search"
                  ><i class="material-icons">search</i></label
               >
               <i class="material-icons">close</i>
            </div>
         </form>
      </div>
   </nav>
</template>

<script>
import { ref, onMounted, watch } from 'vue';
import state from '@/store/diy';
import API from '@/lib/API';

// https://www.reddit.com/search.json?q=javascript?type=sr
export default {
  name: 'NavBar',
  setup() {
    const searchTerm = ref('');
    const subreddit = ref(null);

    // TODO: show loading state in autocomplte dropdown

    onMounted(() => {
      // eslint-disable-next-line no-undef
      const instance = M.Autocomplete.init(subreddit.value, {
        data: {},
        onAutocomplete(result) {
          console.log(result);
          state.subreddit.value = `r/${result}`;
        },
      });
      instance.onAutocomplete = (result) => {
        console.log(result);
      };
      //   console.log(instance);
      //   instance.updatData();
      async function getResults() {
        if (searchTerm.value.length < 3) return;
        const response = await API.getPosts('search', {
          q: searchTerm.value,
          type: 'sr',
        });
        console.log(response);
        const results = response
          .data
          .children
          .reduce((all, child) => {
            if (!child.data.over18) {
              // eslint-disable-next-line no-param-reassign
              all[child.data.display_name] = null;
            }
            return all;
          }, {});
        instance.updateData(results);
        instance.open();
      }

      let debounceTimeout;
      watch(() => searchTerm.value, () => {
        clearTimeout(debounceTimeout);
        debounceTimeout = setTimeout(() => {
          getResults();
        }, 500);
      });
    });
    return {
      searchTerm,
      subreddit,
    };
  },
};
</script>
