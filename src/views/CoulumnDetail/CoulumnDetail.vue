<template>
  <div class="column-detail-page w-75 mx-auto container">
    <div class="row pb-5 shadow-sm p-3 mb-5 bg-white rounded">
      <div class="col-3"><img :src="column.avatar && column.avatar.url" :alt="column.title" class=" rounded-circle rounded-lg imgborder w-100"></div>
      <div class="mt-3 col-9">
        <h4>{{column.title}}</h4>
        <p>{{column.description}}</p>
      </div>
    </div>
    <posh-list :list="list"></posh-list>
  </div>
</template>

<script lang='ts'>
// 单页面数据的 title
import { defineComponent, computed, onMounted } from 'vue'
import { useStore } from 'vuex'
import PoshList from './PoshList.vue'
import { useRoute } from 'vue-router'
export default defineComponent({
  name: 'CoulumnDetail',
  components: {
    PoshList
  },
  setup () {
    const route = useRoute()
    const store = useStore()
    const currentId = route.params.id
    onMounted(() => {
      store.dispatch('fetchColumn', currentId)
      store.dispatch('fetchPosts', currentId)
    })
    const column = computed(() => store.getters.getColumnById(currentId))
    const list = computed(() => store.getters.getPortById(currentId))
    return {
      list,
      column
    }
  }
})
</script>

<style scoped>
  .imgborder{
    border: 1px solid #cccccc;
    width: 100%;
  }
</style>
