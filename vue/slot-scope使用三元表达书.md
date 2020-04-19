# scope中使用运算符

ele-ui：在一些table或者form标签中使用三元表达式

```vue
<el-table-column
  prop="isReply"
  header-align="center"
  align="center"
  label="是否回复"
>
  <template  slot-scope="scope">
    <span v-if="scope.row.isReply===1">是</span>
    <span v-else="">否</span>
  </template>
</el-table-column>
```



