<template>
  <el-button text @click="dialog = true">
    <div style="width: 15px;
                height: 15px">
      <svg color="black" viewBox="0 0 1024 1024" xmlns="http://www.w3.org/2000/svg" data-v-ea893728="">
        <path fill="currentColor"
              d="M480 480V128a32 32 0 0 1 64 0v352h352a32 32 0 1 1 0 64H544v352a32 32 0 1 1-64 0V544H128a32 32 0 0 1 0-64h352z"></path>
      </svg>
    </div>
  </el-button>
  <el-drawer
      ref="drawerRef"
      v-model="dialog"
      title="New"
      direction="rtl"
      class="demo-drawer"
      size="350"
      :with-header="false"
  >
    <div class="demo-drawer__content">
      <el-tabs stretch="true" v-model="activeName" class="demo-tabs" @tab-click="handleClick">
        <el-tab-pane label="Add Contacts/Group" name="first">
          <el-form :model="form">
            <el-form-item>
              <div style="flex: 1"></div>
              <el-radio-group v-model="form.isGroup" class="ml-4">
                <el-radio label="0">Contact</el-radio>
                <el-radio label="1">Group</el-radio>
              </el-radio-group>
              <div style="flex: 1"></div>
            </el-form-item>
            <el-form-item label="ID">
              <el-input clearable="true" class="in" v-model="form.id" autocomplete="off"/>
            </el-form-item>
          </el-form>
          <div class="demo-drawer__footer">
            <el-button @click="cancelForm">Cancel</el-button>
            <el-button type="primary" :loading="loading" @click="onClick">{{
                loading ? 'Submitting' : 'Submit'
              }}
            </el-button>
          </div>
        </el-tab-pane>
        <el-tab-pane label="Create Group" name="second">
          <el-form :model="form">
            <el-form-item label="Group Name">
              <el-input clearable="true" class="in" v-model="form2.name" autocomplete="off"/>
            </el-form-item>
            <el-form-item label="ID 1">
              <el-input clearable="true" class="in" v-model="form2.id1" autocomplete="off"/>
            </el-form-item>
            <el-form-item label="ID 2">
              <el-input clearable="true" class="in" v-model="form2.id2" autocomplete="off"/>
            </el-form-item>
          </el-form>
          <div class="demo-drawer__footer">
            <el-button @click="cancelForm">Cancel</el-button>
            <el-button type="primary" :loading="loading" @click="onClick2">{{
                loading ? 'Submitting' : 'Submit'
              }}
            </el-button>
          </div>
        </el-tab-pane>
      </el-tabs>
    </div>
  </el-drawer>
</template>

<script lang="ts" setup>
import {reactive, ref} from 'vue'
import {ElDrawer, ElMessageBox} from 'element-plus'
import axios from 'axios'
import store from '../../store/index'
import { ElMessage } from 'element-plus'


import {set_Url} from '../../assets/setting';

const formLabelWidth = '80px'
let timer
const activeName = ref('first')
const table = ref(false)
const dialog = ref(false)
const loading = ref(false)

const form = reactive({
  isGroup: '0',
  id: '',
})

const form2 = reactive({
  name: '',
  id1: '',
  id2: '',
})

const drawerRef = ref<InstanceType<typeof ElDrawer>>()
const onClick = () => {
  loading.value = true;
  axios.post(set_Url + '/api/addContact',
      {username: store.state.userid, content: form}).then(res => {
    console.log(res)
  })
  loading.value = false;
  drawerRef.value!.close()
}

const onClick2 = () => {
  loading.value = true;
  axios.post(set_Url + '/api/createGroup',
      {username: store.state.userid, content: form2}).then(res => {
    console.log(res)
    if (res.data.success) {
      // 操作成功，显示提示消息
      ElMessage({
        message: 'Success!',
        type: 'success',
      });
    } else {
      // 操作失败，显示错误消息
      ElMessage({
        message: res.data.message,
        type: 'error',
      });
    }
  }).catch(error => {
    console.error(error);
    // 显示网络错误消息
    ElMessage({
      message: 'Network error!',
      type: 'error',
    });
  }).finally(() => {
    loading.value = false;
    drawerRef.value!.close();
  });
}

const cancelForm = () => {
  console.log(form.isGroup);
  loading.value = false
  dialog.value = false
  clearTimeout(timer)
}
</script>

<style>
.in {
  /*width: 200px;*/
}

/*.demo-drawer__footer{*/
/*  align-content: center;*/
/*}*/
.demo-tabs > .el-tabs__content {
  padding: 32px;
  color: #6b778c;
  font-size: 32px;
  font-weight: 600;
}

</style>