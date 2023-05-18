<!--
 * Licensed to the Apache Software Foundation (ASF) under one
 * or more contributor license agreements.  See the NOTICE file
 * distributed with this work for additional information
 * regarding copyright ownership.  The ASF licenses this file
 * to you under the Apache License, Version 2.0 (the
 * "License"); you may not use this file except in compliance
 * with the License.  You may obtain a copy of the License at
 *
 *     http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing,
 * software distributed under the License is distributed on an
 * "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
 * KIND, either express or implied.  See the License for the
 * specific language governing permissions and limitations
 * under the License.
-->

<template>
  <div class="main-center">
    <div class="main-center-container">
      <!--    <div class="standTable">-->
      <!--      <indicator-list v-if="panelMode === 'list'" :currentData="currentData" />

            <indicator-panel v-else :currentData="currentData" />-->
      <div class="headerbox" style="padding: 15px 15px 10px 15px">
        <!--        <div style="padding: 0 0 10px 0" class="flexBox edit-box">
                  <el-button type="primary" class="newButton" @click="editDevce">{{ $t('device.editTimeseries') }}</el-button>
                  <form-table :form="form" @serchFormData="serchFormData"></form-table>
                </div>-->
        <div style="padding: 0 0 10px 0" class="flexBox edit-box">
          <el-button type="primary" class="newButton" @click="newSource">新增邮箱服务</el-button>
          <!--          <form-table :form="form" @serchFormData="serchFormData"></form-table>-->
        </div>
        <el-table
          :data="testDatas"
          border
          stripe
          style="width: 100%"
          :header-cell-style="{
            background: '#f9fafc',
            color: '#555',
            fontWeight: '600',
            fontSize: '14px',
          }"
          :cell-style="{
            background: '#ffffff',
          }"
        >
          <el-table-column v-for="col in columnList" :prop="col.prop" :label="col.label" :key="col.prop" />

          <el-table-column :label="$t('storagePage.operation')">
            <template #default="scope">
              <!--              <el-button type="text" size="small" @click="goToEntity(scope)">{{ $t('common.detail') }}{{ scope.row.ttl }}</el-button>-->

              <el-button type="text" size="small" @click="editDevice(scope.row)">{{ $t('common.edit') }}{{ scope.row.ttl }} </el-button>
              <el-popconfirm placement="top" :title="'你确定要删除该服务吗?'" @confirm="deleteDevice(scope)">
                <template #reference>
                  <el-button type="text" size="small" class="el-button-delete">{{ $t('common.delete') }}</el-button>
                </template>
              </el-popconfirm>
            </template>
          </el-table-column>
        </el-table>
      </div>
      <NewSource v-if="showDialog" :func="func" :serverId="null" :showDialog="showDialog" :types="types" @close="close()" @successFunc="successFunc(data)" />

      <div class="headerbox" style="padding: 15px 15px 10px 15px">
        <div style="padding: 0 0 10px 0">
          告警开关
          <el-switch v-model="value1" style="--el-switch-on-color: #16c493" />
        </div>
        <br />
        <div style="padding: 0 0 10px 0">
          选择邮件服务
          <el-select v-model="value" class="m-2" placeholder="email1">
            <el-option v-for="item in options" :key="item.value" :label="item.label" :value="item.value" />
          </el-select>
        </div>
        <br />
        <div style="background-color: #f9fbfc; width: 250px; height: 200px">
          <el-button @click="newUser()">
            添加接收邮箱
            <svg class="icon" aria-hidden="true">
              <use xlink:href="#icon-add1"></use></svg
          ></el-button>
          <div v-for="(email, index) in emails" :key="index" style="font-size: small">
            <br />{{ email }}
            <div style="float: right">
              <svg aria-hidden="true" class="icon icon-delete">
                <use xlink:href="#icon-se-icon-delete"></use>
              </svg>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { reactive, ref } from 'vue';
// import {useRoute} from 'vue-router';
import NewSource from './newSource';

export default {
  name: 'Alert',
  props: [/*'handleNodeClick', 'nodekey',*/ 'func'],
  components: {
    NewSource,
  },
  setup(props) {
    const showDialog = ref(false);
    const types = ref(null);
    const funcdata = reactive(props.func);
    const value1 = ref(true);
    const value2 = ref(true);
    const value = ref('');
    let testDatas = [
      { name: 'email1', host: 'smtp.qq.com', port: 25, address: '139312678@qq.com', state: 1 },
      { name: 'email-test', host: 'smtp.qq.com', port: 25, address: '971897647@qq.com' },
    ];

    const options = [
      {
        value: 'email1',
        label: 'email1',
      },
      {
        value: 'email-test',
        label: 'email-test',
      },
    ];

    const newSource = () => {
      showDialog.value = true;
      types.value = 0;
    };

    const successFunc = () => {
      showDialog.value = false;
      types.value = 0;
    };
    const close = () => {
      showDialog.value = false;
      types.value = 0;
    };
    return {
      showDialog,
      newSource,
      successFunc,
      close,
      funcdata,
      types,
      value1,
      value2,
      value,
      options,
      columnList: [
        { prop: 'name', label: '邮件服务名' },
        { prop: 'host', label: 'SMTP主机' },
        { prop: 'port', label: 'SMTP端口' },
        { prop: 'address', label: '邮箱地址' },
      ],
      testDatas,
      emails: ['971897647@qq.com', 'qhy1997s@163.com'],
    };
  },
};
</script>

<style scoped lang="scss">
.main-center {
  min-height: calc(100% - 243px);
  padding: 20px;
  background: #f9fbfc;

  &-container {
    background: #fff;
    border-radius: 4px;
  }
}

.headerbox {
  text-align: left;
  padding: 0 30px;
  border-radius: 4px;
  border: 1px solid #eaecf0;
  margin-bottom: 20px;
  background: #fff;
}

.a-part {
  width: 220px;
  margin-right: 20px;
  border-color: #f9fbfc;
  background: #f9fbfc;
  .title {
    height: 40px !important;
    width: 220px;
    font-size: 12px;
    background: #f9fbfc;
    line-height: 40px;
    padding: 0 20px;
    margin-bottom: 4px;
    border-color: #f9fbfc;
    text-align: left;

    .icon {
      margin-top: 7px;
      float: right;
    }

    button {
      float: right;
      font-size: 12px;
      padding-right: 0;
    }
  }
}
</style>
