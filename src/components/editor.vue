<template>
    <div>
        <div
        class="ql-editor ql-blank"
        id="myEditor"
        style="padding:10px;width: 100%;height: 100%;overflow-y: scroll;"
        contentEditable="true"
        data-placeholder="这里输入内容!"
        ></div>

        <div class="emotion f_emotion"    >

            <div class="show">
                <div style="margin-bottom:20px">
                    以下是插入图片：
                </div>
                <ul>
                    <li v-for="(item, index) in elist" class="vux-center-h" @click=" pick_emotion(index) " >
                        <emotion is-gif>{{item}}</emotion>
                    </li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script>

import {WechatEmotion as Emotion} from 'vux'

export default {
    name: 'editor-view',
    data() {
        return {
            editor:null,
            savedRange:null,
            doc:null,
            win:null,
            elist: ['微笑', '撇嘴', '色', '发呆', '得意', '流泪', '害羞', '闭嘴', '睡', '大哭', '尴尬', '发怒', '调皮', '呲牙', '惊讶', '难过', '酷', '冷汗', '抓狂', '吐', '偷笑', '可爱', '白眼', '傲慢', '饥饿', '困', '惊恐', '流汗', '憨笑', '大兵', '奋斗', '咒骂', '疑问', '嘘', '晕', '折磨', '衰', '骷髅', '敲打', '再见', '擦汗', '抠鼻', '鼓掌', '糗大了', '坏笑', '左哼哼', '右哼哼', '哈欠', '鄙视', '委屈', '快哭了', '阴险', '亲亲', '吓', '可怜', '菜刀', '西瓜', '啤酒', '篮球', '乒乓', '咖啡', '饭', '猪头', '玫瑰', '凋谢', '示爱', '爱心', '心碎', '蛋糕', '闪电', '炸弹', '刀', '足球', '瓢虫', '便便', '月亮', '太阳', '礼物', '拥抱', '强', '弱', '握手', '胜利', '抱拳', '勾引', '拳头', '差劲', '爱你', 'NO', 'OK', '爱情', '飞吻', '跳跳', '发抖', '怄火', '转圈', '磕头', '回头', '跳绳', '挥手', '激动', '街舞', '献吻', '左太极', '右太极']
        }
    },
    components: {
        Emotion
    },
    methods:{

        pick_emotion(key){

            var img = '<img src="https://res.wx.qq.com/mpres/htmledition/images/icon/emotion/'+key+'.gif" />'
            this.win.focus();
            this.setCaretPosition();
            this.doc.execCommand('insertHTML','',img)
            var self = this;
            setTimeout(function(){
                self.getCaretCharacterOffsetWithin();
            }, 100)
        },

        setCaretPosition(){
            var sel;
            if (this.savedRange != null) {

                sel = this.win.getSelection();
                if (sel.rangeCount > 0)
                    sel.removeAllRanges();
                sel.addRange(this.savedRange);
            }
            else if ( (sel = this.doc.selection) && sel.type != "Control")
            {
                this.win.getSelection().addRange(this.savedRange);
            }

        },

        getCaretCharacterOffsetWithin() {
            var caretOffset = 0;
            var sel;
            if (typeof this.win.getSelection != "undefined") {
                sel = this.win.getSelection();
                if (sel.rangeCount > 0) {
                    var range = this.win.getSelection().getRangeAt(0);
                    var preCaretRange = range.cloneRange();
                    preCaretRange.selectNodeContents(this.editor);
                    preCaretRange.setEnd(range.endContainer, range.endOffset);
                    caretOffset = preCaretRange.toString().length;
                    this.savedRange = range
                }
            } else if ( (sel = this.doc.selection) && sel.type != "Control") {
                var textRange = sel.createRange();
                var preCaretTextRange = this.doc.body.createTextRange();
                preCaretTextRange.moveToElementText(this.editor);
                preCaretTextRange.setEndPoint("EndToEnd", textRange);
                caretOffset = preCaretTextRange.text.length;
                this.savedRange = textRange
            }
            console.log('caretOffset:'+caretOffset)
            this.caretOffset = caretOffset;
        },


        setClick(ev){
            var self = this;
            //用settimeout 为了确保dom生效之后,再操作!
            setTimeout(function(){
                self.getCaretCharacterOffsetWithin();
            }, 10)

        },

        setTouchEnd(ev){
            var self = this;
            //用settimeout 为了确保dom生效之后,再操作!
            setTimeout(function(){
                self.getCaretCharacterOffsetWithin();
            }, 10)
        },

        /*keyup_editor(ev){
            this.getCaretCharacterOffsetWithin()
        },*/


        init() {
            var self =  this;
            var ifr = document.getElementById("myEditor");
            this.editor = ifr;
            this.doc = ifr.ownerDocument || ifr.document;
            this.win = this.doc.defaultView || this.doc.parentWindow;


            //设定健盘按下事件
            //ifr.addEventListener("keyup", this.keyup_editor);

            //设定鼠标点击事件,如果你只用于移动端,忽略这个。
            ifr.addEventListener("click", this.setClick);

            //移动端 触屏事件
            ifr.addEventListener("touchend", this.setTouchEnd);

            //绑定输入事件
            ifr.oninput = function(){

                if(self.editor.innerHTML!=''){
                    self.editor.className = 'ql-editor';
                }else{
                    self.editor.className = 'ql-editor ql-blank';
                }
                self.getCaretCharacterOffsetWithin()
            }
            //以上的几个事件,都是绑定了 getCaretCharacterOffsetWithin() 这个方件;

            //设置焦点, 用settimeout 为了dom生效之后,再操作.
            setTimeout(function(){
                document.getElementById("myEditor").focus();
                self.getCaretCharacterOffsetWithin()
            }, 100)

        }
    },
    mounted:function(){
        this.init();
    }
}
</script>


<style type="text/css">
.f_emotion {
    padding-bottom: 5px;
    position: relative!important;
    border: none!important;
    width: 100%!important;
    top: 0!important;
    left: 0!important;
}

.f_emotion li {
    width: 40px;
    height: 40px;
    display: inline-block;
    text-align: center;
}

.ql-editor.ql-blank::before {
  color: rgba(0,0,0,0.6);
  content: attr(data-placeholder);
  font-style: italic;
  left: 15px;
  pointer-events: none;
  position: absolute;
  right: 15px;
}
</style>
