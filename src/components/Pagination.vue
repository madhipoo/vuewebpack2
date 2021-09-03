<template>
    <nav aria-label="Page navigation example" class="row justify-content-center">
            <ul class="pagination">
                <li class="page-item" :class="{'disabled':!pages.has_pre}">
                <a class="page-link" href="#" aria-label="Previous" @click="updatePage(pages.current_page - 1)">
                    <span aria-hidden="true">&laquo;</span>
                </a>
                </li>
                <li class="page-item"  v-for="page in pages.total_pages" :key="page"
                :class="{'active':pages.current_page === page}">
                    <a class="page-link" href="#" @click="updatePage(page)">{{page}}</a>
                </li>
                
                <li class="page-item" :class="{'disabled':!pages.has_next}">
                <a class="page-link" href="#" aria-label="Next" @click="updatePage(pages.current_page + 1)">
                    <span aria-hidden="true">&raquo;</span>
                </a>
                </li>
            </ul>
    </nav>
</template>

<script>
export default {
  name: 'Layout',
  props: ['pages'],
  data() {
    return {
      msg: 'Welcome to Your Vue.js App',
      uid: '',
    };
  },
  methods: {
    updatePage(page) {
      this.$emit('emitPages', page);
    },
  },
};
</script>