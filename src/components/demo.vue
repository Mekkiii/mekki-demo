<template>
  <div class="test">
    <div class="title">table自定义列-demo</div>
    <a class="link" href="https://github.com/Mekkiii/mekki-demo" target="_blank"
      ><GithubFilled
    /></a>
    <a-popover
      class="column-popover"
      placement="bottomLeft"
      v-model="visible"
      trigger="hover"
      :getPopupContainer="
        (triggerNode) => {
          return triggerNode.parentNode;
        }
      "
    >
      <template #content>
        <div>
          <div :style="{ borderBottom: '1px solid #E9E9E9' }">
            <a-checkbox
              :indeterminate="indeterminate"
              v-model:checked="checkAll"
              @change="onCheckAllChange"
            >
              列展示
            </a-checkbox>
          </div>
          <a-checkbox-group
            v-model:value="checkedList"
            :options="plainOptions"
            @change="onCheckChange"
          />
        </div>
      </template>
      <a class="btn">
        <SettingTwoTone />
      </a>
    </a-popover>
    <a-table :columns="deletedata" :data-source="data">
      <template #action>
        <a>action</a>
      </template>
    </a-table>
  </div>
</template>
<script>
import { SettingTwoTone, GithubFilled } from "@ant-design/icons-vue";
import { defineComponent } from "vue";
const plainOptions = [
  "Full Name",
  "Age",
  "Column 1",
  "Column 2",
  "Column 3",
  "Column 4",
  "Column 5",
  "Action",
];
const defaultCheckedList = [
  "Full Name",
  "Age",
  "Column 1",
  "Column 2",
  "Action",
];
const columns = [
  {
    title: "Full Name",
    width: 100,
    dataIndex: "name",
    key: "name",
  },
  { title: "Age", width: 100, dataIndex: "age", key: "age" },
  { title: "Column 1", dataIndex: "address", key: "1" },
  { title: "Column 2", dataIndex: "address", key: "2" },
  { title: "Column 3", dataIndex: "address", key: "3" },
  { title: "Column 4", dataIndex: "address", key: "4" },
  { title: "Column 5", dataIndex: "address", key: "5" },
  {
    title: "Action",
    key: "operation",
    width: 100,
    slots: { customRender: "action" },
  },
];
const data = [
  {
    key: "1",
    name: "John Brown",
    age: 32,
    address: "New York Park",
  },
  {
    key: "2",
    name: "Jim Green",
    age: 40,
    address: "London Park",
  },
];
export default defineComponent({
  components: {
    SettingTwoTone,
    GithubFilled,
  },
  data() {
    return {
      checkedList: defaultCheckedList,
      indeterminate: true,
      checkAll: false,
      plainOptions,
      selectedRowKeys: [],
      visible: false,
      deletedata: [],
      data,
      columns,
    };
  },
  methods: {
    getInitialData(checkedList) {
      this.deletedata = [];
      JSON.parse(JSON.stringify(this.columns)).forEach((item) => {
        for (let i in checkedList) {
          if (checkedList[i] == item.title) {
            this.deletedata.push(item);
          }
        }
      });
    },
    onSelectChange(selectedRowKeys) {
      this.selectedRowKeys = selectedRowKeys;
      console.log(this.selectedRowKeys);
    },
    onCheckChange(checkedList) {
      this.indeterminate =
        !!checkedList.length && checkedList.length < plainOptions.length;
      this.checkAll = checkedList.length === plainOptions.length;
      this.deletedata = [];
      JSON.parse(JSON.stringify(this.columns)).forEach((item) => {
        for (let i in checkedList) {
          if (checkedList[i] == item.title) {
            this.deletedata.push(item);
          }
        }
      });
    },
    onCheckAllChange(e) {
      Object.assign(this, {
        checkedList: e.target.checked ? plainOptions : [],
        indeterminate: false,
        checkAll: e.target.checked,
      });
      this.deletedata = [];
      JSON.parse(JSON.stringify(this.columns)).forEach((item) => {
        for (let i in this.checkedList) {
          if (this.checkedList[i] == item.title) {
            this.deletedata.push(item);
          }
        }
      });
    },
  },
  mounted() {
    this.getInitialData(this.checkedList);
  },
});
</script>
<style lang="less" scoped>
.anticon {
  font-size: 18px;
}
.test {
  position: relative;
  background: #fff;
  padding: 60px 20px;
  margin: 24px;
  /deep/ .ant-popover {
    width: 177px;
  }
  .btn {
    position: absolute;
    right: 30px;
    top: 20px;
  }
  .title {
    position: absolute;
    top: 20px;
    line-height: 22px;
    font-size: 16px;
    padding-left: 10px;
    border-left: 2px solid #1890ff;
  }
  .link {
    position: absolute;
    top: 20px;
    right: 0;
  }
}
</style>
