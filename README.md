
# 药物警戒术语搜集系统-前端

前端部署(基于vue)

1 打开前端项目

2 安装前端项目所有组件

3 运行npm run serve

4 访问8080端口

此为前后端分离项目，所用技术栈为mongoDB+springboot+Vue

后端项目地址：[https://github.com/p1aCoder/CSPT-Backend](https://github.com/p1aCoder/CSPT-Backend)

该项目首先使用scrapy框架爬取某医学网站的疾病、药物以及使用禁忌等来构建知识图谱，并展示

![总体预览](https://github.com/p1aCoder/CSPT-Forend/blob/master/total.png)

可以输入药品或疾病进行搜寻，绿色节点即为搜索到的节点。

![搜索功能展示](https://github.com/p1aCoder/CSPT-Forend/blob/master/search.png)

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
