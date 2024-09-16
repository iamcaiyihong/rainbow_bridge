## 安装
### 安装方法
``brew install rocksdb``
### 验证
rocksdb_ldb --version 

## golang start
以下两个目前还没调试通过
* github.com/tecbot/gorocksdb
* github.com/linxGnu/grocksdb@v1.6.22
```go
package rocksdb

import (
	"testing"

	"github.com/tecbot/gorocksdb"
)

func TestRocksdb(t *testing.T) {
	// 配置 RocksDB Options
	opts := gorocksdb.NewDefaultOptions()
	opts.SetCreateIfMissing(true)

	打开数据库
	db, err := gorocksdb.OpenDb(opts, "test.db")
	if err != nil {
		log.Fatal(err)
	}
	defer db.Close()

	// 配置读写选项
	wo := gorocksdb.NewDefaultWriteOptions()
	ro := gorocksdb.NewDefaultReadOptions()

	// 插入数据
	err = db.Put(wo, []byte("key"), []byte("value"))
	if err != nil {
		log.Fatal(err)
	}

	// 查询数据
	value, err := db.Get(ro, []byte("key"))
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("读取到的值: %s\n", value.Data())
	value.Free()

	// 删除数据
	err = db.Delete(wo, []byte("key"))
	if err != nil {
		log.Fatal(err)
	}
}

```

