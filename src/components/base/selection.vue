<!--下拉选择组件-->
<template>
  <div class="selection-component">
    <div class="selection-show" @click="toggleDrop">
      <span>{{ selections[nowIndex].label }}</span>
      <div class="arrow"></div>
    </div>
    <!--isDrop控制下拉框显示隐藏-->
    <div class="selection-list" v-if="isDrop">
      <ul>
        <li v-for="(item, index) in selections" @click="chooseSelection(index)">{{ item.label }}</li>
      </ul>
    </div>
  </div>
</template>

<script>
  import {eventBus} from '../../eventBus'

  export default {
//    接收数据
    props: {
      selections: {
        type: Array,
        default: [{
          label: 'test',
          value: 0
        }]
      }
    },
    data() {
      return {
        isDrop: false,
//        保存选中的项的index
        nowIndex: 0
      }
    },
    mounted() {
//      监听reset-component事件
      eventBus.$on('reset-component', () => {
        this.isDrop = false
      })
    },
    methods: {
//      下拉框的显示隐藏
      toggleDrop(event) {
//        阻止事件冒泡
        event.stopPropagation()
//        触发reset-component事件给其他selection组件
        eventBus.$emit('reset-component')
        this.isDrop = !this.isDrop

      },
//      下拉选择
      chooseSelection(index) {
        this.nowIndex = index
//        this.isDrop = false
//        触发on-change，发送选中的项
        this.$emit('on-change', this.selections[this.nowIndex])
      }
    }
  }
</script>

<style scoped>
  .selection-component {
    position: relative;
    display: inline-block;
  }

  .selection-show {
    border: 1px solid #e3e3e3;
    padding: 0 20px 0 10px;
    display: inline-block;
    position: relative;
    cursor: pointer;
    height: 25px;
    line-height: 25px;
    border-radius: 3px;
    background: #fff;
  }

  .selection-show .arrow {
    display: inline-block;
    border-left: 4px solid transparent;
    border-right: 4px solid transparent;
    border-top: 5px solid #e3e3e3;
    width: 0;
    height: 0;
    margin-top: -1px;
    margin-left: 6px;
    margin-right: -14px;
    vertical-align: middle;
  }

  .selection-list {
    display: inline-block;
    position: absolute;
    left: 0;
    top: 25px;
    width: 100%;
    background: #fff;
    border-top: 1px solid #e3e3e3;
    border-bottom: 1px solid #e3e3e3;
    z-index: 5;
  }

  .selection-list li {
    padding: 5px 15px 5px 10px;
    border-left: 1px solid #e3e3e3;
    border-right: 1px solid #e3e3e3;
    cursor: pointer;
    background: #fff;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;

  }

  .selection-list li:hover {
    background: #e3e3e3;
  }
</style>
