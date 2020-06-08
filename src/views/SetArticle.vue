<template>
<div class='setarticle'>
    <h1 class="">发布文章</h1>
    <b-form-input placeholder="请输入标题" class="title" v-model="title"></b-form-input>
     <b-form-group label="请选择分类：">
      <b-form-radio-group
        id="btn-radios-1"
        v-model="selected"
        :options="options"
        buttons
        name="radios-btn-default"
      ></b-form-radio-group>
    </b-form-group>
     <div>
         <div>
        <span>请选择封面：</span><input type="file" @change="upload($event)">
         </div>
    <br>
    <label for="tags-basic">输入后按回车键添加标签</label>
    <b-form-tags input-id="tags-basic" v-model="tags" class="mb-2"></b-form-tags>
    </div>

    <div ref="editor" class="editor">
    </div>

     <b-input-group size="size" class="search" prepend="相关游戏">
        <b-form-input placeholder="请输入游戏名" v-model="searchName" @keyup.enter="search"></b-form-input>
        <b-input-group-append>
        <b-button size="sm" text="Button" variant="success" @click="search" >搜索</b-button>
        </b-input-group-append>
    </b-input-group>

    <b-list-group v-if="searchList.length!==0">
        <p>请选择：</p>
        <b-list-group-item button @click="setName(item.attributes.name)"
        v-for="(item,index) in searchList" :key="index" class="result"
        >{{index+1}}·{{item.attributes.name}}</b-list-group-item>
    </b-list-group>
   <b-list-group-item v-if="gamename!==''">已选：{{gamename}}</b-list-group-item>
   <b-button  variant="primary" class="submit-btn" @click="submit" type="submit">发布</b-button>
</div>
</template>

<script>
import E from 'wangeditor'

//这里可以导入其他文件（比如：组件，工具js，第三方插件js，json文件，图片文件等等）
//例如：import 《组件名称》 from '《组件路径》';

export default {
     name: 'setArticle',
//import引入的组件需要注入到对象中才能使用
components: {},
data() {
//这里存放数据
return {
    searchName:'',
    title:'',
    gamename:'',
    searchList:[],
    tags:[],
    editorContent:'',
    img:{},
    options:[
        {text:'攻略',value:'攻略'},
        {text:'杂谈',value:'杂谈'},
        ],
    selected:'',
};
},
//监听属性 类似于data概念
computed: {},
//监控data中的数据变化
watch: {},
//方法集合
methods: {
      getContent: function () {
            console.log(this.editorContent)
        },
        search(){
            const query = new this.$av.Query('game');
            query.contains('name', this.searchName);
            query.find().then((res)=>{
                console.log("结果",res)
                this.searchList=res
            })
        },
        setName(e){
            this.gamename = e
            this.searchList = []
            this.searchName = ''
        },
        upload(e){
            console.log(e)
            let tempImg =  e.target.files[0]
            let fileName = e.target.files[0].name
            let file = new this.$av.File(fileName, tempImg);
            this.img = file
        },
        submit(){
            let Article = this.$av.Object.extend('information');
            let user = this.$av.User.current()
            console.log(user)
            let nickname = user.attributes.nickname
            let art = new Article()
            art.set({
                "article":this.editorContent,
                "title":this.title,
                "gamename":this.gamename,
                "img":this.img,
                "tags":this.tags,
                "author":nickname,
                "classify":this.selected
            })
            art.save().then((todo) => {
                // 成功保存之后，执行其他逻辑
            console.log(`保存成功。objectId：${todo.id}`);
            }, (error) => {
                console.log("错误",error)
            // 异常处理
            });
        }
},
//生命周期 - 创建完成（可以访问当前this实例）
created() {

},
//生命周期 - 挂载完成（可以访问DOM元素）
mounted() {
     var abc = new E(this.$refs.editor)
        abc.customConfig.onchange = (html) => {
          this.editorContent = html
        }
    abc.create()
},
beforeCreate() {}, //生命周期 - 创建之前
beforeMount() {}, //生命周期 - 挂载之前
beforeUpdate() {}, //生命周期 - 更新之前
updated() {}, //生命周期 - 更新之后
beforeDestroy() {}, //生命周期 - 销毁之前
destroyed() {}, //生命周期 - 销毁完成
activated() {}, //如果页面有keep-alive缓存功能，这个函数会触发
}
</script>
<style scoped>
.setarticle{
    background: #fff;
    display: flex;
    align-items: center;
    width: 100%;
    padding-top: 40px;
    flex-direction: column;
}
.title{
    margin: 20px 0;
    font-size: 32px;
    width: 500px;
}
.editor{
    width: 800px;
    margin-top: 40px;
    -ms-overflow-style: auto;
}

.search{
    width: 500px;
    margin: 20px 0;
}
.result{
    width: 400px;
    text-align: left;
}
.submit-btn{
    margin-top: 80px;
    width: 200px;
}

</style>