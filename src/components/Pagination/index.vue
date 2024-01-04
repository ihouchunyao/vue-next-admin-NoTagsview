<template>
  <div>
    <el-pagination
      v-model="currentPage"
      :total="totalItems"
      :page-size="pageSize"
      layout="prev, pager, next, jumper"
      @current-change="handlePageChange"
    />
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  props: {
    totalItems: {
      type: Number,
      required: true,
    },
    pageSize: {
      type: Number,
      default: 10,
    },
  },
  setup(props, context) {
    const currentPage = ref(1);

    function handlePageChange(page) {
      currentPage.value = page;
      // 触发父组件的事件
      context.emit('page-change', page);
    }

    return {
      currentPage,
      handlePageChange,
    };
  },
};
</script>