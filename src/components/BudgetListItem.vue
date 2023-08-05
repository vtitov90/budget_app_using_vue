<template>
  <div>
    <div class="list-item" v-for="(item, prop) in list" :key='prop'>
      <ElIcon v-if="isIncomeType(item)" class="el-icon-top"></ElIcon>
      <ElIcon v-else class="el-icon-bottom"></ElIcon>
      <span class="budget-comment" :class="{'income': isIncomeType(item), 'outcome': !isIncomeType(item)}" >{{item.comment}}</span>
      <span class="budget-value">{{item.value}}</span>
      <ElButton type="danger" size="mini" @click="deleteItem(item.id)">Delete</ElButton>
    </div>
  </div>
  
</template>

<script>
export default {
  name: "BudgetListItem",
  props: {
    list: {
      type: Object,
      default: () => ({})
    }
  },
  methods: {
      deleteItem(id) {
        if(confirm("Are you sure you want to delete this item?"))
          this.$emit("deleteItem", id);
      },
      isIncomeType(item) {
        return item.type === "INCOME";
      } 
  },
}
</script>

<style scoped>
.list-item {
  display:flex;
  align-items: center;
  padding: 10px 15px;
}

.budget-value {
  font-weight: bold;
  margin-left: auto;
  margin-right: 20px;
}

.income {
  color:rgb(4, 131, 59)
}

.outcome{
  color:rgb(178, 14, 14)
}
</style>