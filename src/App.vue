<template>
  <div id="app">
    <FormList @submitForm="onFormSubmit"/>
    <TotalBalance :total="totalBalance"/>
    <span class="button-pane">
          <el-button class="button" type="success"  @click="showIncome"> INCOME </el-button>
          <el-button class="button" type="danger" @click="showExpenses"> OUTCOME </el-button>
          <el-button class="button" type="primary" @click="showAll">ALL</el-button>
    </span>

    <BudgetList :list="list" :show="show" @deleteItem="onDeleteItem"/>
  </div>
</template>

<script>
import BudgetList from "@/components/BudgetList";
import TotalBalance from "@/components/TotalBalance";
import FormList from "@/components/FormList";

export default {
  name: "App",
  components: {
    BudgetList,
    TotalBalance,
    FormList
  },
  data() {
    return {
      list: [
        {type: "INCOME", value: 100, comment: "Some comment", id: 1},
        {type: "OUTCOME", value: -50, comment: "Some outcome comment", id: 2},
      ],
      show: 'ALL',
    };
  },
  computed: {
    totalBalance() {
      return Object.values(this.list).reduce(
          (acc, item) => acc + item.value,
          0
      );
    }
  },
  methods: {
    onDeleteItem(list) {
      this.list = this.list.filter(p => p.id !== list.id)
    },
    onFormSubmit(formData) {
      this.list.push(formData);
    },
    showExpenses() {
      this.show = "OUTCOME";
    },
    showIncome() {
      this.show = "INCOME";
    },
    showAll() {
      this.show = 'ALL';
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.button-pane{
  margin: 15px;
}
</style>
