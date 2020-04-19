# el-select选择

方式一：

```vue
<el-form-item  >
  <el-select v-model="dataForm.valueReply" filterable placeholder="回复状态" style="width: 110px">
    <el-option label="全部" value=""></el-option>
    <el-option label="未回复" value="0"></el-option>
    <el-option label="已回复" value="1"></el-option>
  </el-select>
</el-form-item>
```

方式二：

```vue
<el-form-item  >
    <el-select style="width: 110px" v-model="dataForm.name" filterable placeholder="售后类型" value-key="id" @change="currentSel">
       <el-option v-for="item in typeList" :key="item.id" :label="item.name" :value="item.id"></el-option>
     </el-select>
</el-form-item>
```

2： 指定valueReply的值为 null， 用于显示placeholder的信息



