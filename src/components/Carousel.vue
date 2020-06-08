<template>
  <div>
    <b-carousel id="carousel" controls indicators>
      <b-carousel-slide :caption="item.title" :img-src="item.url" v-for="(item,index) in carouselItems" :key="index">
        <p>
          {{item.info}}
        </p>
      </b-carousel-slide>
    </b-carousel>
      
</div>
</template>

<script>
export default {
  data(){
    return{
      carouselItems:[
        {title:"尼尔机械纪元正在促销",
        info:"《NieR:Automata》讲述了机器人 2B、9S 和 A2 的故事，以及他们为夺回遭强大机械生命体占领、由机器驱动的反乌托邦而进行的战斗。",
        url:"http://lc-3l8A8UM9.cn-n1.lcfile.com/0ecaf40ce096d07c7bf3/1.jpg"},
        {title:"战地5 Origin低至三折",
        info:"加入《战地风云 5》，体验人类史上最大的战争，并随著《战地风云》系列一起追本溯源，以前所未见的角度重临二次世界大战。",
        url:"http://lc-3l8A8UM9.cn-n1.lcfile.com/cf34c9f8a0c3eadc2017/2.jpg"},
        {title:"Control现已登录EPIC平台",
        info:"在纽约的一个秘密机构被异世界力量入侵之后，您成为了新任局长，并努力重新Control(掌控)局面。 由开发商 Remedy Entertainment 打造的这个超自然的第三人称动作冒险游戏将挑战您掌握超自然能力、可修改的枪械射击和互动式环境的组合，在一个无法预测的神秘世界中杀出一条生路。",
        url:"http://lc-3l8A8UM9.cn-n1.lcfile.com/e799db0b857b4c6f9731/3.jpg"},
        {title:"无主之地3正在热销",
        info:"射击夺宝游戏的鼻祖载誉归来，为您带来无穷无尽的枪械和一场躁动无限的冒险！在全新的世界中，与疯狂的新型敌人展开殊死之战，反抗整个星系中最冷酷无情的邪教头目，拯救你的家园。",
        url:"http://lc-3l8A8UM9.cn-n1.lcfile.com/4aa5e098d9ac9ddbf875/4.png"},
        ]
    }
  },
     
}
</script>

<style>
.carousel-caption{
  display:flex;
  align-items: center;
  justify-content: space-between;
  flex-direction: column;
}
.carousel-caption h3{
  background: rgba(1, 1, 1, 0.3);
}
.carousel-caption p{
  width: 800px;
  background: rgba(1, 1, 1, 0.3);
}
.carousel-item{
  height:500px;
}
li:focus{ outline:none; }
</style>