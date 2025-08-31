<template>
  <div class="project">
    <Banner
      type="page"
      title="我的项目"
      desc="想起来就干一干"
      footer="修 BUG 小能手"
      image="https://pic.efefee.cn/uploads/2024/02/27/65dd8dae6043c.webp"
    >
      <template #footer-slot>
        <a class="to-github" href="https://github.com/80yearoldboy/" target="_blank">
          <i class="iconfont icon-github"></i>
          <span>前往 Github</span>
        </a>
      </template>
    </Banner>
  </div>
</template>

<script setup>
import Banner from "@/components/Banner.vue";
</script>

<style lang="scss" scoped>
.project {
  .banner-page {
    .to-github {
      height: 40px;
      padding: 0 16px;
      display: flex;
      align-items: center;
      justify-content: center;
      border-radius: 50px;
      color: #fff;
      background-color: var(--main-dark-opacity);
      backdrop-filter: blur(20px);
      transition:
        color 0.3s,
        background-color 0.3s;
      .iconfont {
        margin-right: 8px;
        transition: color 0.3s;
      }
      &:hover {
        color: var(--main-card-background);
        background-color: var(--main-color);
        .iconfont {
          color: var(--main-card-background) !important;
        }
      }
    }
  }
}
</style>
