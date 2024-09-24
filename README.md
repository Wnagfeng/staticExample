# 文件上传和展示图片——前端

```
Write By CoderJoon
Time: 2024-09-25-05:06
```



| 环境类型     | 版本号   |
| ------------ | -------- |
| Node         | ^14.17.6 |
| `Elemenu—Ui` | ^2.12.0  |
| `Vue—Router` | ^3.0.1   |
| `Vue`        | ^2.5.2`  |
| `Npm`        | 6.14.15  |

关于图片的展示，很简单，但是zbf不干人事，他返回的图片不是http开头的图片链接，所以你在展示图片的时候，你一定要做一个url的拼接

```vue
 <div class="imglist">
        <img v-for="(img, index) in imageList" 
        :key="index" 
        :src="`http://localhost:3001/uploads/${img}`"
        class="image-item" />
  </div>
```

这里我就做了链接的拼接 `http://localhost:3001/uploads/xxx.jpg`，zbf返回的很有可能是一个图片名称，所以你需要使用baseurl加文件名称去展示图片，这里可以看看pc端的 shopOrder.vue里面的第八行代码 ，就是一个很好的案例！

如果你略懂Node以及http的知识，这点对于你来说就是小case。







