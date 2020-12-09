<template>
  <div>
    <button @click="invokeHtmlMethod">调用html种方法</button>
    <div class="iframestyleset">
      <iframe
        name="iframeMap"
        id="iframeMapViewComponent"
        v-bind:src="getPageUrl"
        width="100%"
        :height="pageHeight"
        frameborder="0"
        scrolling="no"
        ref="iframeDom"
      ></iframe>
    </div>
  </div>
</template>

<script>
/*
  总结：
  1.iframe如果有两三页，height的高度要设置对应的200%或300%
  2.如果是本地HTML 则要放在static文件夹里
*/ 
export default {
  data() {
    return {
      pageHeight: '100%',
      pageCount: 3, // 这是pdf页面总共多少页
      // getPageUrl: "static/index.html",
      getPageUrl: "static/PDF/index.html",
    };
  },
  created() {
    // 初始化时为window绑定一个方法
    window["vueDefinedMyProp"] = (receiveParams) => {
      this.receiveParamsFromHtml(receiveParams);
    };

    window["getVueInitData"] = (callback) => {
      // 
      console.log("testVue method");
      let initData = {
        name: "From Vue",
      };
      callback(initData);
    };
  },
  mounted() {
    this.pageHeight = (this.pageCount * 100) + '%';
    console.log("pageHeight:", this.pageHeight);
    // 注意这里要加个延时，才不会报undefined的错
    // setTimeout(() => {
    //   let initData = {
    //     name: "Vue",
    //     content: "测试从Vue传数据给HTML页面",
    //   };
    //     // Vue 传递初始化的数据给HTML
    //   window.frames["iframeMap"].initHtmlData(initData);
    // }, 100);
  },
  methods: {
    receiveParamsFromHtml(res) {
      console.log(res);
    },
    // 1.点击Vue的按钮的时候主动调用HTML的方法获取HTML数据
    invokeHtmlMethod() {
      window.frames["iframeMap"].loadHtmlData((data) => {
        console.log("loadHtmlData:", data);
      });
    },
  },
};
</script>

<style>
.iframestyleset {
   height: 100vh;
}
</style>
