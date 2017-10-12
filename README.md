#### 设置contentEditable="true"作为文本编辑器，定位光标解决办法



html部分

```html
<div class="ql-editor ql-blank" id="myEditor" style="padding:10px;width: 100%;height: 100%;overflow-y: scroll;" contentEditable="true" data-placeholder="这里输入内容!"></div>
```

<!-- more -->

设置焦点

```js
setCaretPosition() {
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
```





获取焦点

```javascript
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
    this.caretOffset = caretOffset;
}
```





源代码使用Vue2 + Vux

为了方便测试，源代码调用了Vux的WechatEmotion组件，在编辑器中插件图片。
