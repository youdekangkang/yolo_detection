<template>
  <el-card style="display: flex; justify-content: center; margin: 50px">
    <iframe src="http://127.0.0.1:5003/camera"
            frameborder="no"
            scrolling="no"
            width="500"
            height="800"
    ></iframe>
  </el-card>
</template>

<script>
import axios from "axios";

export default {
  name: "Content",
  data() {
    return {
      server_url: "http://127.0.0.1:5003",
      activeName: "first",
      active: 0,
      centerDialogVisible: true,
      url_1: "",
      url_2: "",
      textarea: "",
      srcList: [],
      srcList1: [],
      feature_list: [],
      feature_list_1: [],
      feat_list: [],
      url: "",
      visible: false,
      wait_return: "等待上传",
      wait_upload: "等待上传",
      loading: false,
      table: false,
      isNav: false,
      showbutton: true,
      percentage: 0,
      fullscreenLoading: false,
      opacitys: {
        opacity: 0,
      },
      dialogTableVisible: false,
    };
  },
  created: function () {
    document.title = "yolov5 - 检测平台 - 视频检测";
  },
  methods: {
    true_upload() {
      this.$refs.upload.click();
    },
    true_upload2() {
      this.$refs.upload2.click();
    },
    next() {
      this.active++;
    },
    // 获得目标文件
    getObjectURL(file) {
      var url = null;
      if (window.createObjcectURL != undefined) {
        url = window.createOjcectURL(file);
      } else if (window.URL != undefined) {
        url = window.URL.createObjectURL(file);
      } else if (window.webkitURL != undefined) {
        url = window.webkitURL.createObjectURL(file);
      }
      return url;
    },
    // 上传文件
    update(e) {
      this.percentage = 0;
      this.dialogTableVisible = true;
      this.url_1 = "";
      this.url_2 = "";
      this.srcList = [];
      this.srcList1 = [];
      this.wait_return = "";
      this.wait_upload = "";
      this.feature_list = [];
      this.feat_list = [];
      this.fullscreenLoading = true;
      this.loading = true;
      this.showbutton = false;
      let file = e.target.files[0];
      this.url_1 = this.$options.methods.getObjectURL(file);
      let param = new FormData(); //创建form对象
      param.append("file", file, file.name); //通过append向form对象添加数据
      var timer = setInterval(() => {
        this.myFunc();
      }, 30);
      let config = {
        headers: {"Content-Type": "multipart/form-data"},
      }; //添加请求头
      axios
          .post(this.server_url + "/upload", param, config)
          .then((response) => {
            this.percentage = 100;
            clearInterval(timer);
            this.url_1 = response.data.image_url;
            this.srcList.push(this.url_1);
            this.url_2 = response.data.draw_url;
            this.srcList1.push(this.url_2);
            this.fullscreenLoading = false;
            this.loading = false;

            this.feat_list = Object.keys(response.data.image_info);

            for (var i = 0; i < this.feat_list.length; i++) {
              response.data.image_info[this.feat_list[i]][2] = this.feat_list[i];
              this.feature_list.push(response.data.image_info[this.feat_list[i]]);
            }

            this.feature_list.push(response.data.image_info);
            this.feature_list_1 = this.feature_list[0];
            this.dialogTableVisible = false;
            this.percentage = 0;
            this.notice1();
          });
    },
    myFunc() {
      if (this.percentage + 33 < 99) {
        this.percentage = this.percentage + 33;
      } else {
        this.percentage = 99;
      }
    },
    drawChart() {
    },
    notice1() {
      this.$notify({
        title: "预测成功",
        message: "点击图片可以查看大图",
        duration: 0,
        type: "success",
      });
    },
  },
  mounted() {
    this.drawChart();
  },
};
</script>


