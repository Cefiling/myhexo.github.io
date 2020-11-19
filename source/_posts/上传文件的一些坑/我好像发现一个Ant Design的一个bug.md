# 我好像发现一个Ant Design的一个bug



​		今天使用Ant Design上传多个图片时,发现了一个问题。情景如下：

​		如果我上传多个图片后，突然想删除其中的两个图片怎么办。首先我用一个数组存储图片的名字，每次用户点删除的时候，就根据该图片的名字找这个图片的下标，再根据下标删除图片。

​		看起来是一个没什么问题的想法，但实际上缺无法实现。

​		我们首先用官方文档测试

```javascript
handleChange(info) {
                if (info.file.status !== 'uploading') {
                    console.log(info.file, info.fileList);
                }
    			//每上传成功一张图，将图片名添加到img_list中
                if (info.file.status === 'done') {
                    this.img_list.push(info.file['name'])
                    this.$message.success(`${info.file.name} file uploaded 							successfully`);
                } else if (info.file.status === 'error') {
                    this.$message.error(`${info.file.name} file upload failed.`);
                }

            },
handleFileRemove(file) {
                const name = file.name;
                console.log(name)
    			// 根据图片名查询img_list中的下标
                var i = this.img_list.indexOf(name)
                 // 根据下标删除img_list中的
                this.img_list.splice(i,1)
 			   console.log(img_list)
    			// 删除上传至本地的图片
                this.axios.delete('xxxxxxxxx/?name=' + name).then((res) => {
                })
            },
```

​		上面这个方法看起来没什么问题，甚至每次打印出img_list都是删除过图片后的img_list

​		但是，我们在其他地方在执行一次`console.log(img_list)`的时候会发现，它打印的还是最初添加完图片后的`img_list`，就好像我们在`data()`中申明的`img_list`好像在`handleFileRemove（）`方法中变成了另一个数组，虽然在方法内成功删除，但是在其他任何地方用`console.log(img_list)`的时候，都是没有删除图片后的`img_list`。

​		这应该是`Ant Design`中，上传控件的一个`bug`，不过既然知道这是个`bug`，就要想办法去解决。

​		首先，我们在删除方法内执行删除不行，那我们就在其他地方执行删除，所以想到其他方法。

```javascript
handleFileRemove(file) {
                const name = file.name;
                console.log(name)
    			// 根据图片名查询img_list中的下标
                var i = this.img_list.indexOf(name)
                 // 将下标添加到remove_list中
                this.remove_list.push(i)
                this.axios.delete('xxxxxxxxx/?name=' + name).then((res) => {
                })
            },
```

​		既然不能删，那我们就存，先定义一个`remove_list`，存储需要删除图片的下标，最后在上传后端的时候，在根据`remove_list`去删除`img_list`中的图片。

```javascript
sendmsg: function () {
                // 遍历remove_list
                for (var i=0;i<this.remove_list.length;i++){
                    console.log(this.remove_list[i])
                    console.log(this.img_list)
                    // 根据下标删除图片
                    delete this.img_list[this.remove_list[i]]
                }
                this.axios.post('xxxxxxxxx/, this.qs.stringify({
                    'img_list':JSON.stringify(this.img_list)
                })).then((res) => {
                    console.log(res.data);
                });
            },
```

​		这个问题终于是解决了，不过还要补充一个小坑：

​		删除时尽量用`delete`。

​		原因呢，首先要区别`delete`和`splice`的区别 

​		1、*delete 删除后，数组长度不变，被删除的数据变为null*

​		2、*splice 删除后，数组长度会随之减一，被删除的数据会清除*

​		当我用`splice`删除时，数组长度会减一，那么你数组中的下标也会随之改变，最终就会造成删错数据或者少删的问题。用`delete`删除时，数组长度不会改变，下标自然就不会改变。