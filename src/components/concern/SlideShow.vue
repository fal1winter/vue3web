<template>
  <!-- autoplay 自动切换的同时滚动页面有重影 -->
  <a-carousel class="slideshow-content" arrows>
    <template #prevArrow>
      <div
          slot="prevArrow"
          slot-scope="props"
          class="custom-slick-arrow"
          style="left: 10px;zIndex: 1"
      >
        <!-- <a-icon type="left-circle"/> -->
        <LeftCircleOutlined />
      </div>
    </template>
    <template #nextArrow>
      <div slot="nextArrow" slot-scope="props" class="custom-slick-arrow" style="right: 10px">
        <!-- <a-icon type="right-circle"/> -->
        <RightCircleOutlined />
      </div>
    </template>
    <!-- 直接设定已知的图片 URL -->
    <img src="https://xyzh.xidian.edu.cn/__local/2/F3/FF/E647AC783244A898FF2C3C35C38_B2EA259B_42DF7.jpg" alt="轮播图1" />
    <img src="https://hwl-1304734263.cos.ap-shanghai.myqcloud.com/upload/image/2024/05/24/4ee7b9c1-b075-4370-8186-e7866d12c4ad.jpg" alt="轮播图2" />
    <img src="https://pic.rmb.bdstatic.com/cms/15557bff8fd77b5a390beff14983263d_1675408258.3209_383.jpeg" alt="轮播图3" />
  </a-carousel>
</template>

<script>
  import { LeftCircleOutlined, RightCircleOutlined } from '@ant-design/icons-vue';

  export default {
    data() {
      return {
        // carouselData: [], // 注释掉不需要的数据属性
        finish: false,
      }
    },
    components: {
      LeftCircleOutlined,
      RightCircleOutlined,
    },
    methods: {
      // 获取走马灯
      // getCarouselList() { // 注释掉不需要的方法
      //   carouselService.getCarouselList()
      //       .then(res => {
      //         this.carouselData = res.data;
      //         this.finish = true;
      //       })
      //       .catch(err => {
      //         this.finish = true;
      //         this.$message.error(err.desc);
      //       });
      // },
    },

    mounted() {
      // this.getCarouselList(); // 注释掉不需要的生命周期钩子
    }

  };
</script>

<style scoped>
  .ant-carousel >>> .custom-slick-arrow {
    width: 50px;
    height: 50px;
    font-size: 50px;
    color: #fff;
    opacity: 0.3;
  }

  .ant-carousel >>> .custom-slick-arrow:before {
    display: none;
  }

  .ant-carousel >>> .custom-slick-arrow:hover {
    opacity: 0.5;
  }
</style>