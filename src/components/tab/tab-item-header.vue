<!--tab 切换的-头部-->
<template>
    <div class="flex_row flex_center">
        <div class="flex_cell flex_center" v-for="(item,i) in items"
             :key="i"
             @click="changeTab(i)"
             :style="item.style">
            <text :style="item.textStyle">{{item.text}}</text>
        </div>
    </div>
</template>
<script>

   // import {broadcast} from "wxp_weex_componets/src/utils/ExportWeexModel";

    export default {
        props:{
            items:{default:[]},
            defaultStyle:{
                default:{
                    container:{
                        backgroundColor:"#ffffff"
                    },
                    text:{
                        fontSize:"32px",
                        color:"#303030"
                    }
                }
            },
            selectedStyle:{
                default:{
                    container:{
                        backgroundColor:"green"
                    },
                    text:{
                        fontSize:"32px",
                        color:"#FFFFFF"
                    }
                }
            },
            selectedIndex:{default:0}
        },
        data(){
            return {};
        },
        methods:{
            //点击改变tab项
            changeTab(i){
                if(this.selectedIndex===i){
                    return;
                }
                this.changeStyle(this.selectedIndex,i);
                this.$emit("changeTabItem",{
                    currentIndex:this.selectedIndex,
                    index:i,
                });
            },
            /**
             *
             * @param selected 当前选择中的
             * @param next 下一个选中的
             */
            changeStyle(selected,next){
                this.items[next].style=Object.assign({},this.selectedStyle.container);
                this.items[next].textStyle=Object.assign({},this.selectedStyle.text);
                this.items[selected].style=Object.assign({},this.defaultStyle.container);
                this.items[selected].textStyle=Object.assign({},this.defaultStyle.text);
                this.items=Object.assign([],this.items);
            }
        },
        created(){
            this.items.forEach((item,i)=>{
               if(i===this.selectedIndex){
                   item.style=Object.assign({},this.selectedStyle.container);
                   item.textStyle=Object.assign({},this.selectedStyle.text);
               }else {
                   item.style=Object.assign({},this.defaultStyle.container);
                   item.textStyle=Object.assign({},this.defaultStyle.text);
               }
            });

//            //注册事件
//            broadcast.register("tab_item_header","change",({data})=>{
//                if(this.selectedIndex!==data){
//                    this.changeStyle(this.selectedIndex,data);
//                    this.$emit("changeTabIndex",{
//                        currentIndex:this.selectedIndex,
//                        index:data
//                    });
//                }
//            });
        }
    }
</script>
<style scoped>
    .flex_row {
        flex-direction: row;
    }

    .flex_center {
        justify-content: center;
        align-items: center;
    }
    .flex_cell{
        flex: 1;
    }
</style>