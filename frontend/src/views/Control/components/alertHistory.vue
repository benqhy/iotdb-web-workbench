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
          <el-button type="primary" class="newButton" @click="newSource">告警历史</el-button>
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
        >
          <el-table-column type="index" label="No." :width="50" />
          <el-table-column v-for="col in columnList" :prop="col.prop" :label="col.label" :key="col.prop" />
        </el-table>
      </div>
      <NewSource v-if="showDialog" :func="func" :serverId="null" :showDialog="showDialog" :types="types" @close="close()" @successFunc="successFunc(data)" />
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
      columnList: [
        { prop: 'name', label: '姓名' },
        { prop: 'age', label: '年龄' },
        { prop: 'city', label: '城市' },
        { prop: 'tel', label: '电话' },
      ],
      testDatas: [
        { No: 1, name: '张三', age: 24, city: '广州', tel: '13312345678' },
        { No: 2, name: '李四', age: 25, city: '九江', tel: '18899998888' },
        { No: 3, name: '王五', age: 26, city: '六盘水', tel: '13600001111' },
        { No: 4, name: '赵二', age: 27, city: '菏泽', tel: '13145209420' },
      ],
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
</style>
