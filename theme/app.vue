<template>
  <div class="hello">
   <el-button type="primary" size="mini" @click="moren"
        >默认主题</el-button
      >
      <el-button type="danger" size="mini" @click="hei">暗黑</el-button>
      <el-color-picker
        v-model="color"
        show-alpha
        @change="zidingyi"
      ></el-color-picker> 
  </div>
</template>
 
 
<script>
import { setTheme } from "@/theme/styleAll";
export default {
  name: "HelloWorld",
data(){
return{
color:''
}
},
  mounted() {
    this.init(); // 初始化主题
  },
  methods: {
    init() {
      setTheme("default"); // 初始化未默认主题
    },
    // 更改为默认主题
    moren() {
      setTheme("default");
    },
    // 更改为暗黑主题
    hei() {
      setTheme("dark");
    },
    // 更改为自定义主题
    zidingyi() {
      let newPrimaryColor = this.color;
      localStorage.setItem("primaryColor", newPrimaryColor); // 将新的主题色存入本地
      localStorage.setItem("primaryTextColor", newPrimaryColor); // 将新的主题色存入本地
      setTheme();
    },
  },
};
</script>
<style scoped lang="less">
.hello {
background:@primaryColor;
    color: @primaryTextColor;
}
</style>