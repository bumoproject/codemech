<template>
  <ul class="vue-contextmenu-listWrapper" :class="'vue-contextmenuName-' + transferIndex">
    <li v-for="item in contextMenuData.menulists"
      class="context-menu-list"
      :key="item.btnName">
      <button @click.stop="fnHandler(item)" type="button">
        <i :class="item.icoName"></i>
        <span>{{item.btnName}}</span>
      </button>
    </li>
  </ul>
</template>
<script>
  export default {
    props: {
      contextMenuData: {
        type: Object,
        requred: false,
        default () {
          return {
            menuName: null,
            axis: {
              x: null,
              y: null
            },
            menulists: [
              {
                fnHandler: 'adddata',
                icoName: 'fa fa-home fa-fw',
                btnName: 'New'
              },
              {
                fnHandler: 'savedata',
                icoName: 'fa fa-home fa-fw',
                btnName: 'Save'
              }
            ]
          }
        }
      },
      transferIndex: {
        type: String,
        default: '0'
      },
      activeMenu: {
        type: String,
        default: '0'
      }
    },
    computed: {},
    watch: {
      'contextMenuData.axis' (val) {
        var x = val.x
        var y = val.y
        var _this = this
        var index = _this.activeMenu
        var menuName = 'vue-contextmenuName-' + index
        var domArr = document.getElementsByClassName('vue-contextmenu-listWrapper')
        for (let i = 0; i < domArr.length; i++) {
          domArr[i].style.display = 'none'
        }
        var menu = document.getElementsByClassName(menuName)[0]
        menu.style.display = 'block'
        menu.style.left = x + 'px'
        menu.style.top = y + 'px'
        document.addEventListener('mouseup', function (e) {
          // 解决mac电脑在鼠标右键后会执行mouseup事件
          if (e.button === 0) {
            menu.style.display = 'none' 
          }
        }, false)
      }
    },
    methods: {
      fnHandler (item) {
        this.$emit(item.fnHandler)
      }
    }
  }
</script>
<style>
  .vue-contextmenu-listWrapper {
    border-radius: 3px;
    overflow: hidden;
    display: none;
    position: fixed;
    z-index: 9999;
    top: 0;
    left: 0;
  }
  .vue-contextmenu-listWrapper .context-menu-list {
    width: 150px;
    height: 32px;
    border-radius: 4px;
    background: #F3F3F3;
    text-decoration: none;
    list-style: none;
  }
  .vue-contextmenu-listWrapper .context-menu-list button {
    cursor: pointer;
    width: 100%;
    height: 100%;
    display: block;
    outline: 0;
    border: 0;
  }
  .vue-contextmenu-listWrapper .context-menu-list button i,  .vue-contextmenu-listWrapper .context-menu-list button span {
    float: left;
  }
  .vue-contextmenu-listWrapper .context-menu-list button i{
    padding: 0 10px 0 10px;
  }
  .vue-contextmenu-listWrapper .context-menu-list button:hover {
    box-shadow: 0px 1px 3px rgba(34, 25, 25, 0.2);
    color: #ffffff;
    border-radius: 0px;
    background: -webkit-linear-gradient(bottom, #5a6a76 , #2e3940); /* Safari 5.1 - 6.0 */
    background: -o-linear-gradient(bottom, #5a6a76, #2e3940); /* Opera 11.1 - 12.0 */
    background: -moz-linear-gradient(bottom, #5a6a76, #2e3940); /* Firefox 3.6 - 15 */
    background: linear-gradient(to bottom, #5a6a76 , #2e3940);
  }
</style>
