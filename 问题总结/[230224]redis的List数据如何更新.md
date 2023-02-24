# redis的List数据如何更新

项目中使用了redis进行缓存一些请求频率高的数据，只有请求很好解决，直接`lrange`取数据就可以。

但是列表中还需要对列表进行操作，同样需要对redis的list进行操作。

项目中使用的是`ioredis`:
```js
import redis from 'ioredis';

const client = new redis();
// 已知原始数据和要更改的新数据
const modifyListData = (key,oldData,newData)=>{
    try{
        // 先取出list所有数据
        const list = await redis.lrange(key,0,-1);
        // 查找到原始数据在list中的index
        const index = list.indexOf(oldData);
        if(index<0){
            console.log('没有查找到匹配值');
        }
        // 根据index对list进行更改新数据
        await redis.lset(key,index,newData)
    }catch(e){
        console.log('出现了错误', e);
    }
}
// 执行
await modifyList('listKey','this is my old data!','this is my old data!')
```

暂时只想到这种更新方法，后续有新想法会更新，欢迎大家提出意见！