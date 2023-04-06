<template>
  <div class="budget-list-wrap">
    <ElCard :header="header">

      <template v-if="!isEmpty">
        <div class="list-item" v-for="list in filteredTransactions" :key="list.id">
          <span class="budget-comment">{{ list.comment }}</span>
          <template v-if="list.value > 0">
            <span class="budget-value positive">{{ list.value }}</span>
            <i class="el-icon-caret-top icon positive"></i>
          </template>
          <template v-else>
            <span class="budget-value negative">{{ list.value }}</span>
            <i class="el-icon-caret-bottom icon negative"></i>
          </template>
          <ElButton type="danger" size="mini" @click="$emit('deleteItem', list)"> DELETE</ElButton>
        </div>
      </template>
      <ElAlert v-else type="info" :title="emptyTitle" :closable="false"/>
    </ElCard>
  </div>
</template>

<script>
export default {
  name: "BudgetList",
  props: {
    list: {
      type: Array,
      required: true
    },
    show: {
      type: String,
      required: true
    }
  },
  data: () => ({
    header: "Budget List",
    emptyTitle: "Empty List"
  }),
  computed: {
    isEmpty() {
      return !Object.keys(this.list).length;
    },
    filteredTransactions() {
      if (this.show === 'ALL') {
        return this.list;
      } else {
        return this.list.filter(list => list.type === this.show);
      }
    }
  }
};

</script>

<style scoped>
.budget-list-wrap {
  max-width: 500px;
  margin: auto;
  margin-top: 15px;
}

.list-item {
  display: flex;
  align-items: center;
  padding: 10px 15px;
}

.budget-value {
  font-weight: bold;
  margin-left: auto;
  margin-right: 20px;
}

.positive {
  color: #67C23A;
}

.negative {
  color: #ea0505;
}

.icon {
  margin-right: 25px;
}
</style>