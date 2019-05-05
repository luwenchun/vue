<template>
  <div>
    <p>{{name}}</p>
    <!-- 购物车列表 -->
    <table border="1">
      <tr>
        <th>#</th>
        <th>课程名</th>
        <th>单价</th>
        <th>数量</th>
        <th>价格</th>
      </tr>
      <tr v-for="(c,i) in cart" :key="c.id" :class="{active:c.active}">
        <td>
          <input type="checkbox" v-model="c.active">
        </td>
        <td>{{c.text}}</td>
        <td>￥{{c.price}}</td>
        <td>
          <button @click="minus(i)">-</button>
          {{c.count}}
          <button @click="add(i)">+</button>
        </td>
        <td>￥{{c.price*c.count}}</td>
      </tr>
      <tr>
        <td></td>
        <td colspan="2">{{activeCount}}/{{count}}</td>
        <td colspan="2">￥{{total}}</td>
      </tr>
    </table>
  </div>
</template>
<script>
</script>
<style scope>
</style>