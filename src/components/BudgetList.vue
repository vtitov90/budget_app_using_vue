<template>
  <div class="budget-list-wrap" ref="budgetList">
    <ElCard :header="header">
      <template v-if="!isEmpty">
        <BudgetListItem :list="filtered" @deleteItem="deleteItem"/>
      </template>
      <ElAlert v-else type="info" :title="emptyTitle" :closable="false"/>
      <div class="btn-container">
        <ElButton type="success" @click="showIncome">Show Income</ElButton>
        <ElButton type="danger" @click="showOutcome">Show Outcome</ElButton>
        <ElButton type="warning" @click="showAll">Show All</ElButton>  
      </div>
      
    </ElCard>
  </div>
</template>



<script>
import BudgetListItem from "@/components/BudgetListItem";

export default {
  name: "BudgetList",
  components: {
    BudgetListItem
  },
  props: {
    list: {
      type: Object,
      default: () => ({}),
    }
  },
  data: () => ({
    header: "Budget List",
    emptyTitle:"Empty List",
    filtered: {}
  }),
  computed: {
    isEmpty() {
      return !Object.keys(this.list).length;
    }
  },
  created() {
    this.filtered = { ...this.list };
  },
  methods: {
      deleteItem(id) {
        this.$emit("deleteItem", id);
      },
      showIncome() {
        this.filtered = Object.assign({}, this.list);
        const notIncome = Object.values(this.filtered).filter((item) => item.type !== "INCOME");
        for (let key in this.list) {
          if(notIncome.includes(this.list[key])) {
            this.$delete(this.filtered, key);
          }
        }
      },
      showOutcome() {
        this.filtered = Object.assign({}, this.list);
        const notOutcome = Object.values(this.filtered).filter((item) => item.type !== "OUTCOME");
        for (let key in this.list) {
          if(notOutcome.includes(this.list[key])) {
            this.$delete(this.filtered, key);
          }
        }
      },
      showAll() {
        this.filtered = Object.assign({}, this.list);
      },
  },
  watch: {
    list: {
      handler(newList) {
        this.filtered = { ...newList };
      },
      deep: true
    }
  },
};
</script>

<style scoped>
.budget-list-wrap {
  max-width: 500px;
  margin: auto;
}

.btn-container {
  margin-top: 20px;
}
</style>