<template>
  <div id="mixed-keyboard-box" >
    <div id="keyboard-pad">
      <row-view v-for="(row, index) in keyboard" :keyboardIndex="selected" :key="index" :keycount="keycount" :row="row" @onDelEvent="handleDelEvent" @onKeyClick="handleKeyClick" @onKeyEvent="handleKeyEvent"/>
      <!-- <div
        id="keytip" 
        class="r-border"
        v-show="tipText !== ''"
        :style="{'left': tipPosX, 'top': tipPosY}"
      >
        
        {{ tipText }}
      </div> -->
    </div>
  </div>
 
</template>
<script>
import rowView from './components/keyboard-row'

export default {
  name: 'keyboard',
  components: {
    rowView
  },
  props: {
    keycount:{
      type: Number,
      default: 0
    },
    keyboard: {
      type: Object,
      default: ()=>({})
    },
    selected: {
      type: Number,
      default: 0
    }
  },
  data() {
    return {
      tipText: "",
      tipPosX: "0px",
      tipPosY: "0px",
    }
  },
  methods: {
    handleKeyEvent(evt, key) {
      var self = this;
      var _reset = function(){
        self.tipText = "";
      };
      if(key.enabled && key.text !== 'del'){
          this.tipText = key.text;
          var dom = evt.toElement;
          // 60px 是tooltip的固定宽度
          // 62px 是tooltip的固定高度 + 间隔
          this.tipPosX = (dom.offsetLeft - Math.abs(60 - dom.clientWidth)/4 ) + "px";
          this.tipPosY = (dom.offsetTop - 62) + "px";
          setTimeout(_reset, 250);
          // this.$emit('change')
      }else{
          _reset();
      }
    },
    handleKeyClick(key) {
      this.$emit('getKey', key)
    },
    handleDelEvent() {
      this.$emit('delKey')
    }
  }
}
</script>
<style>
#mixed-keyboard-box {
  height: 100%;
}
[role="button"], input[type="button"], button { 
    -webkit-box-sizing: content-box; 
    box-sizing: content-box; 
    background: none;
    border: 0;
    line-height: normal;
    overflow: visible;
    padding: 0;
    -webkit-appearance: none;
    -webkit-user-select: none;
    user-select: none;
}

/*键盘容器*/
div#single-keyboard-box, div#mixed-keyboard-box{
    /*通过Border来设置外边距。颜色与背景色相同*/
    border: 5px solid #eef0f4;
    background: #eef0f4;
    box-sizing: border-box;
}

.r-border {
    border-radius: 4px;
    /*低版本Android WebView兼容性处理*/
    border-top-left-radius: 4px;
    border-top-right-radius: 4px;
    border-bottom-left-radius: 4px;
    border-bottom-right-radius: 4px;
    background-color: #FFFFFF;
    box-sizing: border-box;
}

div#keyboard-pad{
  height: 100%;
}

/*按键提示*/
div#keytip{
    position: absolute;
    background: white;
    margin: 0 auto;
    width: 50px;
    height: 60px;
    line-height: 60px;
    color: #418af9;
    font-size: 28px;  
    text-align: center;
    vertical-align: middle;
    border-radius: 10px;
    border: 1px solid #cccccc; 
    box-shadow: 0 1px 2px #DDDDDD;
}

/*统一按钮样式*/
button.key {
    color: black;
    margin: 0;
    padding: 0;
    width: 100%;
    height: 100% !important;
    text-align: center;
    vertical-align: middle;
    overflow: hidden;
    outline: none;
    line-height: 100%;
}

button.txt-key {
    border: 0;
    box-shadow: 0 1px 2px #DDDDDD;
}

button.disabled, button.key:disabled {
    color: #DDDDDD;
}

button.key:active:not(:disabled) {
    background-color: #b9c2cf;
    color: #ffffff;
}

@media all and (min-width:720px){ button.key { font-size: 6.5vw; } }
@media all and (min-width:640px){ button.key { font-size: 5.5vw; } }
@media all and (max-width:480px){ button.key { font-size: 4.5vw; } }
@media all and (max-width:360px){ button.key { font-size: 4vw; } }

</style>