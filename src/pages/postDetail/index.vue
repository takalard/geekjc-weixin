<template>
  <view>
    <view class="postDetail">
    <view class="title">{{post.title}}</view>
    <view style='margin: 10px 0'>
      <view class="infoLeft">
        <image
          class="avatar"
          :src="post.author && post.author.avatar"
        />
        <view>
          <view>{{post.author && post.author.name}}</view>
          <view>{{post && post.meta && post.meta.updateAt}}
            <text style='margin-left: 5px'>{{post.pv}}</text>
          浏览</view>
        </view>
      </view>
      <button class="weui-btn mini-btn" type="default" size="mini" style='float: right;margin-top: 5px'>关注</button>
    </view>
    <view style="clear: both"><rich-text :nodes="nodes"></rich-text></view>
  </view>
    <div class="fixFooter flex-wrap row-flex">
      <input class="commentInput flex-wrap" style="flex:4" placeholder="发表评论" />
      <view class="flex-wrap page"><gicon type="comment" sy="margin-left: 10px;vertical-align:9px;font-size: 20px"></gicon></view>
      <view class="flex-wrap page"><gicon type="collect" sy="margin-left: 10px;vertical-align:9px;font-size: 20px"></gicon></view>
    </div>
  </view>
</template>

<script>
import { get } from '@/utils/request';
import gicon from '@/components/gicon';
import 'prismjs/themes/prism.css';
import MpvueMarkdownParser from 'mpvue-markdown-parser';
import 'mpvue-markdown-parser/dist/index.css';

export default {
  data() {
    return {
      post: {},
      nodes: {},
    };
  },

  components: {
    gicon,
  },

  methods: {
    getPostDetail(id) {
      get(`/fetch/post/list/${id}`).then((res) => {
        const result = res.data;
        const { post, md } = result;
        post.meta.updateAt = post.meta && post.meta.updateAt ? this.$moment(post.meta.updateAt).format('YYYY-MM-DD') : post.meta.updateAt;
        this.post = post;
        this.nodes = MpvueMarkdownParser(md);
      });
    },
  },

  created() {
  },
  onLoad(options) {
    const { id } = options;
    this.getPostDetail(id);
  },
};
</script>

<style scoped>
.postDetail{
  padding: 10px;
  border-top: 1px solid #ddd;
}
.title{
  text-align: center;
  font-size: 14px;
}
.infoLeft{
  width: 75%;
  float: left;
  color: #666;
  font-size: 12px
}
.avatar{
  float: left;
  width: 40px;
  height: 40px;
  background: #fff;
  border-radius: 40px;
  margin-right: 10px;
}
.fixFooter{
  position: fixed;
  height: 40px;
  line-height: 40px;
  bottom:0;
  background-color:#fff;
  width:100%;
  padding: 5px;
  box-shadow: 0 -2px 2px #999
}
.commentInput{
  width: 50%;
  height: 35px;
  min-height: 35px;
  line-height: 35px;
  font-size: 14px;
  border: 1px solid #ddd;
}
</style>
