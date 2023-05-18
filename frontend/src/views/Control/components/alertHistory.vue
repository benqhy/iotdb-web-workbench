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
          <el-table-column type="index" label="序号" :width="50" />
          <el-table-column label="数据连接" prop="conn" :min-width="15"> </el-table-column>
          <el-table-column label="异常时间戳" prop="ab" :min-width="20"></el-table-column>
          <el-table-column label="接受邮箱" prop="email" :min-width="25"> </el-table-column>
          <el-table-column label="最可能异常的指标" prop="mo" :min-width="40"> </el-table-column>
        </el-table>
        <el-pagination
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          :current-page="currentPage"
          :page-sizes="[10, 15, 20]"
          :page-size="pagesize"
          layout="total, sizes, prev, pager, next"
          :total="testDatas.length"
        >
        </el-pagination>
      </div>
    </div>
  </div>
</template>

<script>
import { reactive, ref } from 'vue';
// import {useRoute} from 'vue-router';

import axios from 'axios';

export default {
  name: 'Alert',
  props: [/*'handleNodeClick', 'nodekey',*/ 'func'],
  components: {},
  setup(props) {
    // this.handleUserList();

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

      testDatas: [
        { conn: 'local', ab: '2023-05-17 21:21:52.253', email: '971897647@qq.com', mo: 'CPU使用率、JVM线程数、内存使用率' },
        { conn: 'local', ab: '2023-05-17 20:49:35.231', email: '971897647@qq.com', mo: 'CPU使用率、JVM线程数、内存使用率' },
        { conn: 'local', ab: '2023-05-16 21:21:52.368', email: '971897647@qq.com', mo: '查询成功率、磁盘IO吞吐、内存使用率' },
        { conn: 'local', ab: '2023-05-16 20:47:12.463', email: '971897647@qq.com', mo: 'CPU使用率、内存使用率、FGC耗时' },
        { conn: 'local', ab: '2023-05-12 17:51:64.126', email: '971897647@qq.com', mo: 'CPU使用率、JVM线程数、内存使用率' },
        { conn: 'local', ab: '2023-05-12 15:22:11.563', email: '971897647@qq.com', mo: '内存使用率、CPU使用率、磁盘IO吞吐' },
        { conn: 'local', ab: '2023-05-12 15:10:35.871', email: '971897647@qq.com', mo: '内存使用率、CPU使用率、磁盘IO吞吐' },
        { conn: 'local', ab: '2023-05-12 15:01:52.432', email: '971897647@qq.com', mo: '内存使用率、CPU使用率、磁盘IO吞吐' },
      ],

      currentPage: 1, //初始页
      pagesize: 20, //    每页的数据
      userList: [1, 2, 3, 9, 2, 21, 1, 3],
    };
  },
  methods: {
    // 初始页currentPage、初始每页数据数pagesize和数据data
    handleSizeChange(size) {
      console.log(size, 'size');
      this.pagesize = size;
      console.log(this.pagesize); //每页下拉显示数据
    },
    handleCurrentChange(currentPage) {
      console.log(currentPage, 'currentPage');
      this.currentPage = currentPage;
      console.log(this.currentPage); //点击第几页
    },
    handleUserList() {
      axios
        .get('/user/list')
        .then((res) => {
          console.log(res, 'res');
          this.userList = res.data.data.list[0].records;
        })
        .catch((error) => {
          console.log(error);
        });
    },
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
