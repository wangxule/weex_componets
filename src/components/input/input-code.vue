<!--带图片验证码的input-->
<template>
    <div class="flex_row" :style="inputRowStyle">
        <image v-if="leftIconURL.length>0"
               :style="leftIconStyle"
               :src="leftIconURL"></image>
        <input type="text" class="input"
               @input="inputText"
               @focus="inputFocus"
               @blur="inputBlur"
               :maxlength="maxLength"
               :value="value"
               :style="inputStyle"
               :autofocus="autoFocus"
               :placeholder="placeholder"/>
        <div :style="clearIconStyle">
            <image v-if="clearIconURL.length>0 && showClearButton"
                   :style="clearIconStyle"
                   :src="clearIconURL"
                   @click="clearValue"></image>
        </div>
        <image :src="imageCodeURL"
               :style="codeStyle"
               @click="changeCode"></image>
    </div>
</template>
<script>
    import weexUtils from "../../utils/WeexUtils";

    export default {
        props: {
            leftIconURL: {
                default: ""
            },
            leftIconStyle: {
                default: {
                    width: "50px",
                    height: "50px",
                    marginRight: "30px"
                }
            },
            clearIconURL: {
                default: weexUtils.getResourcesURL("images/clear_icon.png", weex)
            },
            clearIconStyle: {
                default: {
                    width: "40px",
                    height: "40px",
                    marginRight: "30px"
                }
            },
            imageCodeURL:{
              default:""
            },
            codeStyle:{
              default:{
                  width:"192px",
                  height:"60px"
              }
            },
            placeholder: {
                default: ""
            },
            maxLength: {
                default: 4
            },
            inputRowStyle: {
                default: {
                    padding: "15px",
                    borderBottomWidth: "1px",
                    borderBottomColor: "#e6e6e6",
                    borderBottomStyle: "solid"
                }
            },
            inputStyle: {
                default: {
                    lineHeight: "60px",
                    height: "60px"
                }
            },
            initVal: {
                default: ""
            },
            checkValue: {
                default: false
            }
        },
        data() {
            let web=weex.config.env.platform.toLowerCase()==="web";
            return {
                web,
                value: "",
                showClearButton: false
            }
        },
        methods: {
            inputText({value}) {
                if (value.length > this.maxLength) {
                    this.value = new String(this.value);
                    return false;
                }
                this.setValue(value);

                //检查输入
                if (this.checkValue && this.checkValue.constructor === Function) {
                    this.checkValue(() => {
                        this.setValue("");
                    });
                }
            },
            inputFocus() {
                this.showClearButton = this.value.length > 0
            },
            inputBlur() {
                this.showClearButton = this.value.length > 0 && this.web;
            },
            clearValue() {
                this.setValue("");
            },
            setValue(val) {
                this.value = val;
                this.showClearButton = val.length > 0;
                this.$emit("input", this.value);  //触发input，事件向上传递
            },
            changeCode(){
                this.$emit("changeImageCode");
            },
        },
        beforeMount() {
            this.setValue(this.initVal);
        }
    }
</script>
<style scoped>
    .flex_row {
        flex-direction: row;
        justify-content: center;
        align-items: center;
    }

    .input {
        flex: 1;
        border-width: 0;
        color: #3f3c3c;
        placeholder-color: #919191;
    }
</style>