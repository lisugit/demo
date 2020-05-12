<template>
	<div>
		<input type="text" placeholder="输入文章标题" v-model="biaoti" required="required">
		<textarea style="resize:none" placeholder='畅所欲言'  name="" id="" cols="30" rows="10" v-model="neirong" required="required" ></textarea>
		<el-upload
		required="required"
		ref='upload'
		:on-change='shangchuan'
		:limit='1'
		accept="image/*"
		:auto-upload="false"
		:data="{ptime:this.date,title:this.biaoti,content:this.neirong,username:this.username,nic:this.usernic}"
		  action="http://112.126.89.87:80/wzcl"
		  list-type="picture-card"
		  :on-preview="handlePictureCardPreview"
		  :on-remove="handleRemove">
		  <i class="el-icon-plus"></i>
		</el-upload>
		<el-dialog :visible.sync="dialogVisible" size="tiny">
		  <img width="100%" :src="dialogImageUrl" alt="">
		</el-dialog>
		<mt-button @click='submitupload()'  type="primary" size="large">发布</mt-button>
	</div>
</template>

<script>
	import {Toast} from 'mint-ui'
	Date.prototype.format = function(fmt) {
	     var o = { 
	        "M+" : this.getMonth()+1,                 //月份 
	        "d+" : this.getDate(),                    //日 
	        "h+" : this.getHours(),                   //小时 
	        "m+" : this.getMinutes(),                 //分 
	        "s+" : this.getSeconds(),                 //秒 
	        "q+" : Math.floor((this.getMonth()+3)/3), //季度 
	        "S"  : this.getMilliseconds()             //毫秒 
	    }; 
	    if(/(y+)/.test(fmt)) {
	            fmt=fmt.replace(RegExp.$1, (this.getFullYear()+"").substr(4 - RegExp.$1.length)); 
	    }
	     for(var k in o) {
	        if(new RegExp("("+ k +")").test(fmt)){
	             fmt = fmt.replace(RegExp.$1, (RegExp.$1.length==1) ? (o[k]) : (("00"+ o[k]).substr((""+ o[k]).length)));
	         }
	     }
	    return fmt; 
	}       
	export default
	{
		data:function(){
			return{username:'null',usernic:'nic',pd:false,biaoti:null,neirong:null, dialogImageUrl:null,date:new Date().format("yyyy-MM-dd hh:mm:ss"),
        dialogVisible: false}
		},
		created(){this.m()},
		methods:{  	m:function(){//()=>的参数应该和{}内平级
				           //发送 post 请求
						  
				           this.$http.post('http://112.126.89.87:80/css',{yz:'yzz'},{withCredentials: true,emulateJSON:true}).then(function(res){
							   if(res.body.nic){this.usernic=res.body.nic;this.username=res.body.username};
				            if(res.body=='kong')
				           {
				           									 Toast('请您登录');
				           									 window.location.href="http://112.126.89.87:80/index/#/dl";
				           }
					                });
				            
			
			},as(){
			alert('sda');
		},handleRemove(file, fileList) {
        console.log(file, fileList);
		 this.pd=false;
      },
	  shangchuan(){
		  this.pd=true;
	  }
	  ,
      handlePictureCardPreview(file) {
		  
        this.dialogImageUrl = file.url;
        this.dialogVisible = true;
      },
	
	  submitupload()
	  {if(this.biaoti==null||this.neirong==null){Toast('请输入内容');return};
	if(this.pd==false){Toast('请传入描述图片');console.log();return};
		  this.$http.post('http://112.126.89.87:80/wzcl',{title:this.biaoti,content:this.neirong,ptime:this.date},{withCredentials: true,emulateJSON:true}).then(function(res){
		  								 console.log(res);
		  								if(res.body){
											console.log(res.body);
											Toast('发表成功（测试版）');
											window.location.href="http://112.126.89.87:80/index/#/home";
										}
		  								
		  				            
		   
		     },function(res){
		        Toast('连接失败');
		     });
		  this.$refs.upload.submit();//利用$refs//this.$refs.（ref值） 获取到的是dom元素
	  },
			cs(){
				alert(this.tp)//不要忘了加this
			}
		}
	}
	
</script>

<style scoped="scoped">
	input{
		border: 0px;width: 100%;
	}
	textarea{
		border: 0px;height:200px;width: 100%;
	}
</style>
