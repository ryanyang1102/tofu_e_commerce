<template>
  <div>
    <Carousel />
    <section class="container mb-8 mb-md-12">
      <div class="flex-center flex-column">
        <h3 class="h1 text-darkGray mb-2">我們的堅持</h3>
        <p class="mb-2">每日現做，只想給您最健康的品質</p>
        <img class="d-block mb-4" src="@/assets/image/soy.png" alt="soy_pic">
        <ul class="row">
          <li class="col-12 col-md-4">
            <div class="flex-center flex-column mx-6 mx-md-2 mt-8 mt-md-4">
              <img class="d-block mb-2" src="@/assets/image/introduce_sm01.jpg" alt="introduce_sm-pic01">
              <h4 class="text-darkGray fz-5 mb-2">嚴選食材</h4>
              <p class="text-lightGray fz-2">採用有機種植與友善耕作大豆，除了尊重生命、友善大地，更回歸『真食物』的本質。</p>
            </div>
          </li>
          <li class="col-12 col-md-4">
            <div class="flex-center flex-column mx-6 mx-md-2 mt-8 mt-md-4">
              <img class="d-block mb-2" src="@/assets/image/introduce_sm02.jpg" alt="introduce_sm-pic02">
              <h4 class="text-darkGray fz-5 mb-2">天然深海鹽鹵</h4>
              <p class="text-lightGray fz-2">天然鹽鹵能完整呈現大豆風味，不僅工法繁複、技術層面高，更仰賴師傅經年累月的經驗。</p>
            </div>
          </li>
          <li class="col-12 col-md-4">
            <div class="flex-center flex-column mx-6 mx-md-2 mt-8 mt-md-4">
              <img class="d-block mb-2" src="@/assets/image/introduce_sm03.jpg" alt="introduce_sm-pic03">
              <h4 class="text-darkGray fz-5 mb-2">每日現做的堅持</h4>
              <p class="text-lightGray fz-2">堅持日日現作，道道工序不偷懶，健康新鮮美味才是根本。</p>
            </div>
          </li>
        </ul>
      </div>
    </section>
    <div class="activeBg parallax_01" id="parallax_01" :style="{'background-position-y': positionY1+'px'}"></div>
  </div>
</template>

<script>
import Carousel from '@/components/frontend/Carousel.vue'

export default {
  name: 'Index',
  data () {
    return {
      // positionY1: 0,
      // Y1: 0,
      // ratio: 0.5
    }
  },
  components: {
    Carousel
  }
  // methods: {
  //   handleScroll () {
  //     const scrollTop = window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop
  //     this.positionY1 = this.Y1 - scrollTop * this.ratio
  //   }
  // },
  // mounted () {
  //   window.addEventListener('scroll', this.handleScroll)
  //   const bgScroll = document.getElementById('parallax_01')
  //   this.Y1 = bgScroll.offsetTop * this.ratio
  // }
}
</script>
