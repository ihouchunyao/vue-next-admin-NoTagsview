<template>
  <el-dialog v-model="dialog_Visible" width="100%" fullscreen :show-close="false">
      <template #header>
        <div class="dialogTitle">
          <el-button type="text" :icon="Back" @click="handleClose" class="ebutton"> {{ title }} </el-button>
        </div>
        <div class="dialog-btn-wrap">
          <el-button plain @click="Cancel">
            Cancel
          </el-button>
          <el-button type="primary" @click="Confirm">
            Confirm
          </el-button>
        </div>
      </template>
      <slot name="body"></slot>
      <!-- <slot /> -->
   </el-dialog>
</template>
<script setup lang="ts" name='FullDialog'>
import { ref, computed, onMounted, defineEmits, defineProps } from 'vue'
import { Back } from '@element-plus/icons-vue'

const props = defineProps({
    title: {
        type: String,
        default: () => 'New'
    },
    dialogVisible: {
        type: Boolean,
        default: () => false
    }

})

const emits = defineEmits(['update:dialogVisible'])
const dialog_Visible = computed({
    get: () => props.dialogVisible,
    set: (val: any) => {
        emits('update:dialogVisible', val)
    }
})

const close = () => {
    emits('update:dialogVisible', false)
}
const handleClose = () => {
  close()
}
const Confirm = () => {
    close()
}
const Cancel = () => {
    close()
}

onMounted(()=>{
    // console.log("%c [ props.dialogVisible ]", "font-size:13px; background:linear-gradient(to right , #9B63FF, #462188); color:yellow;", props.dialogVisible)

})
</script>
<style lang="scss">
  // ::v-deep .dialogTitle{
  //   color: #262626;
  //   font-size: 1.4rem;
  // }

  .el-dialog {
    height: 100%;
    margin: 0;
    background: #F9F9F9!important;
  }
  .el-dialog__header {
    background-color: #fff;
    box-shadow: 0px 1px 3px 0px rgba(244,244,244,1);
    display: flex;
    justify-content: space-between;
    padding-bottom: 20px;
    margin-right: 0;
    .ebutton.el-button {
      color: #262626;
      font-size: 1.4rem;
    }
  }
  .el-dialog__body{
    display: flex;
    width: 100%;
    .car1{
      width: 90rem;
      margin-right: 1rem;
      .box-card{
        margin-bottom: 1rem;
        .el-form{
          .e-item {
            display: flex;
          }
        }
        &:nth-child(2){
          margin-bottom: 0rem;
        }
      }
    }

    .box-card3{
      width: 100%;
      .el-form{
        .e-item {
          display: flex;
        }
      }
    }
  }

</style>