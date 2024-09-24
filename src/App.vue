<template>
  <div class="Wrapper">
    <h1 style="text-align: center;">静态资源解决方案</h1>
    <div class="exampleone" style="text-align: center;">
      <h3>图片上传实例</h3>
      <el-upload class="avatar-uploader" action="http://localhost:3001/upload" name="file" :show-file-list="false"
        :on-success="handleAvatarSuccess" :before-upload="beforeAvatarUpload">
        <img v-if="imageUrl" :src="imageUrl" class="avatar" />
        <i v-else class="el-icon-plus avatar-uploader-icon"></i>
      </el-upload>
    </div>

    <div class="exampleone" style="text-align: center;">
      <h3>图片的展示示例</h3>
      <el-button type="success" @click="fetchImageList">请求图片</el-button>
      <div class="imglist">
        <img v-for="(img, index) in imageList" :key="index" :src="`http://localhost:3001/uploads/${img}`"
          class="image-item" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      imageUrl: '',
      imageList: [] // 用于存储图片名称的数组
    };
  },
  methods: {
    handleAvatarSuccess(res, file) {
      console.log(res, file);
      if (res.code === 200) {
        this.imageUrl = res.url;
        this.$message.success('上传成功');
        this.fetchImageList()
      }
    },
    beforeAvatarUpload(file) {
      const isJPG = file.type === 'image/jpeg';
      const isLt2M = file.size / 1024 / 1024 < 2;

      if (!isJPG) {
        this.$message.error('上传头像图片只能是 JPG 格式!');
      }
      if (!isLt2M) {
        this.$message.error('上传头像图片大小不能超过 2MB!');
      }
      return isJPG && isLt2M;
    },
    fetchImageList() {
      axios.get('http://localhost:3001/imglist')
        .then(response => {
          if (response.data.code === 200) {
            this.imageList = response.data.images; // 更新图片名称列表
          } else {
            this.$message.error('获取图片列表失败');
          }
        })
        .catch(error => {
          console.error(error);
          this.$message.error('请求发生错误');
        });
    }
  }
}
</script>

<style lang="scss">
.Wrapper {
  padding: 10px;
  box-sizing: border-box;
}

.exampleone {
  margin-top: 20px;
  border: 1px solid #eee;

  .avatar-uploader .el-upload {
    border: 1px dashed #d9d9d9;
    border-radius: 6px;
    cursor: pointer;
    position: relative;
    overflow: hidden;
  }

  .avatar-uploader .el-upload:hover {
    border-color: #409EFF;
  }

  .avatar-uploader-icon {
    font-size: 28px;
    color: #8c939d;
    width: 178px;
    height: 178px;
    line-height: 178px;
    text-align: center;
  }

  .avatar {
    width: 178px;
    height: 178px;
    display: block;
  }

  .imglist {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;

    .image-item {
      margin: 10px;
      width: 150px;
      /* 或者你想要的其他宽度 */
      height: 150px;
      /* 或者你想要的其他高度 */
      object-fit: cover;
      /* 确保图片不会变形 */
    }
  }
}
</style>
