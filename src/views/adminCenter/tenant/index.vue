<template>
    <section class="search-wrap">
        <div class="input-wrap">
            <el-input
                v-model="input3"
                class="w-50 m-2"
                placeholder="Enter search content"
                :suffix-icon="Search"
                @keyup.enter.native="searchTenantlist"
                clearable
            />
        </div>
        <div class="button-wrap">
            <el-button
              plain
              :icon="Delete"
              @click="disabledHandle"
              >
              <!-- {{ $t('button.Disable') }} -->
              Disable
            </el-button>
            <el-button
              type="primary"
              :icon="Plus"
              @click="newHandle"
              >
              New
            </el-button>
      </div>
    </section>
    <div class="tenant-table">
      <el-table
        border
        stripe
        :data="tableData"
        @selection-change="selectionChange"
        @cell-dblclick="getDetail"
      >
       <!-- height="800" -->
        <el-table-column type="selection" width="40" ></el-table-column>
        <el-table-column prop="address" label="Organization Name" width="240" />
        <el-table-column prop="name" label="Type" width="180" />
        <el-table-column prop="name" label="State" width="180"/>
        <el-table-column prop="name" label="Region" width="180" />
        <el-table-column prop="date" label="Creation Time" width="180" />
        <el-table-column prop="name" label="Created by" />

        <!-- <el-table-column
          :label="$t('title.pureTitle.DeviceName')"
        >
          <template slot-scope="{row}">
            <span>{{ row }}</span>
          </template>
        </el-table-column>
        <el-table-column
          :label="$t('title.pureTitle.EnabledDate')"
        >
          <template slot-scope="{row}">
            {{ row.name }}
          </template>
        </el-table-column>
        <el-table-column
          :label="$t('title.pureTitle.Expirydate')"
        >
          <template slot-scope="{row}">
            {{ row.address }}
          </template>
        </el-table-column> -->
      </el-table>
    </div>
    <div class="pagination-block">
      <el-pagination
        v-model:current-page="currentPage"
        v-model:page-size="pageSize"
        :page-sizes="[10, 20, 30, 50, 100]"
        :small="small"
        :disabled="disabled"
        :background="background"
        layout="total, sizes, prev, pager, next, jumper"
        :total="tableData.length"
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
      />
    </div>


  <!-- <el-dialog v-model="dialogVisible" title="Shipping address" fullscreen>
    efferegreg34wrfregrgfd
  </el-dialog> -->


    <PopDialog
      :title="Tenant_title"
      v-model:dialogVisible="dialogVisible"
    >
      <template v-slot:body>
        <div class="car1">
          <el-card class="box-card">
            <template #header>
              <div class="card-header">
                <span>Basic Infor</span>
              </div>
            </template>
              <el-form
                :label-position="labelPosition"
                label-width="100px"
                :model="formLabelAlign"
                class="el-form"
              >
                <div class="e-item">
                  <el-form-item label="Organization Name">
                    <el-input v-model="formLabelAlign.name"  style="width: 500px;margin-right: 4rem"/>
                  </el-form-item>
                  <el-form-item label="Address">
                    <el-input v-model="formLabelAlign.region" style="width: 500px"/>
                  </el-form-item>
                </div>
            </el-form>
          </el-card>
          <el-card class="box-card">
            <template #header>
              <div class="card-header">
                <span>ADX Infor</span>
              </div>
            </template>
            <el-form
                :label-position="labelPosition"
                label-width="100px"
                :model="formLabelAlign"
                class="el-form"
              >
                <div class="e-item">
                  <el-form-item label="ADX Cluster">
                    <el-input v-model="formLabelAlign.name"  style="width: 500px;margin-right: 4rem"/>
                  </el-form-item>
                  <el-form-item label="ADX Client ID">
                    <el-input v-model="formLabelAlign.region" style="width: 500px"/>
                  </el-form-item>
                </div>
                <div class="e-item">
                  <el-form-item label="ADX Clinet Secret">
                    <el-input v-model="formLabelAlign.region" style="width: 500px;margin-right: 4rem"/>
                  </el-form-item>
                  <el-form-item label="ADX Clinet Secret">
                    <el-input v-model="formLabelAlign.region" style="width: 500px"/>
                  </el-form-item>
                </div>
            </el-form>
          </el-card>
        </div>
        <el-card class="box-card3">
          <template #header>
            <div class="card-header">
              <span>DB Infor</span>
            </div>
          </template>
          <el-form
              :label-position="labelPosition"
              label-width="100px"
              :model="formLabelAlign"
              class="el-form"
          >
              <div class="e-item">
                <el-form-item label="DB Name">
                  <el-input v-model="formLabelAlign.name"  style="width: 230px;margin-right: 2rem"/>
                </el-form-item>
                <el-form-item label="DB User">
                  <el-input v-model="formLabelAlign.region" style="width: 230px"/>
                </el-form-item>
              </div>
              <div class="e-item">
                <el-form-item label="DB Engine">
                  <el-input v-model="formLabelAlign.name"  style="width: 230px;margin-right: 2rem"/>
                </el-form-item>
                <el-form-item label="DB Password">
                  <el-input v-model="formLabelAlign.region" style="width: 230px"/>
                </el-form-item>
              </div>
              <div class="e-item">
                <el-form-item label="DB Host">
                  <el-input v-model="formLabelAlign.name"  style="width: 230px;margin-right: 2rem"/>
                </el-form-item>
                <el-form-item label="DB Port">
                  <el-input v-model="formLabelAlign.region" style="width: 230px"/>
                </el-form-item>
              </div>
            </el-form>
        </el-card>
        <div></div>

      </template>
    </PopDialog>

    <!-- <MiniDialog>456</MiniDialog> -->




