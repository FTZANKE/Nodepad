## 客户

```vue
<template>
<div class="same_text flex_clounm_left_center">
    <label class="require">客户</label>
    <el-select v-model="ruleForm.personId" placeholder="请选择" @change="clentChange">
        <el-option v-for="item in clientList" :key="item.id" :label="item.customerName" :value="item.id"/>
    </el-select>
    </div>
</template>
<script>
    import {getAccountCustomersList} from '@/api/financeSetting';//客户列表
    export default {
        methods: {
            // 客户列表
            getAccountCustomersList(obj = {}) {
                obj = Object.assign(obj, {
                    customersType: 2
                })
                getAccountCustomersList(obj).then(res => {
                    this.clientList = res.data.list;
                })
            },
        }
    }
</script>
```

## 供应商

```vue
<template></template>
<div class="same_text flex_clounm_left_center">
    <label class="require">供应商</label>
    <el-select v-model="ruleForm.clientId" placeholder="请选择" @change="clentChange">
        <el-option v-for="item in clientList" :key="item.id" :label="item.customerName" :value="item.id"/>
    </el-select>
</div>
</template>
<script>
    import {queryClients} from "@/api/fundsManagement";// 供应商列表
    export default {
        methods: {
            // 供应商列表
            queryClients() {
                queryClients({customersType: 2, status: 1}).then((res) => {
                    this.clientList = res.data;
                });
            },
        }
    }
</script>
```

AirPods(第三代) RMB 1399   58元/月(24期)

iPhone 13 Pro  Max RMB 9799 408元/月(24期)

MacBook Pro 16英寸  64+1T 29499元 1299元/月(24期)

=40697元
