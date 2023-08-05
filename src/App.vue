<template>
  <div id="app">
    <BudgetForm @submitForm="onFormSubmit" />
    <TotalBalance :class="{'above-zero': isTotalAbove, 'belov-zero': isTotalBelow}" :total="totalBalance"/>
    <BudgetList :list="list" @deleteItem="onDeleteItem" />
  </div>
</template>

<script>
import BudgetList from "@/components/BudgetList";
import TotalBalance from "@/components/TotalBalance";
import BudgetForm from "@/components/Form";

export default {
  name: "app",
  components: {
    BudgetList,
    TotalBalance,
    BudgetForm
  },
  data: () => ({
    list: {
      1: {
        type: "INCOME",
        value: 100,
        comment: "Some comment",
        id: 1
      },
      2: {
        type: "OUTCOME",
        value: -50,
        comment: "Some outcome comment",
        id: 2
      }
    },
    isTotalAbove : true,
    isTotalBelow: false
  }),
  computed: {
    totalBalance() {
      return Object.values(this.list).reduce((acc, item) => {
        return acc + item.value;
      }, 0)
    }
  },
  methods: {
    onDeleteItem(id) {
      this.$delete(this.list, id);
    },
    onFormSubmit(data) {
      const newObj = {
        ...data,
        id: String(Math.random())
      };

      this.$set(this.list, newObj.id, newObj);

    },
    
  },
   watch: {
    totalBalance(newValue) {
      if (newValue < 0) {
        this.isTotalAbove = false;
        this.isTotalBelow = true;
      } else {
        this.isTotalAbove = true;
        this.isTotalBelow = false;
      }
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

.above-zero {
  color:rgb(4, 131, 59)
}

.belov-zero {
  color:rgb(178, 14, 14)
}
</style>