</template>
<script setup lang="ts" name="Tenant">
import { ref, reactive } from 'vue'
import { Search, Delete, Plus } from '@element-plus/icons-vue'
import { ElMessage, ElMessageBox } from 'element-plus'
import PopDialog from '/@/components/PopDialog/fullDialog.vue'
import MiniDialog from '/@/components/PopDialog/miniDialog.vue'
import type { FormProps } from 'element-plus'




const input3 = ref('')
const tableData = [
  {
    date: '2016-05-03',
    name: 'Tom',
    address: 'No. 189, Grove St, Los Angeles',
    id: 1
  },
  {
    date: '2016-05-02',
    name: 'Tom',
    address: 'No. 189, Grove St, Los Angeles',
    id: 2
  },
  {
    date: '2016-05-04',
    name: 'Tom',
    address: 'No. 189, Grove St, Los Angeles',
    id: 3
  },
  {
    date: '2016-05-01',
    name: 'Tom',
    address: 'No. 189, Grove St, Los Angeles',
    id: 4
  },
    {
    date: '2016-05-03',
    name: 'Tom',
    address: 'No. 189, Grove St, Los Angeles',
    id: 5
  },
  {
    date: '2016-05-02',
    name: 'Tom',
    address: 'No. 189, Grove St, Los Angeles',
    id: 6
  },
  {
    date: '2016-05-04',
    name: 'Tom',
    address: 'No. 189, Grove St, Los Angeles',
    id: 7
  },
  {
    date: '2016-05-01',
    name: 'Tom',
    address: 'No. 189, Grove St, Los Angeles',
    id: 8
  },
    {
    date: '2016-05-03',
    name: 'Tom',
    address: 'No. 189, Grove St, Los Angeles',
    id: 9
  },
  {
    date: '2016-05-02',
    name: 'Tom',
    address: 'No. 189, Grove St, Los Angeles',
    id: 10
  },
  {
    date: '2016-05-04',
    name: 'Tom',
    address: 'No. 189, Grove St, Los Angeles',
    id: 11
  },
  {
    date: '2016-05-01',
    name: 'Tom',
    address: 'No. 189, Grove St, Los Angeles',
    id: 12
  },
  {
    date: '2016-05-01',
    name: 'Tom',
    address: 'No. 189, Grove St, Los Angeles',
    id: 13
  },
  {
    date: '2016-05-01',
    name: 'Tom',
    address: 'No. 189, Grove St, Los Angeles',
    id: 14
  },
  {
    date: '2016-05-01',
    name: 'Tom',
    address: 'No. 189, Grove St, Los Angeles',
    id: 15
  },
  {
    date: '2016-05-01',
    name: 'Tom',
    address: 'No. 189, Grove St, Los Angeles',
    id: 16
  },
  {
    date: '2016-05-01',
    name: 'Tom',
    address: 'No. 189, Grove St, Los Angeles',
    id: 17
  },
  {
    date: '2016-05-01',
    name: 'Tom',
    address: 'No. 189, Grove St, Los Angeles',
    id: 18
  }
]
const currentPage = ref(1)
const pageSize = ref(10)
const small = ref(false)
const disabled = ref(false)
const background = ref(false)
const checkedTenant = ref([])
const Tenant_title = ref('New Tenant')
const dialogVisible = ref(false)
const labelPosition = ref<FormProps['labelPosition']>('top')
const formLabelAlign = reactive({
  name: '',
  region: '',
  type: '',
})

