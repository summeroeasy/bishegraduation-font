<template>
  <div class="content">
    <div class="text main-text">欢迎使用 {{ this.$project.projectName }}</div>
    <el-card class="poem-card" shadow="hover">
      <div slot="header" class="poem-header">
        <p>落霞与孤鹜齐飞，秋水共长天一色。</p>
        <p>渔舟唱晚，响穷彭蠡之滨，雁阵惊寒，声断衡阳之浦。</p>
      </div>
      <el-divider></el-divider>
      <div class="poem-content">
        随着夕阳的余晖和孤鹜的共舞，湖面与天空融为一色。在这宁静的晚歌中，渔舟轻唱激起心灵的波澜，寒雁横空而过，回声在远方消散。
        在知识的海洋中，让我们同舟共济，探寻学术的深度，一同绘制属于我们的未来。
      </div>
    </el-card>
    <div class="current-time">{{ currentTime }}</div>
  </div>
</template>

<script>
import router from '@/router/router-static'
export default {
  data() {
    return {
      currentTime: new Date().toLocaleString()
    };
  },
  mounted() {
    this.init();
    this.updateTime();
  },
  methods: {
    init() {
      if (this.$storage.get('Token')) {
        this.$http({
          url: `${this.$storage.get('sessionTable')}/session`,
          method: "get"
        }).then(({ data }) => {
          if (data && data.code != 0) {
            router.push({ name: 'login' })
          }
        });
      } else {
        router.push({ name: 'login' })
      }
    },
    updateTime() {
      setInterval(() => {
        this.currentTime = new Date().toLocaleString();
      }, 1000);
    }
  }
};
</script>

<style lang="scss" scoped>
// 引入动画库，例如animate.css
@import '~animate.css';

// 定义色彩板
$color-background: #f8f9fa;
$color-primary-text: #343a40;
$color-secondary-text: #6c757d;
$color-accent: #007bff;
$color-poem-background: #e9ecef;
$color-header: #28a745;

.content {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  min-height: 100vh;
  background-color: $color-background;
  padding: 1rem;

  .main-text {
    font-size: 3rem;
    color: $color-primary-text;
    margin-bottom: 1rem;
    animation: fadeInDown; // 添加动画效果
    animation-duration: 2s;
  }

  .current-time {
    color: $color-secondary-text;
    margin-top: 1rem;
    animation: fadeIn; // 添加动画效果
    animation-duration: 2s;
    font-size: 1.25rem;
  }

  .poem-card {
    margin-top: 1rem, 0;
    padding: 2rem;
    border-radius: 0.5rem;
    background-color: $color-poem-background;
    box-shadow: 0 0.5rem 1rem rgba(0, 0, 0, 0.15);
    width: 100%;
    max-width: 800px;
    animation: fadeInUp; // 添加动画效果
    animation-duration: 2s;

    .poem-header {
      color: $color-header;
      font-size: 1.5rem;
    }

    .poem-content {
      padding: 1rem;
      font-size: 1.25rem;
      line-height: 1.6;
      color: $color-secondary-text;
      animation: fadeIn 3s ease-in-out;
    }

    .poem-header {
      font-weight: bold;
      color: $color-header;
      animation: fadeIn 2s ease-in-out;
    }
  }

  .poem-card {
    transition: transform 0.3s ease-in-out, box-shadow 0.3s ease-in-out;

    &:hover {
      transform: translateY(-5px);
      box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
    }
  }

  @keyframes fadeIn {
    from {
      opacity: 0;
    }

    to {
      opacity: 1;
    }
  }

  @keyframes fadeInDown {
    0% {
      opacity: 0;
      transform: translateY(-50px);
    }

    100% {
      opacity: 1;
      transform: translateY(0);
    }
  }

  @keyframes fadeInUp {
    0% {
      opacity: 0;
      transform: translateY(50px);
    }

    100% {
      opacity: 1;
      transform: translateY(0);
    }
  }
}
</style>