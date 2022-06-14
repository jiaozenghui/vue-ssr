<template>
  <Tutorial />
</template>

<script lang="ts">
import Vue from 'vue'
import { mapActions } from 'vuex'
export default Vue.extend({
  name: 'IndexPage',
  data() {
    return {
      tmp: {},
      categoryId: '',
      isAdd: false,
    }
  },
  mounted() {
    this.setDefault()
    this.getCategories()
  },
  methods: {
    ...mapActions([
      'getCategories',
      'postCategory',
      'patchCategory',
      'deleteCategory',
    ]),
    async handleDelete(item) {
      const val = confirm(`确定删除 ${item.title} 分类吗`)
      if (val) {
        await this.deleteCategory(item.id)
        this.getCategories()
      }
    },
    handle(isAdd) {
      this.setDefault()
      this.isAdd = isAdd
    },
    handlePatch(item) {
      this.tmp = {
        ...item,
      }
    },
    async handlePost() {
      if (this.tmp.id) {
        await this.patchCategory(this.tmp)
        this.getCategories()
        this.setDefault()
        this.isAdd = false
      } else {
        await this.postCategory(this.tmp)
        this.getCategories()
        this.setDefault()
        this.isAdd = false
      }
    },
    setDefault() {
      this.tmp = {
        id: '',
        title: '',
        keywords: '',
        description: '',
        isShow: true,
        sort: 0,
      }
    },
  },
})
</script>