const searchTenantlist = () => {
    console.log('getlist')
}

const newHandle = () => {
  dialogVisible.value = true
}

const disabledHandle = () => {
  if(tableData.length > 0 && checkedTenant.value.length > 0) {
    ElMessageBox.confirm(
      // this.$t('title.tipsTitle.device.n493pg').toString(),
      // this.$t('title.tipsTitle.device.3i1215').toString(),
        'Do you want to disable the selected data?',
      {
        confirmButtonText: 'OK',
        cancelButtonText: 'Cancel',
        type: 'warning'
      }
    ).then(async () => {
      ElMessage({
        type: 'success',
        message: 'Delete completed',
      })
    })
  } else {
    ElMessage({
      message: 'Please check the data first',
      // this.$t('title.tipsTitle.device.t51bj8').toString(),
      type: 'warning',
      duration: 5 * 1000
    })
  }
}

const selectionChange = (val: any) => {
  const checkedArr = val.reduce((arr: any, item: any)=> {
    const { id } = item
    arr.push(id)
    return arr
  }, [])
  checkedTenant.value = checkedArr
  console.log("%c [ checkedTenant.value ]", "font-size:13px; background:linear-gradient(to right , #9B63FF, #462188); color:yellow;", checkedTenant.value.length)
}
const getDetail = () => {
    console.log("%c [ getDetail ]", "font-size:13px; background:linear-gradient(to right , #9B63FF, #462188); color:yellow;")
}
const handleSizeChange = (val: number) => {
  console.log(`${val} items per page`)
  currentPage.value = 1
  pageSize.value = val
}
const handleCurrentChange = (val: number) => {
  console.log(`current page: ${val}`)
  currentPage.value = val
}










</script>

<style scoped lang="scss">
.search-wrap {
    display: flex;
    justify-content: space-between;
    height: 2.6rem;
}
.input-wrap {
    width: 34.9rem;
    padding: 12px;
}
.button-wrap {
    padding: 12px;
}
.tenant-table {
    padding: 0 12px;
    margin-top: 2rem;
    height:calc(100% - 9rem);
    overflow: auto;
    &::-webkit-scrollbar {
    width: 4px; /* 设置滚动条宽度 */
    }
    &::-webkit-scrollbar-thumb {
    background-color: #DCDFE6; /* 设置滚动条滑块颜色 */
    border-radius: 4px; /* 设置滚动条滑块圆角 */
    }

    &::-webkit-scrollbar-track {
    background-color: #f5f5f5; /* 设置滚动条背景颜色 */
    }
}
.pagination-block {
  margin: 10px 0 10px 20px;
}
    

</style>